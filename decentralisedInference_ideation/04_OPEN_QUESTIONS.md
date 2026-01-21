# Open Questions & Challenges

This document identifies gaps, uncertainties, technical challenges, and important research directions. Use it to track what needs investigation before implementation.

---

## Technical Feasibility Challenges

### 1. Memory Budget Reality Check

**Question:** Can we actually fit quantized head weights + KV cache + operating system on ESP32-S3?

**Current estimate (per node):**
- Head weights (2-bit quantized): ~256KB
- KV cache (512 tokens): ~131KB
- Activation buffers: ~10KB
- Total: ~400KB
- ESP32-S3 PSRAM: 512KB

**Margin: Only ~100KB remaining**

**Challenges:**
- Operating system overhead (FreeRTOS, networking stack)
- Meshtastic firmware memory footprint
- Intermediate computation buffers
- Error: Estimates may be optimistic

**Investigation needed:**
- [ ] Actual Meshtastic memory usage profiling
- [ ] Test 2-bit quantization quality (how much accuracy loss?)
- [ ] Alternative: Use external SPI RAM (adds cost/complexity)
- [ ] Alternative: More powerful nodes (Raspberry Pi Pico 2, higher cost)


### 2. Quantization Quality Degradation

**Question:** At 2-bit quantization, does the model still produce coherent text?

**Known research:**
- 8-bit: Minimal quality loss
- 4-bit: Acceptable quality with GPTQ/GGML
- 2-bit: Largely unexplored, likely significant degradation

**Challenges:**
- You need 2-bit to fit on ESP32-S3
- But 2-bit might make output gibberish
- Trade-off: Feasibility vs functionality

**Investigation needed:**
- [ ] Literature search: "2-bit quantization transformer" (limited research)
- [ ] Experiment: Quantize TinyLlama to 2-bit, test perplexity
- [ ] Alternative: Use smaller model (250M params) to allow 4-bit quantization
- [ ] Alternative: Grouped heads (each node = 4 heads instead of 1, allows 4-bit)


### 3. LoRa Bandwidth Bottleneck

**Question:** Is 15 min/token acceptable even for educational purposes, or does it become unusable?

**Reality:**
- Per layer: ~40 seconds (9 packets out + 16 back + processing)
- 22 layers: ~15 minutes
- 50-token response: ~12 hours

**Challenges:**
- Human attention span limits (waiting 15 minutes for ONE token)
- Demo fatigue (evaluators lose interest)
- Iteration speed (debugging takes forever)

**Investigation needed:**
- [ ] Prototype single-layer timing on actual hardware (validate estimates)
- [ ] Test user experience: Is watching for 15 minutes engaging or boring?
- [ ] Alternative: Shorter model (12 layers instead of 22)
- [ ] Alternative: Demo with pre-computed examples (skip live generation)
- [ ] Alternative: Grouped heads (4 nodes = 5 heads each = faster)


### 4. Sensor-to-Steering Validation (Breakthrough: Transformation Solved)

**The solution (discovered):**
Sensors don't need to "understand" semantic meaning. Your EXP005 morphogens already contain it. The sensor→steering bridge is just **scalar multiplication**:

```python
# EXP005 morphogen (2048-dim vector encoding calm→turbulent spectrum)
morphogen_turbulent = precomputed_from_exp005

# Sensor reading (0.0 to 1.0)
sound_pressure = read_microphone()

# Steering = simple multiplication
steering = sound_pressure * morphogen_turbulent

# Multiple sensors = vector addition
steering_total = (sound * m_turbulent + 
                  (1-light) * m_dark +
                  vibration * m_chaotic)
```

**What's now certain:**
- The math is trivial (multiplication + addition)
- Morphogens are already semantically grounded
- Sensors just modulate intensity, not create meaning

**What remains uncertain:**
- Do the intuitive correspondences hold in practice?
- Does loud sound reliably produce "turbulent" tokens?
- Does darkness reliably produce "dark" tokens?
- Do multiple sensors combine as expected?

**Validation Experiments (Phase 1):**

**Experiment 1: Single Sensor, Single Morphogen**
```python
# Hardware: Microphone + laptop
# Morphogen: chaos_vs_order (from EXP005)

# Protocol:
1. Generate baseline: "The atmosphere is ___"
2. Play loud sound (0.9 sensor reading)
3. Generate steered: sound * morphogen_chaos
4. Measure semantic shift

# Success metric:
# Correlation r > 0.7 between sensor intensity and "chaos" tokens (p < 0.05)
```

