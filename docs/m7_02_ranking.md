# 7.2 Ranking & Filtering Designs

**Time:** ~30 minutes reading + notebook hands-on

---

## The Goal of This Step

You have a CSV with 10–50 accepted designs, each with 10+ metrics. Your job is to:

1. **Filter** — remove designs with any metric that raises a red flag
2. **Rank** — sort the remaining designs by overall quality
3. **Select** — choose your top 5 for the Design Recommendation Report

This is both a quantitative exercise (the numbers) and a qualitative one (interpreting what the numbers mean). Neither alone is sufficient.

---

## Step 1: Load the Data

In Notebook 05, the first step is always:

```python
import pandas as pd

df = pd.read_csv("PDL1/final_design_stats.csv")

# How many designs?
print(f"Total accepted designs: {len(df)}")

# What columns are available?
print(df.columns.tolist())
```

Focus on the key columns (all of these already passed the automated filters, but you want to see the values):

```python
key_cols = [
    "binder_name",
    "Average_i_pTM",
    "Average_pLDDT",
    "Average_Binder_pLDDT",
    "Average_Binder_RMSD",
    "Average_ShapeComplementarity",
    "Average_dG",
    "Average_i_pAE",
    "Average_n_InterfaceHbonds",
    "Average_Relaxed_Clashes"
]

df_view = df[key_cols].copy()
df_view = df_view.sort_values("Average_i_pTM", ascending=False)
df_view
```

---

## Step 2: Apply a Ranking Score

While i_pTM is the single most important metric, you want to consider all metrics together. A simple approach: **weighted composite score**.

```python
# Normalize each metric to 0–1 range
from sklearn.preprocessing import MinMaxScaler

# Metrics where higher = better
higher_better = [
    "Average_i_pTM",
    "Average_pLDDT",
    "Average_Binder_pLDDT",
    "Average_ShapeComplementarity",
    "Average_n_InterfaceHbonds"
]

# Metrics where lower = better (we invert them)
lower_better = [
    "Average_Binder_RMSD",
    "Average_i_pAE",
    "Average_Relaxed_Clashes"
]

# For dG: more negative = better (invert)
# Average_dG is already negative for good designs

# Composite score (equal weighting first)
weights = {
    "Average_i_pTM": 0.35,
    "Average_ShapeComplementarity": 0.20,
    "Average_Binder_RMSD": 0.15,    # inverted
    "Average_Binder_pLDDT": 0.10,
    "Average_pLDDT": 0.10,
    "Average_dG": 0.10               # inverted (more negative = better)
}
```

> **Note:** You don't need to implement this precisely. The notebook provides a pre-built ranking function. This is shown so you understand the logic.

---

## Step 3: The Traffic Light Check

Even though all designs in `final_design_stats.csv` passed the automated filters, you should manually scan them using the traffic light system:

```
For each design:
  i_pTM > 0.55       → ✅
  i_pTM > 0.65       → ✅✅ (extra good)
  pLDDT > 0.85       → ✅
  Binder_pLDDT > 0.80 → ✅
  i_pAE < 10         → ✅
  Binder_RMSD < 2.0  → ✅
  Binder_RMSD < 1.0  → ✅✅ (excellent self-consistency)
  ShapeComp > 0.55   → ✅
  ShapeComp > 0.65   → ✅✅ (excellent fit)
  dG < -10           → ✅
  dG < -20           → ✅✅ (strong predicted binding)
  Clashes < 5        → ✅
```

Highlight designs with multiple ✅✅ — these are your top candidates.

---

## Step 4: Visualizing the Distribution

Before selecting individual designs, look at the distribution of each metric across all accepted designs:

```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(2, 4, figsize=(16, 8))
metrics = [
    "Average_i_pTM", "Average_ShapeComplementarity",
    "Average_Binder_RMSD", "Average_dG",
    "Average_Binder_pLDDT", "Average_i_pAE",
    "Average_n_InterfaceHbonds", "Average_Relaxed_Clashes"
]

for ax, col in zip(axes.flatten(), metrics):
    ax.hist(df[col], bins=15, edgecolor='black', color='steelblue')
    ax.set_title(col.replace("Average_", ""))
    ax.axvline(df[col].mean(), color='red', linestyle='--', label='mean')

plt.tight_layout()
plt.show()
```

**What to look for:**
- Is there a spread, or are all designs clustered near the threshold?
- Designs at the *far end* of each distribution (not just "above threshold" but *well* above threshold) are the most promising
- Outliers that are much worse than the group — investigate these before excluding them (sometimes visual inspection reveals they're actually fine)

---

## Step 5: The Scatter Plot Method

The most useful plot for selection: i_pTM vs. ShapeComplementarity.

```python
plt.figure(figsize=(8, 6))
plt.scatter(
    df["Average_i_pTM"],
    df["Average_ShapeComplementarity"],
    c=df["Average_Binder_RMSD"],
    cmap="RdYlGn_r",
    s=100, edgecolors='black'
)
plt.colorbar(label="Binder RMSD (lower = better)")
plt.xlabel("Average i_pTM")
plt.ylabel("Average Shape Complementarity")
plt.axvline(0.55, color='gray', linestyle='--')
plt.axhline(0.55, color='gray', linestyle='--')
plt.title("Design Quality: i_pTM vs. Shape Complementarity")
plt.show()
```

**Reading the plot:**
- You want designs in the **top-right quadrant** (high i_pTM + high ShapeComp)
- Color = Binder RMSD: green = low RMSD (good), red = high RMSD (bad)
- Best designs: top-right, green

---

## Step 6: Select Your Top 5

From the ranked, visualized, traffic-lighted list, select 5 designs to recommend:

**Selection criteria (in order of priority):**
1. Highest i_pTM (must be > 0.60 minimum for top selections)
2. Shape complementarity > 0.60
3. Binder RMSD < 1.5 Å (good self-consistency)
4. Diverse structures — don't pick 5 identical binders; pick designs with different binding geometries
5. Visual inspection passes (see Module 6.4 activity)

**Diversity note:** Run the following to check if your top 5 have different sequences:
```python
from difflib import SequenceMatcher

top5 = df_sorted.head(5)["binder_sequence"].tolist()
for i in range(len(top5)):
    for j in range(i+1, len(top5)):
        similarity = SequenceMatcher(None, top5[i], top5[j]).ratio()
        print(f"Seq {i+1} vs {j+1}: {similarity:.1%} similar")
```

If two designs are > 90% similar in sequence, they'll likely behave similarly in the wet lab — prefer to diversify.

---

> **Key takeaway:** Ranking uses a combination of i_pTM (primary), ShapeComplementarity and Binder_RMSD (secondary), and all other metrics as supporting signals. Visualize distributions before selecting. Choose 5 diverse designs that are well above threshold — not just the 5 highest i_pTM if they're all the same structure.

**Next:** [7.3 Good vs. Bad Designs →](m7_03_good_vs_bad.md)
