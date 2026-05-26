# 6.4 Visualizing Your Designs

**Time:** ~25 minutes reading + notebook hands-on

---

## Two Ways to Visualize

You'll use two visualization tools in this internship:

1. **Mol*** — web browser, great for detailed exploration, screenshots, manual rotation
2. **py3Dmol in Colab** — programmatic visualization inside the notebook, fast, reproducible

Both show the same structures. Use Mol* for careful analysis; use py3Dmol for quick checks during the notebook workflow.

---

## Visualizing in Mol*

### Opening Your Design

1. In Mol*, click **Open** → **Open Files**
2. Upload `1_PDL1.pdb` from `Accepted/Ranked/`
3. The design loads — you'll see two chains: PD-L1 (chain A) and your binder (chain B)

### What to Look For

**1. The overall geometry — does the binder "sit" on PD-L1?**
- Color by chain. Chain A = target, chain B = binder
- The binder should be clearly docked against one face of PD-L1
- It should *not* be floating away or only touching at a single point

**2. Is the binder helical?**
- Switch to secondary structure coloring
- BindCraft's default designs are helical binders — you should see one or more helices in chain B
- A well-designed helical binder wraps against the flat beta-sandwich face of PD-L1

**3. Are the hotspots covered?**
- Highlight residues 54, 56, 111, 115, 123 in the sequence panel
- Are they at the contact zone between the two chains?
- The binder should be *over* these residues, not off to the side

**4. Does the interface look compact?**
- Switch to Spacefill representation
- Look for a buried interface with no large gaps
- If there are obvious voids between the chains, the shape complementarity score will be low

---

## Visualizing with py3Dmol in Notebook

### The Standard View (from Notebook 06)

```python
import py3Dmol

# Load PDB file
with open("PDL1/Accepted/Ranked/1_PDL1.pdb", "r") as f:
    pdb_string = f.read()

# Create viewer
view = py3Dmol.view(width=800, height=600)
view.addModel(pdb_string, "pdb")

# Color chain A (PD-L1) in steel blue
view.setStyle({"chain": "A"}, {"cartoon": {"color": "#3c5b6f"}})

# Color chain B (binder) in rose gold
view.setStyle({"chain": "B"}, {"cartoon": {"color": "#B76E79"}})

# Add surface for chain A (semi-transparent)
view.addSurface(
    py3Dmol.VDW, 
    {"opacity": 0.5, "color": "#3c5b6f"}, 
    {"chain": "A"}
)

view.zoomTo()
view.show()
```

**Color scheme:**
- Chain A (target): `#3c5b6f` — a muted steel blue
- Chain B (binder): `#B76E79` — rose gold

This color scheme makes it easy to distinguish the binder from the target.

### Highlighting Hotspot Residues

```python
# Add hotspot residues as spheres
hotspots = [54, 56, 111, 115, 123]
for res in hotspots:
    view.addStyle(
        {"chain": "A", "resi": str(res)},
        {"sphere": {"color": "yellow", "radius": 0.8}}
    )
view.show()
```

Hotspots show as yellow spheres. Does the rose binder cover them?

### Rotating Programmatically

```python
# Set a specific viewing angle
view.rotate(90, "y")  # rotate 90 degrees around Y axis
view.show()
```

---

## The Animation HTML

BindCraft generates HTML animation files showing the hallucination trajectory evolving. These are in `Accepted/Animation/`.

To view:
- Download the HTML file
- Open it in a web browser (double-click)
- Press Play to watch the binder "snap into" its designed geometry

These animations are excellent for understanding what the design process looks like — and great to include in your final presentation.

---

## Making Screenshots for Your Report

### From Mol*
1. Position the structure how you want it
2. Right-click the viewport → **Screenshot** (or use the Camera icon in the toolbar)
3. Set resolution to 1920×1080 or higher for reports
4. Click **Download**

### From py3Dmol in Colab
py3Dmol doesn't have a built-in screenshot button. Options:
1. Use your OS screenshot tool (Cmd+Shift+4 on Mac)
2. Right-click the viewer → "Save Image As" (works in some browsers)

---

## What Good vs. Bad Designs Look Like

**A good design:**
- Binder sits directly on top of PD-L1's beta-sandwich face
- Helical structure, well-folded appearance
- Hotspot residues are at the center of the contact zone
- Interface is tight (no large gaps in spacefill view)
- One or two helices making most contacts

**A borderline design:**
- Binder touches PD-L1 but off-center from hotspots
- Very small interface area
- Mostly loops (not helices) — may be less stable

**A bad design:**
- Binder appears to "hover" far from PD-L1
- Only 1–2 contacts
- The binder folds into itself, not against the target

> You'll see all three types when you look at the `Rejected/` folder contents. Comparing rejected vs. accepted designs builds intuition fast.

---

## Activity: Rank Your Top 5 by Eye

Using Mol*, open the top 10 designs from `Accepted/Ranked/`. For each:
1. Take a screenshot from a consistent viewing angle
2. Note (Y/N): Does the binder sit over the hotspots?
3. Note (Y/N): Does the interface look tight (spacefill view)?
4. Rate it 1–5 subjectively based on visual quality

Compare your visual ranking to the `Average_i_pTM` ranking in the CSV. Do they match?

This is an important exercise: quantitative metrics and visual inspection should agree. When they don't, investigate why — sometimes a high-scoring design has a subtle geometric issue that visual inspection catches.

---

> **Key takeaway:** Visualizing designs in Mol* and py3Dmol lets you check that the binder actually sits on the hotspot region, has a compact interface, and looks like a real binding event. Quantitative metrics and visual inspection should agree — when they don't, look more carefully.

**Next:** [Module 7: Analyzing Results →](m7_01_metrics.md)
