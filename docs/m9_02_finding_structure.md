# 9.2 Finding the Structure

**Time:** ~30 minutes hands-on

---

## Why the Structure Is Everything

BindCraft needs a target PDB file. Without a 3D structure, you can't define hotspot residues or design a binder. Your first job for the independent project is to find (or generate) a high-quality structure of your target's binding domain.

---

## Strategy 1: Search the PDB

Go to [rcsb.org](https://www.rcsb.org) and search your target protein name.

**What to look for:**

| Priority | What to look for | Why |
|----------|-----------------|-----|
| 1st | Complex structure (target + natural binding partner) | Shows you exactly where the interface is |
| 2nd | High-resolution apo structure (< 2.5 Å, no binding partner) | Clean reference structure for design |
| 3rd | Structure with a drug/peptide in the binding site | Shows the binding site but may be distorted |

**Evaluating multiple structures:**

If there are many structures (> 10 PDB entries), filter by:
- Resolution (< 2.5 Å preferred)
- Human protein (*Homo sapiens*)
- Extracellular domain only (if the target is a membrane protein)

Choose the highest-resolution structure of the domain you're targeting.

---

## Strategy 2: Use the AlphaFold Database

If no experimental structure exists (rare for major checkpoints but possible for newer targets):

1. Go to [alphafold.ebi.ac.uk](https://alphafold.ebi.ac.uk)
2. Search by protein name or UniProt ID
3. Download the AF2 structure (PDB format)

**Important caveats with AF2 structures:**
- Color by pLDDT immediately — only use regions where pLDDT > 70
- Transmembrane regions often have low pLDDT and should be removed
- AF2 structures of monomers don't tell you about binding interfaces

If using an AF2 structure, you'll need to determine hotspot residues from the literature (published mutagenesis data or complex structures of closely related proteins).

---

## Preparing the PDB File for BindCraft

Raw PDB files from the PDB often need cleaning before BindCraft can use them. Common issues:

### Issue 1: Multiple chains

A complex structure has both target and natural binding partner. You only want the target chain.

**Solution:** In PyMol or Mol*, select only the chain you want and save as a new PDB.

In PyMol (if available):
```python
# Load structure
fetch 5X8M

# Select only chain A (PD-L1)
select target, chain A

# Save just chain A
save PDL1_clean.pdb, target
```

In Python:
```python
# Read PDB, keep only ATOM lines for chain A
with open("5x8m.pdb", "r") as f, open("target_chain.pdb", "w") as out:
    for line in f:
        if line.startswith("ATOM") and line[21] == "A":
            out.write(line)
        elif line.startswith("END"):
            out.write(line)
```

### Issue 2: Non-standard residues, water, ligands

Remove HETATM records (heteroatoms = everything except standard amino acids: waters, ligands, metals).

```python
with open("5x8m.pdb", "r") as f, open("target_clean.pdb", "w") as out:
    for line in f:
        if line.startswith("ATOM") or line.startswith("END"):
            out.write(line)
        # Skip HETATM lines
```

### Issue 3: Multiple models (NMR structures)

NMR files (METHOD = NMR) contain multiple models. Keep only the first one (MODEL 1).

```python
with open("nmr.pdb", "r") as f, open("model1.pdb", "w") as out:
    in_model1 = False
    for line in f:
        if line.startswith("MODEL        1"):
            in_model1 = True
        elif line.startswith("ENDMDL") and in_model1:
            out.write("END\n")
            break
        if in_model1:
            out.write(line)
```

### Issue 4: Residue numbering gaps

Some PDB files have gaps in residue numbering (e.g., jump from residue 100 to 150 because a flexible loop wasn't resolved). BindCraft handles this, but verify that your hotspot residue numbers match the numbering in your PDB file.

**To check:** Open in Mol* and use the sequence panel to confirm that residue 54 (for example) is where you think it is.

---

## Notebook 09: Target Preparation

Notebook 09 (the independent project notebook) has a section for target preparation:

```python
# === STEP 1: Choose your PDB structure ===
TARGET_PDB_ID = "YOUR_PDB_ID_HERE"    # e.g., "7YNW"
TARGET_CHAIN  = "A"                    # which chain is your target
OUTPUT_NAME   = "MyTarget"

# === STEP 2: Download and clean ===
import requests

url = f"https://files.rcsb.org/download/{TARGET_PDB_ID}.pdb"
response = requests.get(url)
raw_pdb = response.text

# Keep only ATOM records for the target chain
clean_lines = []
for line in raw_pdb.split("\n"):
    if line.startswith("ATOM") and line[21] == TARGET_CHAIN:
        clean_lines.append(line)
clean_lines.append("END")

with open(f"{OUTPUT_NAME}.pdb", "w") as f:
    f.write("\n".join(clean_lines))

print(f"Saved {OUTPUT_NAME}.pdb")
print(f"Total ATOM records: {len(clean_lines) - 1}")
```

---

## Verifying Your Structure

After cleaning, verify before using as BindCraft input:

1. **Open in Mol*** — does it look right? Is it just the domain you want?
2. **Check residue numbering** — do the numbers match what's in the literature for your hotspot residues?
3. **Check pLDDT** (if AlphaFold) — is the domain you're targeting high-confidence?
4. **Check resolution** (if experimental) — is it < 3 Å? Higher resolution = more reliable hotspot identification

---

> **Key takeaway:** Finding and preparing the target structure is the first and most important step for your independent project. Use the highest-resolution complex structure if available; otherwise use the best apo structure plus literature data on the interface. Clean the PDB by keeping only the relevant chain and removing heteroatoms before loading into BindCraft.

**Next:** [9.3 Identifying Hotspot Residues →](m9_03_hotspots.md)