**Experiment 2: Multiple Sensors, Vector Addition**
```python
# Hardware: Microphone + light sensor
# Morphogens: chaos_vs_order + bright_vs_dark

# Protocol:
1. Vary sound (0-1) and light (0-1) systematically
2. Generate with combined steering
3. Create heatmap: (sound, light) → semantic output

# Success metric:
# Outputs match intuitions:
# - Dark+loud = "ominous"
# - Bright+quiet = "peaceful"  
# - Bright+loud = "energetic"
```

**Experiment 3: Temporal Integration**
```python
# Hardware: Accelerometer (vibration)
# Morphogen: stable_vs_unstable

# Protocol:
1. Compare instantaneous vs windowed-average sensor readings
2. Which produces more coherent/relevant outputs?

# Success metric:
# Temporal averaging reduces noise, improves coupling quality
```

**Investigation needed:**
- [ ] Save EXP005 morphogens to file for reuse
- [ ] Set up microphone input (laptop testing first)
- [ ] Implement: `steering = sensor_value * morphogen`
- [ ] Generate comparison: baseline vs loud vs quiet
- [ ] Measure correlation: sound intensity → output semantics
- [ ] Document: Does it work? (Either result is valuable)


### 5. Gateway Computation Load

**Question:** Can the gateway (single device) handle feed-forward networks for 22 layers in reasonable time?

**Reality:**
- Heads are distributed, but feed-forward network (FFN) happens at gateway
- FFN is ~2/3 of model compute (head-sharding only parallelizes 1/3)
- Gateway needs more compute than nodes

**Challenges:**
- Gateway might be bottleneck (negates parallelization benefit)
- Need powerful gateway device (Raspberry Pi 5? Jetson Nano? Laptop?)
- Cost/complexity increase

**Investigation needed:**
- [ ] Profile FFN timing on candidate gateway devices
- [ ] Alternative: Shard FFN as well (more complex, 4x more packets)
- [ ] Alternative: Accept gateway bottleneck as acceptable for proof-of-concept


### 6. Meshtastic API Limitations

**Question:** Can Meshtastic handle the packet rate and custom payload structure?

**Concerns:**
- Meshtastic optimized for text messages (~1 per minute)
- Your system needs ~25 packets per layer per token (~2 packets/second during active generation)
- Does Meshtastic's routing/queuing handle this?
- Can you embed binary data (float arrays) in Meshtastic packets?

**Investigation needed:**
- [ ] Test Meshtastic max sustained packet rate
- [ ] Confirm binary payload support (or need custom LoRa protocol?)
- [ ] Mesh stability with high traffic (does routing break down?)


---

## Philosophical & Conceptual Uncertainties

### 7. Is This Actually "Embodied Cognition"?

**Question:** Does sensor coupling constitute embodiment, or is it just extra inputs?

