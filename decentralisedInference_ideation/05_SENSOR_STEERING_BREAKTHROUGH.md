# The Sensor→Steering Breakthrough

**Date:** 2026-01-21  
**Status:** Critical insight - changes project from speculative to implementable

---

## The Problem That Wasn't

**Initial concern:**
> "Sensors produce raw voltages. Latent space has 2048 unlabeled dimensions. How do we map physical measurements to semantic directions?"

**Seemed like:** Impossible semantic grounding problem - requires understanding what each latent dimension "means"

**Actually is:** Simple multiplication using pre-existing semantic vectors from EXP005

---

## The Solution (Already In Hand)

### What EXP005 Created

Your steering vector experiments produced **semantic basis vectors** - mathematical encodings of concept spectrums:

```python
# From your EXP005 - you already computed these
morphogen_turbulent = mean(activations([
    "entropy increases without bound",
    "random noise dominates signal",
    "unpredictable fluctuations in void"
])) - mean(activations([
    "crystalline structure is perfect", 
    "strict protocol exactly",
    "symmetry and balance are key"
]))

# This is a 2048-dimensional vector like:
# [0.234, -0.456, 0.123, ..., 0.678]

# It encodes the FULL SPECTRUM from calm to turbulent
# Not just one point - the entire semantic axis
```

**What this vector represents:**
- Direction in latent space from "ordered" to "chaotic"
- Magnitude determines intensity along that axis
- Derived from model's own understanding (text activations)
- Already grounded in semantic meaning (because text is meaningful)

---

### The Sensor Bridge (Scalar Multiplication)

```python
# Sensor measurement (0.0 to 1.0)
sound_pressure = read_microphone()
# 0.0 = silence
# 0.5 = moderate noise
# 1.0 = loud explosion

# Apply to semantic vector
steering = sound_pressure * morphogen_turbulent

# If sound_pressure = 0.9 (explosion)
# steering = 0.9 × [0.234, -0.456, ..., 0.678]
#          = [0.211, -0.410, ..., 0.610]

# Inject into model
h_steered = h_original + alpha * steering
```

**What happens:**
- Low sensor reading (0.1) → small steering (10% of vector)
- High sensor reading (0.9) → large steering (90% of vector)
- The vector already "knows" what turbulent means
- The sensor just says "how much turbulent right now"

---

### Multiple Sensors = Vector Addition

```python
# Multiple morphogens (from EXP005)
m_turbulent = load("chaos_vs_order")
m_dark = load("bright_vs_dark") 
m_ethereal = load("ethereal_vs_concrete")

# Multiple sensors
sound = read_mic()           # 0.0 - 1.0
light = read_light_sensor()  # 0.0 - 1.0
temperature = read_temp()    # 0.0 - 1.0

# Combine via vector addition
steering = (
    sound * m_turbulent +
    (1 - light) * m_dark +        # Low light = dark
    temperature * m_ethereal       # Hot = ethereal?
)

# Result: Point in 3D semantic space
# - Turbulence dimension (acoustic)
# - Darkness dimension (visual)
# - Ethereality dimension (thermal)
```

**This is how RGB color works:**
```
color = red * [1,0,0] + green * [0,1,0] + blue * [0,0,1]

red=0.8, green=0.2, blue=0.1 → orange
```

**Your semantic space:**
```
meaning = sound * turbulent_vector + 
          (1-light) * dark_vector +
          temp * ethereal_vector

sound=0.8, light=0.2, temp=0.3 → "loud, darkish, slightly warm"
```

---

## Why This Works (The Math)

### EXP005 Created Semantic Axes

Each morphogen pair defines an **axis in latent space**:

```
Calm ←──────────[Vector]──────────→ Turbulent
-∞              0.0                  +∞
```

