# 2.1 A Timeline: From X-Ray Crystallography to AlphaFold

**Time:** ~45 minutes reading + activity

---

## The 70-Year Quest to See Proteins

Understanding protein structure required developing entirely new science — experimental, computational, and eventually AI-based. Here is the story of how we got here.

---

## The Timeline

### 1951 — Linus Pauling Predicts the Alpha Helix
Linus Pauling used the known chemistry of amino acids and bond angles to *predict* that proteins should form helical structures — before any protein structure had been experimentally determined. He was right. This showed that physics could constrain what structures were possible.

### 1958 — First Protein Structure: Myoglobin
John Kendrew at Cambridge used **X-ray crystallography** to solve the first protein structure — myoglobin, a small oxygen-carrying protein from sperm whale muscle. Resolution: 6Å (later refined to 2Å). Kendrew won the 1962 Nobel Prize in Chemistry.

**How X-ray crystallography works:**
1. Grow crystals of the protein (often the hardest step — can take years)
2. Shoot X-rays through the crystal
3. X-rays scatter off electrons; collect the diffraction pattern
4. Use Fourier mathematics to reconstruct the 3D electron density
5. Fit a model of the protein into the density

> **Limitation:** Requires a crystal. Many proteins (membrane proteins, disordered proteins, large complexes) are extremely difficult or impossible to crystallize.

### 1971 — The Protein Data Bank (PDB) Founded
Brookhaven National Laboratory established the PDB as a public repository for protein structures. Starting with just 7 structures, it now contains over 230,000 structures.

### 1970s–1990s — Homology Modeling
Once enough structures were known, scientists noticed that **proteins with similar sequences tend to have similar structures**. This enabled **homology modeling**: use a known structure (the "template") to build a model of a related protein.

The workflow (detailed in [section 2.3](m2_03_homology_to_dl.md)):
- Find a template with >30–40% sequence identity
- Align target and template sequences
- Substitute amino acids, rebuild variable regions
- Optimize and validate

*Accuracy depends strongly on template similarity.* Still used today for closely related proteins.

### 1994 — CASP Begins
The **Critical Assessment of Structure Prediction (CASP)** is a blind prediction challenge: organizers collect sequences of proteins whose structures have been solved but not yet published, and groups worldwide try to predict the structures before the answer is revealed.

CASP runs every 2 years and is the field's gold standard for measuring progress. For 25 years, progress was slow — until 2020.

### 1997 — Rosetta Launched
David Baker's lab at the University of Washington developed **Rosetta** — a physics + statistics based energy function for protein structure prediction and design. Rosetta computes a score for any protein conformation and searches for the lowest-energy structure.

Rosetta became the workhorse of computational protein design — and is still used today inside BindCraft (PyRosetta is used for energy minimization and scoring in BindCraft's pipeline).

### 2018 — AlphaFold1 Wins CASP13
DeepMind entered CASP13 with the first deep learning approach. AlphaFold1 significantly outperformed all previous methods — but still made large errors on difficult targets.

### 2021 — AlphaFold2: The Revolution
AlphaFold2 entered CASP14 and achieved **atomic-level accuracy** on most protein targets. The median GDT score (a structure accuracy metric) was >92 — better than most experimental structures at medium resolution.

The scientific community was stunned. *Nature* published the paper in August 2021. Within months, DeepMind released AlphaFold structures for nearly all human proteins, and the AlphaFold Protein Structure Database now covers over 200 million proteins.

> **Why was AF2 so different?** Three key innovations: (1) Using evolutionary co-variation information (MSA) as the main input signal, (2) The Evoformer attention architecture that reasons about pairs of residues simultaneously, (3) The Structure Module that directly builds 3D coordinates from the pair representation. (See [Module 4](m4_01_alphafold2.md) for full details.)

### 2024 — AlphaFold3 and Beyond
AlphaFold3 extended the approach to protein-ligand, protein-DNA, and protein-RNA complexes, using a diffusion model for the structure module. It also improved antibody-antigen interface prediction.

Other tools continue to emerge: **ESMFold** (Meta, uses a language model instead of MSA — 60× faster), **Boltz-2** (protein + small molecule in a single run), **RoseTTAFold** (Baker lab alternative to AF2).

---

## A Note on Cryo-EM

A second revolution in structural biology happened alongside AlphaFold: **cryo-electron microscopy (cryo-EM)**.

Unlike X-ray crystallography, cryo-EM doesn't require crystals — proteins are flash-frozen in solution and imaged directly. A 2013 hardware breakthrough ("the resolution revolution") made cryo-EM competitive with X-ray crystallography.

Today, the PDB receives more cryo-EM structures per year than X-ray structures. Structures of huge complexes (ribosomes, virus capsids, membrane protein channels) that were impossible to crystallize are now routinely solved.

---

## Timeline Summary

| Year | Milestone | Impact |
|------|-----------|--------|
| 1951 | Pauling predicts alpha helix | Established structural theory |
| 1958 | First crystal structure (myoglobin) | Proof of concept |
| 1971 | PDB founded | Open data infrastructure |
| 1970s–90s | Homology modeling | First computational structures |
| 1994 | CASP begins | Objective benchmarking |
| 1997 | Rosetta | Physics-based design |
| 2013 | Cryo-EM revolution | Structures without crystals |
| 2018 | AlphaFold1 (CASP13) | Deep learning enters |
| 2021 | **AlphaFold2 (CASP14)** | Problem largely solved |
| 2024 | AlphaFold3, ESMFold, Boltz-2 | Extension to complexes & ligands |

---

## Activity: Timeline Poster

On a large sheet of paper, recreate this timeline with:
- One key diagram or sketch per milestone (e.g., sketch a diffraction pattern for X-ray, a coiled helix for Pauling, a cartoon AF2 architecture for 2021)
- Annotate each milestone with: *"What became possible that wasn't possible before?"*

This poster will hang in the lab for the duration of your internship.

---

> **Key takeaway:** It took 70 years of science to go from the first protein structure to a tool that predicts any protein's structure in minutes. AlphaFold2 didn't appear out of nowhere — it built on decades of crystallography, homology modeling, and machine learning.

**Next:** [2.2 The Protein Data Bank →](m2_02_pdb.md)
