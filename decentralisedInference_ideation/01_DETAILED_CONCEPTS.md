# Detailed Concepts: Technical Deep-Dives

This document expands on concepts from `00_OVERVIEW.md` with technical depth and mathematical foundations.

---

## 1. Head-Sharding Architecture


### What Is It?

Instead of distributing complete layers of the neural network across nodes, we distribute the **attention heads** within each layer.

Transformer models use **multi-head attention** - the hidden state is split into multiple "heads" that each learn different patterns. For TinyLlama:
- Hidden dimension: 2048
- Number of heads: 16
- Dimensions per head: 2048 ÷ 16 = **128**


### Why Does This Fit LoRa?

**The Mathematics:**
```
One head's output = 128 float values
With 8-bit quantization = 128 bytes
LoRa packet capacity ≈ 230 bytes
Result: One head fits in one packet
```

This is elegant because the **natural granularity of the model architecture matches the packet size constraint**.


### Per-Layer Computation Flow

**Step 1: Gateway Broadcasts Hidden State**
- Current hidden state: 2048 dimensions × 1 byte (int8) = 2KB
- Must be split into: 2048 ÷ 230 ≈ **9 packets**
- Time: 9 packets × 1.5s = **~13.5 seconds**

**Step 2: Each Node Computes Its Head**
- Receives full 2048-dim state
- Computes Q, K, V projections (matrix multiply with local weights)
- Performs attention using cached Keys and Values from previous tokens
- Output: 128-dim vector

This happens **in parallel** across all 16 nodes (no transmission during local compute)

**Step 3: Nodes Return Results**
- Each node broadcasts its 128-byte head output
- 16 nodes × 1 packet each = **16 packets**
- Time: 16 × 1.5s = **24 seconds**

**Step 4: Gateway Reassembles**
- Concatenates 16 head outputs → 2048-dim vector
- Applies feed-forward network (happens locally at gateway)
- This becomes input to next layer

**Total per layer: ~40 seconds**


### Full Token Generation

**For TinyLlama (22 layers):**
```
40 seconds/layer × 22 layers = 880 seconds = ~15 minutes per token
```

**For 50-token response:**
```
15 min/token × 50 tokens = 750 minutes = 12.5 hours
```

**Critical note:** This is **auto-regressive generation**. Each new token requires passing through all 22 layers again because the model must incorporate the previous token into its KV cache.


### Memory Requirements Per Node

Each node must store:

**1. Weight Matrices (per head):**
- Q projection: 2048 × 128 weights
- K projection: 2048 × 128 weights  
- V projection: 2048 × 128 weights
- Output projection: 128 × 2048 weights

Total: ~1M weights per head

At 8-bit: 1MB  
At 4-bit: 512KB  
At 2-bit: 256KB  

**2. KV Cache:**
- Per head: 128 dims × 2 (K and V) × sequence_length tokens × 1 byte
- For 512-token context: 128 × 2 × 512 = **131KB**

**3. Activation buffers:**
- Input state: 2KB
- Intermediate computations: ~10KB
- Output: 128 bytes

**Total per node (2-bit quantization): ~300-400KB**  
**ESP32-S3 PSRAM: 512KB → Feasible**


---

## 2. Steering Vectors & Sensor Coupling


### What Are Steering Vectors?

In mechanistic interpretability research, steering vectors are directions in the model's latent space that correspond to semantic concepts.

**Standard Method (from your code):**
```python
# Get activations for "positive" examples
pos_acts = [model(text) for text in positive_examples]
avg_pos = mean(pos_acts)

# Get activations for "negative" examples  
neg_acts = [model(text) for text in negative_examples]
avg_neg = mean(neg_acts)

# The difference is the steering direction
steering_vector = avg_pos - avg_neg
```

**Usage:**
```python
# Add to model's hidden state at specific layer
hidden_state_steered = hidden_state + alpha * steering_vector
```

This **biases** the model's output toward the "positive" concept and away from the "negative".

