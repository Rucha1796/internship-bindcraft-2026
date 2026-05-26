# 4.2 Reading Confidence Scores

**Time:** ~30 minutes reading + hands-on

---

## Why Confidence Scores Matter

AlphaFold2 doesn't just give you a structure — it tells you *how confident* it is about each part of the structure. This is critical for protein design: we use these scores to filter out bad designs before spending money on experiments.

There are three main confidence metrics you'll encounter in this internship.

---

## pLDDT — Per-Residue Confidence

**pLDDT** (predicted Local Distance Difference Test) is a score from **0 to 100** assigned to every residue in the structure.

It answers the question: *"How confident is AF2 that this residue is placed correctly relative to its local neighbors?"*

| pLDDT range | Interpretation | Color in Mol*/AF2 viewer |
|-------------|---------------|--------------------------|
| **90–100** | Very high confidence | Dark blue |
| **70–90** | Confident | Light blue |
| **50–70** | Low confidence | Yellow |
| **< 50** | Very low confidence — likely disordered | Orange/Red |

> **What low pLDDT means:** It usually means the region is genuinely disordered (no fixed structure in solution), OR it means AF2 didn't have enough information to model it. Both are important signals.

**In BindCraft:**
- We require `Average_pLDDT > 0.85` (note: in BindCraft, pLDDT is on the 0–1 scale)
- Low pLDDT on the binder means AF2 doesn't think it folds well — reject it
- High pLDDT on the entire complex = confident prediction

---

## PAE — Predicted Aligned Error

**PAE** (Predicted Aligned Error) is the most important metric for assessing **protein-protein interfaces**.

It answers a different question: *"If I align the structure on residue i, how far off is the predicted position of residue j?"*

PAE is shown as a **2D matrix** — rows and columns represent residues, and the color at position (i, j) indicates the predicted error for their relative placement.

### Reading a PAE Matrix

```
Low PAE (dark blue) = confident relative placement
High PAE (light/yellow) = uncertain relative placement
```

For a two-chain complex (target chain A + binder chain B):

- **Top-left block (A×A):** Confidence in relative placement *within* the target — should be low (dark)
- **Bottom-right block (B×B):** Confidence in relative placement *within* the binder — should be low (dark)
- **Off-diagonal blocks (A×B and B×A):** Confidence in the *interface* — this is the critical one

> **Key insight:** If the off-diagonal blocks (A×B) have LOW PAE, AlphaFold2 is confident that the binder and target are interacting as predicted. This is the strongest signal that your design is genuinely binding.

**In BindCraft:**
- `Average_i_pAE` = average interface PAE (lower is better)
- Typical threshold: `Average_i_pAE < 10`

---

## pTM and i_pTM

**pTM** (predicted Template Modeling score) is a single number (0–1) summarizing the overall accuracy of the whole structure.

**i_pTM** (interface pTM) is the same but focused specifically on the interface between chains.

| Metric | What it measures | Range | Good threshold |
|--------|-----------------|-------|----------------|
| pTM | Overall predicted structural accuracy | 0–1 | > 0.7 |
| i_pTM | Interface-specific predicted accuracy | 0–1 | > 0.55 |

> **i_pTM is the single most important filter in BindCraft.** A high i_pTM means AF2 is confident that the binder and target are genuinely interacting the way the design predicts. It is the best proxy for "is this design actually binding?"

---

## How These Scores Work Together

Think of them as three questions:

| Question | Metric | If bad... |
|----------|--------|-----------|
| Does the binder fold? | pLDDT (binder region) | Reject — unstable binder |
| Are the two proteins confidently interacting? | i_pTM | Reject — no confident interface |
| Is the binder positioned exactly where we designed it? | i_pAE | Reject — uncertain placement |

A good design passes **all three**. In BindCraft's filtering pipeline, designs that fail any filter are moved to the `Rejected/` folder.

---

## Visualizing Confidence: What You'll See in ColabFold

When you run ColabFold on PD-L1 in the notebook (Notebook 02), you'll see:

1. **3D structure colored by pLDDT** — well-folded regions in blue, flexible regions in orange/red
2. **PAE plot** — a heatmap matrix showing inter-residue confidence
3. **pTM score** — printed as a number in the output

For PD-L1's extracellular domain: you should see mostly blue (pLDDT > 80) because it's a well-structured beta-sandwich domain. The flexible linker regions at the termini may show lower confidence.

---

## Activity: Interpret a PAE Matrix

Given the PAE matrix image from the ColabFold output (your mentor will print one):

1. Find the diagonal blocks — are they dark or light?
2. Find the off-diagonal blocks — are they dark or light?
3. What does this tell you about whether AF2 is confident in the interface?

Then check: what is the pTM score? Is it above 0.7?

---

> **Key takeaway:** pLDDT tells you how confident AF2 is per-residue. PAE tells you how confident it is about inter-residue relationships — especially the interface. i_pTM is the single best proxy for binding confidence in BindCraft.

**Next:** [4.3 Running ColabFold →](m4_03_colabfold.md)
