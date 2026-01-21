# Decentralised LLM Inference: Conceptual Overview

**Purpose:** Big-picture map of the project architecture and how concepts interconnect.

---

## The Core Idea (One Sentence)

A mesh network of sensor-equipped microcontrollers, each computing one attention head of a distributed LLM, where environmental sensor readings act as steering vectors to couple the model's linguistic output to physical reality.

---

## System Architecture (Visual Map)

```
┌─────────────────────────────────────────────────────────────┐
│                    GATEWAY NODE                              │
│  • Receives user prompt                                      │
│  • Broadcasts hidden state to all nodes (2KB → 9 packets)   │
│  • Aggregates head outputs into full layer                   │
│  • Applies feed-forward network                              │
│  • Repeats for 22 layers                                     │
│  • Returns final token                                       │
└─────────────────────────────────────────────────────────────┘
                            ↓ ↑
                    (LoRa Mesh Network)
                            ↓ ↑
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   NODE 1     │  │   NODE 2     │  │   NODE 3     │  ...  [NODE 16]
│              │  │              │  │              │
│ Head 1       │  │ Head 2       │  │ Head 3       │
│ Sensor A     │  │ Sensor B     │  │ Sensor C     │
│ (sound)      │  │ (vibration)  │  │ (light)      │
│              │  │              │  │              │
│ Computes:    │  │ Computes:    │  │ Computes:    │
│ Q, K, V      │  │ Q, K, V      │  │ Q, K, V      │
│ + Steering   │  │ + Steering   │  │ + Steering   │
│              │  │              │  │              │
│ Returns:     │  │ Returns:     │  │ Returns:     │
│ 128 bytes    │  │ 128 bytes    │  │ 128 bytes    │
└──────────────┘  └──────────────┘  └──────────────┘

         ↓                ↓                ↓
    Environment      Environment      Environment
    (explosion)      (calm)           (bright flash)
```

---

## Three Layers of the System

### Layer 1: Hardware Substrate

**Components:**
- ESP32-S3 microcontrollers (one per node)
- LoRa radios (SX1276/SX1262)
- Environmental sensors (microphone, accelerometer, light sensor, etc.)
- Gateway device (likely more powerful - Raspberry Pi or laptop)


**Constraints:**
- ESP32-S3 RAM: ~512KB PSRAM
- LoRa bandwidth: 230 bytes/packet, ~1-2s latency
- Power: Battery or solar for remote nodes


---

### Layer 2: Computational Architecture

**Head-Sharding Strategy:**

Each node stores:
- Weights for ONE attention head (~4.4MB at 8-bit, target ~1MB with 2-bit quantization)
- KV cache for its head (~131KB for 512-token context)

Per-layer computation flow:
1. Gateway broadcasts hidden state (2048 floats)
2. Node receives state
3. Node computes Q, K, V projections for its head
4. Node performs attention using cached K, V
5. Node adds sensor-derived steering vector
6. Node broadcasts 128-byte result back to gateway

Gateway assembles 16 head outputs → full layer → next layer


**Timing:**
- Per token: ~13-15 minutes (with optimizations: 5-7 minutes)
- This is INTENTIONAL - allows visualization of computation


---

### Layer 3: Semantic Coupling

**Steering Vectors:**

Environmental sensors transform physical state into latent space vectors:

```python
# Each node independently
sensor_reading = {
    'sound_pressure': 0.8,  # loud
    'vibration': 0.6,       # unstable
    'light': 0.2            # dark
}

# Transform to model's hidden dimension (2048)
steering_vector = sensor_to_latent(sensor_reading)

# Add to head output (biases semantic direction)
head_output = attention(Q, K, V) + alpha * steering_vector
```

**Effect:**
- Calm environment → output tokens drift toward "peaceful", "stable"
- Loud environment → output tokens drift toward "turbulent", "alert"
- Heterogeneous sensors → spatially distributed environmental map encoded in token probabilities


---

## How Concepts Interconnect

### Technical ↔ Biological

| Transformer Architecture | Mycelial Network |
|--------------------------|------------------|
| Attention heads | Hyphal tips |
| KV cache | Local memory of nutrient gradients |
| Steering vectors | Environmental chemotaxis |
| Aggregate softmax | Emergent routing decisions |
| No single node has full model | No central brain coordinates |


### Technical ↔ Philosophical

| System Component | Deleuzian Concept | Your Framework |
|------------------|-------------------|----------------|
| ESP32 nodes | Substrate | That which affects and is affected |
| Mesh topology | Assemblage | Structure producing emergent effects |
| Sensors | Interface | Differential engagement with environment |
| Attention weights + KV cache | Organization | Capacity for affecting patterns |
| Token output | Line of Flight | New territory opened by encounter |


### Biological ↔ Philosophical

| Levin's Basal Cognition | Deleuze/Your Framework |
|-------------------------|------------------------|
| Cellular collective intelligence | Assemblage without subject |
| Bioelectric signaling | Flows through substrate |
| Goal-directed behavior without brain | Intentionality at multiple scales |
| Scale-free cognition | Timescale relativism |


---

## The Three Key Insights

### 1. Slowness Is The Feature

Traditional framing: "LoRa is too slow for LLM inference."  
Reframe: "LoRa operates at educational visualization timescales."

The 11-hour token generation allows you to:
- See which nodes are computing (LED visualization)
- Watch "thought" propagate through the mesh
- Understand distributed cognition viscerally
- Create "Infrastructure Art" (makes invisible computation visible)


### 2. Sensors Couple Computation To Reality

Traditional LLM: Text in → Text out (closed linguistic loop)  
This system: Environment + Text in → Environment-influenced Text out

The embedding-space trajectories encode environmental topology. Deviations in token probabilities reveal which regions of the mesh are sensing novel stimuli.

**Potential applications:**
- Distributed environmental monitoring
- Early warning system (seismic, acoustic anomalies)
- Research tool for studying embodied inference


### 3. Consciousness Across Timescales

From `consciousness_complexity_epistemology.md`:
> "The pebble IS conscious - just at geological timescales."

Your mesh network operates at **LoRa consciousness timescales** (minutes to hours). Not slower AI - different temporal mode of intelligence.

This is legitimate research question: Does the timescale of inference change the nature of cognition?


---

## Project Variants (Complexity Trade-Offs)

### Variant A: Full Council (16 Nodes)
- One node per attention head
- Complete head-sharding
- Maximum theatrical effect
- Highest complexity


### Variant B: Grouped Heads (4-6 Nodes)
- Each node computes 3-5 heads
- Faster per-token generation
- Easier debugging
- Still demonstrates core concept


### Variant C: Agentic Mesh (3-5 Nodes)
- Don't shard heads - shard cognitive roles
- Node 1: Retrieval (RAG database)
- Node 2: Reasoning (small local model)
- Node 3: Filtering (safety, coherence)
- Node 4: Sensors → steering for Node 2
- Easiest to implement
- Most practical utility


---

## Current Status

**Phase:** Conceptual foundations complete  
**Next:** Technical feasibility analysis (memory budgets, quantization research)

See `intialIdea.md` for detailed evolution log.

---

## Navigation

- **Detailed Explanations:** `01_DETAILED_CONCEPTS.md`
- **Term Definitions:** `02_GLOSSARY.md`
- **Philosophical Framework:** `03_PHILOSOPHY.md`
- **Open Problems:** `04_OPEN_QUESTIONS.md`
- **Evolution Log:** `intialIdea.md`

---

*"Not a distributed chatbot. A mechanical brain that thinks at geological speeds."*
