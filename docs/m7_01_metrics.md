# 7.1 The Metrics Explained

**Time:** ~30 minutes reading

---

## Why So Many Metrics?

The `final_design_stats.csv` from BindCraft contains over 150 columns. Most of them are individual model predictions (5 AF2 models per design) — the key ones you need are the **Average_** columns.

This page is your reference guide. Bookmark it.

---

## The Essential 10

These are the metrics that matter most for selecting designs to recommend for wet lab testing.

| Metric | What it measures | Range | Good threshold | Analogy |
|--------|-----------------|-------|----------------|---------|
| **Average_i_pTM** | Interface confidence (AF2) | 0–1 | > 0.55 | "How sure is AF2 that they're really touching?" |
| **Average_pLDDT** | Overall structural confidence | 0–1 | > 0.85 | "How confident is AF2 overall?" |
| **Average_Binder_pLDDT** | Binder-specific confidence | 0–1 | > 0.80 | "Does the binder fold well on its own?" |
| **Average_i_pAE** | Interface positioning error | Å | < 10 | "How precisely is the binder placed?" |
| **Average_Binder_RMSD** | Self-consistency: binder alone vs. in complex | Å | < 2.0 | "Does the binder fold the same with or without the target?" |
| **Average_ShapeComplementarity** | Geometric fit of binder to target surface | 0–1 | > 0.55 | "Do they fit like puzzle pieces?" |
| **Average_dG** | Predicted binding free energy | kcal/mol | < -10 | "Is binding energetically favorable?" |
| **Average_dG/dSASA** | Binding efficiency per buried surface area | — | < -0.015 | "Good binding relative to how much surface is buried?" |
| **Average_n_InterfaceHbonds** | Number of H-bonds at interface | count | higher = better | "Are there polar contacts holding them together?" |
| **Average_Relaxed_Clashes** | Atomic clashes after energy minimization | count | < 5 | "Do atoms overlap (bad) or fit cleanly?" |

---

## Deep Dives

### i_pTM — The Most Important Metric

i_pTM (interface predicted Template Modeling score) is a number between 0 and 1 that represents AlphaFold2's confidence that the interface it predicted is accurate.

- i_pTM = 1.0 → AF2 is completely confident the two proteins interact exactly as shown
- i_pTM = 0.5 → borderline; AF2 isn't sure
- i_pTM < 0.4 → AF2 doesn't believe they're really binding

In the pregenerated PDL1 dataset, accepted designs typically have i_pTM between 0.55 and 0.75.

> **Why not aim for i_pTM = 1.0?** Perfectly confident designs are rare and not required — 0.6+ is considered a strong signal. Insisting on > 0.8 would likely filter out many genuinely good designs.

---

### Shape Complementarity — The Puzzle Piece Metric

Shape complementarity (SC) measures how well the molecular surfaces of the binder and target fit together geometrically.

- SC = 1.0 → perfect geometric fit (like a key in a lock)
- SC = 0.5 → reasonable fit (most good protein-protein interfaces are 0.6–0.8)
- SC < 0.5 → poor fit; surfaces don't match well

The SC is calculated by PyRosetta after energy minimization.

> **Intuition:** Imagine pressing two lumps of clay together. If they perfectly mold to each other, SC is high. If there are big gaps or only a few points of contact, SC is low.

---

### Binder RMSD — The Self-Consistency Check Metric

After the 4-step BindCraft pipeline, each design has two AF2 predictions:
1. The binder **in complex** with PD-L1
2. The binder **alone** (no target)

Binder RMSD measures how different these two structures are. A low RMSD means the binder folds the same way whether or not the target is present — this is what we want.

- RMSD < 1Å → essentially identical; binder is independent and stable
- RMSD 1–2Å → acceptable; small differences
- RMSD > 2Å → the binder relies on the target to fold; likely won't work in solution

---

### dG — Predicted Binding Energy

dG (ΔG) is calculated by PyRosetta's interface scoring function. Negative = favorable binding; more negative = stronger binding.

> **Important caveat:** PyRosetta's dG is a physics-based estimate, not a measurement. It correlates with experimental affinity, but imperfectly. Use it as one signal among many, not as an absolute predictor.

Typical values for good designs: -10 to -30 kcal/mol (purely computational — not directly comparable to experimental Kd values).

---

## Metrics to Ignore (for Now)

These columns are in the CSV but are less important for first-round selection:

- Individual model predictions (1_pLDDT, 2_pLDDT, etc.) — use the Average_ versions
- PackStat — redundant with ShapeComplementarity for most purposes
- Interface helix/betasheet percentages — interesting but not a filter criterion
- Surface hydrophobicity — useful if optimizing for solubility, but not a primary filter

---

## The Traffic Light System

When selecting designs, mentally apply this traffic light:

```
For each design in final_design_stats.csv:

  i_pTM > 0.55       →  green
  pLDDT > 0.85       →  green
  Binder_pLDDT > 0.8 →  green
  i_pAE < 10         →  green
  Binder_RMSD < 2.0  →  green
  ShapeCompl > 0.55  →  green
  dG < -10           →  green
  Clashes < 5        →  green

All green → recommend for synthesis
Any red  → explain why you'd still include it (or exclude it)
```

---

> **Key takeaway:** i_pTM is the single most important metric. ShapeComplementarity and Binder_RMSD are the next most important. dG is a supporting signal. Always look at multiple metrics together — no single number tells the whole story.

**Next:** [7.2 Ranking & Filtering Designs →](m7_02_ranking.md)
