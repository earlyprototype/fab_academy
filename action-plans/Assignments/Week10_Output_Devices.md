# Week 10: Output Devices - Status Analysis

**Current Evaluator Score:** TBC (Need to verify with Oscar)  
**Actual Documentation Status:** 100% COMPLETE  
**Priority:** VERIFY - May not need any work

---

## Quick Status Check

✅ **Documentation EXISTS and appears complete**  
✅ **All assignment checkboxes are checked**  
✅ **Group assignment is linked**  
⚠️ **Need to confirm evaluator's actual score**

---

## What's Actually Documented (from week10-content.html)

### ✅ Complete Process Documentation

1. **Step 1: Schematic Design** (lines 186-213)
   - Mapped connections between Pico W and Adafruit TFT display
   - Complete pin mapping for SPI communication:
     - VIN → 5V/VBUS or 3V3
     - GND → GND
     - CLK → GP18 (SPI0 SCK)
     - MOSI → GP19 (SPI0 TX)
     - CS → GP17
     - D/C → GP16
     - RST → GP15
     - MISO → GP20 (SPI0 RX)
   - Schematic created in Fusion 360 Electronics

2. **Step 2: Design Adaptation** (lines 219-222)
   - Strategy for single-sided PCB routing
   - Breakout headers to overcome routing limitations
   - Jumper wire connections documented

3. **Step 3: PCB Layout** (lines 224-227)
   - Pico W and display connector footprints
   - Header footprints for power and SPI signals
   - Power trace routing (0.8mm width)
   - Decoupling capacitor placement
   - Mounting holes
   - Surface lettering for jumper positions

4. **Step 4: Export for Manufacturing** (lines 229-232)
   - PNG export suitable for MODS
   - Links to Week 6 process documentation

5. **Step 5: Milling Toolpath** (lines 235-237)
   - MODS platform toolpath generation
   - Links to Week 8 detailed instructions

6. **Step 6: Milling** (lines 239-242)
   - Roland SRM-20 fabrication
   - Process photos included

7. **Step 7: Assembly and Wiring** (lines 245-247)
   - Soldering headers for Pico W
   - Display connector headers
   - Decoupling capacitor
   - Button wiring (GP22 to GND)

8. **Step 8: Software Setup** (lines 251-276)
   - CircuitPython firmware installation
   - Complete library installation guide:
     - adafruit_ili9341.mpy
     - fourwire.mpy
     - adafruit_display_shapes.mpy
     - adafruit_display_text folder
     - adafruit_bus_device folder
     - adafruit_bitmap_font folder
   - Step-by-step library download and installation

9. **Step 9: Testing and Verification** (lines 279-287)
   - Test scripts loaded
   - REPL monitoring
   - **Video demonstration** of blinking eye output

### ✅ Assignment Completion

From the Assignments tab (lines 293-311):

**Group Assignment:**
- ✅ Measure the power consumption of an output device
- ✅ Document your work on the group work page and reflect on your individual page what you learned
- ✅ **Group page linked:** https://fabacademy.org/2025/labs/creativespark/assignments/8.OutputDevices/

**Individual Assignment:**
- ✅ Add an output device to a microcontroller board you've designed and program it to do something

### ✅ Project Files

From Project Files section (lines 314-328):
- Schematic and PCB design images included
- **CircuitPython code files:**
  - `test.py` - Initial display testing script
  - `blinking_eye.py` - Interactive eye animation script
- PCB milling PNG shown

### ✅ Reflection Section (lines 330-333)

Complete reflection covering:
- Learning experience with TFT display integration
- Single-sided PCB limitations
- Workaround with breakout headers
- CircuitPython library setup
- Interactive animation success
- Importance of documentation
- AI-assisted coding workflow

---

## Evaluator Checklist Verification

Need to confirm with Oscar which items he marked. The standard checklist should include:

- [ ] Linked to the group assignment page → **✅ DONE (line 302)**
- [ ] Documented how you determined power consumption → **⚠️ CHECK: May need explicit power calc section**
- [ ] Documented what you learned from interfacing output device(s) → **✅ DONE (reflection)**
- [ ] Documented your design and fabrication process → **✅ DONE (complete)**
- [ ] Explained the programming process(es) you used → **✅ DONE (CircuitPython setup)**
- [ ] Explained any problems you encountered and how you fixed them → **✅ DONE (routing workaround)**
- [ ] Included original design files → **✅ DONE (images + code links)**
- [ ] Included a 'hero shot' of your output device → **✅ DONE (main image)**

---

## Possible Missing Item (if evaluator marked incomplete)

### Power Consumption Measurement

**What might be missing:**
- Explicit power consumption calculations/measurements
- Voltage and current readings
- Power supply specifications

**Quick fix if needed (1-2 hours):**

1. **Measure Current Draw:**
   ```
   Test Setup:
   - Bench power supply with ammeter
   - Pico W + TFT display
   - Measure at different states:
     * Idle (no display)
     * Display active (static image)
     * Display active (animation)
   ```

2. **Document Measurements:**
   ```
   Power Consumption Results:
   - Pico W alone: ~40mA @ 5V = 0.2W
   - TFT backlight: ~80mA @ 5V = 0.4W
   - Total system: ~120mA @ 5V = 0.6W
   ```

3. **Add to Documentation:**
   - Create subsection "Power Consumption Analysis"
   - Include measurement setup photo
   - Table of results
   - Comparison with datasheet values

---

## Action Plan

### Step 1: Verify Status (5 minutes)
- [ ] Ask Oscar: "What is Week 10's current score?"
- [ ] If 100%: **No action needed**
- [ ] If incomplete: Get specific feedback on missing items

### Step 2: If Power Measurement Missing (1-2 hours)
- [ ] Set up measurement rig with bench supply
- [ ] Measure current at different display states
- [ ] Document setup with photos
- [ ] Create "Power Consumption Analysis" section
- [ ] Update documentation

### Step 3: Verification (5 minutes)
- [ ] Show updated documentation to Oscar
- [ ] Request re-evaluation to 100%

---

## Evidence Summary

**What You CAN Show:**
1. Complete process documentation (9 steps)
2. Working video demonstration
3. Schematic and PCB design
4. Source code files
5. Group assignment link
6. Comprehensive reflection
7. All checkboxes marked complete

**What MIGHT Need Adding:**
- Explicit power consumption measurements (if evaluator requires it)

---

## Time Estimate

**If already complete:** 0 hours (just verify)  
**If power measurement needed:** 1-2 hours  
**Total risk:** LOW - appears complete

---

## Notes

- This week appears to be one of your most thoroughly documented
- Video demonstration is excellent evidence
- Single-sided PCB workaround shows good problem-solving
- CircuitPython library guide is very detailed
- **Likely just needs evaluator verification**

---

## Success Criteria

✅ Week marked as 100% complete  
✅ All checklist items verified as checked  
✅ No additional documentation required

**Confidence Level:** 95% - This looks complete, just needs confirmation

---

*Last Updated: 19 January 2026*
