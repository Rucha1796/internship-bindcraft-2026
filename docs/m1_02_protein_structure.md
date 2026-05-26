# 1.2 Protein Structure

**Time:** ~45 minutes reading + activity

---

## Four Levels of Structure

Proteins are described at four levels of organization, each building on the last.

### Primary Structure — The Sequence

The primary structure is simply the linear sequence of amino acids, written from N-terminus (start) to C-terminus (end). For example:

```
Met-Ala-Arg-Leu-Ser-Glu-Trp-...
```

Or in single-letter code (each amino acid has a one-letter abbreviation):
```
MARLSEW...
```

This sequence is directly encoded in your DNA. A gene is essentially a recipe that specifies which amino acids go in which order.

---

### Secondary Structure — Local Folding Patterns

The amino acid chain doesn't stay flat — it folds into repeating local patterns stabilized by **hydrogen bonds** between atoms in the backbone.

The two most common patterns are:

**Alpha helix (α-helix)**
- The chain coils into a right-handed spiral, like a corkscrew
- One full turn every 3.6 amino acids
- Hydrogen bonds form between residue *i* and residue *i+4*
- Looks like a ribbon coiled into a tube in structure visualizations
- Common in membrane-spanning regions and protein-protein interfaces

**Beta sheet (β-sheet)**
- Strands of the chain stretch out and align side by side, connected by hydrogen bonds
- Can be parallel (strands run in same direction) or antiparallel (alternate directions)
- Looks like flat arrows in structure visualizations
- Forms the rigid framework of many proteins

**Loops and turns**
- Regions that connect helices and sheets — often disordered and flexible
- The CDR loops of antibodies (which we'll study soon) are loops that are the most variable part of antibody structure

> **Why does secondary structure matter for design?** BindCraft mostly designs helical binders — alpha-helix bundles pack well against flat protein surfaces and are easy for AlphaFold2 to predict confidently.

---

### Tertiary Structure — The 3D Shape

The entire protein chain folds into a specific 3D shape — this is what we call the "structure" and what we visualize in tools like Mol*.

Key forces that drive folding:

| Force | Description |
|-------|-------------|
| **Hydrophobic effect** | Hydrophobic amino acids bury themselves in the core away from water — the main driver of folding |
| **Hydrogen bonds** | Polar groups form bonds that stabilize helices, sheets, and surface features |
| **Electrostatic interactions** | Opposite charges attract; same charges repel — salt bridges between charged residues |
| **Van der Waals forces** | Weak short-range attractions between atoms in close contact — important at tightly packed interfaces |
| **Disulfide bonds** | Covalent bonds between two cysteine residues — particularly important for extracellular proteins like antibodies |

> **The protein folding problem:** For decades, predicting 3D structure from sequence was considered one of the hardest problems in biology. A 100-amino-acid protein theoretically has 10^47 possible conformations — yet the protein finds its correct shape in milliseconds. AlphaFold2 (2021) largely solved this problem.

---

### Quaternary Structure — Multiple Chains Together

Many proteins function as complexes of multiple chains. For example:

- **Haemoglobin** = 4 chains (2 alpha + 2 beta subunits)
- **Antibodies** = 4 chains (2 heavy + 2 light chains)
- **PD-1:PD-L1** = 2 chains interacting at an interface

When we use BindCraft, we design a *new* chain (the binder, chain B) to interact with the target protein (PD-L1, chain A). The result is a two-chain quaternary complex.

---

## Visualizing Secondary Structure

In structure visualization tools like Mol*, secondary structure elements are shown as:

- **Ribbons / tubes** = alpha helices (usually red or purple)
- **Flat arrows** = beta strands (usually yellow)
- **Thin lines / coils** = loops

Here's what to expect when you look at PD-L1:
- PD-L1 has a **beta-sandwich** fold — two sheets of beta strands stacked face-to-face
- The binding interface for PD-1 is relatively flat and largely composed of beta-strand edges
- The designed binders tend to be **helix bundles** that press against this flat surface

---

## The Protein Folding Problem — A 50-Year Quest

For 50 years after Anfinsen's experiment, the question remained: *can we predict the 3D structure from the sequence?*

- **1970s–2000s:** Physics-based energy functions (Rosetta) — simulate the forces and find the lowest energy state. Slow, inaccurate for large proteins.
- **2018 (AlphaFold1):** DeepMind's first deep learning attempt — significantly better than physics-based methods.
- **2020 (CASP14):** AlphaFold2 achieves atomic accuracy on most proteins — median RMSD < 1Å for many targets. The problem is largely solved.

> **RMSD** (Root Mean Square Deviation) measures how far atoms are from their "true" positions. < 1Å is essentially indistinguishable from a crystal structure. For context, 1 Ångström = 10⁻¹⁰ meters — about the width of an atom.

---

## Activity: Draw a Protein

Sketch a simple protein with:
1. An N-terminus and C-terminus (label them)
2. At least one alpha helix (draw as a coiled ribbon)
3. At least one beta sheet (draw as a flat arrow)
4. A loop connecting them
5. Label which region you'd expect to be buried (hydrophobic core) vs. on the surface

Compare your sketch to how PD-L1 looks when you open it in Mol* next week.

---

> **Key takeaway:** Proteins have four levels of structure. The 3D shape (tertiary structure) is what determines function. Alpha helices and beta sheets are the two main building blocks. BindCraft designs binders that are mostly helical.

**Next:** [1.3 How Proteins Interact →](m1_03_protein_interactions.md)
