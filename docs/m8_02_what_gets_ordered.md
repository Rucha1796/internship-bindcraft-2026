# 8.2 What Gets Ordered & Why

**Time:** ~20 minutes reading

---

## The Selection Decision

After your analysis in Module 7.4, you have a ranked list of top designs. But "top 5 by i_pTM" isn't always the final answer. This page explains the additional considerations that go into deciding exactly which designs get ordered for synthesis.

---

## Why Not Order All Designs?

Each design ordered for gene synthesis costs:
- **$150–300** for the gene (DNA synthesis)
- **3–5 days** of researcher time for cloning and transformation
- **1–2 weeks** for expression and purification
- **2–3 days** for binding assays

For 5 designs in parallel: roughly $2000+ in reagents and 1–2 months of co-mentor time.

Budget and capacity are real constraints. You're choosing 5 candidates from potentially 50+ designs — the decision matters.

---

## The Criteria Beyond Metrics

### 1. Structural Diversity

If your top 5 by i_pTM all have very similar sequences and structures, you're essentially testing the same design 5 times. If that binding mode doesn't work, you've wasted all 5 slots.

**Better strategy:** Choose designs that cover different binding geometries — different contact angles, different sets of contacts at the hotspot residues. This maximizes the information gained from each round.

> Example: If designs 1–3 all have the helix running north-south across the hotspot, and design 4 has it running east-west with different contacts, design 4 is more valuable even if its i_pTM is slightly lower.

### 2. Expression Likelihood

Some sequences are harder to express than others:
- **Long stretches of hydrophobic residues** → aggregation in *E. coli*
- **Very low net charge** → poor solubility
- **Cysteines** → disulfide bonds can cause problems in *E. coli* cytoplasm

Your co-mentor may flag designs with potential expression problems and suggest dropping them.

### 3. Sequence Features to Avoid

```python
# Quick expression risk check
def check_sequence(seq):
    warnings = []
    
    # Check for cysteines
    cys_count = seq.count("C")
    if cys_count > 0:
        warnings.append(f"Contains {cys_count} Cys (disulfide risk)")
    
    # Check for long hydrophobic stretches
    hydrophobic = set("VILMFYW")
    for i in range(len(seq) - 6):
        if all(aa in hydrophobic for aa in seq[i:i+7]):
            warnings.append(f"Long hydrophobic stretch at position {i}")
    
    # Check net charge at pH 7
    pos = seq.count("K") + seq.count("R")
    neg = seq.count("D") + seq.count("E")
    net_charge = pos - neg
    if abs(net_charge) < 2:
        warnings.append(f"Low net charge ({net_charge}) — solubility risk")
    
    return warnings
```

### 4. The Control Design

Always include at least one design with a specific feature as a positive control:
- **Highest absolute i_pTM** (regardless of other metrics) — confirms AF2's confidence is predictive
- **Most similar to the reference binder (8AOK)** — if it works, validates your pipeline

If this control doesn't bind experimentally, it tells you something important about whether i_pTM predicts real binding for this target.

---

## The "Design Report" Format

Your mentor will ask you to justify each design selection in writing. A good justification template:

---

**Design [Name]** — Recommended for synthesis

**Key metrics:**
- i_pTM: 0.69 (well above threshold of 0.55)
- ShapeComplementarity: 0.67 (good geometric fit)
- Binder RMSD: 1.1 Å (excellent self-consistency)
- dG: -16.2 kcal/mol (strong predicted binding)

**Structural rationale:**
The binder makes a three-helix bundle contact with PD-L1, covering hotspot residues 54, 56, and 111 directly. Visual inspection in Mol* confirms tight interface packing with no obvious gaps. The binding geometry is distinct from designs #2 and #3, providing structural diversity in the selection.

**Potential risks:**
Contains one cysteine (Cys47). Expression in the E. coli periplasm (oxidizing environment) rather than cytoplasm may be considered to allow correct disulfide formation.

---

This level of written justification:
1. Shows you understand the metrics
2. Shows you looked at the structure, not just the numbers
3. Anticipates potential problems
4. Gives the wet lab team information they need

---

## A Note on What "Success" Means

You're recommending designs before they've been tested. Not all 5 will bind. In fact, hit rates from first-round computational design typically range from:
- **10–30%** for de novo designed miniproteins (this is the current state of the field)
- **50–70%** for antibodies selected from large experimental libraries

If 1–2 out of your 5 designs bind PD-L1 in an SPR experiment, that's a genuinely good outcome. If even 1 binds with Kd < 100 nM, that's an exciting result.

The goal of this first round isn't to find the perfect drug — it's to confirm that the computational approach works and identify a starting point for further optimization (DMTA cycle continues).

---

> **Key takeaway:** Deciding what gets ordered requires metrics, structural analysis, expression considerations, and strategic thinking about diversity. Your written justification for each selection is as important as the selection itself. Even if only 1–2 designs bind, that's a meaningful scientific result.

**Next:** [8.3 How We Test Binders →](m8_03_testing.md)
