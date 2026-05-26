# Glossary

Plain-English definitions for every technical term used in this wiki. When you see an unfamiliar word, look it up here first.

---

## A

**Affinity (Kd)** — How tightly two proteins bind. Kd is the dissociation constant — lower Kd = tighter binding. Typical therapeutic antibody affinities: 1–100 nM (nanomolar). A Kd of 1 nM means the binder stays bound for ~1 second on average; 1 µM means it dissociates in milliseconds.

**AlphaFold2 (AF2)** — A deep learning model by DeepMind (2021) that predicts protein 3D structure from sequence with near-experimental accuracy. Used as both the prediction engine and the scoring function inside BindCraft.

**Alpha helix (α-helix)** — A common secondary structure element where the protein backbone coils into a right-handed spiral, stabilized by hydrogen bonds. BindCraft designs are typically helical binders.

**Amino acid** — The building blocks of proteins. There are 20 standard amino acids, each with a unique side chain that determines its chemical properties. Proteins are chains of amino acids joined by peptide bonds.

**Attention mechanism** — A neural network operation that learns which parts of an input are most relevant to understanding other parts. Used in AlphaFold2's Evoformer to identify which residue pairs are most informative for structure prediction.

---

## B

**Beta sheet (β-sheet)** — A secondary structure where adjacent strands of the protein chain align side by side, connected by hydrogen bonds. PD-L1 has a beta-sandwich fold.

**BindCraft** — A protein binder design pipeline (Pacesa, Nickel et al., *Nature* 2025) that uses AF2 hallucination + ProteinMPNN + PyRosetta to design de novo protein binders against a target of interest.

**BLOSUM** — A substitution matrix that scores how likely one amino acid is to be substituted for another during evolution. Used in sequence alignment algorithms. BLOSUM62 is the most common.

---

## C

**CDR loops** — Complementarity-Determining Regions. The six loops at the tip of an antibody variable domain that make contact with the antigen. H3 (the third heavy chain CDR) is the most variable and hardest to predict.

**ColabDesign** — An open-source library by Sergey Ovchinnikov that enables "protein hallucination" — using AF2's gradient to design sequences de novo. The core of BindCraft's step 1.

**ColabFold** — A fast implementation of AlphaFold2 accessible via Google Colab, using MMseqs2 for rapid MSA generation. Used in Notebook 02 of this internship.

**Cryo-EM** — Cryo-electron microscopy. Proteins are flash-frozen and imaged directly with electrons (no crystals needed). Rival to X-ray crystallography; excellent for large complexes.

---

## D

**De novo design** — Designing a protein with no natural template — creating an entirely new sequence-structure pair from scratch. BindCraft does de novo design of binders.

**dG (ΔG)** — Predicted binding free energy, calculated by PyRosetta. More negative = stronger predicted binding. Used as one filter metric in BindCraft.

**DMTA cycle** — Design-Make-Test-Analyze. The iterative loop in modern drug discovery where computational design feeds experimental testing, which feeds better computational models.

**dSASA** — Change in Solvent-Accessible Surface Area upon binding. Measures how much surface area is buried at the interface when two proteins interact.

---

## E

**Energy minimization** — A computational procedure that adjusts atomic positions to remove steric clashes and optimize geometry, without changing the overall fold. Used in BindCraft after hallucination (PyRosetta relax).

**Evoformer** — The attention-based neural network block at the heart of AlphaFold2. It jointly updates the MSA representation and pair representation using attention across both sequences and residue pairs.

---

## F

**FAIR data** — Findable, Accessible, Interoperable, Reusable. A set of principles for making scientific data usable by machines and other researchers.

---

## G

**GDT (Global Distance Test)** — A protein structure similarity metric. GDT_TS > 90 is considered excellent — roughly equivalent to < 1Å RMSD for well-predicted regions.

**GPU** — Graphics Processing Unit. Originally designed for rendering video game graphics, GPUs excel at the matrix multiplications used in deep learning. AlphaFold2 and BindCraft require a GPU to run at practical speed.

---

## H

**Hallucination (protein hallucination)** — Running AlphaFold2 in reverse: instead of predicting structure from sequence, optimize a sequence using gradient descent to produce a desired structure and interface. Step 1 of BindCraft.

**Homology modeling** — Building a 3D model of a protein using a closely related protein's experimental structure as a template. Accuracy depends on sequence identity to the template.

**Hotspot residues** — The specific amino acids on the target protein that we instruct BindCraft to design the binder against. For PD-L1: residues 54, 56, 111, 115, 123.

