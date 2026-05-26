# 8.4 The Design-Make-Test-Analyze (DMTA) Cycle

**Time:** ~30 minutes reading

---

## Science as a Loop, Not a Line

Traditional drug discovery was linear: hypothesis → experiment → result → publication → next hypothesis. This worked, but was slow — often 10–15 years from target identification to approved drug.

Modern computational biologics design is **iterative**. The DMTA cycle compresses the loop:

```
DESIGN (computational)
    ↓
MAKE (synthesis + expression)
    ↓
TEST (binding assays, biophysics)
    ↓
ANALYZE (what worked, what didn't, why)
    ↓
DESIGN again (smarter, faster, with new data)
    ↑______________________________________|
```

Each cycle produces both a result (a potential drug candidate) and data (a richer dataset for the next cycle). The cycles compound — each round makes the next faster and more targeted.

---

## What Each Step Looks Like in This Project

### DESIGN — What You're Doing in Weeks 1–4

In the computational phase, you:
1. Set up BindCraft with the target PDB and hotspot residues
2. Run the design pipeline (hallucination → MPNN → filtering → self-consistency)
3. Analyze the output CSVs
4. Select top designs based on metrics (i_pTM, shape complementarity, dG, Binder RMSD)
5. Produce a "Design Recommendation Report" with your top 5 picks

**Your output:** A table of sequences + predicted structures + justification for each selection

### MAKE — Your Co-Mentor's Role

After you hand off your recommendations, the wet lab side:
1. **Gene synthesis** — the selected protein sequences are converted to DNA and ordered from a gene synthesis company (Twist Bioscience, IDT, etc.)
2. **Cloning** — the DNA is inserted into an expression vector
3. **Expression** — bacteria (E. coli) or mammalian cells produce the protein
4. **Purification** — the protein is isolated using affinity tags (His-tag, IMAC column) and size-exclusion chromatography
5. **Quality check** — SDS-PAGE, mass spectrometry to confirm identity and purity

**Timeline:** 2–6 weeks depending on expression system and protein difficulty

### TEST — Binding Assays

Once purified protein is in hand:
- **ELISA** — enzyme-linked immunosorbent assay; detects binding with a color change readout; qualitative (yes/no binding)
- **SPR (Surface Plasmon Resonance)** — measures binding kinetics in real time; gives on-rate (k_on), off-rate (k_off), and equilibrium affinity (Kd)
- **BLI (Biolayer Interferometry)** — similar to SPR, measures optical interference changes as protein binds to sensor
- **Flow cytometry** — measures binding to PD-L1 on actual cell surfaces (more biologically relevant)

**What a "hit" looks like:**
- ELISA: absorbance significantly above background
- SPR: clear association and dissociation phases; Kd in nanomolar range (< 100 nM is excellent)

### ANALYZE — Closing the Loop

After testing, the data goes back to the design team:
- Which designs bound? Which didn't?
- Which metrics (i_pTM, shape complementarity) best predicted the experimental hits?
- Are there patterns in the sequences of binders vs. non-binders?
- What should the next design round prioritize?

This analysis improves the computational model for the next DMTA cycle. Over multiple cycles, the hit rate improves and the designs get better.

---

## The Power of Iteration

| What | Before DMTA platforms | With DMTA platforms |
|------|-----------------------|---------------------|
| Designs per round | Dozens (manual) | Hundreds–thousands (automated) |
| Cycle time | 6–12 months | 4–8 weeks |
| Data captured | Paper lab notebooks | Searchable digital databases |
| Model improvement | Slow | Compounding |

> **Keytruda comparison:** Pembrolizumab (Keytruda) took ~12 years from initial antibody discovery to FDA approval. Modern DMTA platforms can identify initial binders in weeks. The clinical development pathway (safety and efficacy trials) still takes years — but the *discovery* phase is dramatically faster.

---

## Partial Diffusion: Refining Hits

One powerful application of DMTA in the BindCraft world: **partial diffusion** (from RFDiffusion).

If a design shows weak binding in SPR (say, Kd = 1 µM instead of the desired < 10 nM), you don't have to start over. You can:

1. Take the existing binder structure
2. Add a small amount of noise to it (partial diffusion)
3. Re-diffuse → get a slightly modified binder that explores nearby sequence/structure space
4. Re-test

This is more efficient than starting from scratch and often improves affinity by 10–100 fold in one round.

---

## Your Role in the Cycle

In this internship, you complete the **DESIGN** step of one full DMTA cycle:

- Weeks 1–2: Learn the biology and tools
- Weeks 3–4: Run BindCraft, analyze outputs, select top 5 designs
- Week 4 (capstone): Hand off your Design Recommendation Report to your co-mentor

The wet lab testing will happen after your internship ends — but your computational work directly determines what gets synthesized and tested.

**That's real science.**

---

> **Key takeaway:** DMTA is iterative, not linear. Each cycle produces results AND data that improve the next cycle. In this internship, you complete the Design step. Your co-mentor's wet lab work completes the Make and Test steps. Analysis closes the loop.

**Next:** [Module 9: Your Independent Project →](m9_01_new_target.md)
