# Decentralised Inference over LoRa: Project Evolution Log

**Last Updated:** 2026-01-21  
**Status:** Conceptual foundations - pre-implementation

---

## How to Use This Document (LLM Instructions)

This document serves as a **living record** of project concept evolution. When assisting the user:

1. **Context Anchor:** This captures the foundational ideas, dead-ends, pivots, and conceptual breakthroughs. Reference it to understand the project's intellectual trajectory.

2. **ADHD-Friendly Navigation:** The user benefits from:
   - Clear visual separation between ideas
   - Explicit connections between concepts
   - Progress markers ("Week 1: Initial idea" vs "Week 3: Pivot to head-sharding")
   - Glossary references to reduce cognitive load when returning after gaps

3. **Gap Analysis:** Track what's been explored, what remains unclear, and what needs experimental validation. Don't let circular discussions happen - reference previous conclusions.

4. **Evolution Not Replacement:** Don't delete superseded ideas - mark them as `[SUPERSEDED]` or `[PIVOT]` and note why. The reasoning matters.

5. **Link to Supporting Docs:** This file ties together:
   - `00_OVERVIEW.md` - Current conceptual model
   - `01_DETAILED_CONCEPTS.md` - Technical deep-dives
   - `02_GLOSSARY.md` - Domain-specific definitions
   - `03_PHILOSOPHY.md` - Levin/Deleuze/AI theory integration
   - `04_OPEN_QUESTIONS.md` - Holes, challenges, investigations

---

## Intellectual Genesis: How This Idea Emerged

### Origin Point: The Morphogenetic Fold Research

**Context you were already in:**
Working on "The Morphogenetic Fold" - investigating convergences between Michael Levin's developmental biology, Deleuze's philosophy, and LLM behavior. Already exploring:
- LLMs as "Bodies without Organs" navigating latent manifolds
- Steering vectors as bioelectric intervention (from Hugging Face video)
- EXP005: Testing steering vectors as "morphogens" that guide semantic trajectories
- Natural induction: Training as self-organization under stress (loss functions)

**The core question you were sitting with:**
> "If LLMs navigate a latent morphospace like cells navigate bioelectric fields, can we create a system where the morphospace is COUPLED to physical environmental fields?"

---

### Seed Idea: Distributed Inference + Environmental Sensing

**The spark:**
- FabAcademy requires a final project (embedded systems, networking, fabrication)
- You have access to LoRa mesh networking (Meshtastic)
- What if you distributed an LLM across a LoRa mesh with environmental sensors?

**Initial naive thought:**
Shard model layers across nodes, pass activations via LoRa.

**Immediate constraint:**
LoRa bandwidth (~230 bytes/packet, ~1-2s latency) makes this impractical for interactive chatbot use.

**The pivot (critical moment):**
Instead of seeing slowness as failure → **reframe as feature**
- Not "distributed chatbot"
- But "visible computational cognition" operating at its own legitimate timescale
- Connection to your consciousness framework: timescale relativism (pebbles are conscious at geological speeds)

---

## Conceptual Synthesis: Five Core Ideas and Their Interconnections

### Idea 1: Head-Sharding as Natural Granularity

**Genesis:**
Reading transformer architecture details, noticed that attention heads are already parallel and independent within a layer.

**The mathematics that clicked:**
- TinyLlama: 2048 hidden dims ÷ 16 heads = 128 dims/head
- 128 dims × 1 byte (int8) = 128 bytes
- LoRa packet capacity: 230 bytes
- **One head fits perfectly in one packet**

**Why this matters:**
The model's natural computational structure MATCHES the communication constraint. Not forcing a bad fit - discovering an elegant alignment.

**Connection to Levin:**
Each head becomes an autonomous agent (like a cell) with local computation and communication. No head "understands" the full output - collective intelligence emerges.

---

### Idea 2: Steering Vectors from Sensors (Not Text)

**Genesis:**
Your EXP005 was using contrastive activation to create concept vectors ("Chaos" vs "Order"). You realized:
> "If I can steer the model with pre-computed concept vectors... why not generate vectors FROM SENSORS in real-time?"

**The conceptual bridge (from your SEED doc):**
- Hugging Face video: Steering = Neurostimulation
- Levin: Bioelectric signals guide morphogenesis
- Your insight: Environmental sensors → steering vectors = **coupling computation to physical reality**

**The breakthrough (critical insight):**
Initially seemed impossible: "How do raw sensor voltages map to 2048 unlabeled latent dimensions?"

