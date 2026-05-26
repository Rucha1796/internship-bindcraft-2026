# 7.3 Good vs. Bad Designs

**Time:** ~25 minutes reading + case study

---

## Learning to Distinguish Quality

In Module 7.1, you learned the thresholds. In Module 7.2, you learned how to rank and select. This module shows you *what good and bad designs actually look like* — both in the numbers and in the structures.

The best way to build intuition is to compare real examples.

---

## Case 1: The Clear Accept

```
Average_i_pTM:              0.72  ✅✅
Average_pLDDT:              0.91  ✅
Average_Binder_pLDDT:       0.87  ✅
Average_i_pAE:              7.3   ✅
Average_Binder_RMSD:        0.9   ✅✅
Average_ShapeComplementarity: 0.69 ✅✅
Average_dG:                 -18.4 ✅✅
Average_Relaxed_Clashes:    1     ✅
Average_n_InterfaceHbonds:  8
```

**Assessment:**
- i_pTM = 0.72 — AF2 is very confident this interface is real
- ShapeComp = 0.69 — excellent geometric fit
- Binder_RMSD = 0.9 Å — the binder folds identically with or without the target
- dG = -18.4 kcal/mol — strong predicted binding energy
- 8 hydrogen bonds at interface — a rich polar contact network

**Recommendation:** High-priority synthesis candidate. This design has multiple excellent metrics.

**What it looks like in Mol*:**
- Tight helical bundle sitting directly over the hotspot residues
- No gaps visible in spacefill representation
- Smooth, complementary surfaces

---

## Case 2: The Clear Reject (Clashing)

```
Average_i_pTM:              0.62  ✅
Average_pLDDT:              0.88  ✅
Average_Binder_pLDDT:       0.83  ✅
Average_i_pAE:              8.1   ✅
Average_Binder_RMSD:        1.4   ✅
Average_ShapeComplementarity: 0.47 ❌ (below 0.55)
Average_dG:                 -8.2  ❌ (not below -10)
Average_Relaxed_Clashes:    12    ❌ (above 5)
```

**Assessment:**
- i_pTM looks acceptable — but look at shape complementarity and clashes
- ShapeComp = 0.47 — the surfaces don't fit well; there are gaps
- 12 clashes — atoms are overlapping even after energy minimization; the geometry is physically impossible
- dG = -8.2 — not energetically favorable enough

**What happened:** The hallucination found an interface that AF2 is moderately confident about, but the geometry didn't work out — the binder is forced into an awkward position with steric conflicts.

**Recommendation:** Reject. The geometric problems are fundamental, not fixable by sequence optimization.

---

## Case 3: The Borderline Design

```
Average_i_pTM:              0.58  ✅ (barely)
Average_pLDDT:              0.86  ✅
Average_Binder_pLDDT:       0.81  ✅
Average_i_pAE:              9.4   ✅ (barely)
Average_Binder_RMSD:        1.8   ✅ (barely)
Average_ShapeComplementarity: 0.61 ✅
Average_dG:                 -11.3 ✅
Average_Relaxed_Clashes:    3     ✅
```

**Assessment:**
- Passes all filters — technically accepted
- But many metrics are near the threshold (not comfortably above)
- i_pTM = 0.58 — AF2 is only moderately confident
- Binder_RMSD = 1.8 Å — the binder slightly changes shape without the target

**Recommendation:** Deprioritize in the first round. If you have fewer than 5 good designs, include it with a note about why it's borderline. If you have 10+ good designs, leave it out.

**Important:** Always document why you're including or excluding borderline designs in your report. Scientific reasoning matters more than the number.

---

## Case 4: The Self-Consistency Failure

```
Average_i_pTM:              0.68  ✅✅
Average_pLDDT:              0.90  ✅
Average_Binder_pLDDT:       0.72  ❌ (below 0.80)
Average_Binder_RMSD:        2.8   ❌ (above 2.0)
Average_ShapeComplementarity: 0.65 ✅✅
Average_dG:                 -16.2 ✅✅
```

**Assessment:**
- Paradox: i_pTM is excellent and dG is very negative — apparently a great binder
- But: Binder_pLDDT = 0.72 (binder alone isn't confident) and Binder_RMSD = 2.8 Å (fold changes significantly without target)
- The binder needs the target to fold into the designed conformation

**What this means:** In solution, the binder won't fold into the binding shape. It will exist as a partially disordered protein. When it encounters PD-L1, it might partially fold against the surface, but the binding will be weak and inconsistent.

**What it looks like:** The binder-alone prediction shows a different shape than the complex prediction — helices are rearranged or partially unfolded.

**Recommendation:** Reject. This is exactly the situation the Binder_RMSD filter is designed to catch. The high i_pTM is misleading here — the target is "helping" the binder fold, which won't happen in experimental conditions.

---

## Patterns to Watch For

| Pattern | What it means | What to do |
|---------|--------------|-----------|
| High i_pTM but low ShapeComp | AF2 confident but surfaces don't fit | Usually reject; geometry is wrong |
| High ShapeComp but low i_pTM | Good geometric fit but AF2 uncertain | May be worth noting but deprioritize |
| High i_pTM but high Binder_RMSD | Interface looks good but binder isn't self-folding | Reject; self-consistency failure |
| Low dG but high i_pTM | PyRosetta disagrees with AF2 | Trust i_pTM more; but investigate |
| Many H-bonds | Rich polar contact network | Good signal; prioritize |
| Very few H-bonds (< 3) | Mostly hydrophobic interface | May still work, but risky |

---

## The 2D Comparison Plot

A useful exercise in Notebook 07: plot the metrics of *accepted* vs. *rejected* designs side by side.

```python
import matplotlib.pyplot as plt

accepted = pd.read_csv("PDL1/final_design_stats.csv")
rejected = pd.read_csv("PDL1/failure_csv.csv")

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

# i_pTM distribution
ax1.hist(accepted["Average_i_pTM"], bins=15, alpha=0.6, label="Accepted", color="green")
ax1.hist(rejected["Average_i_pTM"], bins=15, alpha=0.6, label="Rejected", color="red")
ax1.axvline(0.55, color='black', linestyle='--', label='threshold')
ax1.set_title("i_pTM: Accepted vs. Rejected")
ax1.legend()

# Binder RMSD
ax2.hist(accepted["Average_Binder_RMSD"], bins=15, alpha=0.6, label="Accepted", color="green")
ax2.hist(rejected["Average_Binder_RMSD"], bins=15, alpha=0.6, label="Rejected", color="red")
ax2.axvline(2.0, color='black', linestyle='--', label='threshold')
ax2.set_title("Binder RMSD: Accepted vs. Rejected")
ax2.legend()

plt.show()
```

This plot makes the filter thresholds tangible — you can see that accepted and rejected designs form distinct clusters.

---

> **Key takeaway:** Good designs have high i_pTM, high ShapeComplementarity, and low Binder_RMSD — and these should all agree. When metrics contradict each other (high i_pTM but high Binder_RMSD), investigate the structure. The self-consistency check is a critical safeguard against "cheating" designs that only fold in the context of the target.

**Next:** [7.4 PDL1 Case Study →](m7_04_case_study.md)
