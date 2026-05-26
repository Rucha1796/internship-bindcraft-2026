# 4.3 Running ColabFold

**Time:** ~20 minutes reading + notebook hands-on

---

## What Is ColabFold?

**ColabFold** is a fast, accessible implementation of AlphaFold2 that runs in Google Colab. It was developed by Milot Mirdita and colleagues to make AF2 accessible to researchers without access to expensive GPU servers.

Key differences from the original AF2:
- Uses **MMseqs2** instead of JackHMMMER for MSA generation — much faster (seconds vs. minutes)
- Runs on Google Colab's free GPU
- Accessible to anyone with a Google account
- Slightly lower accuracy in some edge cases (MSA depth can be less)

> **Why you're using it:** ColabFold lets you run a real protein structure prediction in minutes, on a laptop, for free. In this internship, Notebook 02 walks you through predicting the PD-L1 structure and reading the outputs.

---

## What ColabFold Produces

After a ColabFold run, you get:

### 1. Structure Files (.pdb)

Five ranked models, named:
```
{protein}_unrelaxed_rank_001_alphafold2_model1_seed_000.pdb
{protein}_unrelaxed_rank_002_alphafold2_model2_seed_000.pdb
...
```

- `rank_001` is the highest-confidence model (by pTM)
- "Unrelaxed" means the geometry hasn't been energy-minimized yet (slight bond length errors; fine for visualization)

### 2. JSON Confidence File

```
{protein}_scores_rank_001_...json
```

Contains per-residue pLDDT scores and the PAE matrix as a Python-readable dictionary.

### 3. Coverage Plot

A graphic showing how many sequences were found in the MSA for each position. Regions with low MSA coverage may have lower prediction confidence.

### 4. PAE Plot

A heatmap of the Predicted Aligned Error matrix for the best model.

---

## Reading the ColabFold Output

### The pLDDT Graph

ColabFold automatically generates a graph of pLDDT vs. residue position. Reading this:
- **High plateau** (> 80) = well-structured domain
- **Dips** = flexible loops, disordered regions, or N/C termini

For PD-L1's extracellular domain (residues ~19–238), you should see mostly high pLDDT. The transmembrane and cytoplasmic regions (if included) will show lower confidence because they're not the folded domain.

### The PAE Plot

The 2D heatmap (described in detail in Module 4.2). For PD-L1 alone (single chain), you're looking at:
- A diagonal block of low PAE (dark blue) = AF2 is confident in the internal structure
- The plot should look like a single dark-blue block for a well-folded protein

### pTM Score

Printed in the notebook output. For PD-L1's extracellular domain, expect pTM > 0.85 — it's a very well-characterized structure.

---

## Notebook 02: Step-by-Step

Here's what you'll do in the ColabFold notebook:

**Cell 1: Setup**
```python
# Install ColabFold (takes ~2 minutes)
```

**Cell 2: Input your sequence**

You'll paste in the PD-L1 sequence:
```
MRIFAVFIFMTYWHLLNAFTVTVPKDLYVVEYGSNMTIECKFPVEKQLDLAALIVYWEMEDKNIIQFVHGEEDLKVQHSSYRQRARLLKDQLSLGNAALQITDVKLQDAGVYRCMISYGGADYKRITVKVNAPYNKINQRILVDPQRLYLVHNQKLCILHQKFTTL
```

> This is the extracellular domain of human PD-L1 (residues 19–238). The signal peptide (residues 1–18) is removed.

**Cell 3: Run ColabFold**
```python
# This takes ~3–5 minutes on Colab GPU
# Progress prints to the output
```

**Cell 4: Visualize**

py3Dmol colors the structure by pLDDT automatically. You'll see the beta-sandwich fold in blue.

**Cell 5: Download**

The notebook provides a button to download the PDB and JSON files to your computer.

---

## What to Look For

After running ColabFold on PD-L1:

1. **pTM score** — should be > 0.85
2. **pLDDT plot** — should be mostly > 80 for the structured domain
3. **3D structure** — should look like a compact beta-sandwich (matches 5C3T from X-ray)
4. **PAE plot** — single dark-blue block (confident internal placement)

**Compare to the experimental structure:**
- Open your ColabFold PDB and 5C3T side by side in Mol*
- They should look nearly identical (RMSD typically < 1 Å for a well-studied protein like PD-L1)

---

> **Key takeaway:** ColabFold makes AlphaFold2 accessible on a free Colab GPU. It takes minutes and produces five ranked structure models plus pLDDT and PAE confidence outputs. For PD-L1, the prediction should closely match the experimental X-ray structure.

**Next:** [4.4 MSAs and Evolution →](m4_04_msa.md)