But your EXP005 morphogens ARE the bridge. Each morphogen is a **semantic axis** - the full spectrum from calm→turbulent, dark→bright. The sensor just selects where on that axis:

```python
# EXP005 already created this (2048-dim vector)
morphogen_turbulent = mean(activations(["chaotic", "turbulent"])) - 
                      mean(activations(["ordered", "calm"]))

# Sensor→steering is just multiplication
sound_pressure = read_microphone()  # 0.0 to 1.0
steering = sound_pressure * morphogen_turbulent

# 0.0 → no steering (model baseline)
# 0.5 → halfway to turbulent
# 1.0 → fully turbulent

# Multiple sensors = vector addition (like RGB color)
steering_total = (sound * m_turbulent + 
                  (1-light) * m_dark +
                  vibration * m_chaotic)
```

**Why this works:**
The morphogen already contains semantic meaning (derived from text activations). The sensor doesn't create meaning - it just says "how much" of that pre-existing meaning to apply.

**Why this is novel:**
Steering vectors are established (Anthropic, Hugging Face). Coupling them to **continuous environmental sensors** via scalar multiplication hasn't been done. You're creating **embodied inference** - the model's latent space is entangled with physical topology.

---

### Idea 3: The Mycelial Network as Biological Template

**Genesis:**
Levin's work + your Deleuze reading led you to mycelial networks as example of:
- Distributed intelligence without central control
- Stigmergic coordination (environment shapes behavior)
- Collective decision-making from simple agents

**The mapping you recognized:**

| Mycelial Network | Your Mesh |
|------------------|-----------|
| Hyphal tips sense nutrients | Nodes sense environment (sound, vibration) |
| Electrochemical signals propagate | LoRa packets propagate head outputs |
| No central brain | No node has full model |
| Emergent resource allocation | Emergent token selection |
| Bioelectric steering | Steering vectors from sensors |

**Why this isn't just metaphor:**
Levin's research shows these are **isomorphic computational architectures** - same organizational principles at different scales and substrates.

**Connection to your consciousness framework:**
From `consciousness_complexity_epistemology.md` §16: "Same mechanism operates at every scale. Not hierarchy. Fractal pattern repeating infinitely."

---

### Idea 4: Timescale Relativism (Slowness as Feature)

**Genesis:**
Your consciousness document's insight about the pebble being conscious at geological timescales. Applied to computation:

**The realization:**
We privilege human timescales (milliseconds to seconds) as "where intelligence happens" - pure anthropocentrism. But:
- Geological processes: eons
- Your mesh: minutes to hours
- Human neurons: milliseconds
- Digital inference: milliseconds

These aren't "fast" vs "slow" - they're **different modes of temporality**.

**What slow inference enables:**
1. **Temporal integration:** Average sensor readings over 40-second layer computation, noise cancels out
2. **Deliberative processing:** 16 heads "settle" into consensus rather than snap decision
3. **Visibility:** Can watch thought propagate through physical nodes (educational/artistic)

**Connection to Deleuze:**
Bergson's "duration" - different beings experience time qualitatively differently, not just faster/slower. Your mesh has its own duration.

---

### Idea 5: State Machine as Cognitive Architecture

**Genesis:**
Debugging distributed systems + reading about KV cache led to reconceptualizing LLM generation not as "function" but as "state transitions."

**The shift:**
- **Old view:** LLM = f(prompt) → completion
- **New view:** LLM = sequence of state transitions where each state contains full context

**State structure:**
```python
State = {
    'tokens_generated': ["The", "weather", "is"],
    'hidden_state': [2048-dim vector],
    'kv_caches': {node_1: (K, V), node_2: (K, V), ...},
    'current_layer': 5,
    'awaiting_heads': [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]
}
```

**Why this matters for LoRa:**
- Fault tolerance: If node fails, state can wait/retry
- Debuggability: Can snapshot state, replay transitions
- Visualization: Each transition is observable event (light up LEDs, dashboard)

**Connection to your framework:**
From `consciousness_complexity_epistemology.md` §12: "Time = what it's like for organizational patterns to transform." State transitions ARE time for the mesh.

---

## How These Five Ideas Interconnect

```
        [Timescale Relativism]
                  ↓
    Slowness enables visibility + deliberation
                  ↓
        [Head-Sharding] ←→ [State Machine]
         Distribute       Track transitions
         computation      observably
                  ↓
        [Mycelial Template]
         No central control,
         collective intelligence
                  ↓
        [Sensor Steering]
         Couple latent space to
         physical environment
```