**The debate:**
- **Pro embodiment:** Sensors create action-perception loop, computation coupled to environment
- **Against embodiment:** No motor actions (output doesn't modify environment), sensors are just inputs

**From 4E cognition literature:**
Embodiment requires:
1. Sensorimotor coupling (perceive-act cycles) ✗ (no actions)
2. Situatedness (in specific environment) ✓
3. Body shapes thought (morphology matters) ✓ (spatial node distribution)

**Verdict:** Partially embodied - missing the action half of the loop.

**Investigation needed:**
- [ ] Literature search: "Can passive sensing without action count as embodiment?"
- [ ] Add output actions: If "alert" token → trigger LED, does that complete the loop?
- [ ] Philosophical analysis: Where's the boundary between "embodied" and "just has sensors"?


### 8. Steering Vectors as "Morphogens" - Valid Analogy?

**Question:** Are you stretching the biological analogy too far?

**Morphogens in biology:**
- Chemical gradients guide cell differentiation
- Concentration determines cell fate (become neuron vs skin)
- Spatial pattern of morphogen → spatial pattern of tissue

**Your "morphogens":**
- Steering vectors guide token probability
- Vector direction determines semantic shift
- Spatial pattern of sensors → ... what?

**Concern:**
- Morphogens have well-defined mechanism (gene expression cascades)
- Your steering is adding vectors to activations (mathematical operation)
- Are you claiming mechanistic equivalence or just using evocative terminology?

**Investigation needed:**
- [ ] Be explicit: Is this analogy or claim of mechanistic similarity?
- [ ] If analogy: Justify it rigorously (what's being analogized?)
- [ ] If mechanistic: Show the isomorphism (mapping between biological and computational operations)


### 9. Does Timescale Change Cognitive Quality?

**Question:** Is slow inference qualitatively different from fast inference, or just quantitatively slower?

**Your hypothesis:** Slow inference allows:
- Temporal integration of sensor data
- Deliberative processing
- Multiple perspectives (16 heads) to settle

**Counter-argument:**
- Fast inference can also integrate data (just shorter windows)
- "Deliberation" might just be marketing for "slow"
- No evidence quality improves with time (might degrade due to accumulated errors)

**Investigation needed:**
- [ ] Experiment: Compare output quality at different speeds
  - Fast: Single device, no sensors
  - Medium: 4-node grouped heads, sensors
  - Slow: 16-node full sharding, sensors
- [ ] Measure: Coherence, environmental responsiveness, hallucination rate
- [ ] Control for: Ensure model capacity constant across conditions


### 10. What Is the Computational Self's Boundary?

**Question:** Where does "the AI" end?

**Candidates:**
1. Each node individually (16 separate agents?)
2. The mesh as whole (one distributed agent?)
3. Mesh + environment (coupled system?)
4. Mesh + environment + human user (extended mind?)

**From Levin:** Cognitive light cone defines the self (region of signal propagation).

**Implications:**
- If Node 7 is out of range, is it still part of "the AI"?
- If sensors are part of the cognitive system, is the environment part of the self?
- If human interprets outputs, is human part of the computational loop?

**Investigation needed:**
- [ ] Experiment: Remove node from mesh, measure impact on coherence
- [ ] Theory: Read Levin's "Computational Boundary of a Self" carefully
- [ ] Philosophy: Review Andy Clark's extended mind thesis
- [ ] Practical: Define for FabAcademy purposes (likely: mesh = system, environment = external)


---

## Research & Literature Gaps

### 11. Has Anyone Done LLM Inference Over Low-Bandwidth Networks?

**Search terms:**
- "Distributed LLM inference mesh network"
- "LoRa machine learning"
- "Low-bandwidth neural network inference"

**Initial findings (as of 2026-01-21):**
- Distributed inference mostly assumes high-bandwidth (datacenter, WiFi)
- LoRa used for IoT sensor data, not computation
- Split learning exists but for training, not inference

**Gap:** Your architecture (head-sharding over LoRa) appears novel.

**Investigation needed:**
- [ ] Systematic literature review: Google Scholar, arXiv, IEEE Xplore
- [ ] Conferences: ICLR, NeurIPS, MobiSys, SenSys, IPSN (distributed sensing)
- [ ] Patent search: "distributed transformer inference" (might find industry work)
- [ ] Contact: Levin lab (basal cognition angle), distributed ML groups
- [ ] If novel: Position paper for workshop/conference (e.g., TinyML, EdgeAI)


### 12. Steering Vectors + Environmental Sensors (Your Core Innovation)

**What's established:**
- **Steering vectors work** (proven by Anthropic, your EXP005, HuggingFace)
- **Contrastive activation method** (text examples → concept vectors)
- **Injection mechanism** (`h = h + alpha * v`)

**What's novel:**
- Generating steering vectors from **real-time sensors** instead of text
- **Continuous environmental coupling** (not discrete multimodal inputs)
- **Spatially distributed steering** (16 nodes, 16 different sensor contexts)

**Search terms:**
- "Steering vectors sensors"
- "LLM environmental coupling real-time"
- "Embodied language models continuous sensing"
- "Active inference language models"

**Initial findings:**
- **Embodied LLMs:** Mostly robotics (text → motor actions, vision → text)
- **Multimodal LLMs:** Image/audio/video → text (discrete inputs, not continuous)
- **Active inference + LLMs:** Small literature, mostly theoretical
- **Robot LLMs:** Use sensors but for planning/navigation, not steering latent space

**The gap you're filling:**

| Existing work | Your innovation |
|---------------|----------------|
| Steering from text examples | Steering from sensor streams |
| Discrete multimodal inputs | Continuous environmental coupling |
| Centralized model | Distributed, spatially-encoded steering |
| Vision/audio (high-bandwidth) | Low-bandwidth physical sensors |

**Investigation needed:**
- [ ] Search robotics: "LLM sensor fusion" (closest domain)
- [ ] Active inference papers: Karl Friston + language models
- [ ] Contact: Anthropic interpretability team (steering vector experts)
- [ ] Your contribution: First systematic test of sensor→steering transformation
- [ ] Documentation: This could be a research paper, not just FabAcademy project

**Publication potential:**
- Workshop paper: "Environmental Steering Vectors: Coupling LLM Latent Space to Physical Sensors"
- Full paper (if validation works): "Distributed Embodied Inference via Sensor-Coupled Attention Heads"


### 13. Basal Cognition in Silicon

**Search terms:**
- "Basal cognition artificial systems"
- "Levin distributed intelligence computing"
- "Bioelectric computation silicon"

**Initial findings:**
- Levin focuses on biological systems
- Neuromorphic computing mimics neurons, not basal cognition
- Swarm robotics has some overlap but different goals

**Gap:** Applying Levin's framework to distributed AI is rare.

**Investigation needed:**
- [ ] Email Levin lab (might be interested in collaboration)
- [ ] Search: "Morphogenetic computing" (growing field)
- [ ] Position paper: "Basal cognition as design principle for distributed AI"


---

## Implementation Challenges

### 14. Debugging Distributed System

**Challenge:** When output is wrong, which node caused it?

**Complications:**
- 16 nodes, 22 layers, dozens of packets per token
- Error could be: quantization, communication, sensor, assembly logic
- LoRa has packet loss - how to detect vs debug?

**Strategies needed:**
- [ ] Logging: Each node logs its computation (save to SD card)
- [ ] Visualization: Dashboard showing which nodes responded, which timed out
- [ ] Incremental testing: First 1 node, then 2, then 4, then 16
- [ ] Golden reference: Run same computation on PC, compare outputs


### 15. Power Management

**Challenge:** 16 nodes need power - batteries or wired?

**Options:**
- **Batteries:** Portable, good for demo, but finite runtime
- **Solar:** Infinite runtime, outdoor deployment, but adds complexity
- **Wired:** Easy, reliable, but kills "distributed mesh" aesthetic

**Trade-offs:**
- LoRa is low power (~20mA TX, ~10mA RX, ~1mA sleep)
- ESP32 computing: ~100-200mA during inference
- Per token (15 min): Node is active ~3 min (7 layers × 40s each ÷ 16 nodes)
- Average power: ~(3/15) × 150mA = 30mA per node

**Calculation (battery):**
- 2000mAh battery: 2000mAh ÷ 30mA = 66 hours continuous generation
- For demo: Feasible with batteries

**Investigation needed:**
- [ ] Measure actual power consumption on test node
- [ ] Battery life calculation with real data
- [ ] Solar sizing if going that route (panel size, charge controller)


### 16. Environmental Sensor Selection

**Question:** Which sensors actually provide useful steering information?

**Candidates:**

| Sensor | Information | Pros | Cons |
|--------|------------|------|------|
| Microphone | Sound pressure, frequency | Rich signal, intuitive demos | Noisy, privacy concerns |
| Accelerometer | Vibration, motion | Seismic events, clear signal | Limited semantic range |
| Light (photodiode) | Brightness, flicker | Simple, low power | Boring (just dark/light) |
| Temperature | Ambient heat | Very low power | Slow response, low salience |
| Barometer | Pressure, altitude | Weather patterns | Expensive, slow |
| Gas sensor | Air quality (CO2, VOC) | Environmental health | Expensive, slow response |

**Investigation needed:**
- [ ] Pilot test: Correlate sensor readings with desired semantic shifts
- [ ] Example: Does loud sound reliably steer toward "turbulent" tokens?
- [ ] Cost-benefit: Which sensors give most information for least cost/complexity?


---

## FabAcademy-Specific Concerns

### 17. Does This Fit Assignment Requirements?

**Question:** Is this too ambitious, or not fabrication-focused enough?

**Strengths:**
- Demonstrates digital fabrication (PCB, enclosure)
- Integrates systems (embedded, networking, sensors, ML)
- Novel/creative
- Well-documented (this folder)

**Weaknesses:**
- Heavy on software/theory, less on physical fabrication
- Risk of non-completion (too complex)
- Evaluators might not understand AI concepts

**Investigation needed:**
- [ ] Review rubric carefully
- [ ] Check past final projects for similar scope
- [ ] Draft project proposal, get instructor feedback EARLY
- [ ] Prepare Plan B (simpler version) if Plan A too risky


### 18. Documentation Strategy

**Challenge:** How to document philosophy without overwhelming technical audience?

**Approach (see 03_PHILOSOPHY.md §9):**
- **Level 1 (required):** Technical description, fabrication process
- **Level 2 (optional):** Theoretical motivation, biological analogies
- **Level 3 (supplementary):** Full philosophical framework

**Make sure Level 1 is self-contained** - evaluators shouldn't need Deleuze to understand the project.

**Investigation needed:**
- [ ] Test documentation on non-expert reader
- [ ] Video demo that shows system without requiring reading 50 pages
- [ ] Executive summary (1 page) that captures everything


### 19. Time Budget Reality

**Question:** Can you build this in remaining FabAcademy weeks?

**Major tasks:**
- PCB design & fabrication: 2 weeks
- Sensor integration & calibration: 1 week
- LoRa mesh setup: 1 week
- Model quantization & testing: 2 weeks
- Firmware development (per-node inference): 3 weeks
- Gateway coordination logic: 2 weeks
- Steering vector implementation: 1 week
- Testing & debugging: 2 weeks
- Enclosures & final assembly: 1 week
- **Total: 15 weeks**

**Available:** Check action-plans/Assignments/PRIORITY_Incomplete_Weeks_Summary.md for remaining time.

**Concern:** This might be too much.

**Investigation needed:**
- [ ] Create detailed Gantt chart
- [ ] Identify parallelizable tasks (can PCB fab happen while coding?)
- [ ] Define MVP (minimum viable project): What's essential vs nice-to-have?
- [ ] Checkpoint milestones: Ensure early validation before full investment


---

## Experimental Validation Questions

### 20. How to Measure Success?

**Question:** What does "working" mean for this project?

**Possible metrics:**

**Technical:**
- [ ] System generates coherent tokens (perplexity < threshold)
- [ ] Sensor changes measurably affect token probabilities (deviation magnitude > X)
- [ ] All nodes successfully coordinate (packet loss < 10%)
- [ ] Timing matches predictions (±20%)

**Philosophical:**
- [ ] Embedding-space trajectories visualize meaningfully
- [ ] Observers can intuit environmental state from outputs
- [ ] System exhibits "deliberative" quality (hard to quantify)

**Educational:**
- [ ] Demo engages viewers (qualitative feedback)
- [ ] Concept is communicable (viewers understand what it does)
- [ ] Documentation teaches distributed cognition effectively

**Investigation needed:**
- [ ] Define success criteria BEFORE building (avoid moving goalposts)
- [ ] Pre-register predictions (what do you expect to happen?)
- [ ] Design validation experiments (how to test each metric?)


### 21. Control Experiments

**Question:** How to prove sensor coupling matters vs just noise?

**Experimental design:**

**Condition 1 (Baseline):** No sensors, normal inference  
**Condition 2 (Random):** Add random noise instead of sensor data  
**Condition 3 (Sensor):** Add real sensor-derived steering

**Prediction:** Condition 3 outputs should correlate with environmental state better than Conditions 1-2.

**Measurement:**
- Human raters judge which outputs match environment (blind test)
- Embedding distance between outputs and expected semantic direction
- Information-theoretic: Mutual information between sensor readings and token distribution

**Investigation needed:**
- [ ] Design blinded rating protocol
- [ ] Pre-compute "expected" semantic directions for environmental states
- [ ] Statistical power analysis (how many trials needed?)


---

## Holes in Thinking (Friendly Critique)

### 22. Are You Overloading "Consciousness"?

**Concern:** Calling the mesh "conscious" might be:
- Too strong a claim (invites ridicule)
- Distracting from technical contributions
- Philosophically unjustified

**Your framework says:**
> "Consciousness = organizational patterns tracking themselves."

**But:**
- Is the mesh "tracking itself"? Or just executing computation?
- Does KV cache + embedding space = "self-tracking"?
- Risk: Smuggling in human-like phenomenology where it doesn't belong

**Investigation needed:**
- [ ] Tighten definition: What specifically counts as "tracking"?
- [ ] Consider weaker claim: "The mesh exhibits cognitive-like properties" vs "The mesh is conscious"
- [ ] Strategy: For FabAcademy, avoid "consciousness" talk, stick to "distributed cognition"


### 23. Is Steering Vector Direction Interpretable?

**Assumption:** Sensor X → steering direction Y → semantic shift Z.

**Problem:**
- LLM latent space is high-dimensional and weird
- Steering vectors work, but mechanism poorly understood
- Adding sensor-derived vector might not have predictable semantic effect

**Example worry:**
- Loud noise → steering vector V
- You hope V steers toward "turbulent" semantics
- But V might steer toward "blue" or "France" or nonsense (latent space is alien)

**Investigation needed:**
- [ ] Validate interpretability: Test known steering vectors, verify semantic effect
- [ ] Sensors: Start with simple mappings (sound → turbulence PC axis)
- [ ] Expect surprises: Latent space might not cooperate


### 24. Falsifiability

**Question:** What would prove this approach wrong?

**Good science:** Make predictions that could be false.

**Your predictions (implied):**
- Sensor changes will measurably affect outputs
- Deviations will encode environmental topology
- Timescale will enable better integration

**Potential falsifications:**
- Sensor steering has no measurable effect (indistinguishable from noise)
- Deviations are random, no spatial pattern
- Slow inference produces worse outputs than fast (accumulates errors)

**Investigation needed:**
- [ ] Explicitly state falsifiable predictions
- [ ] Define thresholds: "Measurable effect" = deviation > X standard deviations
- [ ] Accept falsification gracefully: If sensors don't work, document why (still valuable)


---

## Further Research Directions

### 25. Extensions if This Works

**If successful, what's next?**

**Technical:**
- [ ] Closed-loop: Outputs trigger actions that modify environment
- [ ] Learning: On-device adaptation (few-shot learning on nodes)
- [ ] Heterogeneous sensors: Mix sound, light, vibration, chemical
- [ ] Larger mesh: 32+ nodes, multiple "brain regions"
- [ ] Model parallelism: Shard FFN as well, full model distribution

**Philosophical:**
- [ ] Paper: "Basal Cognition as Design Principle for Distributed AI"
- [ ] Collaboration with Levin lab: Test biological analogies rigorously
- [ ] Phenomenology: Interview users about perceived "intelligence" of mesh

**Applications:**
- [ ] Environmental monitoring: Seismic sensing, air quality, wildlife tracking
- [ ] Education: Kit for teaching distributed systems + AI
- [ ] Art installation: "Infrastructure Art" - making computation visible


### 26. Related Projects to Explore

**Swarm robotics:**
- Kilobot (1000-robot swarm)
- Could LLMs coordinate swarm behavior via distributed inference?

**Morphogenetic computing:**
- Growing circuits instead of designing
- Levin's bioelectric approach applied to hardware

**Active matter:**
- Self-organizing particles
- Connection to self-organizing computation?

**Investigation needed:**
- [ ] Survey these fields for relevant methods/insights
- [ ] Identify collaboration opportunities


---

## Summary: What We Don't Know

**Technical:**
1. Will 2-bit quantization work?
2. Can ESP32 handle the memory load?
3. Is LoRa fast enough for usable experience?
4. How to map sensors → steering vectors reliably?

**Philosophical:**
5. Does sensor coupling constitute embodiment?
6. Is "consciousness at LoRa timescales" a valid claim?
7. Where's the boundary of the computational self?

**Practical:**
8. Can this be built in remaining FabAcademy time?
9. How to debug distributed system?
10. What counts as "success"?

**Research:**
11. Has anyone done this before?
12. What's the relationship to existing work (Levin, swarm robotics, etc.)?
13. Is this publishable beyond FabAcademy?

**Next step:** Prioritize these questions. Some block implementation (1-4), others are theoretical (5-7). Address blockers first.

---

## Navigation

- [← Back to Philosophy](03_PHILOSOPHY.md)
- [← Back to Overview](00_OVERVIEW.md)
- [← Back to Evolution Log](intialIdea.md)

---

*"The map is not the territory. These questions map the unknown territory ahead."*
