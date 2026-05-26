# 9.5 Analyzing Your Results

**Time:** ~2–3 hours hands-on (spread across Week 6)

---

## Independent Analysis

This is your chance to apply everything you've learned independently. The analysis workflow mirrors Module 7 (metrics, ranking, selection), but now:
- You're working with your own designed binders
- The target is new — you'll need to interpret results in the context of that target
- Your analysis feeds directly into your final presentation

---

## Step 1: Data Loading and Overview

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load your results
df = pd.read_csv("MyTarget/final_design_stats.csv")
failures = pd.read_csv("MyTarget/failure_csv.csv")

print("=== Run Summary ===")
print(f"Accepted designs: {len(df)}")
print(f"Rejected designs: {len(failures)}")
print(f"Total trajectories: {len(df) + len(failures)}")
print(f"Acceptance rate: {len(df) / (len(df) + len(failures)) * 100:.1f}%")

print("\n=== Metric Summary (Accepted Designs) ===")
key_metrics = [
    "Average_i_pTM",
    "Average_pLDDT",
    "Average_Binder_pLDDT",
    "Average_Binder_RMSD",
    "Average_ShapeComplementarity",
    "Average_dG"
]
print(df[key_metrics].describe().round(3))
```

---

## Step 2: Failure Analysis

```python
# What filter was most commonly failed?
print("=== Most Common Filter Failures ===")

filter_cols = [col for col in failures.columns if "fail" in col.lower()]
if filter_cols:
    failure_reason = failures[filter_cols[0]].value_counts()
    print(failure_reason)
```

**Key questions:**
- What filter rejected the most designs?
- Is the acceptance rate similar to the PD-L1 run, or very different?
- If most failures are on i_pTM: the hallucination had trouble finding good interfaces → hotspot choice may need revision
- If most failures are on Binder_RMSD: designs aren't self-folding → try shorter lengths
- If most failures are on ShapeComplementarity: the geometry isn't working → the hotspot surface may be difficult

---

## Step 3: Metric Distributions

```python
fig, axes = plt.subplots(2, 3, figsize=(15, 8))
metrics = [
    ("Average_i_pTM", 0.55, "Interface Confidence"),
    ("Average_ShapeComplementarity", 0.55, "Shape Complementarity"),
    ("Average_Binder_RMSD", 2.0, "Binder RMSD (lower=better)"),
    ("Average_dG", -10, "Binding Energy (more neg=better)"),
    ("Average_Binder_pLDDT", 0.80, "Binder pLDDT"),
    ("Average_n_InterfaceHbonds", 5, "H-bonds at Interface")
]

for ax, (col, threshold, title) in zip(axes.flatten(), metrics):
    ax.hist(df[col], bins=20, color="steelblue", edgecolor="black", alpha=0.7)
    ax.axvline(threshold, color="red", linestyle="--", linewidth=1.5, label=f"threshold={threshold}")
    ax.set_title(title)
    ax.set_xlabel(col.replace("Average_", ""))
    ax.legend(fontsize=8)

plt.suptitle(f"BindCraft Results: {len(df)} accepted designs for MyTarget", fontsize=14)
plt.tight_layout()
plt.savefig("metric_distributions.png", dpi=150, bbox_inches="tight")
plt.show()
```

---

## Step 4: Ranking and Selection

```python
# Sort by i_pTM (primary) then ShapeComplementarity (secondary)
df_ranked = df.sort_values(
    ["Average_i_pTM", "Average_ShapeComplementarity"],
    ascending=[False, False]
)

# Display top 10
display_cols = [
    "binder_name",
    "Average_i_pTM",
    "Average_ShapeComplementarity",
    "Average_Binder_RMSD",
    "Average_dG",
    "Average_n_InterfaceHbonds"
]
print("=== Top 10 Designs ===")
print(df_ranked.head(10)[display_cols].to_string(index=False))
```

**Your selection criteria:**
1. i_pTM > 0.60 minimum for top 5 selections
2. Binder_RMSD < 1.5 Å (self-consistent)
3. ShapeComplementarity > 0.60
4. Structural diversity (check sequences aren't all identical)
5. Visual inspection in Mol*

---

## Step 5: Visual Inspection

For each of your top 5 candidates, open in Mol* and document:

| Design | i_pTM | Does binder cover hotspots? | Interface tight? | Helix count | Notes |
|--------|-------|----------------------------|-----------------|-------------|-------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

---

## Step 6: Comparison to PD-L1 Results

This comparison gives context to your results:

| Feature | PD-L1 run | Your target run |
|---------|----------|----------------|
| Acceptance rate | ~X% | ~Y% |
| Mean i_pTM (accepted) | | |
| Mean ShapeComp (accepted) | | |
| Mean Binder RMSD (accepted) | | |
| Mean dG (accepted) | | |
| Typical binder type | Helical | |

**Interpret:**
- Is your target "easier" or "harder" to design against than PD-L1?
- What does the comparison tell you about this target's binding surface?

---

## Step 7: Reflection Questions

Answer these in writing for your final presentation:

1. **What surprised you about the results?** Were metrics better or worse than expected?

2. **If you could do one more BindCraft run, what would you change?** (Different hotspots? Different lengths? Relaxed filters?)

3. **How confident are you in your top 5 selections?** What's the weakest link in each design?

4. **What experiment would you do first after expressing the proteins?** ELISA? SPR? Why?

5. **If design #1 fails in the wet lab, what does that tell you?** What would you conclude? What would you try next?

These reflection questions demonstrate scientific maturity — the ability to think critically about your own work and plan next steps under uncertainty.

---

> **Key takeaway:** Analysis of your independent project results uses the same skills as the PD-L1 case study. Compare your results to PD-L1 for context. Document visual inspection alongside quantitative metrics. The reflection questions are as important as the selection itself — they show that you understand *why* you made each choice and what the limits of the computational prediction are.

**Next:** [9.6 Final Presentation Guide →](m9_06_presentation.md)