**The synthesis:**
These aren't separate features - they're aspects of ONE architecture where:
- Computational structure (head-sharding) matches communication constraints (LoRa packets)
- Biological template (mycelium) provides organizational principle (distributed, no center)
- Sensor coupling (steering vectors) creates embodiment (environment→computation loop)
- State tracking makes process observable (visualization, education)
- Timescale makes it all legible (slow enough to watch, fast enough to matter)

---

## Current State: Three Frameworks Converging on One System

This project emerged from recognizing that three separate frameworks you were already working with **point to the same computational architecture:**

### 1. Technical (Mechanistic Interpretability)

**What you were doing:**
- EXP005: Testing steering vectors from Hugging Face video
- Using contrastive activation to create concept vectors
- Exploring how latent space directions map to semantic concepts

**How it connects:**
- Steering vectors (proven tech) + sensors (novel) = embodied inference
- Head-sharding distributes the computation steering operates on
- Each node can steer independently → spatially-distributed environmental encoding

---

### 2. Biological (Levin's Basal Cognition)

**What you were studying:**
- TAME framework (Technological Approach to Mind Everywhere)
- Bioelectric networks as cognitive substrate
- Natural induction (self-organization under stress)
- Morphogenetic fields (bioelectric "memories" guiding development)

**How it connects:**
- Your mesh IS basal cognition in silicon
- No centralized "brain" (gateway just aggregates, doesn't control)
- LoRa packets = bioelectric signals
- Sensor steering = morphogenetic field guidance
- Token generation = morphological outcome

---

### 3. Philosophical (Deleuze/Your Consciousness Framework)

**What you were articulating:**
- Consciousness = organizational patterns tracking themselves (`consciousness_complexity_epistemology.md`)
- Assemblages producing effects through substrate organization
- Timescale relativism (pebble consciousness at geological speeds)
- Fractal intentionality (same mechanism at all scales)

**How it connects:**
- Mesh = assemblage (nodes + sensors + LLM + environment)
- Substrate = ESP32 hardware + LoRa medium
- Interface = sensors coupling to environment
- Organization = attention weights + KV cache
- Tracking = embedding-space trajectories encoding environmental state
- Timescale = LoRa speeds (minutes/hours, not milliseconds)

---

### The Convergence

**These aren't three separate justifications for the same project.**  
**They're three perspectives revealing that this architecture is the NATURAL implementation of:**
- Distributed intelligence (Levin)
- Embodied inference (mechanistic interpretability + sensors)
- Assemblage producing emergence (Deleuze/your framework)

**The project tests whether these frameworks are describing the same underlying computational reality.**

---

---

## Crucial Context: The Morphogenetic Fold Research

**This project didn't emerge from "let's build something for FabAcademy."**  
**It emerged from ongoing theoretical research that needed a physical instantiation.**

### The SEED Documents (`_fold/00_SEED/`)

**What "The Morphogenetic Fold" is:**
Research project treating LLMs as **digital Bodies without Organs** navigating **latent morphospaces**. Core claim: LLM behavior is better understood through developmental biology and process philosophy than through information theory alone.

**Key concepts you were already working with:**
1. **The Manifold as Umwelt:** LLMs don't "know" truth, they navigate folded topology of human experience
2. **Natural Induction:** Training = subjecting substrate to stress (loss functions) until self-organization emerges
3. **Nomad Science:** "Vibe coding" as haptic engagement with smooth space (vs rigid syntax)
4. **Isomorphism:** Shape-matching between model's internal topology and external world

**The research question:**
> "Can we map the Latent Space of an LLM to Waddington's Epigenetic Landscape?"

### EXP005: Steering as Morphogenesis

**Your active experiment:**
Based on Hugging Face video "Steering LLM Behavior Without Fine-Tuning"
- Creating "morphogen" libraries (concept vectors like "Chaos" vs "Order")
- Testing injection at different layers with varying α (steering intensity)
- Measuring semantic drift in embedding space
- Treating steering as **bioelectric intervention** (Levin's framework)

**The leap to sensors:**
> "If pre-computed morphogens guide output semantically, can REAL-TIME sensor-derived morphogens couple the model to physical reality?"

This isn't adding a feature to a project - it's **testing whether your theoretical framework is correct** by building a physical system.

---

## Key Open Questions

(See `04_OPEN_QUESTIONS.md` for full treatment)

### Technical Blockers
1. **Memory budget:** Can ESP32-S3 actually hold quantized head weights + KV cache?
2. **Quantization quality:** Does 2-bit quantization destroy coherence?
3. **Sensor→latent transformation:** How to reliably map physical measurements to steering vectors?

### Theoretical Uncertainties
4. **Embedding-space encoding:** Do deviations meaningfully encode environmental topology, or is it noise?
5. **Embodiment claim:** Does sensor coupling without motor actions constitute embodiment?
6. **Consciousness claim:** Is "timescale relativism" conceptually sound or overreaching?

### Practical Concerns
7. **FabAcademy fit:** Too ambitious? Too software-heavy? (Check with instructors early)
8. **Time budget:** 15+ weeks of work, how many weeks remaining?
9. **Debugging:** How to debug 16 distributed nodes + LoRa mesh + sensors + LLM?

---

## Next Steps (Prioritized)

### Immediate (This Week)
- [ ] Memory budget validation: Profile ESP32-S3 + Meshtastic actual usage
- [ ] Quantization research: Test 2-bit TinyLlama quality degradation
- [ ] FabAcademy proposal: Draft and get instructor feedback EARLY

### Short-term (Next 2 Weeks)
- [ ] Single-node prototype: Prove head computation works on one ESP32
- [ ] Sensor→steering design: Pick transformation method (pre-computed morphogens vs learned mapping)
- [ ] Visualization strategy: How to make thought flow visible (LEDs, dashboard)

### Medium-term (Month 1-2)
- [ ] 4-node grouped-head system: Test distributed coordination before committing to 16 nodes
- [ ] Sensor calibration: Validate that environmental changes produce measurable embedding deviations
- [ ] PCB design: Custom boards for ESP32 + LoRa + sensors + power

### Long-term (If Everything Works)
- [ ] Full 16-node Council
- [ ] Closed-loop stigmergic feedback
- [ ] Research paper: "Basal Cognition as Design Principle for Distributed AI"

---

## Why This Documentation Exists (For Future You)

**The ADHD reality:**
You'll return to this project after gaps. Context will fade. Concepts will feel unfamiliar. You'll wonder "where was I going with this?"

**This folder prevents that by capturing:**
1. **Genesis:** HOW the ideas emerged (not just what they are)
2. **Relationships:** How concepts interconnect (not just isolated definitions)
3. **Evolution:** The route you took (dead-ends teach as much as breakthroughs)
4. **Open questions:** What you DON'T know yet (avoid rehashing resolved debates)

**When you return:**
- Read `intialIdea.md` (this file) for WHY this project exists
- Read `00_OVERVIEW.md` for WHAT the system is
- Read `02_GLOSSARY.md` when terms feel fuzzy
- Read `03_PHILOSOPHY.md` to reconnect with theoretical grounding
- Read `04_OPEN_QUESTIONS.md` to see what needs investigation

---

## References & Source Material

### Your Prior Research
- **Theoretical foundation:** `../_theory/LifeTheUniverseAndEveryting/consciousness_complexity_epistemology.md`
- **Morphogenetic Fold:** `../_fold/00_SEED/original.md`
- **Steering research:** `../_fold/01_SMOOTHSPACE/_LITERATURE/_SOURCES/YouTube - Steering/`
- **EXP005:** Your steering vector experiments (inspired by Hugging Face video)

### Supporting Documentation (This Folder)
- **Conceptual map:** `00_OVERVIEW.md`
- **Technical depth:** `01_DETAILED_CONCEPTS.md`
- **Cross-domain terms:** `02_GLOSSARY.md`
- **Philosophy integration:** `03_PHILOSOPHY.md`
- **Challenges:** `04_OPEN_QUESTIONS.md`

### FabAcademy Context
- **Assignment requirements:** `../action-plans/Assignments/CRITICAL_Final_Project_Requirements_15percent.md`
- **Incomplete weeks:** `../action-plans/Assignments/PRIORITY_Incomplete_Weeks_Summary.md`

### External Research
- **Levin:** TAME framework, bioelectric cognition, morphogenetic fields
- **HuggingFace:** Steering vectors video (your EXP005 basis)
- **Anthropic:** Mechanistic interpretability, sparse autoencoders
- **Deleuze:** Assemblages, Body without Organs, difference and repetition

---

## The Through-Line (If You Only Remember One Thing)

You're testing whether three frameworks (Levin's basal cognition, Deleuze's assemblages, your consciousness-as-organization) converge on the same computational architecture by building a physical system that instantiates all three simultaneously.

**It's not an engineering project with philosophical decoration.**  
**It's a philosophical hypothesis that requires engineering to test.**

---

*"We're not building a fast chatbot. We're building proof that consciousness operates at multiple timescales using the same organizational principles."*
