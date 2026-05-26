# 7.4 PDL1 Case Study

**Time:** ~45 minutes hands-on

---

## Putting It All Together

This is your capstone analysis for the PD-L1 design phase. Using the pregenerated PDL1_8aok dataset (or your own BindCraft output), you will:

1. Load and explore the full output
2. Apply the traffic light system to all accepted designs
3. Select your top 5 with written justifications
4. Visualize each top design
5. Write a brief Design Recommendation Report

This is exactly what you'll do for your independent project in Module 9.

---

## The PDL1_8aok Dataset

The pregenerated dataset `PDL1_8aok/` contains results from a full BindCraft run using PDB `8AOK` (PD-L1 with a known designed binder in the crystal structure). This is an excellent reference — you can compare your designed binders to the experimental structure.

Location: `PDL1_8aok/final_design_stats.csv`

### Dataset Overview

Run the following in Notebook 05:

```python
import pandas as pd

df = pd.read_csv("PDL1_8aok/final_design_stats.csv")

print(f"Total accepted designs: {len(df)}")
print(f"\ni_pTM range: {df['Average_i_pTM'].min():.3f} – {df['Average_i_pTM'].max():.3f}")
print(f"ShapeComp range: {df['Average_ShapeComplementarity'].min():.3f} – {df['Average_ShapeComplementarity'].max():.3f}")
print(f"Binder RMSD range: {df['Average_Binder_RMSD'].min():.3f} – {df['Average_Binder_RMSD'].max():.3f}")
print(f"dG range: {df['Average_dG'].min():.1f} – {df['Average_dG'].max():.1f}")
```

---

## Case Study Analysis

### Part 1: The Best Design

Find the design with the highest i_pTM:

```python
best = df.loc[df["Average_i_pTM"].idxmax()]

print("=== Best Design ===")
print(f"Name: {best['binder_name']}")
print(f"Sequence: {best['binder_sequence']}")
print(f"Length: {len(best['binder_sequence'])} residues")
print(f"\nKey metrics:")
print(f"  i_pTM:          {best['Average_i_pTM']:.3f}")
print(f"  pLDDT:          {best['Average_pLDDT']:.3f}")
print(f"  Binder pLDDT:   {best['Average_Binder_pLDDT']:.3f}")
print(f"  Binder RMSD:    {best['Average_Binder_RMSD']:.2f} Å")
print(f"  ShapeComp:      {best['Average_ShapeComplementarity']:.3f}")
print(f"  dG:             {best['Average_dG']:.1f} kcal/mol")
print(f"  H-bonds:        {best['Average_n_InterfaceHbonds']:.1f}")
print(f"  Clashes:        {best['Average_Relaxed_Clashes']:.1f}")
```

**Questions to answer:**
1. Does the best design pass ALL the green thresholds from Module 7.1?
2. Are any metrics exceptional (well above threshold)?
3. Are any metrics borderline?

### Part 2: Comparing Top 5 to Bottom 5

```python
top5 = df.nlargest(5, "Average_i_pTM")
bottom5 = df.nsmallest(5, "Average_i_pTM")

comparison_cols = [
    "binder_name", "Average_i_pTM", "Average_ShapeComplementarity",
    "Average_Binder_RMSD", "Average_dG"
]

print("=== TOP 5 (by i_pTM) ===")
print(top5[comparison_cols].to_string(index=False))
print("\n=== BOTTOM 5 (lowest i_pTM) ===")
print(bottom5[comparison_cols].to_string(index=False))
```

**Questions to answer:**
1. What's the range of i_pTM across accepted designs?
2. Does shape complementarity correlate with i_pTM?
3. Are any bottom-5 designs unexpectedly good in other metrics?

### Part 3: Visualizing Two Contrasting Designs

Open in Mol*:
- The best design: `PDL1_8aok/Accepted/Ranked/1_PDL1.pdb`
- A borderline design (lower i_pTM): find in `Accepted/Ranked/` near the bottom

**Describe (in your own words) the visual differences:**
- Where does each binder sit relative to PD-L1?
- Does one look more compact, more interface-covering?
- Do the hotspot residues (54, 56, 111, 115, 123) appear to be contacted by each binder?

### Part 4: Comparing to the Experimental Binder

Load the crystal structure `8AOK.pdb` alongside your designed binder. The 8AOK structure has a real experimental binder in it.

**Questions:**
1. Does your designed binder use the same face of PD-L1 as the experimental binder?
2. Are they similar in size (number of residues)?
3. Do they both use helices as the primary binding element?

---

## Writing Your Design Recommendation Report

Your report should be ~1 page (or a table + short text). Template:

---

### PD-L1 Binder Design Report
**Date:** [Date]  
**Intern:** [Your name]  
**BindCraft run:** PDL1_8aok dataset  
**Total designs accepted:** [N]

#### Top 5 Recommendations

| Rank | Design Name | i_pTM | ShapeComp | Binder RMSD | dG | Why Recommended |
|------|------------|-------|-----------|-------------|-----|----------------|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |

#### Justification for Selection

**Design 1:** [2-3 sentences explaining why this is the top design — what makes its metrics stand out, what does it look like structurally, does it cover the hotspot residues?]

**Design 2:** [...]

...

#### Notes on Excluded Designs

[1-2 sentences about any borderline designs you excluded and why]

#### Recommendations for the Next Design Round

[Based on what you saw in the failure analysis — if most designs failed on Binder_RMSD, for example — what would you suggest trying in the next BindCraft run?]

---

## Submission

Your completed Design Recommendation Report (as a document or filled-in table) is your deliverable for the end of Week 4. You will present it to your mentor and co-mentor before the wet lab ordering decision is made.

---

> **Key takeaway:** The PDL1 case study integrates everything from Modules 1–7. You're doing real science — making a recommendation about which protein sequences should be synthesized and tested experimentally. The quality of your analysis directly determines what gets tested.

**Next:** [Module 8: Wet Lab Handoff →](m8_01_bits_to_molecules.md)
