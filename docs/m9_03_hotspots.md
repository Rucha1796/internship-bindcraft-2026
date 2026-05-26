# 9.3 Identifying Hotspot Residues

**Time:** ~30 minutes reading + hands-on

---

## What Are Hotspot Residues?

Hotspot residues are the amino acids on your target protein that you instruct BindCraft to design the binder against. They define *where* on the protein surface the binder should bind.

Choosing the right hotspots is one of the most important decisions you'll make for the independent project. Poor hotspot selection can lead to:
- Binders that don't block the desired function
- Binders that target an inaccessible site (buried in a membrane, for example)
- Binders that are computationally successful but functionally irrelevant

Good hotspot selection leads to:
- Binders that compete with the natural binding partner
- Binders that block the therapeutic target's function
- Designs that are relevant to the biology you care about

---

## Method 1: Use a Complex Structure (Gold Standard)

If a structure of your target in complex with its natural binding partner exists in the PDB:

1. **Load the complex in Mol***
2. **Find the interface residues** — which residues on the target are within 4–5 Å of the binding partner?
3. **Identify the "hot" residues** — which interface residues make the most specific contacts (H-bonds, salt bridges)? These are your hotspots.

### Finding Interface Residues in Mol*

In Mol*, use the Selection mode:

1. Load the complex (e.g., `5X8M.pdb` for PD-L1/PD-1)
2. Go to **Select → Select Within** → set radius to 5 Å → select around chain B (the binding partner)
3. This selects all chain A residues within 5 Å of chain B — these are your interface residues
4. Note their residue numbers

From this list, choose the **3–6 most central residues** at the interface core. Avoid residues at the very edge of the interface (they contribute less to binding energy).

### Using Distance Measurements

For each candidate hotspot residue:
1. Measure distances from it to the nearest atoms in the binding partner
2. Residues < 4 Å = direct contact (hydrogen bond, van der Waals)
3. Residues 4–6 Å = indirect contact (important for shape but not direct H-bond)

Prioritize direct contact residues.

---

## Method 2: Published Mutagenesis Data

Many important protein-protein interactions have been studied by **alanine scanning mutagenesis** — researchers systematically replace each interface residue with Alanine and measure how much binding decreases.

Residues where Ala substitution causes > 2-fold reduction in binding are "hot spots" in the thermodynamic sense — they contribute significantly to binding energy.

**Search PubMed for:**
- `[target name] alanine scanning`
- `[target name] mutagenesis binding`
- `[target name] interface residues`

These papers tell you exactly which residues matter most. Choose those as your BindCraft hotspots.

---

## Method 3: AlphaFold Multimer + Interface Analysis

If no complex structure exists, you can predict the interface:

1. Use **AlphaFold Multimer** (or AlphaFold3) to predict the complex structure of your target + its natural binding partner
2. Analyze the predicted interface (focus on low-PAE residue pairs in the off-diagonal block)
3. Choose hotspots from the predicted interface

**Caveat:** Predicted interfaces are less reliable than experimental ones. Cross-reference with mutagenesis data if available.

---

## Hotspot Selection Criteria

| Criterion | Good hotspot | Bad hotspot |
|-----------|-------------|------------|
| Surface exposure | Yes — accessible to a binder | No — buried in protein core |
| Location | At or near the natural interface | Far from natural binding site |
| Conservation | Conserved across species | Hypervariable (may be species-specific) |
| Contact type | H-bond donors/acceptors, specific contacts | Only van der Waals (less specific) |
| Number | 3–6 hotspots | < 2 (too sparse) or > 10 (too constrained) |

---

## Case Study: How PD-L1 Hotspots Were Chosen

For reference, here's the reasoning behind the PD-L1 hotspots (54, 56, 111, 115, 123):

**From the 5X8M structure (PD-L1/PD-1 complex):**

| Residue | Contact type | Binding partner residue |
|---------|-------------|------------------------|
| Gln54 | H-bond (amide) | PD-1 Asn66 |
| Tyr56 | H-bond + hydrophobic | PD-1 Pro89 |
| Asp111 | Salt bridge | PD-1 Arg139 |
| Tyr115 | Hydrophobic stacking | PD-1 Ile126 |
| Tyr123 | H-bond (hydroxyl) | PD-1 Gly124 |

All five are directly at the PD-1 binding interface. Three are tyrosines (useful for both hydrophobic and polar contacts). All are solvent-exposed.

---

## Your Hotspot Decision

After your analysis, document your hotspot selection:

**Target:** [protein name]  
**Complex structure used:** [PDB ID or "AF2 prediction" or "mutagenesis data only"]

| Hotspot residue | Amino acid | Justification |
|----------------|-----------|--------------|
| | | |
| | | |
| | | |
| | | |
| | | |

**Alternative hotspot sets to consider:**
If your first choice doesn't produce good designs (you can check after a preliminary run of 20–30 trajectories), consider:
- Shifting 1–2 hotspots to adjacent residues
- Reducing the number of hotspots to 3 (less constrained)
- Targeting a slightly different face of the interface

Share your hotspot selection with your mentor before running BindCraft.

---

> **Key takeaway:** Hotspot selection is the most important design decision for your independent project. The gold standard is using a complex structure to identify direct interface contacts, validated by mutagenesis data. Choose 3–6 solvent-exposed residues at the center of the natural binding interface. Document your reasoning — you'll explain it in your final presentation.

**Next:** [9.4 Running BindCraft →](m9_04_running.md)
