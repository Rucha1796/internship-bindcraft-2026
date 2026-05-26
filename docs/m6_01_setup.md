# 6.1 Setting Up: Target & Settings

**Time:** ~30 minutes reading + notebook setup

---

## Before You Run BindCraft

BindCraft needs two things to start:
1. A **target PDB file** — the 3D structure of the protein you're designing against
2. A **settings JSON file** — configuration specifying where to bind, how long the binder should be, and other parameters

This page walks you through preparing both for PD-L1.

---

## The Target PDB File

### Which PDB Structure to Use

For PD-L1, you'll use `PDL1.pdb` — a prepared version of PD-L1's extracellular domain that your mentor has already set up. This file:
- Contains only chain A (the extracellular domain)
- Has been cleaned (waters and ligands removed)
- Has numbering consistent with the hotspot residue numbers (54, 56, 111, 115, 123)

> **Why not use the raw 5C3T.pdb directly?** Raw PDB files sometimes contain multiple chains, water molecules, or non-standard residues that confuse BindCraft. The provided PDL1.pdb is pre-cleaned.

### Where the PDB File Goes

In the Colab notebook, the PDB file is uploaded to Google Drive:
```
/content/drive/MyDrive/bindcraft/PDL1.pdb
```

BindCraft references this path in the settings JSON.

---

## The Settings JSON

The settings file tells BindCraft everything about the design run. Here is the PD-L1 settings file you'll use:

```json
{
    "binder_name": "PDL1",
    "starting_pdb": "/content/bindcraft/example/PDL1.pdb",
    "chains": "A",
    "target_hotspot_residues": "54,56,111,115,123",
    "lengths": [65, 90],
    "number_of_final_designs": 10
}
```

### Parameter Explanations

**`binder_name`** — A label for this run. Output files will be prefixed with this name. Use something descriptive: `"PDL1"`, `"PDL1_v2"`, etc.

**`starting_pdb`** — Full path to the target PDB file. Must be accessible from inside Colab (drive must be mounted).

**`chains`** — Which chain(s) in the PDB file is the target. `"A"` means chain A is the receptor; BindCraft will design the binder to bind this chain.

**`target_hotspot_residues`** — The residue numbers you want the binder to contact. These should be on the chain specified above.
- Format: comma-separated residue numbers
- PD-L1 hotspots: `"54,56,111,115,123"`
- For your independent project, you'll need to determine these from the structure and literature

**`lengths`** — Min and max binder length in residues. `[65, 90]` means BindCraft will randomly pick a length between 65 and 90 for each trajectory. Longer binders can make more contacts but are harder to express in bacteria.

**`number_of_final_designs`** — How many accepted designs to collect before stopping. Set to 10 for your initial run. BindCraft will keep running trajectories until 10 designs pass all filters (or until you manually stop it).

---

## Advanced Settings (for Reference)

The notebook's Cell 4 has advanced settings with sensible defaults:

```
design_protocol:     "default"      → uses standard hallucination settings
prediction_protocol: "default"      → predicts 5 AF2 models per design
interface_protocol:  "default"      → default interface scoring
template_protocol:   "default"      → no template input (true de novo)
```

You generally don't need to change these for your first run. Your mentor will explain when and why to modify them.

---

## Choosing Hotspot Residues

For PD-L1, the hotspot residues are given to you (54, 56, 111, 115, 123). But in Module 9, you'll need to choose hotspots for your independent project. The process:

1. **Find the binding interface** in the literature or from a known complex structure
2. **Identify key contact residues** — which residues make direct contacts with the natural binding partner?
3. **Focus on the most central contacts** — 3–6 residues that are at the core of the interface
4. **Confirm on the structure** in Mol* — make sure they're surface-exposed and near each other

Choosing too many hotspots → constrains the design too much; binders may not find a good geometry
Choosing too few → the binder may not target the desired site

3–6 hotspot residues is typically optimal for helical binder design.

---

## Pre-Run Checklist

Before clicking "Run All" in the notebook:

- [ ] Google Drive is mounted
- [ ] PDL1.pdb is at the correct path
- [ ] Settings JSON has the correct PDB path
- [ ] `chains`: "A" (or whichever chain is the target)
- [ ] `target_hotspot_residues` matches residue numbers in your PDB
- [ ] `number_of_final_designs` is set (10 for initial run)
- [ ] You have a GPU runtime selected in Colab (Runtime → Change runtime type → T4 GPU)

---

> **Key takeaway:** BindCraft needs a cleaned target PDB and a settings JSON. The most important parameters are the starting_pdb path, chains, and target_hotspot_residues. For PD-L1 these are provided; for your independent project you'll determine the hotspots yourself. Always verify the GPU is active before starting.

**Next:** [6.2 Launching the Design Run →](m6_02_running.md)