**Resources:**
- [Anthropic: Steering Language Models](https://www.anthropic.com/index/steering-language-models)


### Sensor-to-Steering Transformation

**The Breakthrough:** This is simpler than it initially appears. Your EXP005 morphogens already contain semantic meaning - sensors just modulate their intensity.

**The Mathematics (Scalar Multiplication):**

```python
# EXP005 created semantic axes (2048-dim vectors)
morphogen_turbulent = mean(activations(["chaotic", "turbulent", "unstable"])) - 
                      mean(activations(["ordered", "calm", "stable"]))
# Result: [0.234, -0.456, 0.123, ..., 0.678]
# This vector encodes the FULL SPECTRUM from calm to turbulent

# Sensor reading (0.0 to 1.0)
sound_pressure = read_microphone()

# Steering is just multiplication
steering = sound_pressure * morphogen_turbulent

# If sound_pressure = 0.9 (loud explosion)
# steering = [0.211, -0.410, 0.111, ..., 0.610]
```

**Multiplying by sensor reading:**
- `0.0 × vector` = no steering (model baseline)
- `0.5 × vector` = halfway to turbulent
- `1.0 × vector` = fully turbulent
- `2.0 × vector` = beyond training distribution (may break coherence)

**Multiple Sensors = Vector Addition (Like RGB Color):**

```python
# Multiple morphogens from EXP005
m_turbulent = load("chaos_vs_order")
m_dark = load("bright_vs_dark")
m_ethereal = load("ethereal_vs_concrete")

# Multiple sensors
sound = read_mic()          # 0.0-1.0
light = read_light()        # 0.0-1.0
temperature = read_temp()   # 0.0-1.0

# Combine via vector addition
steering = (
    sound * m_turbulent +
    (1 - light) * m_dark +         # Low light = dark
    temperature * m_ethereal
)

# Result: Point in 3D semantic space
# - Turbulence (acoustic dimension)
# - Darkness (visual dimension)
# - Ethereality (thermal dimension)
```

This is exactly like RGB color mixing:
```
color = red*[1,0,0] + green*[0,1,0] + blue*[0,0,1]
```

**Why This Works:**
- EXP005 morphogens are already semantically grounded (derived from text activations)
- Each morphogen is an axis in latent space
- Sensors select coordinates on those axes
- The model doesn't "learn" the mapping - you define it manually based on intuitive correspondences

**Sensor→Morphogen Mappings (Design Choice):**

```python
SENSOR_SEMANTICS = {
    'sound_pressure': {
        'morphogen': 'chaos_vs_order',
        'high': 'turbulent',
        'low': 'calm'
    },
    'light_intensity': {
        'morphogen': 'bright_vs_dark',
        'high': 'bright',
        'low': 'dark'
    },
    'vibration': {
        'morphogen': 'stable_vs_unstable', 
        'high': 'chaotic',
        'low': 'stable'
    }
}
```

These mappings are intuitive, not arbitrary:
- Loud sound → turbulence (obvious)
- Bright light → visibility (obvious)
- Vibration → instability (obvious)


### Non-Linear Phenomena to Explore

**Naive assumption:** `2× sensor reading = 2× semantic effect` (linear)

**Reality:** Latent space is curved, vectors interfere, effects saturate. Here's what to measure:

#### 1. Magnitude Saturation
```python
for alpha in [0.1, 0.5, 1.0, 2.0, 5.0, 10.0]:
    output = generate(steering=alpha * morphogen_turbulent)
    semantic_shift = measure_distance(baseline, output)
    plot(alpha, semantic_shift)

# Expect: Sigmoid curve, not straight line
# There's a "sweet spot" before coherence breaks
```

#### 2. Vector Interference (Multiple Morphogens)
```python
# Are morphogens orthogonal, parallel, or at an angle?
similarity = cosine(morphogen_A, morphogen_B)

# Predict how they combine:
# cos(θ) =  1.0 → Aligned (amplify each other)
# cos(θ) =  0.0 → Orthogonal (independent effects)
# cos(θ) = -1.0 → Opposed (cancel each other)

# Test:
out_A = generate(steering=morphogen_A)
out_B = generate(steering=morphogen_B)
out_AB = generate(steering=morphogen_A + morphogen_B)

# Is out_AB = combination of out_A and out_B?
```

#### 3. Embedding Space Trajectories
```python
# Track hidden states as you vary steering intensity
trajectory = []
for intensity in range(0, 100):
    h = generate(steering=(intensity/100) * morphogen)
    trajectory.append(h[-1])  # Last layer activation

# Visualize in 2D (PCA or t-SNE)
plot_trajectory(trajectory)

# Expect: Smooth arc with possible attractors
# (Semantic "basins" the model falls into)
```

#### 4. Layer-Specific Effects
```python
# Different layers handle different aspects
for layer in [0, 5, 10, 15, 20]:
    output = generate(steering_at_layer={layer: morphogen})
    
# Hypothesis:
# - Early layers (0-5): Syntax/structure
# - Middle layers (6-16): Semantics (probably best)
# - Late layers (17-22): Token selection/surface form
```

#### 5. Context Dependence
```python
prompts = [
    "The weather is ___",
    "The politician is ___",
    "The music sounds ___"
]

for prompt in prompts:
    output = generate(prompt, steering=morphogen_turbulent)
    
# Same steering → different manifestations
# "turbulent" weather vs "turbulent" politics vs "turbulent" music
```

**Research Terms:**
- **Riemannian manifolds** (curved spaces)
- **Geodesics** (shortest paths in curved space)
- **Attractors** (stable semantic basins)
- **Phase transitions** (qualitative output shifts)


### Per-Head vs Global Steering

**Global Steering (Standard):**
Add the same steering vector to the full hidden state at one layer:
```python
hidden_state_layer_5 += alpha * steering_vector  # 2048 dims
```

**Per-Head Steering (Your Architecture):**
Each node adds its **own local steering** to its head's output:

```python
# Node 7 (Head 7)
head_7_output = attention(Q, K, V)  # 128 dims

# Node 7's sensor reading
local_steering = sensor_to_steering(node_7_sensors)  # 128 dims

# Node 7 broadcasts
final_output = head_7_output + alpha * local_steering
```

**Implication:** Each head can be steered **independently** based on spatially distributed sensor readings. The gateway receives a heterogeneous mixture of 16 differently-steered heads.

This creates **spatially-encoded environmental information** in the aggregate hidden state.


---

## 3. KV Cache & State Machines


### What Is KV Cache?

In transformer attention, each token attends to all previous tokens by computing:

```
Attention(Q, K, V) = softmax(Q @ K.T / sqrt(d)) @ V
```

Where:
- **Q (Query):** Current token asking "what should I attend to?"
- **K (Keys):** All previous tokens saying "here's what I represent"
- **V (Values):** All previous tokens providing "here's my information"

**The Problem:** For a 512-token context, you'd need to recompute K and V for all 512 previous tokens at every new token generation.

**The Solution (KV Cache):** Store the computed K and V matrices for all previous tokens:

```python
# Generation of token 1
k_cache = [k_token_1]
v_cache = [v_token_1]

# Generation of token 2  
k_cache.append(k_token_2)
v_cache.append(v_token_2)
# Attention now uses k_cache and v_cache (no recomputation)

# Generation of token 3
k_cache.append(k_token_3)
v_cache.append(v_token_3)
# ... etc
```

**In your distributed system:**
- Each node maintains its **own KV cache for its specific head**
- Node 7's cache: Keys and Values only for Head 7, for all previous tokens
- This is why memory per node stays manageable (~131KB for 512 tokens)


### Model-as-State-Machine

**Traditional View:**  
LLM is a function: f(prompt) → completion

**State Machine View:**  
LLM is a sequence of state transitions where each state contains:

```python
State = {
    'tokens_generated': ["The", "weather", "is"],
    'current_hidden_state': [2048-dim vector],
    'kv_caches': {
        'layer_1': {
            'head_1': (K_cache, V_cache),
            'head_2': (K_cache, V_cache),
            # ... 16 heads
        },
        # ... 22 layers
    },
    'current_layer': 5,
    'awaiting_heads': [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]
}
```

**State Transition:**
```
State(layer=5, awaiting=[1..16]) 
    → [Nodes compute]
    → State(layer=5, awaiting=[])
    → [Gateway processes]
    → State(layer=6, awaiting=[1..16])
```

**Why This Matters for LoRa:**

1. **Fault Tolerance:** If Node 7 fails, you can:
   - Wait and retry
   - Use cached value from previous token
   - Mark head 7 as "missing" and continue with 15 heads

2. **Debuggability:** You can serialize the entire state to disk, inspect KV caches, replay specific transitions

3. **Visualization:** Each state transition is an observable event that can trigger LEDs, dashboard updates, etc.

**Resources:**
- [Hugging Face: How to generate text](https://huggingface.co/blog/how-to-generate)


---

## 4. Timescale Relativism


### The Conceptual Foundation

From `consciousness_complexity_epistemology.md`:

> "The pebble IS conscious - just at geological timescales. We privilege our timescale (milliseconds to decades) as 'where consciousness happens' - pure anthropocentrism."

**Applied to computation:**

| System | Inference Time | Timescale |
|--------|---------------|-----------|
| Human brain | ~100ms per "thought" | Biological |
| GPT-4 (cloud) | ~50ms per token | Digital-fast |
| Edge device (Raspberry Pi) | ~5s per token | Digital-slow |
| Your LoRa mesh | ~15 min per token | Geological-fast |
| Tectonic processes | ~1M years per "event" | Geological-slow |


### Why Slowness Enables New Capabilities

**1. Visibility of Process**

When inference happens in milliseconds, it's a black box. At 15 minutes per token:
- You can watch each node light up as it computes
- You can visualize the hidden state vector as it propagates
- You can see which heads contribute most to the final token
- Students can follow the **actual mechanics** of attention

**2. Deliberative vs Reactive Intelligence**

Fast AI: Immediate response (can be impulsive, pattern-matching)  
Slow AI: Time to integrate distributed sensor data, multiple "perspectives" (the 16 heads), environmental context

This isn't worse - it's **different mode of cognition**. Like the difference between:
- Gut reaction (fast, System 1)
- Careful deliberation (slow, System 2)

**3. Environmental Integration**

With 15-minute token generation, sensor readings can be averaged over time:
```python
# Collect sensor data during the 40s that Layer 5 computes
sensor_samples = []
for t in range(40):  # 40 seconds
    sensor_samples.append(read_sensors())
    sleep(1)

# Use the aggregate as steering
steering = sensor_to_latent(mean(sensor_samples))
```

Noise cancels out. True environmental patterns emerge.


### Active Inference Across Timescales

**Active Inference** (from Karl Friston): Organisms minimize surprise by:
1. Predicting sensory input
2. Comparing prediction to actual input
3. Updating internal model
4. Taking actions to make predictions come true

Your mesh performs active inference but at LoRa timescales:
- **Prediction:** LLM's prior token probabilities
- **Sensory input:** Steering vectors from sensors
- **Surprise:** Deviation in embedding space
- **Update:** Next token incorporates both linguistic prior and environmental evidence

**This is legitimate research territory**: Does slowing down the inference loop change the quality of environmental integration?

**Resources:**
- [Active Inference (Wikipedia)](https://en.wikipedia.org/wiki/Active_inference)
- Karl Friston's work on free energy principle


---

## 5. Embedding Space Topology


### What Is Embedding Space?

The model represents tokens as high-dimensional vectors. For TinyLlama: **2048 dimensions**.

Similar concepts cluster together:
```
"cat" ≈ "dog" ≈ "pet"
"king" - "man" + "woman" ≈ "queen"
```

**Embedding space is the model's "concept space"** - the topology of meaning.


### Deviations as Environmental Signal

**Baseline (no sensor input):**
```
Prompt: "The weather is"
Model output: "calm" (p=0.7), "nice" (p=0.2), "cloudy" (p=0.1)
Embedding: v_baseline = [0.23, -0.45, 0.89, ..., 0.12]
```

**With Steering (Node 7 senses explosion):**
```
Prompt: "The weather is"
Model output: "turbulent" (p=0.5), "chaotic" (p=0.3), "unstable" (p=0.2)  
Embedding: v_steered = [0.67, -0.21, 0.34, ..., 0.56]
```

**Measure the deviation:**
```python
deviation_magnitude = ||v_steered - v_baseline||  # Euclidean distance
deviation_direction = (v_steered - v_baseline) / ||...||  # Unit vector

# Or use cosine similarity
similarity = cosine(v_steered, v_baseline)  # closer to 1 = more similar
```


### Topological Map of Environment

If you have 16 nodes with spatially-distributed sensors:

```
Node 1 (northwest) senses: loud explosion → large deviation
Node 7 (center) senses: calm → small deviation  
Node 12 (southeast) senses: bright flash → medium deviation, different direction
```

The **pattern of deviations across all 16 nodes** encodes:
- **Which region** of the physical mesh sensed something (spatial encoding)
- **What type** of stimulus (direction in embedding space)
- **How intense** (magnitude of deviation)

You've created a **semantic map of environmental topology**.

**Open question:** Can you reconstruct the physical stimulus pattern from the embedding-space pattern? (This is an experiment to run)


### Visualization Strategies

**1. Real-time Vector Plot**

As each head returns its output, plot it in 2D (use PCA or t-SNE to reduce 128 dims → 2 dims):
```
Baseline head positions: [○] [○] [○] ... 
Steered head positions:  [●]     [●]     [●] ...
                          ↑       ↑       ↑
                        drift   drift   drift
```

**2. Deviation Heatmap**

Color-code each node by its deviation magnitude:
```
[Node 1: RED]    [Node 5: YELLOW]  [Node 9: GREEN]
[Node 2: ORANGE] [Node 6: GREEN]   [Node 10: GREEN]
...
```

**3. Embedding Trajectory Over Time**

Plot the hidden state's position in embedding space as the mesh generates tokens:
```
Token 1: ●
Token 2:   ●
Token 3:      ●
Token 4:         ●  ← trajectory shows "semantic drift"
```

**Resources:**
- [Visualizing embeddings (TensorFlow)](https://www.tensorflow.org/tensorboard/tensorboard_projector_plugin)


---

## 6. Mycelial Network Analogy (Computational Biology)


### Why This Isn't Just Metaphor

Mycelial networks exhibit:

1. **Distributed sensing:** Hyphal tips sense nutrients, toxins, obstacles
2. **Signal propagation:** Electrochemical pulses travel through hyphae
3. **Collective decision-making:** No central brain, yet resources flow to where needed
4. **Stigmergy:** Environment shapes the network, network shapes environment

Your mesh exhibits:

1. **Distributed sensing:** Each node senses environment via sensors
2. **Signal propagation:** LoRa packets carry attention head outputs
3. **Collective decision-making:** No node has full model, yet coherent tokens emerge
4. **Stigmergy:** Sensors couple output to environment, output could trigger actions that change environment


### Levin's Basal Cognition Framework

Michael Levin studies how cellular collectives (planaria, xenobots) exhibit goal-directed behavior without neurons or brains.

**Key findings:**
- Bioelectric signaling creates "cognitive light cones" (regions that can coordinate)
- Cells perform active inference (minimize surprise about their niche)
- Intelligence is **scale-free** - emerges at any level of organization

**Your mesh as basal cognition:**
- LoRa signaling creates "cognitive mesh" (nodes that can coordinate)
- Heads perform attention (minimize surprise about linguistic patterns)
- Intelligence emerges from **16 simple agents** (heads) with no central control

**Resources:**
- [Michael Levin's Lab](https://ase.tufts.edu/biology/labs/levin/)
- [Collective intelligence of morphogenesis](https://www.sciencedirect.com/science/article/pii/S1084952118300135)


### Stigmergy in the Mesh

**Stigmergy:** Agents modify environment, which guides future actions.

**Example in termites:**
1. Termite drops mud → creates small mound
2. Mound releases pheromone
3. Other termites attracted to pheromone → drop more mud
4. Mound grows → releases more pheromone
5. Complex structure emerges without blueprint

**In your mesh:**
1. Node senses explosion → steering vector
2. Model output shifts → tokens reflect "alarm"
3. Alarm tokens displayed to user → user investigates
4. User's new prompt references the alarm → feeds back into model
5. System has "learned" environment altered behavior

Or, in closed-loop version:
1. Alarm tokens trigger action (e.g., broadcast warning, activate other sensors)
2. Actions change environment
3. Sensors detect changed environment
4. New steering vectors reflect new state
5. Feedback loop established

**This is how mycelium "solves" mazes** - not through planning, but through stigmergic feedback between network and environment.


---

## 7. FabAcademy Project Framing


### Why This Fits FabAcademy

**From Final Project Requirements:**
- Must demonstrate digital fabrication skills (PCB design, enclosure 3D printing, sensor integration)
- Must integrate multiple systems (embedded, networking, sensors)
- Must be well-documented (process, theory, results)
- Should be novel/creative


### What You Would Fabricate

**1. Custom PCBs**
- ESP32-S3 + LoRa radio + sensor breakouts
- Power management (battery/solar)
- LED arrays for visualization

**2. Physical Enclosures**
- 3D printed or CNC-milled cases for each node
- Mounting hardware for spatial deployment
- Weatherproofing for outdoor sensing

**3. Sensor Integration**
- MEMS microphones (sound)
- IMU/accelerometers (vibration)
- Light sensors (photodiodes)
- Optional: temperature, humidity, gas sensors

**4. Visualization Dashboard**
- Web interface showing:
  - Node status
  - Current hidden state (visualized)
  - Deviation heatmap
  - Token generation progress


### Documentation Strategy

**Week-by-week fabrication log:**
- PCB design and milling
- Component selection and testing
- Sensor calibration
- LoRa mesh configuration
- First single-head computation
- Multi-node coordination
- Steering vector integration
- Final assembly and testing

**Theoretical documentation:**
- This folder structure (ideation/)
- Weekly blog posts connecting theory to practice
- Video demonstrations of "thought propagation"


### Evaluation Criteria Fit

| Requirement | How This Project Satisfies |
|-------------|---------------------------|
| Technical complexity | Distributed systems + ML + embedded networking |
| Fabrication skills | Custom PCBs, enclosures, integration |
| Originality | Novel architecture (not seen in literature) |
| Documentation | This folder + weekly logs + theory integration |
| Impact | Educational tool for teaching distributed AI + biological analogies |


---

## Navigation

- [← Back to Overview](00_OVERVIEW.md)
- [→ Glossary](02_GLOSSARY.md)
- [→ Philosophy Integration](03_PHILOSOPHY.md)
- [→ Open Questions](04_OPEN_QUESTIONS.md)
