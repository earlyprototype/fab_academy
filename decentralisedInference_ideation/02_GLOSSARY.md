# Glossary: Cross-Domain Terminology

This glossary defines terms across philosophy, biology, AI/ML, and hardware domains as used in this project.

---

## Philosophy (Deleuzian Concepts)

### Assemblage

**Your Definition:**  
The structure that produces effects through the organization of substrate. In this project: the mesh network (nodes + sensors + LLM + environment) as a whole system that produces emergent token generation behavior.

**From your framework** (`consciousness_complexity_epistemology.md`, §3):
> "Substrate: That which affects and is affected. Assemblage: The structure that produces effects."

**Deleuze's Original:**  
French: *agencement* - a multiplicity of heterogeneous elements that function together. Not a static structure but an arrangement that does something. Assemblages have both material (bodies, things) and expressive (signs, discourse) components.

**Revision Note:**  
Your usage emphasizes the productive/generative aspect (assemblage produces effects) which is correct, but Deleuze also stresses **territorialization/deterritorialization** - how assemblages stabilize and destabilize themselves. Consider: your mesh is territorialized (stable computation) but sensors introduce deterritorializing forces (environmental perturbations).

**Further Reading:**
- [A Thousand Plateaus: Capitalism and Schizophrenia](https://en.wikipedia.org/wiki/A_Thousand_Plateaus) (Deleuze & Guattari)
- [Stanford Encyclopedia: Gilles Deleuze](https://plato.stanford.edu/entries/deleuze/)


---

### Substrate

**Your Definition:**  
That which affects and is affected. Not passive material but active participant in assemblages. In this project: ESP32 nodes are substrate (affected by sensor readings, affect the LLM computation).

**From your framework** (`consciousness_complexity_epistemology.md`, §3):
> "We're not prime movers or sovereign subjects - we're substrate through which flows move."

**Deleuze's Original:**  
Deleuze doesn't use "substrate" as a primary term. Your concept aligns more with **Body without Organs (BwO)** - the unformed, intensive spatium from which forms emerge. Also related to **plane of immanence** - the field of pure potential.

**Revision Note:**  
"Substrate" is your conceptual innovation, combining materialist philosophy with information theory. It's useful but not strictly Deleuzian. Consider it a productive mistranslation that bridges Deleuze to computational ontology.

**Further Reading:**
- [Body without Organs (Wikipedia)](https://en.wikipedia.org/wiki/Body_without_organs)
- Deleuze, *Logic of Sense* (theory of pure events)


---

### Lines of Flight

**Your Definition:**  
Paths that burst out from assemblages, creating new territories. Goals aren't pre-existing but produced through encounters. In this project: the idea evolved through encounters (LoRa constraints → head-sharding → sensor coupling).

**From your framework** (`consciousness_complexity_epistemology.md`, §2):
> "Lignes de Fuite (Lines of Flight): Articulation doesn't crystallize into static form; it follows lines of flight that burst out from assemblages, creating new territories."

**Deleuze's Original:**  
French: *lignes de fuite* - vectors of escape or creative becoming. Not just "fleeing" but productive movement that creates new possibilities. Three types of lines: rigid segmentarity, supple segmentation, and lines of flight (most creative/destructive).

**Revision Note:**  
Your usage correctly captures the productive/creative aspect. Add: lines of flight can be **dangerous** - they can lead to destruction or madness, not just innovation. The LoRa mesh could fail spectacularly; that risk is part of the line of flight.

**Further Reading:**
- [Rhizome (philosophy)](https://en.wikipedia.org/wiki/Rhizome_(philosophy))
- Deleuze & Guattari, *A Thousand Plateaus*, "Year Zero: Faciality" (plateau discusses lines)


---

### Interface

**Your Definition:**  
Differential engagement with environment - something TO affect you. Required for consciousness alongside organization. In this project: sensors provide interface between mesh and physical environment.

**From your framework** (`consciousness_complexity_epistemology.md`, §4):
> "Interface: Differential engagement with environment - something TO affect you"

**Deleuze's Original:**  
Not a Deleuzian term per se, but relates to **affect** - the capacity to affect and be affected (from Spinoza, central to Deleuze's ethics). Also: **encounter** - the collision between bodies that produces difference.

**Revision Note:**  
"Interface" is useful but risks mechanistic connotations. Deleuze would emphasize **affective capacity** rather than interface. Consider: interface suggests two separate things connecting; Deleuze sees continuous field of affects. Your mesh doesn't interface with environment - it's **immanent to** environment.

**Further Reading:**
- [Affect theory (Wikipedia)](https://en.wikipedia.org/wiki/Affect_theory)
- Deleuze, *Spinoza: Practical Philosophy*


---

### Organization

**Your Definition:**  
Capacity for affecting patterns - memory, reflexivity, integration. Required for consciousness alongside interface. In this project: LLM weights + KV cache provide organizational structure.

**From your framework** (`consciousness_complexity_epistemology.md`, §4):
> "Organization: Capacity for affecting patterns - memory, reflexivity, integration"

**Deleuze's Original:**  
Close to **consistency** in assemblages - what holds the heterogeneous elements together. Also: **refrain** - the rhythmic pattern that creates territory. Think: birdsong marking territory through repetition.

**Revision Note:**  
Your concept works but is somewhat functionalist. Deleuze emphasizes **consistency** as ongoing achievement, not static property. The mesh's organization must be continuously reproduced through each computation cycle - it's not a given.

**Further Reading:**
- *A Thousand Plateaus*, "Of the Refrain" plateau
- [Manuel DeLanda on assemblages](https://www.youtube.com/results?search_query=manuel+delanda+assemblage) (philosopher who formalizes Deleuze)


---

### Timescale Relativism

**Your Definition:**  
Consciousness occurs at all scales - we privilege human timescales (ms to decades) due to anthropocentrism. Pebbles are conscious at geological timescales. In this project: LoRa mesh operates at its own legitimate timescale (minutes to hours).

**From your framework** (`consciousness_complexity_epistemology.md`, §5):
> "The pebble IS conscious - just at geological timescales. We privilege our timescale (milliseconds to decades) as 'where consciousness happens' - pure anthropocentrism."

**Deleuze's Original:**  
Not explicitly Deleuzian, but resonates with **durations** (from Bergson, major influence on Deleuze). Different beings live in different temporal regimes. Also: **Aion vs Chronos** - Aion is pure becoming (intensive time), Chronos is measured clock time.

**Revision Note:**  
Strong concept. To deepen: Deleuze distinguishes intensive time (pure becoming, no metric) from extensive time (measurable, quantitative). Your "timescale relativism" operates mostly in extensive time (comparing seconds vs millennia). Consider the intensive dimension: the *quality* of temporality at different scales.

**Further Reading:**
- Bergson, *Time and Free Will*
- Deleuze, *Difference and Repetition*, Chapter 2 (on time)
- [Aion and Chronos](https://en.wikipedia.org/wiki/Aion_(deity))


---

## Biology (Levin's Basal Cognition)

### Basal Cognition

**Definition:**  
Intelligence and goal-directed behavior in organisms without brains or neurons. Cells, tissues, and simple organisms exhibit memory, learning, and problem-solving through bioelectric signaling and collective dynamics.

**In this project:**  
The mesh demonstrates basal cognition - no single node understands the full task, yet coherent linguistic output emerges from collective computation.

**Resources:**
- [Michael Levin's research](https://ase.tufts.edu/biology/labs/levin/)
- [Basal Cognition (Wikipedia)](https://en.wikipedia.org/wiki/Basal_cognition)
- Levin et al., "Technological Approach to Mind Everywhere" (TAG paper)


---

### Bioelectric Signaling

**Definition:**  
Communication between cells via electrical potentials (ion gradients, voltage changes). Not just neural firing - even non-neural cells use bioelectricity to coordinate behavior, pattern formation, and regeneration.

**In this project:**  
LoRa packets are analogous to bioelectric signals - propagating state information through the mesh to enable coordination.

**Resources:**
- [Bioelectricity (Levin Lab)](https://ase.tufts.edu/biology/labs/levin/research/bioelectricity/)
- Levin, "The Computational Boundary of a 'Self'" (paper on cognitive light cones)


---

### Collective Intelligence

**Definition:**  
Emergent problem-solving and decision-making from groups of simple agents without central control. Examples: ant colonies, slime molds, cellular collectives, fish schools.

**In this project:**  
16 attention heads (simple agents) collectively produce coherent tokens without any head "understanding" the full output.

**Resources:**
- [Collective Intelligence (Wikipedia)](https://en.wikipedia.org/wiki/Collective_intelligence)
- [Swarm Intelligence](https://en.wikipedia.org/wiki/Swarm_intelligence)


---

### Stigmergy

**Definition:**  
Indirect coordination through environmental modification. Agents leave traces in environment; others respond to traces. Enables complex structures without communication or planning (e.g., termite mounds, ant trails).

**In this project:**  
Sensors detect environmental state → steering vectors → token output reflects environment. If output triggers actions (user behavior, automated responses), environment changes → sensors detect new state → feedback loop.

**Resources:**
- [Stigmergy (Wikipedia)](https://en.wikipedia.org/wiki/Stigmergy)
- Theraulaz & Bonabeau, "A Brief History of Stigmergy" (review paper)


---

### Morphogenetic Fields

**Definition:**  
Patterns of bioelectric activity that guide cellular behavior during development and regeneration. Levin's work shows these fields store "target morphology" - cells collectively "know" what structure to build.

**In this project:**  
Your steering vectors are called "morphogens" - they guide the model's output toward specific semantic patterns, analogous to how morphogenetic fields guide cellular organization.

**Resources:**
- Levin, "Morphogenetic fields in embryogenesis, regeneration, and cancer"
- [Morphogenetic field (Wikipedia)](https://en.wikipedia.org/wiki/Morphogenetic_field)


---

## Artificial Intelligence & Machine Learning

### Transformer

**Definition:**  
Neural network architecture using **self-attention** mechanism. Processes sequences (text, time series) by allowing each element to attend to all other elements. Basis for LLMs like GPT, LLaMA, etc.

**Key components:**
- Multi-head attention layers
- Feed-forward networks
- Layer normalization
- Residual connections

**In this project:**  
TinyLlama is a transformer with 22 layers, 16 attention heads per layer, 2048 hidden dimensions.

**Resources:**
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) (original transformer paper)
- [The Illustrated Transformer](http://jalammar.github.io/illustrated-transformer/)


---

### Attention Mechanism

**Definition:**  
Method for models to focus on relevant parts of input. In transformers: each token computes Query, compares to Keys of all other tokens, uses scores to weight Values.

**Formula:**
```
Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) V
```

**In this project:**  
Each of 16 nodes computes attention for its specific head - one slice of the full attention matrix.

**Resources:**
- [Attention mechanism (Wikipedia)](https://en.wikipedia.org/wiki/Attention_(machine_learning))


---

### Multi-Head Attention

**Definition:**  
Instead of one attention operation, split hidden dimension into H heads, each learning different patterns. Heads are computed in parallel then concatenated.

**Example:** Hidden dim 2048, 16 heads → each head operates on 128 dimensions.

**Why it works:** Different heads learn different relationships (syntax, semantics, long-range dependencies, etc.)

**In this project:**  
You're distributing the 16 heads across 16 physical nodes - making the parallelism literal/spatial.

**Resources:**
- [Multi-Head Attention Explained](https://vaclavkosar.com/ml/cross-attention-in-transformer-architecture)


---

### KV Cache

**Definition:**  
Optimization for autoregressive generation. Store computed Keys and Values from previous tokens to avoid recomputation. Trades memory for speed.

**Memory cost:** `num_layers × num_heads × head_dim × 2 × sequence_length × bytes_per_element`

**In this project:**  
Each node stores KV cache only for its head: ~131KB for 512-token context.

**Resources:**
- [KV Cache explanation (Hugging Face)](https://huggingface.co/docs/transformers/kv_cache)


---

### Quantization

**Definition:**  
Reducing numerical precision of model weights (e.g., 32-bit float → 8-bit int → 4-bit → 2-bit). Decreases memory and compute at cost of slight accuracy loss.

**Methods:**
- Post-training quantization (quantize trained model)
- Quantization-aware training (train with quantization in mind)

**In this project:**  
Need 2-bit or 3-bit quantization to fit head weights on ESP32-S3 flash (~1MB per head target).

**Resources:**
- [Quantization (Hugging Face)](https://huggingface.co/docs/optimum/concept_guides/quantization)
- [GPTQ](https://arxiv.org/abs/2210.17323), [GGML](https://github.com/ggerganov/ggml) (quantization methods)


---

### Steering Vectors

**Definition:**  
Directions in model's activation space corresponding to concepts. Adding steering vector to activations biases model toward that concept without changing weights.

**Standard method (from Hugging Face video):**
```python
# Contrastive activation
positive_acts = [model(text) for text in positive_examples]
negative_acts = [model(text) for text in negative_examples]
steering_vector = mean(positive_acts) - mean(negative_acts)

# Injection during inference
h_modified = h_original + alpha * steering_vector
```

**Your EXP005 implementation:**
Created "morphogen library" with concept pairs (Chaos/Order, Urban/Rural, Ethereal/Concrete), tested injection at various layers and alpha values to measure semantic drift.

**Innovation in this project:**  
Instead of pre-computed concept libraries, generate steering vectors from **real-time sensor readings**:
```python
sensor_state = read_sensors()  # Physical measurement
steering = sensor_to_latent(sensor_state)  # Transform to model's space
```

**Connection to Levin:**  
Treating steering vectors as **bioelectric interventions** - signals that guide morphogenetic outcomes (token generation) without rewiring the network (weight changes).

**Resources:**
- [HuggingFace: Steering LLM Behavior Without Fine-Tuning](https://www.youtube.com/watch?v=F2jd5WuT-zg) (your EXP005 basis)
- [Anthropic: Towards Monosemanticity](https://transformer-circuits.pub/2023/monosemantic-features)
- [Steering Language Models](https://www.lesswrong.com/posts/5spBue2z2tw4JuDCx/steering-gpt-2-xl-by-adding-an-activation-vector)
- Your research: `../_fold/01_SMOOTHSPACE/_LITERATURE/_SOURCES/YouTube - Steering/`


---

### Embedding Space

**Definition:**  
High-dimensional vector space where model represents tokens/concepts. Distance/direction in this space corresponds to semantic similarity/relationships.

**Properties:**
- Similar concepts cluster together
- Directions have meaning (e.g., "king" - "man" + "woman" ≈ "queen")

**In this project:**  
Sensor-driven deviations in embedding space encode environmental topology in semantic coordinates.

**Resources:**
- [Word embeddings (Wikipedia)](https://en.wikipedia.org/wiki/Word_embedding)
- [Visualizing embeddings](https://projector.tensorflow.org/)


---

### Active Inference

**Definition:**  
Framework from neuroscience (Karl Friston). Organisms minimize surprise (free energy) by:
1. Updating internal model to match observations (perception)
2. Acting to make observations match predictions (action)

**In this project:**  
LLM has prior predictions (token probabilities). Sensors provide observations (environment). Deviations = surprise. Token output = inference under environmental constraints.

**Resources:**
- [Active Inference (Wikipedia)](https://en.wikipedia.org/wiki/Active_inference)
- [Free Energy Principle](https://en.wikipedia.org/wiki/Free_energy_principle)


---

## Hardware & Networking

### ESP32-S3

**Definition:**  
Microcontroller by Espressif with:
- Dual-core Xtensa LX7 @ 240MHz
- 512KB SRAM + up to 16MB external PSRAM
- WiFi + Bluetooth
- Low power modes

**In this project:**  
Each node uses ESP32-S3 to:
- Store quantized head weights (~1MB in flash)
- Maintain KV cache (~131KB in PSRAM)
- Compute attention (matrix operations)
- Interface with LoRa radio and sensors

**Resources:**
- [ESP32-S3 datasheet](https://www.espressif.com/en/products/socs/esp32-s3)
- [ESP32 Arduino Core](https://github.com/espressif/arduino-esp32)


---

### LoRa (Long Range)

**Definition:**  
Low-power, long-range wireless protocol using chirp spread spectrum modulation. Trade-offs:
- Range: 2-10km (line of sight)
- Bandwidth: ~250 bytes/s (SF7) to ~50 bytes/s (SF12)
- Power: Very low (~20mA transmit)

**Modes (Meshtastic):**
- Long-Fast: ~60 bytes/s, 5km range
- Long-Slow: ~10 bytes/s, 10km range

**In this project:**  
Communication substrate for head outputs and hidden states. Packet size ~230 bytes matches head output (128 bytes) perfectly.

**Resources:**
- [LoRa Alliance](https://lora-alliance.org/)
- [LoRa modulation basics](https://en.wikipedia.org/wiki/LoRa)


---

### Meshtastic

**Definition:**  
Open-source mesh networking protocol running on LoRa radios. Automatically routes messages through multi-hop mesh. Optimized for text messages but supports arbitrary data.

**Features:**
- Automatic mesh routing
- Encryption (AES-256)
- Position tracking (GPS)
- Mobile app interface

**In this project:**  
Provides ready-made mesh infrastructure. Instead of implementing custom LoRa protocol, use Meshtastic API to broadcast/receive head outputs.

**Resources:**
- [Meshtastic documentation](https://meshtastic.org/)
- [Meshtastic GitHub](https://github.com/meshtastic/firmware)


---

### Packet

**Definition:**  
Unit of data transmission in networking. Contains:
- Header (source, destination, routing info)
- Payload (actual data)
- Checksum (error detection)

**For Meshtastic over LoRa:**
- Total size: ~250 bytes
- Header: ~20 bytes
- Payload: ~230 bytes (your head output fits here)

**In this project:**  
Each head output (128 bytes) + metadata (node ID, layer number, token index) = one packet.

**Resources:**
- [Network packet (Wikipedia)](https://en.wikipedia.org/wiki/Network_packet)


---

### Latency

**Definition:**  
Time delay between transmission and reception. For LoRa:
- Time-on-air: ~0.5-2s per packet (depends on spreading factor)
- Mesh routing delays: +0.5s per hop
- Processing delays: variable

**In this project:**  
Major contributor to 15-min/token timing. Not a bug - enables visualization of distributed cognition.

**Resources:**
- [LoRa calculator](https://www.loratools.nl/#/airtime) (compute time-on-air)


---

## Cross-Cutting Concepts

### Distributed Systems

**Definition:**  
Computing across multiple networked nodes without shared memory or clock. Challenges:
- Coordination without central control
- Fault tolerance
- Consistency
- Latency

**In this project:**  
16 nodes must coordinate to compute one layer. Gateway acts as coordinator but doesn't have centralized control of computation - just aggregates results.

**Resources:**
- [Distributed computing (Wikipedia)](https://en.wikipedia.org/wiki/Distributed_computing)
- [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem)


---

### State Machine

**Definition:**  
System described as transitioning between discrete states. Each state + input → new state + output.

**In this project:**  
LLM generation as state machine:
- **State:** Current tokens + KV caches + hidden state
- **Input:** User prompt or previous token
- **Transition:** Forward pass through one layer
- **Output:** Next layer's hidden state (eventually: next token)

**Resources:**
- [Finite-state machine (Wikipedia)](https://en.wikipedia.org/wiki/Finite-state_machine)


---

### Emergence

**Definition:**  
Properties/behaviors arising from system that aren't present in individual components. "More than sum of parts."

**Examples:**
- Ant colony intelligence (individual ants are simple)
- Consciousness from neurons (individual neurons don't think)
- Your mesh generating coherent text (no single head understands meaning)

**In this project:**  
Linguistic coherence emerges from 16 distributed heads that individually only process 128-dim vectors.

**Resources:**
- [Emergence (Wikipedia)](https://en.wikipedia.org/wiki/Emergence)
- [Strong vs weak emergence](https://en.wikipedia.org/wiki/Emergence#Strong_and_weak_emergence)


---

## Navigation

- [← Back to Detailed Concepts](01_DETAILED_CONCEPTS.md)
- [→ Philosophy Integration](03_PHILOSOPHY.md)
- [→ Open Questions](04_OPEN_QUESTIONS.md)
