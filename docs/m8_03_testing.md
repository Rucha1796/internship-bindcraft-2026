# 8.3 How We Test Binders

**Time:** ~25 minutes reading

---

## From Purified Protein to Binding Data

Once your designed binder has been expressed in *E. coli* and purified, the next question is: **does it actually bind PD-L1?**

This requires experimental assays — methods that detect whether two proteins interact. Each assay answers a slightly different question, and they're typically done in order from fastest/cheapest to most informative/expensive.

---

## The Testing Hierarchy

```
Week 1: ELISA (qualitative binding — yes or no?)
  ↓ (if positive)
Week 2: BLI / SPR (quantitative — Kd measurement)
  ↓ (if Kd < 100 nM)
Week 3+: Cell assays, biophysical characterization, further optimization
```

---

## ELISA — Enzyme-Linked Immunosorbent Assay

**What it tells you:** Does your binder detect PD-L1? (Yes/No with rough signal strength)

**How it works:**
1. Coat a plastic 96-well plate with PD-L1 protein
2. Wash away unbound PD-L1
3. Add your designed binder (with a detection tag, e.g., His-tag or biotin)
4. Wash away unbound binder
5. Add an enzyme-linked detection reagent (anti-His antibody + HRP enzyme, or streptavidin-HRP)
6. Add a color substrate — HRP converts it to a colored product
7. Read absorbance at 450 nm

**What you see:** A yellow/orange color in wells with binding; clear wells with no binding. The darker the color, the more binder attached.

**Controls (essential):**
- **Positive control:** A known PD-L1 binder (e.g., the 8AOK binder peptide if available, or an anti-PD-L1 antibody like atezolizumab)
- **Negative control:** Binder added to wells coated with an irrelevant protein (confirms binding is specific to PD-L1)
- **Background:** Wells with no binder at all (measures the baseline absorbance)

**Limitations:**
- Qualitative only — you can't measure Kd from ELISA
- High sensitivity to plate coating conditions
- Can give false positives with sticky/aggregated proteins

**Result cutoff:** Your binder is a "hit" in ELISA if absorbance is > 2-fold above the negative control.

---

## SPR — Surface Plasmon Resonance

**What it tells you:** Full binding kinetics — on-rate (k_on), off-rate (k_off), and equilibrium affinity (Kd)

**How it works:**
1. PD-L1 is chemically attached to a gold sensor chip
2. Your binder flows over the chip in solution
3. As the binder binds PD-L1, the mass on the chip increases → changes the refractive index near the gold surface → changes the angle of reflected light (this is the "surface plasmon resonance")
4. The instrument records this angle change in real time

**Reading an SPR sensorgram:**

```
Response
(RU)
 100 │            ╭──────────╮
     │           ╱            ╲    association
  50 │          ╱              ╲   dissociation
     │         ╱                ╲
   0 │────────╯                  ──────────────
     │        ↑               ↑
     │    binder IN        binder OUT (wash)
     └──────────────────────────────────────────
                            Time (s)
```

- **Association phase:** Binder flowing over the chip; signal rises as it binds
- **Dissociation phase:** Buffer flows over; signal falls as bound binder leaves
- **k_on** = rate of signal rise (how fast it binds)
- **k_off** = rate of signal fall (how fast it dissociates)
- **Kd = k_off / k_on** = equilibrium dissociation constant

**What good results look like:**
- Clear association curve that rises proportionally to binder concentration
- Clear dissociation curve (not flat — a flat dissociation means the binder stuck permanently, which is usually artifactual)
- Kd < 100 nM for a useful initial hit
- Kd < 10 nM for a strong lead

**Example:** If Kd = 50 nM, that means at 50 nM concentration, 50% of PD-L1 is bound to your binder. This is similar to Keytruda's affinity for PD-L1.

---

## BLI — Bio-Layer Interferometry

**What it tells you:** Same as SPR — k_on, k_off, Kd

**How it's different from SPR:**
- Uses optical fiber tips coated with PD-L1 instead of a gold chip
- Measures interference in reflected light rather than surface plasmon resonance
- Generally faster, cheaper per experiment, and easier to set up
- Slightly less sensitive than SPR for very tight binders (sub-nanomolar Kd)

BLI (e.g., Octet instruments) is often used for initial screening because you can run many samples in parallel (up to 96 at once). SPR is preferred for detailed characterization.

---

## Flow Cytometry — Cell-Based Binding

**What it tells you:** Does your binder bind to PD-L1 on the surface of actual cells?

This is more biologically relevant than binding to purified protein on a chip.

**How it works:**
1. Grow cells that express PD-L1 on their surface (e.g., cancer cell lines like MDA-MB-231)
2. Incubate cells with your labeled binder
3. Wash
4. Run cells through a flow cytometer — a laser measures fluorescence on each cell
5. Cells with binder attached fluoresce; cells without binder don't

**Result:** A histogram of fluorescence intensity. Cells incubated with your binder should have higher fluorescence than negative control cells.

Cell-based binding is done later in the pipeline (after confirming biochemical binding first) because it's more complex and uses more binder.

---

## Connecting Back to Computational Metrics

After testing, your co-mentor will share the experimental results. A key question: **do the computational metrics predict the experimental outcomes?**

Things to look for in the data:

| Computational prediction | Likely experimental observation |
|--------------------------|-------------------------------|
| High i_pTM (> 0.65) | Higher chance of ELISA hit |
| Low Binder_RMSD (< 1.0 Å) | Better chance of cell-based binding |
| High ShapeComp (> 0.65) | Tighter Kd in SPR |
| Many H-bonds (> 8) | Slower k_off (longer bound time) |
| High dG (< -20) | Not always predictive — dG is noisy |

These correlations aren't perfect — that's why we do experiments. But over multiple DMTA cycles, patterns emerge that improve your next round of computational design.

---

> **Key takeaway:** Binding testing proceeds from qualitative (ELISA: yes/no) to quantitative (SPR/BLI: Kd measurement) to cell-based (flow cytometry: relevant biology). A hit in ELISA with Kd < 100 nM in SPR is an excellent first-round result. After testing, analyzing which computational metrics predicted experimental outcomes improves the next design cycle.

**Next:** [8.4 The DMTA Cycle →](m8_04_dmta.md)
