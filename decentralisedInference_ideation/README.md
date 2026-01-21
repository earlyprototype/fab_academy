# Decentralised LLM Inference over LoRa: Project Documentation

**Status:** Conceptual foundations (pre-implementation)  
**Last Updated:** 2026-01-21

---

## What This Is

A FabAcademy final project exploring distributed artificial intelligence through a mesh network of sensor-equipped microcontrollers running sharded LLM inference over LoRa.

**The technical idea:** Distribute attention heads across physical nodes that communicate via low-bandwidth LoRa radio.

**The philosophical angle:** Test whether basal cognition principles (Levin), assemblage theory (Deleuze), and consciousness-as-organizational-patterns converge in a working system.

**The aesthetic:** "Slow AI" operating at geological timescales - making invisible computation visible through 15-minute token generation.

---

## Document Structure (How to Navigate)

This folder is organized for ADHD-friendly exploration with generous whitespace and clear hierarchies.

### Start Here

**[intialIdea.md](intialIdea.md)** - Evolution log tracking how the concept developed. READ THIS FIRST for context.

### Core Documentation

**[00_OVERVIEW.md](00_OVERVIEW.md)** - Big-picture conceptual map. What the system is and how pieces connect.

**[01_DETAILED_CONCEPTS.md](01_DETAILED_CONCEPTS.md)** - Technical deep-dives. Mathematics, timing analysis, memory budgets, architecture details.

**[02_GLOSSARY.md](02_GLOSSARY.md)** - Cross-domain terminology. Deleuze (as you've defined), Levin (biology), AI/ML, hardware. Includes your definitions, original sources, revision notes, and further reading links.

**[03_PHILOSOPHY.md](03_PHILOSOPHY.md)** - Integration of theoretical frameworks. How Levin + Deleuze + your consciousness work all map to this technical project.

**[04_OPEN_QUESTIONS.md](04_OPEN_QUESTIONS.md)** - Challenges, holes in thinking, research gaps, implementation risks. What we don't know.

---

## Quick Links by Interest

**Just want the technical overview?**  
→ [00_OVERVIEW.md](00_OVERVIEW.md)

**Need to understand a term?**  
→ [02_GLOSSARY.md](02_GLOSSARY.md)

**Curious about the philosophy?**  
→ [03_PHILOSOPHY.md](03_PHILOSOPHY.md)

**Want to know what's uncertain?**  
→ [04_OPEN_QUESTIONS.md](04_OPEN_QUESTIONS.md)

**Need to explain this to someone?**  
→ [intialIdea.md](intialIdea.md) §"Project Genesis" section

---

## The One-Paragraph Summary

A mesh network of 16 ESP32 nodes, each with environmental sensors and LoRa radio, collectively runs distributed LLM inference by sharding attention heads. Sensor readings transform into steering vectors that couple the model's linguistic output to physical environmental state. Token generation takes ~15 minutes, making distributed cognition visible. Tests whether basal cognition principles (collective intelligence without central control) apply to silicon substrates operating at LoRa timescales. For FabAcademy final project.

---

## Current Status & Next Steps

**Conceptual foundations:** Complete (this documentation)

**BREAKTHROUGH (2026-01-21):** Sensor→steering transformation solved. It's scalar multiplication using EXP005 morphogens, not a complex mapping problem. This changes the project from speculative to implementable.

**Immediate next actions:**
1. **Sensor validation (Week 1):** Test `sound_pressure × morphogen_turbulent` on laptop with microphone
2. **Measure correlation:** Does loud sound → turbulent tokens? (Need r > 0.7)
3. Memory budget validation (profile ESP32-S3 + Meshtastic)
4. 2-bit quantization testing (assess quality degradation)
5. FabAcademy proposal review (ensure fits requirements)

**See:** 
- [intialIdea.md](intialIdea.md) §"Idea 2" for breakthrough explanation
- [01_DETAILED_CONCEPTS.md](01_DETAILED_CONCEPTS.md) §"Sensor-to-Steering Transformation" for math
- [04_OPEN_QUESTIONS.md](04_OPEN_QUESTIONS.md) §"Question 4" for validation experiments

---

## For LLMs Assisting This Project

When the user returns to work on this:

1. **Check [intialIdea.md](intialIdea.md)** for evolution history - avoid rehashing resolved discussions
2. **Reference [04_OPEN_QUESTIONS.md](04_OPEN_QUESTIONS.md)** for known unknowns - don't present them as new insights
3. **Use [02_GLOSSARY.md](02_GLOSSARY.md)** definitions consistently - especially Deleuzian terms as the user has defined them
4. **Keep [00_OVERVIEW.md](00_OVERVIEW.md)** and [03_PHILOSOPHY.md](03_PHILOSOPHY.md) as anchors - these capture the conceptual framing

**ADHD accommodation:** Visual space, clear hierarchy, cross-references, concrete examples over abstractions.

---

## Related Files in Repository

**Theoretical foundation:**  
[../_theory/LifeTheUniverseAndEveryting/consciousness_complexity_epistemology.md](../_theory/LifeTheUniverseAndEveryting/consciousness_complexity_epistemology.md)

**FabAcademy context:**  
[../action-plans/Assignments/](../action-plans/Assignments/)

---

## Contact & Collaboration

If you're from:
- **Levin lab:** Would love to discuss basal cognition in silicon
- **Distributed systems research:** Novel head-sharding architecture
- **FabAcademy community:** Happy to share documentation approach
- **Philosophy of mind:** Testing assemblage theory empirically

---

*"We're not building a fast chatbot. We're building a mechanical brain that thinks at LoRa speeds."*
