# 2.2 The Protein Data Bank

**Time:** ~20 minutes reading + hands-on

---

## What Is the PDB?

The **Protein Data Bank (PDB)** is the world's single repository for experimentally determined three-dimensional structures of proteins, nucleic acids, and their complexes.

- Founded in **1971** with just 7 structures
- Today: **>220,000 structures** and growing
- Free to access at [rcsb.org](https://www.rcsb.org)
- Every structure has a **4-character PDB ID** (e.g., `5C3T`)

When a scientist determines a new protein structure by X-ray crystallography, cryo-EM, or NMR, they are required to deposit it in the PDB before their paper is published. This makes all experimental structures publicly available.

> The PDB is one of science's greatest success stories in open data — it has enabled thousands of discoveries across biology, chemistry, and medicine.

---

## How to Search the PDB

### By PDB ID (if you know it)

Go to [rcsb.org](https://www.rcsb.org) and type the ID directly into the search bar.

**PD-L1 structures you'll use in this internship:**

| PDB ID | What it is | Resolution | When to use |
|--------|-----------|-----------|-------------|
| `5C3T` | PD-L1 extracellular domain | 1.58 Å | Reference structure (default) |
| `5X8M` | PD-L1 in complex with natural ligand PD-1 | 2.90 Å | Understanding the natural interface |
| `8AOK` | PD-L1 with a designed miniprotein binder | 2.10 Å | Example of what you're trying to create |

### By Protein Name

Type "PD-L1" or "CD274" (the gene name) into the search bar. Use filters on the left side to narrow by:
- Organism (human = *Homo sapiens*)
- Method (X-ray, cryo-EM)
- Resolution (lower Å = better)

---

## What's on a PDB Entry Page

When you open a structure like `5C3T`, you'll see:

1. **Structure view** — interactive 3D viewer (you can rotate it right there in the browser)
2. **Entry information** — authors, year, experimental method, resolution
3. **Sequence** — the amino acid sequence of each chain
4. **Ligands** — any small molecules co-crystallized with the protein
5. **FASTA download** — download the sequence as text
6. **Download** → **PDB format** — the actual atomic coordinates file

---

## PDB File Format

When you download a structure, you get a `.pdb` file — a plain text file that lists every atom's coordinates.

A simplified example:

```
ATOM      1  N   MET A   1      11.281  86.699  94.383  1.00 35.88           N
ATOM      2  CA  MET A   1      11.736  85.359  93.875  1.00 29.23           C
ATOM      3  C   MET A   1      13.228  85.248  93.566  1.00 22.40           C
ATOM      4  O   MET A   1      13.773  86.088  92.916  1.00 23.14           O
ATOM      5  CB  MET A   1      10.855  85.003  92.660  1.00 37.24           C
```

Each `ATOM` line contains:
- Atom serial number
- Atom name (N = backbone nitrogen, CA = alpha carbon, C = carbonyl carbon, O = carbonyl oxygen, CB = beta carbon)
- Residue name (MET = methionine)
- Chain identifier (A = chain A)
- Residue sequence number
- **X, Y, Z coordinates in Ångströms** — these are the actual positions in 3D space
- Occupancy
- B-factor (measure of atomic motion/uncertainty)
- Element symbol

The coordinates are in **Ångströms** (Å). 1 Å = 10⁻¹⁰ meters. A hydrogen atom is about 1 Å in diameter.

---

## Chains, Residues, and Atoms

Every PDB structure has:

- **Chains** — labeled A, B, C, etc. Each chain is usually one polypeptide. PD-L1 in complex with a binder will have at least two chains.
- **Residues** — the amino acids, numbered sequentially. Residue 54 on chain A = the 54th amino acid in that chain.
- **Atoms** — every heavy atom (non-hydrogen) is listed. A single amino acid has 4–15 atoms depending on its size.

> **Why this matters:** When you set up BindCraft for PD-L1, you specify `"chains": "A"` — this tells BindCraft that chain A is the target. The hotspot residues (`54,56,111,115,123`) refer to residue numbers within chain A.

---

## Hands-On Activity: Find PD-L1 on the PDB

1. Go to [rcsb.org](https://www.rcsb.org)
2. Search for `5C3T`
3. On the entry page:
   - What organism is this from?
   - What experimental method was used?
   - What is the resolution?
   - How many chains are in this structure?
   - How many residues total?
4. Click "Download" → download the PDB format file
5. Open it in a text editor (not Word) and find the lines for residue 54 on chain A

---

> **Key takeaway:** The PDB is the universal repository of protein structures. Every structure is identified by a 4-character ID. You'll download PDB files to use as input for BindCraft — the target's 3D coordinates are the starting point for everything.

**Next:** [2.3 Homology Modeling to Deep Learning →](m2_03_homology_to_dl.md)
