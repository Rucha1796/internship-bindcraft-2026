# 5.4 Filtering Designs

**Time:** ~20 minutes reading

---

## The Filtering Philosophy

BindCraft generates many design candidates — potentially hundreds — in a single run. Not all of them are worth synthesizing and testing. Wet lab experiments cost money and time; ordering a bad design is wasteful.

Filtering is the quality control step that sorts designs into:
- **Accepted:** Meets all thresholds → go to synthesis consideration
- **Rejected:** Failed one or more thresholds → logged but not recommended

The goal: send only the most promising candidates to the wet lab.

---

## When Filtering Happens

Filtering occurs **twice** in the BindCraft pipeline:

### Filter 1: After Hallucination (Step 1 → Step 2)

Quick checks on the hallucination trajectory output:
- pLDDT of the complex: is AF2 confident about the overall structure?
- i_pTM: is AF2 confident the interface is real?
- Clashes: are atoms overlapping?

Trajectories that fail here go to `LowConfidence/` or `Clashing/` folders. They never reach MPNN.

### Filter 2: After MPNN Redesign (Step 3)

Comprehensive scoring using 8+ metrics. This is the main filtering step you'll analyze. Designs that fail here go to the `Rejected/` folder. Only those passing all filters appear in `final_design_stats.csv`.

---

## The Filter Criteria (Step 3)

| Filter | Threshold | What it catches |
|--------|-----------|----------------|
| Average_pLDDT | > 0.85 | Disordered or poorly structured complex |
| Average_i_pTM | > 0.55 | Interface AF2 doesn't believe in |
| Average_i_pAE | < 10 Å | Poorly positioned binder |
| Average_ShapeComplementarity | > 0.55 | Poor geometric fit |
| Average_dG | < -10 kcal/mol | Unfavorable predicted binding energy |
| Average_Relaxed_Clashes | < 5 | Persistent atomic clashes after energy minimization |
| Average_Binder_pLDDT | > 0.80 | Binder doesn't fold well on its own |
| Average_Binder_RMSD | < 2.0 Å | Binder fold depends on target (bad) |

> Note: These are the **default** filter settings. BindCraft allows you to switch between preset filter levels (Default, Relaxed, Peptide, None) depending on the application.

---

## Why Each Filter Exists

### Average_pLDDT > 0.85
AF2 must be confident the whole complex is well-structured. A low overall pLDDT usually means the binder is disordered or clashing with the target in ways that confuse AF2.

### Average_i_pTM > 0.55
This is the most important filter. A design with i_pTM < 0.55 is one AF2 doesn't believe is really binding — the interface is predicted to be wrong or absent.

### Average_i_pAE < 10 Å
Even if i_pTM is good, high i_pAE means the binder is positioned imprecisely — AF2 is uncertain about exactly *where* the binder sits relative to the target. Low i_pAE = confident, precise placement.

### Average_ShapeComplementarity > 0.55
PyRosetta measures how well the surfaces geometrically match. A good interface isn't just "touching" — the surfaces complement each other like a key in a lock.

### Average_dG < -10 kcal/mol
Binding should be energetically favorable. PyRosetta's Rosetta energy function estimates the binding free energy. More negative = more favorable. A design with dG = 0 or positive isn't predicted to bind spontaneously.

### Average_Relaxed_Clashes < 5
After energy minimization (relax), some clashes may resolve. If more than 5 remain, the geometry is genuinely problematic.

### Average_Binder_pLDDT > 0.80
The binder itself (not the whole complex) must be confidently folded. A design that only folds when the target "props it up" is not useful — it won't fold in solution on its own.

### Average_Binder_RMSD < 2.0 Å
The self-consistency check (Step 4). RMSD between the binder structure predicted in complex vs. alone. If these differ by > 2 Å, the binder is relying on the target to fold — it won't work as a free binder in solution.

---

## The Failure CSV

BindCraft saves a `failure_csv.csv` that tracks which filter each rejected design failed. This is extremely useful for analysis:

- If most designs fail on i_pTM → the hallucination parameters may need tuning
- If most designs fail on Binder_RMSD → designs aren't self-folding; try shorter binders or different helicity settings
- If most designs fail on ShapeComplementarity → the surface doesn't fit well; the target surface may need different hotspot choices

Analyzing failures teaches you as much as analyzing successes.

---

## The accepted/ → Ranked/ Folder

Designs that pass all filters are:
1. Moved to `Accepted/`
2. Ranked by `Average_i_pTM` (descending)
3. Renamed: `1_design.pdb`, `2_design.pdb`, etc. (1 = best)
4. Listed in `final_design_stats.csv`

When you open `final_design_stats.csv` in Module 7, you're looking at only the designs that survived all filters.

---

## The Trade-off Between Stringency and Yield

Stricter filters → fewer designs pass → higher average quality but potentially miss good candidates
Looser filters → more designs pass → more to review but some will be false positives

In practice:
- Use Default filters for first-round selection
- If you have very few accepted designs (< 5), consider Relaxed filters and manually evaluate borderline cases
- Never use No filters — you'll waste wet lab resources on obviously bad designs

---

> **Key takeaway:** Filtering separates designs that are predicted to work from those that aren't. The 8 default filters cover structural confidence, interface quality, geometric fit, binding energy, and self-consistency. Only designs passing all filters appear in final_design_stats.csv. The failure analysis is equally valuable — it tells you how to improve the next design round.

**Next:** [Module 6: Running BindCraft →](m6_01_setup.md)