**Hydrogen bond** — A non-covalent interaction between a hydrogen atom bonded to an electronegative atom (N, O) and another electronegative atom. Important for secondary structure and interface contacts.

---

## I

**i_pTM (interface pTM)** — Interface Predicted Template Modeling score. The single most important BindCraft metric. Measures AF2's confidence that the predicted interface is accurate. > 0.55 is required.

**Interface** — The surface area where two proteins make contact when they bind. BindCraft is designed to engineer this interface.

**IMAC** — Immobilized Metal Affinity Chromatography. A purification method that uses a metal-charged resin to capture His-tagged proteins. Standard first step in protein purification.

---

## K

**Kd** — See Affinity.

---

## L

**Loss function** — In machine learning, a function that measures how far the model's output is from the desired output. Gradient descent minimizes the loss function. In BindCraft hallucination, the loss rewards high i_pTM, good interface contacts, and low clashes.

---

## M

**Mol*** — (pronounced "Mol Star") A free, web-based protein structure visualization tool from RCSB PDB. Used in Notebooks 01 and 06 of this internship. Available at molstar.org.

**MPNN (ProteinMPNN)** — A neural network model (Dauparas et al., *Science* 2022) that designs amino acid sequences for a given protein backbone. Step 2 of the BindCraft pipeline.

**MSA (Multiple Sequence Alignment)** — A table of related protein sequences aligned column by column. The evolutionary co-variation signal in an MSA is the primary input to AlphaFold2's Evoformer.

---

## P

**PAE (Predicted Aligned Error)** — A matrix output from AlphaFold2 indicating the expected error in residue positions. Off-diagonal low PAE = confident inter-chain interface prediction.

**PD-1** — Programmed Death receptor 1. A surface receptor on T cells. When it binds PD-L1, it sends a "stand down" signal to the T cell.

**PD-L1** — Programmed Death Ligand 1. Expressed on many cell types; overexpressed by tumors to evade the immune system. The target protein in this internship.

**PDB** — Protein Data Bank. The worldwide repository of experimentally determined protein structures. All structures have a 4-character PDB ID (e.g., 5C3T for PD-L1).

**pLDDT** — Predicted Local Distance Difference Test. A per-residue confidence score from AlphaFold2 (0–100 or 0–1). > 90 (or > 0.9) is excellent; < 50 (or < 0.5) suggests disorder.

**ProteinMPNN** — See MPNN.

**pTM** — Predicted Template Modeling score. A global confidence metric from AlphaFold2 (0–1). > 0.7 is good.

**PyRosetta** — A Python interface to the Rosetta molecular modeling suite. Used in BindCraft for energy minimization (relax), interface scoring (dG, shape complementarity), and clash detection.

**py3Dmol** — A Python library for 3D molecular visualization inside Jupyter/Colab notebooks. Used in the analysis notebooks.

---

## R

**RMSD (Root Mean Square Deviation)** — The average distance between corresponding atoms after superposing two structures. RMSD < 1Å = very similar; > 2Å = significant differences.

**Rosetta** — A molecular modeling and design software suite from David Baker's lab (University of Washington). Foundational tool in computational protein design. PyRosetta is used inside BindCraft.

**Rotamer** — One of the discrete preferred conformations of an amino acid side chain. Rotamer libraries catalog these conformations and their frequencies in crystal structures.

---

## S

**SC (Shape Complementarity)** — A metric measuring how well the molecular surfaces of two proteins fit together geometrically. Ranges 0–1; > 0.6 is typical for good protein-protein interfaces.

**Secondary structure** — Local folding patterns in a protein: alpha helices, beta sheets, and loops/turns.

**Self-consistency check** — In BindCraft: verifying that the designed sequence folds into the designed structure both in complex with the target AND in isolation. RMSD between the two predictions should be < 2Å.

**SPR (Surface Plasmon Resonance)** — An optical technique for measuring protein-protein binding kinetics in real time. Gives on-rate, off-rate, and Kd.

---

## T

**T cell** — A type of white blood cell (lymphocyte) that can directly kill abnormal cells or coordinate immune responses. Central to cancer immunotherapy.

**Tertiary structure** — The full 3D fold of a single protein chain.

---

## V

**Voxel grid** — A 3D version of a pixel grid used to represent protein structures for 3D convolutional neural networks. Each "voxel" can encode multiple chemical properties.

---

## X

**X-ray crystallography** — The most common method for determining protein structure. Protein crystals diffract X-rays; the diffraction pattern is used to reconstruct the 3D electron density and build an atomic model.
