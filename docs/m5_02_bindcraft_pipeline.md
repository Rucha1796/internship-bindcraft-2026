# 5.2 The BindCraft Pipeline

**Time:** ~45 minutes reading + activity

---

## Overview

BindCraft (Pacesa, Nickel, Schellhaas et al., *Nature* 2025) is a four-step automated pipeline that designs protein binders using AlphaFold2 as a structure prediction and scoring engine.

The key idea: instead of using AF2 to *predict* a structure from a known sequence, BindCraft runs AF2 *backwards* — using gradient descent to find a sequence that makes AF2 produce a structure with the properties we want.

---

## The Four Steps

### Step 1 — AF2 Hallucination (ColabDesign)

**What happens:** The pipeline generates a completely novel protein sequence-structure pair from scratch, guided by a loss function.

**How:** Using **ColabDesign** (by Sergey Ovchinnikov), BindCraft runs AlphaFold2 in "design mode" — instead of predicting structure from a fixed sequence, it uses gradient descent to *optimize the sequence* to minimize a loss function.

The loss function rewards:
- High **interface pLDDT** — confident interface prediction
- High **interface pTM** — confident interface prediction
- Low **inter-chain pAE** — confident relative positioning of binder and target
- High **contact count** — enough contacts at the interface
- Appropriate **helicity** — controls whether to design helices, sheets, or mixed
- Appropriate **radius of gyration** — binder isn't too spread out

Think of it like tuning a radio: gradient descent keeps adjusting the frequency (the sequence) until the signal is as clear as possible (the loss is minimized).

**What gets rejected at this step:**
- Trajectories where the binder **clashes** with the target (overlapping atoms) — checked every few gradient steps
- Trajectories with pLDDT or pTM below threshold after optimization

> **Sculptor analogy:** Step 1 is like a sculptor who simultaneously discovers *what shape to make* and *where to put their hands* — finding a binding site and a binder shape at the same time.

---

### Step 2 — ProteinMPNN Sequence Redesign

**What happens:** ProteinMPNN redesigns the amino acid sequence of the binder scaffold (non-interface residues), keeping the binding interface fixed.

**Why:** ColabDesign (hallucination) is excellent at finding good binding sites and geometries, but the sequences it produces can be sticky, aggregation-prone, or otherwise problematic for the non-interface regions.

ProteinMPNN is a separate model trained specifically to design sequences that fold onto a given backbone. Applied to the non-interface scaffold, it improves:
- Solubility
- Stability
- Foldability of the binder in isolation

**Key detail:** The interface residues (those making direct contacts with the target at the hotspot positions) are **held fixed** from step 1. Only the non-interface scaffold is redesigned.

> **Materials engineer analogy:** The sculptor (step 1) chose the shape. Now a materials engineer (step 2) selects the best material — keeping the shape exactly as designed, but ensuring the structure is stable and won't fall apart.

---

### Step 3 — Multi-Metric Filtering

**What happens:** Every MPNN-redesigned sequence is predicted by AlphaFold2 (in complex with the target and in isolation) and scored on ~15 metrics. Designs that don't pass all filters go to the `Rejected/` folder.

**Key filters:**

| Metric | Threshold | Plain English |
|--------|-----------|---------------|
| Average_pLDDT | > 0.85 | AF2 is confident the complex is well-structured |
| Average_i_pTM | > 0.55 | AF2 is confident the interface is real |
| Average_i_pAE | < 10 | Interface positioning is accurate |
| Average_ShapeComplementarity | > 0.55 | Binder fits the target surface like a puzzle piece |
| Average_dG | < -10 | Predicted binding energy is favorable |
| Average_Relaxed_Clashes | < 5 | No atomic clashes after energy minimization |
| Average_Binder_pLDDT | > 0.80 | Binder folds well on its own |

> **Traffic light model:** Think green (pass), yellow (borderline), red (reject) for each metric. A design must be green on every metric to proceed. Even one red metric sends it to `Rejected/`.

---

### Step 4 — Self-Consistency Check

**What happens:** The final check ensures the designed sequence actually encodes the designed structure — not just in the context of the target, but by itself.

**How:**
1. Take the MPNN-redesigned sequence
2. Predict its structure with AF2 **in complex** with the target → check that it matches the hallucinated complex (RMSD < 2Å, usually 0.5–1.5Å)
3. Predict its structure with AF2 **alone** (binder only, no target) → check that it still folds into the same shape

**Why this matters:** A bad design might look like it's binding when the target is present (because the target "props it up"), but fall apart on its own. The binder-only self-consistency check catches this — a real binder should fold independently into the designed shape.

> **Self-consistency analogy:** Imagine designing a chair leg that only stays standing when you hold it. The self-consistency check says: "put it down and let go — does it still stand?"

---

## Why BindCraft Works: The Synergy

| Step | Tool | What it contributes |
|------|------|-------------------|
| 1 | ColabDesign (AF2 hallucination) | Finds good binding sites and interface geometries |
| 2 | ProteinMPNN | Improves the non-interface scaffold for solubility and stability |
| 3 | AF2 prediction + PyRosetta | Multi-metric quality control |
| 4 | AF2 self-consistency | Ensures the binder is genuinely self-folding |

Neither ColabDesign nor ProteinMPNN alone would be sufficient:
- ColabDesign alone = good binding sites, but sticky/unstable scaffolds
- ProteinMPNN alone = stable folds, but on a pre-existing backbone (not de novo)
- Together = both good binding AND stable scaffold

---

## What the Output Looks Like

For each trajectory (design attempt), BindCraft produces:

```
trajectory_stats.csv       ← metrics for every AF2 hallucination trajectory
mpnn_design_stats.csv      ← metrics for every MPNN-redesigned sequence
final_design_stats.csv     ← ranked list of designs that passed all filters
failure_csv.csv            ← which filters failed most often

Trajectory/                ← PDB files and animation HTMLs for hallucinated trajectories
  Relaxed/                 ← energy-minimized trajectories
  LowConfidence/           ← failed at step 1 (low pLDDT)
  Clashing/                ← failed at step 1 (clashes)
  Animation/               ← HTML animations of the hallucination trajectory
  Plots/                   ← pLDDT, pTM, i_pTM curves during optimization

MPNN/                      ← MPNN-redesigned PDB files
  Relaxed/                 ← energy-minimized MPNN designs

Accepted/                  ← designs that passed all filters
  Ranked/                  ← ranked by i_pTM: 1_design.pdb, 2_design.pdb, ...
  Animation/               ← animations of accepted trajectories
  Plots/                   ← metric plots for accepted designs

Rejected/                  ← designs that failed one or more filters
```

---

## Activity: Walk Through a Trajectory on Paper

Your mentor will give you a printed row from `trajectory_stats.csv`. For that design:

1. Look up each metric against the threshold table above
2. Mark it green (pass) or red (fail)
3. Would this design proceed to MPNN (step 2)? Or get rejected?
4. If rejected — which filter did it fail first?

---

> **Key takeaway:** BindCraft combines AF2 hallucination (finding the interface) with ProteinMPNN (improving the scaffold) and uses multi-metric filtering + self-consistency to ensure only high-quality designs proceed. It is automated, reproducible, and runs start-to-finish in hours on a GPU.

**Next:** [5.3 What Is Hallucination? →](m5_03_hallucination.md)
