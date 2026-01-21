# Fab Academy Final Project Overview

**Project Title:** Meshtastic Mesh Network System with Edge Computing
**Student:** Thom Conaty  
**Lab:** Creative Spark Enterprise FabLab - Dundalk  
**Cohort:** Fab Academy 2025  
**Project Page:** [https://fabacademy.org/2025/labs/creativespark/students/thom-conaty/project-concept.html](https://fabacademy.org/2025/labs/creativespark/students/thom-conaty/project-concept.html)

---

## Current Status: In Progress (January 2026)

**Overall Completion:** ~60-70% (estimate)  
**Status:** Returning to complete after 2025 semester challenges

---

## Project Concept

### What Is It?
A distributed mesh communication system using **Meshtastic** (LoRa-based mesh networking) with edge computing capabilities provided by an **NVIDIA Jetson Orin Nano Developer Kit**. The system enables off-grid communication across multiple nodes that can relay messages, share data, and potentially process information at the edge using AI/ML capabilities.

### Why This Project?
- Addresses need for resilient, off-grid communication systems
- Demonstrates integration of multiple Fab Academy competencies:
  - Electronics design and production
  - Embedded programming
  - Networking and communications
  - 3D CAD and enclosure design
  - Moulding and casting (for enclosures)
  - System architecture and integration
- Practical applications: emergency communications, remote monitoring, distributed sensor networks

### Key Innovation
Integration of edge computing (Jetson) with mesh networking (Meshtastic) to enable intelligent processing at the network level, rather than simple message relay. This could enable swarm coordination, data aggregation, local AI processing, or distributed decision-making.

---

## System Architecture (Current Understanding)

### Components:
1. **Multiple Meshtastic Nodes**
   - LoRa-based mesh communication
   - Low power microcontrollers (ESP32/nRF52 family likely)
   - GPS capability (TBD which nodes)
   - Battery powered
   - Custom designed enclosures

2. **NVIDIA Jetson Orin Nano (Hub/Gateway)**
   - Central processing unit for the swarm
   - Edge AI/ML capabilities
   - Coordinates mesh network
   - Processes data from nodes
   - Potential interfaces: network gateway, data logging, visualization

3. **Communication Architecture**
   - Node-to-node: Meshtastic (LoRa mesh)
   - Node-to-Jetson: [TBD - needs definition]
   - Jetson-to-outside world: [TBD - Ethernet/WiFi/cellular?]

### Current Block Diagram:
```
[Meshtastic Node 1] ←──→ [Meshtastic Node 2] ←──→ [Meshtastic Node 3]
         ↓                        ↓                          ↓
         └────────────────→ [Jetson Orin Nano] ←────────────┘
                                   ↓
                          [External Interface?]
```

*Note: This architecture needs refinement and documentation*

---

## What's Been Completed

### Documentation & Planning:
- [x] Initial project concept defined
- [x] Project page created on GitLab
- [x] Jetson documentation structure started (see `_Jetson/` folder)
- [ ] Complete system architecture documented
- [ ] Design Thinking methodology documentation

### Hardware & Electronics:
- [ ] Meshtastic node designs (status unknown)
- [ ] PCB designs (if custom boards)
- [ ] PCB production (X-Tool laser cutter available)
- [ ] Component sourcing and acquisition
- [ ] Jetson Orin Nano acquired (January 2026)
- [ ] Initial Jetson setup (in progress)

### Software & Firmware:
- [ ] Meshtastic firmware configuration
- [ ] Node-to-node communication tested
- [ ] Jetson integration software
- [ ] Communication protocol Jetson↔Meshtastic

### Mechanical & Enclosures:
- [ ] CAD designs for node enclosures
- [ ] CAD design for Jetson enclosure
- [ ] Enclosure fabrication method decided (concrete pour? 3D print?)
- [ ] Mould design (if concrete approach)
- [ ] Enclosures produced
- [ ] Assembly and fit testing

### Testing & Integration:
- [ ] Individual node functional testing
- [ ] Mesh network formation testing
- [ ] Swarm behavior testing (not fully tested as of context)
- [ ] Jetson integration testing
- [ ] Range testing
- [ ] Reliability/edge case testing
- [ ] System demonstration

---

## Major Outstanding Hurdles

### 1. System Architecture Definition
**Status:** Incomplete  
**Issue:** The overall architecture needs clear documentation:
- How many nodes exactly?
- What is Jetson's specific role?
- How do components communicate?
- What data flows where?
- What processing happens where?

**Action Needed:**
- Create detailed system architecture diagram
- Define data flow and communication protocols
- Document design decisions and trade-offs

### 2. Meshtastic Swarm Testing
**Status:** Not fully tested  
**Issue:** The mesh network hasn't been validated as a working swarm

**Action Needed:**
- Set up multiple nodes
- Test mesh formation
- Test message routing through multiple hops
- Test reliability and failure modes
- Document range and performance characteristics

### 3. Enclosure Design and Fabrication
**Status:** Not started/incomplete  
**Issue:** Nodes need physical enclosures (possibly concrete pour approach)

**Action Needed:**
- Design enclosures in CAD (requires CAD upskilling)
- Consider: weatherproofing, mounting, access, antenna placement
- If concrete: design moulds, test casting process
- If 3D print: prototype and test
- Fabricate final enclosures
- Assembly and fit testing

### 4. Jetson Integration
**Status:** Hardware acquired, integration planning needed  
**Issue:** How Jetson connects to and enhances the Meshtastic network needs definition

**Action Needed:**
- Complete Jetson setup and capability testing
- Define exactly what Jetson does in the system
- Develop integration software/firmware
- Test integrated system
- Document integration process

---

## Skills Development Needed

### Critical Path Skills:
1. **3D CAD** (applies across programme and specifically for enclosures)
   - Enclosure design
   - Mould design (if concrete approach)
   - Design for manufacturing

2. **Casting** (for concrete enclosures if proceeding with that approach)
   - Concrete mix ratios
   - Mould making
   - Release agents and techniques

3. **System Architecture** (for project coherence)
   - Defining clear system boundaries
   - Communication protocol design
   - Data flow documentation

---

## Available Resources

### Equipment:
- X-Tool laser cutter (rapid PCB production)
- 3D printers
- Standard FabLab equipment
- NVIDIA Jetson Orin Nano Developer Kit

### Constraints:
- Limited electronics prototyping equipment
- No reflow oven (hand soldering required)
- Need to purchase some prototyping components
- Lab equipment booking may be needed

### Knowledge Base:
- Your experience bringing electronics products to market (patchblocks)
- Your electronics teaching experience (maker.ie)
- Meshtastic open source community
- NVIDIA developer resources

---

## Design Thinking Integration

*To be woven throughout final project documentation*

### 1. Empathise
**Who needs this?**
- Emergency responders in areas without infrastructure
- Remote exploration teams
- Off-grid communities
- Disaster relief scenarios
- [To be expanded with research]

**What problem does it solve?**
- Communication when cellular/internet unavailable
- Resilient, decentralized networking
- [To be expanded]

### 2. Define
**Problem Statement:**
[To be clearly articulated]

**Project Goals:**
- Create working mesh network with [X] nodes
- Demonstrate edge computing integration
- Show real-world use case
- Document process comprehensively

### 3. Ideate
**Alternative Approaches Considered:**
[To be documented - what other architectures were considered?]

### 4. Prototype
**Iterations:**
[Document the build iterations, what worked, what didn't]

### 5. Test
**Validation:**
[Document testing process, results, refinements]

---

## Project Timeline (Target)

### Phase 1: Foundation & Planning (Week 1-2)
- [ ] Reconnect with Global Evaluator
- [ ] Define complete system architecture
- [ ] Audit current work status
- [ ] Create detailed project plan

### Phase 2: Skills Development (Weeks 2-4, parallel)
- [ ] CAD upskilling
- [ ] Jetson setup and testing
- [ ] Casting research and practice

### Phase 3: Core Development (Weeks 3-6)
- [ ] Meshtastic swarm setup and testing
- [ ] Jetson integration development
- [ ] Enclosure design

### Phase 4: Fabrication (Weeks 5-7)
- [ ] Enclosure production
- [ ] System assembly
- [ ] Integration testing

### Phase 5: Documentation & Completion (Weeks 7-8)
- [ ] Complete all documentation
- [ ] Final testing and demonstration
- [ ] Video production
- [ ] Submit for evaluation

---

## Success Criteria

### Minimum Viable Project (MVP):
- [ ] At least 3 functioning Meshtastic nodes
- [ ] Nodes can form mesh and relay messages
- [ ] Jetson integrated and performing defined function
- [ ] At least one node in completed enclosure
- [ ] System architecture clearly documented
- [ ] Full process documentation on GitLab
- [ ] Demonstration video showing system working
- [ ] Design Thinking methodology evident in documentation

### Stretch Goals (if time permits):
- [ ] All nodes in custom enclosures
- [ ] Concrete enclosures as originally envisioned
- [ ] Advanced AI/ML features on Jetson
- [ ] Extended range/reliability testing
- [ ] Multiple use case demonstrations

---

## Risk Assessment

### High Priority Risks:
1. **Time pressure** - Completing in compressed timeframe
   - Mitigation: Focus on MVP, have descoping plan ready

2. **System architecture complexity** - Integration may be more complex than anticipated
   - Mitigation: Start with simple integration, add features incrementally

3. **Casting/enclosure fabrication** - May not work as planned
   - Mitigation: Have 3D printing as backup approach

4. **Meshtastic reliability** - Network may not perform as expected
   - Mitigation: Early testing, join community for support, have backup communication method

5. **Jetson integration** - Technical challenges in connecting Jetson to Meshtastic
   - Mitigation: Research early, use community resources, simplify if needed

---

## Related Documentation

- [Context Document](./context.md) - Overall Fab Academy completion plan
- [Action Plans Folder](./action-plans/) - Detailed task breakdowns
- [Jetson Documentation](./_Jetson/) - Jetson Orin Nano setup and testing
- [Assignments Tracker](./action-plans/assignments-tracker.md) - Weekly assignment status
- [Final Project Breakdown](./action-plans/final-project-breakdown.md) - Detailed project planning

---

## Questions to Resolve

1. What is the exact hardware specification for each Meshtastic node?
2. How many nodes are planned? (3? 5? more?)
3. What specific role will the Jetson play? (Gateway? Coordinator? Data processor?)
4. What is the communication protocol between Jetson and Meshtastic nodes?
5. What is the target deployment scenario? (Indoor? Outdoor? Range requirements?)
6. What specific AI/ML functionality (if any) will run on Jetson?
7. Are concrete enclosures still the plan, or has this pivoted?
8. What are the power requirements and battery life targets?
9. Which specific assignment needs to be redone? (Per Global Evaluator feedback)

---

## Next Immediate Actions

1. **Reconnect with Global Evaluator** - Get clarity on requirements and the assignment that needs redoing
2. **System Architecture Workshop** - Sit down and clearly define the system
3. **Current Work Audit** - Document exactly what exists vs what's needed
4. **Jetson Setup** - Complete basic setup and capability testing
5. **Create MVP Definition** - Define minimum viable deliverables

---

*Last Updated: January 2026*  
*Status: Returning to complete - determined to graduate!*
