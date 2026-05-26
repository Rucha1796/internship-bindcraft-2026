# 6.3 Understanding the Output Files

**Time:** ~25 minutes reading

---

## The Output Folder Structure

After a BindCraft run, your output folder looks like this:

```
PDL1/
├── trajectory_stats.csv
├── mpnn_design_stats.csv
├── final_design_stats.csv
├── failure_csv.csv
│
├── Trajectory/
│   ├── Relaxed/          ← Energy-minimized trajectory PDBs
│   ├── LowConfidence/    ← Trajectories that failed hallucination
│   ├── Clashing/         ← Trajectories with atom clashes
│   ├── Animation/        ← HTML animations of each trajectory
│   └── Plots/            ← pLDDT, pTM, i_pTM curves during optimization
│
├── MPNN/
│   └── Relaxed/          ← Energy-minimized MPNN-redesigned PDBs
│
├── Accepted/
│   ├── Ranked/           ← Final accepted designs: 1_design.pdb, 2_design.pdb, ...
│   ├── Animation/        ← Animations of accepted trajectories
│   └── Plots/            ← Metric plots for accepted designs
│
└── Rejected/             ← Designs that failed one or more filters
```

The files you'll spend most time with: `final_design_stats.csv` and the PDB files in `Accepted/Ranked/`.

---

## The CSV Files

### trajectory_stats.csv

One row per hallucination trajectory. Columns include:

| Column | Meaning |
|--------|---------|
| `name` | Trajectory identifier |
| `pLDDT` | Overall pLDDT at end of optimization |
| `i_pTM` | i_pTM at end of optimization |
| `i_pAE` | Interface PAE |
| `Clashes` | Number of atomic clashes |
| `Status` | Accepted / LowConfidence / Clashing |

This shows you the raw hallucination output before MPNN redesign.

### mpnn_design_stats.csv

One row per MPNN-designed sequence (multiple MPNN sequences per trajectory). Columns include all the hallucination stats plus the MPNN sequence and MPNN-specific scores.

### final_design_stats.csv

The most important file. One row per design that passed **all filters**. Columns include ~150 metrics; the most important are the `Average_` prefixed columns (average across 5 AF2 models).

Key columns:
- `binder_name` — the sequence identifier
- `binder_sequence` — the amino acid sequence (one letter code)
- `Average_i_pTM` — overall quality score
- `Average_pLDDT` — complex confidence
- `Average_Binder_pLDDT` — binder-only confidence
- `Average_i_pAE` — interface PAE
- `Average_Binder_RMSD` — self-consistency RMSD
- `Average_ShapeComplementarity` — geometric fit
- `Average_dG` — predicted binding energy
- `Average_n_InterfaceHbonds` — number of H-bonds at interface
- `Average_Relaxed_Clashes` — post-relax clashes

### failure_csv.csv

One row per **rejected** design, with a column indicating which filter it failed. Use this to diagnose why most designs are being rejected.

---

## The PDB Files

### Trajectory/Relaxed/ PDBs

These are the binder structures at the end of the hallucination step — before MPNN redesign. They represent the backbone geometry that ColabDesign found.

### MPNN/Relaxed/ PDBs

The same backbone as the trajectory, but with the MPNN-redesigned amino acid sequence. Side chains are now full-length and energy-minimized.

### Accepted/Ranked/ PDBs

The final designs. File naming:
```
1_PDL1_trajectory1_mpnn3.pdb   ← best design (highest i_pTM)
2_PDL1_trajectory2_mpnn1.pdb   ← second best
3_PDL1_trajectory5_mpnn2.pdb
...
```

The number prefix is the rank. These are the PDB files you'll visualize and include in your Design Recommendation Report.

---

## Reading final_design_stats.csv in Python

In Notebook 05 (and 06), you'll use pandas to read this CSV:

```python
import pandas as pd

# Load the CSV
df = pd.read_csv("PDL1/final_design_stats.csv")

# View first few rows
df.head()

# Check how many designs passed filters
print(f"Total accepted designs: {len(df)}")

# Sort by i_pTM (best first)
df_sorted = df.sort_values("Average_i_pTM", ascending=False)

# View the top metrics for the best designs
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
print(df_sorted[key_cols].to_string())
```

This gives you the ranked list of accepted designs.

---

## Quick Reference: Where Is What?

| What you want | Where to find it |
|--------------|----------------|
| Which designs passed all filters | `final_design_stats.csv` |
| Why designs were rejected | `failure_csv.csv` |
| Raw hallucination quality | `trajectory_stats.csv` |
| 3D structure of best design | `Accepted/Ranked/1_design.pdb` |
| Animation of best design forming | `Accepted/Animation/` |
| Binder amino acid sequence | `binder_sequence` column in `final_design_stats.csv` |
| How the hallucination evolved | `Trajectory/Plots/` |

---

> **Key takeaway:** BindCraft produces four CSV files (trajectory, MPNN, final, failure) and organized PDB folders. final_design_stats.csv is the main analysis file. The Accepted/Ranked/ PDBs are the structures you'll visualize and recommend. failure_csv.csv tells you what to improve in the next run.

**Next:** [6.4 Visualizing Your Designs →](m6_04_visualization.md)
