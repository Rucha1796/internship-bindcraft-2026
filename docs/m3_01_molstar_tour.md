# 3.1 Tour of Mol*

**Time:** ~30 minutes hands-on

---

## What Is Mol*?

**Mol*** (pronounced "Mol Star") is a free, web-based protein structure visualization tool. It runs entirely in your browser — no installation needed.

- Available at [molstar.org/viewer](https://molstar.org/viewer)
- Also embedded directly in every RCSB PDB entry page
- Created by the PDB team and CZ Institute of Bioinformatics
- Used by researchers worldwide to inspect and analyze protein structures

In this internship, you'll use Mol* to:
1. Explore PD-L1's 3D structure
2. Identify where the hotspot residues are located
3. Visualize the binders you design with BindCraft

---

## Opening a Structure

### Option A: From RCSB PDB

1. Go to [rcsb.org](https://www.rcsb.org) and search `5C3T`
2. Click **"3D View"** in the top right corner of the structure card
3. The Mol* viewer opens inline

### Option B: Directly in Mol*

1. Go to [molstar.org/viewer](https://molstar.org/viewer)
2. Click **"Open"** in the top left
3. Type `5C3T` in the PDB ID box → Enter
4. The structure loads and centers in the viewer

---

## The Interface: What You're Looking At

```
┌──────────────────────────────────────────────────────────┐
│  Menu bar  [Open] [Download] [Settings] ...               │
├───────────────────────────┬──────────────────────────────┤
│                           │  Right Panel:                 │
│                           │  - Components list            │
│    3D Structure Viewer    │  - Sequence viewer            │
│     (main viewport)       │  - Measurements               │
│                           │  - State tree                 │
├───────────────────────────┴──────────────────────────────┤
│  Status bar                                               │
└──────────────────────────────────────────────────────────┘
```

**Mouse controls:**
- **Rotate:** Left-click and drag
- **Zoom:** Scroll wheel
- **Pan/Translate:** Right-click and drag (or Ctrl + left-click drag)
- **Center on atom:** Left-click on any atom to select it; double-click to center the view on it

---

## Color Schemes

By default, Mol* colors proteins by **chain** (each chain gets a different color). You can change this.

### Color by pLDDT (for AlphaFold structures)

AlphaFold structures from the PDB are automatically colored by pLDDT confidence:
- **Blue** (dark) = high confidence (pLDDT > 90)
- **Light blue** = confident (70–90)
- **Yellow** = low confidence (50–70)
- **Orange/Red** = very low confidence (< 50)

For PD-L1 (5C3T from X-ray crystallography), this color scheme doesn't apply — X-ray structures are colored differently (by B-factor, which measures atomic mobility).

### Color by Secondary Structure

To see helices vs. sheets clearly:
1. In the right panel, click on the structure component
2. Under "Color," select "Secondary Structure"
3. Helices = purple/pink, sheets = yellow, loops = gray

---

## Representations

Mol* offers several ways to draw the protein:

| Representation | What it shows | When to use |
|---------------|--------------|-------------|
| **Cartoon** | Ribbon with helices as coils, sheets as arrows | Overview of fold |
| **Spacefill (CPK)** | Every atom as a sphere (van der Waals radius) | Seeing surface area, packing |
| **Ball & Stick** | Atoms as balls connected by sticks | Individual residue detail |
| **Surface** | Molecular surface (solvent-accessible) | Visualizing the interface |

To change representation:
1. Right-click on the structure in the viewport
2. Click "Add/Edit Representation"
3. Choose from the list

---

## Selecting Residues

To inspect a specific residue (like hotspot residue 54):

1. In the right panel, click **"Sequence"**
2. Scroll to residue 54 (Gln 54) in chain A
3. Click it — it highlights in the 3D view
4. You can also click directly on atoms in the 3D viewport

When you select a residue, the status bar at the bottom shows:
- Chain, residue name, residue number
- Atom name and coordinates (if you clicked an atom)

---

## Activity: Explore PD-L1

Open `5C3T` in Mol* and complete the following:

1. **Rotate** the structure to see it from different angles. How would you describe the overall shape?

2. **Color by secondary structure.** Does PD-L1 look mostly helical or mostly sheet-like?

3. **Switch to Spacefill** representation. What does the surface look like? Is there an obvious groove or pocket?

4. **Find residue 54** using the sequence panel. Where is it located — on the surface, in the core, in a loop?

5. **Measure a distance:** In the Menu, click Tools → Measurement → Distance. Click two atoms across the structure to measure how wide PD-L1 is in Ångströms.

Write down what you observe — you'll use this knowledge when we identify the hotspot residues in Module 3.3.

---

> **Key takeaway:** Mol* is your main tool for seeing proteins in 3D. Learning to rotate, zoom, select residues, and switch representations will make all the downstream analysis much more intuitive. Spend time exploring before moving on.

**Next:** [3.2 Reading a Structure →](m3_02_reading_structure.md)