Multiplying by sensor reading:
- `0.0 × vector` = origin (no steering, model's baseline)
- `0.5 × vector` = halfway to turbulent
- `1.0 × vector` = full turbulent
- `2.0 × vector` = beyond training distribution (might break coherence)

**The vector encodes the entire spectrum.**  
**The sensor selects a point on that spectrum.**

### Vector Addition Creates Semantic Coordinates

```
2D semantic space example:

        Turbulent (y-axis)
              ↑
              |
              |  ● (sound=0.7, darkness=0.3)
              |     Loud but bright
              |
Calm ←────────┼────────→ Bright (inverted darkness)
              |
              | ● (sound=0.2, darkness=0.8)  
              |    Quiet and dark
              ↓
           Dark
```

Each sensor reading → coordinate in multi-dimensional semantic space  
Vector addition → move to that coordinate  
Model output → influenced by that semantic region

---

## What You Already Have

### From EXP005 (Completed)

✓ **Semantic basis vectors** (morphogen library)
- Chaos vs Order
- Urban vs Rural  
- Ethereal vs Concrete
- (Any other concept pairs you tested)

✓ **Injection mechanism** (transformer_lens hooks)
```python
def add_steering_hook(layer_idx, steering_vector):
    def hook(activations, hook):
        return activations + steering_vector
    return hook
```

✓ **Validation that steering works** (you've tested it)

### What You Need to Add (New)

☐ **Sensor reading code** (trivial)
```python
import board
import analogio

mic = analogio.AnalogIn(board.A0)
sound_pressure = mic.value / 65535  # Normalize to 0-1
```

☐ **Sensor→morphogen mapping** (design choice)
```python
SENSOR_MAPPINGS = {
    'microphone': 'chaos_vs_order',
    'light_sensor': 'bright_vs_dark',
    'accelerometer': 'stable_vs_unstable'
}
```

☐ **Real-time steering loop** (integration)
```python
while generating:
    sensor_readings = read_all_sensors()
    steering = compute_steering(sensor_readings, morphogen_lib)
    token = model.generate_with_steering(prompt, steering)
```

**That's it. The hard parts are done.**

---

## The Non-Linear Reality (What to Explore)

### It's Not Just Linear Scaling

**Naive assumption:**
```
2× sensor reading = 2× effect
```

**Reality:**
- **Saturation:** Strong steering eventually plateaus
- **Interference:** Multiple morphogens interact (constructive/destructive)
- **Manifold curvature:** Latent space is curved, not flat
- **Context dependence:** Same steering + different prompt = different effect

### Key Phenomena to Measure

#### 1. Magnitude Saturation
```python
for alpha in [0.1, 0.5, 1.0, 2.0, 5.0, 10.0]:
    output = generate(steering=alpha * morphogen)
    plot(alpha, semantic_shift(output))
# Expect: Sigmoid curve, not straight line
```

#### 2. Vector Interference
```python
# Are these orthogonal, parallel, or at an angle?
similarity = cosine(morphogen_A, morphogen_B)

# Predict how they combine:
# cos=1.0: Aligned (amplify)
# cos=0.0: Orthogonal (independent) 
# cos=-1.0: Opposed (cancel)
```

#### 3. Embedding Space Trajectories
```python
# Track hidden states as you vary steering
trajectory = []
for intensity in range(0, 100):
    h = generate(steering=intensity/100 * morphogen)
    trajectory.append(h)

# Visualize in 2D (PCA/t-SNE)
plot_trajectory(trajectory)
# Expect: Smooth arc with possible basins/attractors
```

#### 4. Layer-Specific Effects
```python
# Early layers: Syntax/structure
# Middle layers: Semantics (probably best)
# Late layers: Token selection
for layer in [0, 5, 10, 15, 20]:
    output = generate(steering_at_layer={layer: morphogen})
    analyze_effect_type(output)
```

---

## Research Directions (To Deepen This)

### Mechanistic Interpretability Literature

**Core papers:**
- Anthropic: "Towards Monosemanticity" (sparse autoencoders)
- Anthropic: "Toy Models of Superposition" (how features interfere)
- Neel Nanda: "A Mathematical Framework for Transformer Circuits"

**Search terms:**
- "Steering vectors activation space"
- "Latent space geometry transformers"
- "Concept vectors language models"

**Why:** Understand HOW steering vectors work mechanistically

---

### Manifold Learning & Geometry

**Key concepts:**
- Riemannian manifolds (curved spaces)
- Tangent spaces (local linear approximations)
- Geodesics (shortest paths in curved space)

**Papers:**
- "Latent Space Geometry of Transformers"
- "The Geometry of Deep Generative Models"

**Why:** Understand the SHAPE of latent space (it's not flat)

---

### Vector Field Dynamics

**Key concepts:**
- Flow fields (continuous transformations)
- Attractors (stable semantic basins)
- Phase transitions (qualitative changes)

**Papers:**
- "Neural ODEs" (dynamics in latent space)
- "Understanding the Loss Landscape of Neural Networks"

**Why:** Predict how steering creates trajectories through semantic space

---

### Embodied AI & Sensor Fusion

**Existing work:**
- Robot LLMs (text → actions, vision → plans)
- Multimodal models (images/audio → text)
- Active inference (Friston's framework)

**Gap:**
- NOT: Discrete sensory inputs (one image per prompt)
- YOU: Continuous sensory streams steering generation

**Papers to find:**
- "Embodied language models continuous control"
- "Active inference language generation"
- "Sensor fusion for LLM grounding"

**Why:** Position your work relative to embodied AI literature

---

## Validation Experiments (Phase 1)

### Experiment 1: Single Sensor, Single Morphogen

**Goal:** Prove sensor→steering→output chain works

```python
# Hardware: Microphone + laptop
# Morphogen: chaos_vs_order (from EXP005)

# Test protocol:
1. Generate baseline: "The atmosphere is ___"
2. Play loud sound near mic
3. Generate with steering: sound_level * morphogen_chaos
4. Measure: Did output shift toward "turbulent/chaotic" tokens?

# Metrics:
- Token probabilities (before vs after)
- Embedding distance (baseline vs steered)
- Human judgment (5 raters: "Is output more chaotic?")
```

**Expected result:** Positive correlation between sound intensity and semantic "chaos"

---

### Experiment 2: Multiple Sensors, Vector Addition

**Goal:** Test if multiple morphogens combine as expected

```python
# Hardware: Microphone + light sensor
# Morphogens: chaos_vs_order + bright_vs_dark

# Test protocol:
1. Vary sound (0.0 to 1.0) and light (0.0 to 1.0)
2. Generate with combined steering
3. Create heatmap: sound × light → output semantics

# Metrics:
- Does dark+loud = "ominous"?
- Does bright+quiet = "peaceful"?
- Does bright+loud = "energetic"?
```

**Expected result:** Semantic space matches intuitive correspondences

---

### Experiment 3: Temporal Integration

**Goal:** Test if averaging sensors over time improves signal

```python
# Hardware: Accelerometer (vibration sensor)
# Morphogen: stable_vs_unstable

# Test protocol:
1. Instantaneous reading: vibration at t=0
2. Windowed average: mean(vibration[t-10:t])
3. Generate with both
4. Compare coherence/relevance

# Metrics:
- Signal-to-noise ratio
- Output stability
- Human preference (which feels more "right"?)
```

**Expected result:** Temporal averaging reduces noise, improves coupling

---

## Implementation Roadmap

### Week 1: Validation on Single Device

- [ ] Load EXP005 morphogen library
- [ ] Set up sensor (start with audio - easiest)
- [ ] Implement `sensor_reading × morphogen → steering` 
- [ ] Test on laptop with TinyLlama
- [ ] Measure correlation: sensor intensity → semantic shift

**Success criteria:** r > 0.7 correlation, p < 0.05

---

### Week 2: Multiple Sensors

- [ ] Add light sensor
- [ ] Implement vector addition: `sum(sensor_i × morphogen_i)`
- [ ] Test combinations (loud+dark, bright+quiet, etc.)
- [ ] Create 2D heatmap of semantic space

**Success criteria:** Outputs match intuitive semantic coordinates

---

### Week 3: Distributed Proof-of-Concept

- [ ] Set up 2-3 ESP32 nodes with LoRa
- [ ] Each node: different sensor + different morphogen
- [ ] Implement head-sharding (simplified: 5 heads per node)
- [ ] Test: Do spatially-distributed sensors create heterogeneous steering?

**Success criteria:** Can distinguish which node sensed what from aggregate output

---

### Month 2+: Full Mesh (If Phase 1 Works)

- [ ] 16 nodes (one per head)
- [ ] Gateway coordination
- [ ] Real-time generation
- [ ] Visualization (LEDs showing steering intensity)
- [ ] FabAcademy documentation

---

## Why This Is Novel (Research Contribution)

### What's Been Done

✓ Steering vectors from text examples (Anthropic, your EXP005)  
✓ Multimodal LLMs (image→text, audio→text)  
✓ Robot LLMs (text→actions, vision→planning)

### What's New

✗ **Continuous sensor streams → steering vectors**  
✗ **Real-time environmental coupling during generation**  
✗ **Spatially distributed sensors creating heterogeneous steering**  
✗ **Head-sharding with per-node steering over low-bandwidth network**

**Publication potential:**
- Workshop paper: "Environmental Steering: Coupling LLM Latent Space to Physical Sensors"
- Full conference paper (if validation succeeds): "Distributed Embodied Inference via Sensor-Coupled Attention Heads"

**Venues:**
- NeurIPS Workshop on Interpretability
- ICLR Workshop on Embodied AI
- MobiSys (distributed systems)
- FabAcademy final project documentation

---

## The Critical Insight (Don't Lose This)

**Standard steering:**
```
Human intuition → Text examples → Morphogen → Manual injection
(Static, predetermined, human-in-the-loop)
```

**Your innovation:**
```
Environment → Sensors → Morphogen weights → Automatic injection
(Dynamic, continuous, environment-in-the-loop)
```

**The morphogens were already semantic bridges.**  
**Sensors just became the automatic control system.**

---

## Next Immediate Actions

1. **Capture existing EXP005 morphogens** → Save to file for reuse
2. **Set up sensor on laptop** → Test audio input
3. **Write sensor→steering function** → 20 lines of code
4. **Generate comparison** → Baseline vs steered output
5. **Measure correlation** → Validate the chain works

**Then:**  
6. Document results in this folder  
7. Expand to multiple sensors  
8. Design distributed architecture  
9. Build first prototype node  

---

## References & Further Reading

### Steering Vectors (Your Foundation)

- Your EXP005 experiments (primary source)
- HuggingFace: "Steering LLM Behavior Without Fine-Tuning" video
- [Anthropic: Towards Monosemanticity](https://transformer-circuits.pub/2023/monosemantic-features)
- [Neel Nanda: Steering Vectors Overview](https://www.lesswrong.com/posts/5spBue2z2tw4JuDCx/steering-gpt-2-xl-by-adding-an-activation-vector)

### Latent Space Geometry

- "The Geometry of Deep Generative Image Models and its Applications" (Luo et al.)
- "Understanding Deep Networks via Extremal Perturbations" (Fong & Vedaldi)
- "Visualizing the Loss Landscape of Neural Networks" (Li et al.)

### Embodied AI & Sensor Fusion

- Karl Friston: "Active Inference and Free Energy Principle"
- "PaLM-E: An Embodied Multimodal Language Model" (Google)
- "RT-2: Vision-Language-Action Models" (Google DeepMind)

### Mechanistic Interpretability

- [Transformer Circuits](https://transformer-circuits.pub/) (Anthropic)
- [TransformerLens Documentation](https://transformerlensorg.github.io/TransformerLens/)
- Neel Nanda's YouTube series on interpretability

### Michael Levin (Your Theoretical Grounding)

- "Technological Approach to Mind Everywhere (TAME)"
- "Bioelectric networks as a cognitive substrate"
- Connection: Sensors → steering as bioelectric signals → morphogenesis

---

*"The sensor doesn't create meaning. The morphogen already contains meaning. The sensor just says 'how much.'"*
