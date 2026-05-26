# 3.2 Reading a Structure

**Time:** ~25 minutes reading

---

## From Sequence to Structure: What You're Actually Seeing

When you look at a protein structure in Mol*, you're looking at:
- The result of months (sometimes years) of experimental work
- The positions of every heavy atom (non-hydrogen) in the protein
- A snapshot of the protein in one conformation — the crystal form

Understanding *how to read* a structure is a skill. This page gives you the vocabulary.

---

## Secondary Structure Elements

### Alpha Helix (α-helix)

In Mol* cartoon view, shown as a **coiled ribbon** or thick tube (usually purple/pink in secondary structure coloring).

Characteristics:
- 3.6 residues per turn
- Backbone NH groups hydrogen-bond to backbone C=O groups 4 residues ahead in sequence
- A helix of 10 residues spans ~15 Å
- The side chains point *outward* from the helix axis

Most BindCraft designs are helical binders — alpha helices are compact, stable, and easy for ProteinMPNN to design sequences for.

### Beta Sheet (β-sheet)

In Mol* cartoon view, shown as **flat arrows**.

Characteristics:
- Adjacent strands aligned side by side, hydrogen bonds running between strands
- Can be **parallel** (strands run same direction) or **antiparallel** (strands alternate direction)
- Very rigid — beta sheets resist deformation

PD-L1's extracellular domain is a **beta-sandwich**: two beta sheets stacked face-to-face, like a sandwich. This gives it a very rigid, flat surface — perfect for a binder to sit against.

### Loops and Turns

Everything that isn't a helix or sheet. Shown as thin lines or tubes in Mol*.

- Connect helices and sheets
- Often at the surface, exposed to solvent
- Can be flexible (high B-factor) or rigid
- CDR loops in antibodies are the key binding elements

---

## B-Factors: Measuring Atomic Mobility

In X-ray structures, every atom has a **B-factor** (also called temperature factor or Debye-Waller factor). It measures how much that atom "wobbles" in the crystal.

- **Low B-factor** = atom is well-ordered, position is precise
- **High B-factor** = atom is mobile, position is uncertain

In Mol*, you can color by B-factor (blue = low = ordered, red = high = mobile).

> **Connection to pLDDT:** AlphaFold2's pLDDT score is conceptually similar to the inverse of B-factor — high pLDDT (confident, well-predicted) corresponds to the same regions that have low B-factors in crystal structures.

---

## Resolution

Structure quality is primarily described by **resolution** in Ångströms (Å):

| Resolution | Quality | What you can see |
|-----------|---------|-----------------|
| < 1.5 Å | Excellent | Individual atoms clearly; bond lengths accurate |
| 1.5–2.5 Å | Good | Most side chains visible; typical for most studies |
| 2.5–3.5 Å | Moderate | Main chain clear; side chains less certain |
| > 3.5 Å | Low | Overall fold visible; details uncertain |

5C3T is 1.58 Å — an excellent, well-determined structure. This is why it's a good template for BindCraft.

---

## Chains and Crystal Contacts

Almost all structures have multiple chains in the asymmetric unit. Some chains are:
- **Biological** — actually present in the functional complex (like PD-L1 chain A + binder chain B)
- **Crystal contacts** — artifacts of how the protein packed in the crystal

To tell the difference, look for:
- Large buried interface area (> 1000 Ų) = likely biological
- Small contacts (< 400 Ų) = likely crystal artifact

PDB pages will often indicate the "biological assembly" separately from the asymmetric unit. Always check which one you're looking at.

---

## How to Read the Sequence Panel

In Mol*'s sequence panel (right side), you see:

```
Chain A:  M  Q  I  F  G  L  M  V  G  G  V  I  A  L  ...
          1  2  3  4  5  6  7  8  9  10 11 12 13 14
```

- Each letter is a one-letter amino acid code (see Glossary)
- Numbers are residue sequence numbers
- Click any residue to highlight it in the 3D view
- Residues colored by secondary structure: helices in one color, sheets in another

> **Important:** Residue numbers in PDB files match the original protein sequence numbering, which may have gaps (some residues not ordered in the crystal). Always use residue *numbers*, not just position in the chain.

---

## Activity: Three-Question Structure Analysis

Open `5C3T` in Mol*. Answer these questions, writing down your observations:

**Question 1: What fold is PD-L1?**
Color by secondary structure. Count the approximate number of beta strands you can identify. How do they relate to each other? (Hint: look for the sandwich arrangement.)

**Question 2: Where is the surface?**
Switch to "Surface" representation. Look for flat regions vs. grooves. The PD-1 binding site is on one face of the beta-sandwich. Can you identify a relatively flat, accessible face?

**Question 3: Where are the flexible regions?**
Switch back to Cartoon. Look for thin, wiggly lines (loops) vs. thick, structured elements. Where are the most disordered-looking parts? (These are usually the N- and C-termini, plus loop regions.)

---

> **Key takeaway:** Reading a protein structure means recognizing secondary structure elements (helices, sheets, loops), understanding resolution as a measure of data quality, and being able to find specific residues of interest. Practice this with PD-L1 — you'll need these skills for Module 3.3.

**Next:** [3.3 The PD-L1 Binding Interface →](m3_03_pdl1_interface.md)
