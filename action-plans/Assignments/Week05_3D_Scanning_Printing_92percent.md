# Week 5: 3D Scanning and Printing

**Week Summary:**  
Successfully completed SLS printing on Formlabs Fuse 1, scanned an ear using Artec LEO, and designed a lattice structure chair brace demonstrating additive manufacturing capabilities. Strong documentation with detailed process steps.

**Assignment Page:** [Week 5 on GitLab](https://fabacademy.org/2025/labs/creativespark/students/thom-conaty/week5.html)  
**Status:** ● 92% Complete - **NEARLY THERE!**  
**Date:** February 19 - February 26, 2025

---

## What You've Completed ✅

### Excellent Documentation:
- ✅ Linked to the group assignment page
- ✅ Explained what you learned from testing the 3D printers (SLS workflow documented extensively)
- ✅ **Documented how you scanned an object** (Artec LEO ear scanning with full post-processing)
- ✅ **Included your original design files for 3D printing**

### Strong Technical Content:
- ✅ SLS printing process (Formlabs Fuse 1)
- ✅ Artec LEO scanning workflow
- ✅ Artec Studio post-processing (Registration, Fusion, Hole Fill, Texture)
- ✅ Lattice structure design in Fusion 360
- ✅ Gyroid lattice with gradient solidity
- ✅ Volumetric design extension usage
- ✅ Offset function to preserve drill points
- ✅ Mesh conversion and export
- ✅ Slicing in Ultimaker Cura

---

## What's Missing (8%) ⚠️

### According to Evaluator:

#### 1. ❌ "Documented how you designed and 3D printed your object and explained why it could not be easily made subtractively"

**MY ASSESSMENT: THIS IS ACTUALLY COMPLETE** ✅

**Evidence:**
You wrote (lines 383-387):
> "I wanted to explore the unique capabilities of additive manufacturing by designing a structure that cannot be made using traditional subtractive methods. I chose to create a brace with an **internal lattice structure**, taking advantage of 3D printing's ability to produce complex, lightweight geometries with **variable density**."

And in reflection (lines 533-536):
> "The ability to create internal lattice structures with variable density is something that **simply isn't possible with subtractive methods**."

**HOWEVER** - To make it crystal clear for evaluator:

**Action Required:**
- [ ] Add a dedicated subsection titled "Why This Design Requires Additive Manufacturing"
- [ ] Include bullet points explicitly stating:
  - Internal geometry inaccessible to cutting tools
  - Variable density gradient impossible with traditional machining
  - Complex organic gyroid structure
  - No assembly required (monolithic structure)

**Time Estimate:** 30 minutes  
**Location:** Add after line 386 in week5-content.html

---

#### 2. ❌ "Included your hero shots"

**MY ASSESSMENT: YOU HAVE ONE, BUT NEED MORE** ⚠️

**Current Status:**
You have a "Hero Shot" section (lines 522-532) with one image: `images/weekly/week5/hero.jpg`

**Why Evaluator May Want More:**
- Single angle only
- Could benefit from multiple perspectives
- No scale reference
- Could show internal structure better

**Action Required:**
- [ ] Take 2-3 additional high-quality photos:
  - **Front view** with good lighting
  - **Angle view** showing 3D form
  - **Detail shot** showing lattice structure internal geometry
  - **Scale reference** (with ruler or hand)
  - **In-context shot** (if possible, shown as chair brace)
- [ ] Create a small gallery section
- [ ] Add captions describing what each shot shows

**Time Estimate:** 1-2 hours (photo session + editing + upload)  
**Location:** Expand the Hero Shot section (lines 522-532)

---

## Specific Actions to Reach 100%

### Action 1: Clarify Subtractive Impossibility

**Add this section** after your introduction to the lattice structure:

```html
<h4>Why This Design Requires Additive Manufacturing</h4>
<p><strong>This structure fundamentally cannot be produced using subtractive manufacturing methods for the following reasons:</strong></p>
<ul>
    <li><strong>Internal Enclosed Geometry:</strong> The gyroid lattice exists entirely within the solid outer shell. Subtractive tools (mills, lathes, routers) cannot access internal voids without destroying the outer structure.</li>
    <li><strong>Variable Density Gradient:</strong> The lattice density varies smoothly from one end to the other using the "Gradient Along Path" feature. This requires precise, continuous variation of material density that cannot be achieved by removing material.</li>
    <li><strong>Complex Organic Structure:</strong> The gyroid cell shape features smooth, flowing curves in three dimensions. These forms have no straight tool paths and would require impossible multi-axis repositioning in subtractive processes.</li>
    <li><strong>Monolithic Integration:</strong> The lattice is seamlessly integrated with the outer shell as a single piece. Creating this with subtractive methods would require assembly of multiple parts, negating the strength and efficiency benefits.</li>
    <li><strong>Undercuts and Re-entrant Features:</strong> The lattice structure contains features that face multiple directions, creating undercuts impossible to machine without collisions.</li>
</ul>
<p>In contrast, additive manufacturing builds this structure layer by layer from the inside out, with each layer supporting the next, making even the most complex internal geometries achievable.</p>
```

---

### Action 2: Enhanced Hero Shots

**Photo Session Checklist:**

#### Setup:
- [ ] Clean the printed piece
- [ ] Set up good lighting (natural light or softbox)
- [ ] Use plain background (white or grey)
- [ ] Have ruler/scale reference
- [ ] Charge camera/phone

#### Shots Needed:
1. **Main Hero Shot** (Front, slightly angled)
   - Center the piece
   - Show overall form
   - Good lighting, no harsh shadows
   - Sharp focus

2. **Detail Shot** (Close-up of lattice)
   - Use macro mode
   - Show internal structure clearly
   - Demonstrate the variable density
   - Sharp focus on lattice details

3. **Scale Reference Shot**
   - Include ruler or standard object (coin, etc.)
   - Show actual size
   - Clear scale marks visible

4. **Contextual Shot** (Optional but impressive)
   - Show it as a chair brace (even if just mock-up)
   - Demonstrate the intended use
   - Helps evaluator understand purpose

#### Update HTML:

```html
<!-- Hero Shot Section -->
<header class="major">
    <h3>Hero Shots Gallery</h3>
</header>
<p>Final 3D printed lattice structure for chair brace, showcasing the complex internal geometry only possible with additive manufacturing.</p>

<div class="row">
    <div class="col-6">
        <span class="image fit">
            <img src="images/weekly/week5/hero_main.jpg" alt="Main view of lattice structure" />
        </span>
        <p class="image-caption">Main view showing overall form and outer shell integration</p>
    </div>
    <div class="col-6">
        <span class="image fit">
            <img src="images/weekly/week5/hero_detail.jpg" alt="Detail of internal lattice" />
        </span>
        <p class="image-caption">Close-up of gyroid lattice showing variable density gradient</p>
    </div>
</div>

<div class="row">
    <div class="col-6">
        <span class="image fit">
            <img src="images/weekly/week5/hero_scale.jpg" alt="Scale reference" />
        </span>
        <p class="image-caption">Scale reference showing actual dimensions</p>
    </div>
    <div class="col-6">
        <span class="image fit">
            <img src="images/weekly/week5/hero_context.jpg" alt="In-context usage" />
        </span>
        <p class="image-caption">Demonstration as chair brace component</p>
    </div>
</div>
```

---

## Files Checklist

### Already Uploaded:
- ✅ 3D Scan (STL): `ear_right_stl.zip`
- ✅ Process images (SLS, scanning, design)

### Need to Upload:
- [ ] Enhanced hero shots (4 images)
- [ ] Lattice structure STL file (if not already included)
- [ ] Fusion 360 source file for lattice (.f3d or .f3z)

---

## Testing Your Changes

Before requesting re-evaluation:
1. [ ] View your page in a browser
2. [ ] Verify all new images load correctly
3. [ ] Check that the "why additive?" section is prominent
4. [ ] Confirm hero shots display in gallery format
5. [ ] Test on mobile view
6. [ ] Git commit and push
7. [ ] Verify on GitLab Pages URL

---

## Communication with Evaluator

**Email Template:**

> Subject: Week 5 Re-evaluation Request - 3D Scanning and Printing
> 
> Hi Oscar,
> 
> I've reviewed my Week 5 documentation and believe there may have been an oversight in the evaluation. Specifically:
> 
> 1. **Subtractive impossibility explanation:** I have documented why the lattice structure cannot be made subtractively (lines 383-387 and 533-536 of my page). However, I've now added a dedicated section with explicit bullet points to make this even clearer.
> 
> 2. **Hero shots:** I've expanded from one hero shot to a gallery of four professional shots showing different angles, details, scale, and context.
> 
> Could you please re-review this week? I believe it should now clearly meet 100% completion criteria.
> 
> Page: https://fabacademy.org/2025/labs/creativespark/students/thom-conaty/week5.html
> 
> Thank you,
> Thom

---

## Time Estimate to Complete

- **Add "Why Additive?" section:** 30 minutes
- **Photo session:** 45 minutes
- **Edit and upload photos:** 30 minutes
- **Update HTML:** 30 minutes
- **Testing and push:** 15 minutes

**TOTAL: ~2.5 hours to achieve 100%**

---

*Status: Ready for immediate action - this is a quick win!*
