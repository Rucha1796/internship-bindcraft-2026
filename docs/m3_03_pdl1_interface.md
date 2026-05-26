# 3.3 The PD-L1 Binding Interface

**Time:** ~30 minutes hands-on

---

## What We're Looking At

In Module 1.4, you learned that PD-L1 sends a "stand down" signal to T cells by binding PD-1. The key to blocking this interaction is understanding *exactly where* PD-1 touches PD-L1 — the binding interface.

In this module, you'll:
1. Load the PD-L1/PD-1 complex structure (5X8M)
2. Identify the contact residues at the interface
3. Understand *why* residues 54, 56, 111, 115, and 123 were chosen as BindCraft hotspots

---

## The Natural PD-1/PD-L1 Interface

The structure **5X8M** shows PD-L1 (chain A) in complex with PD-1 (chain B). This is the natural interaction that Keytruda blocks — and that you're designing a binder to block.

### Opening 5X8M in Mol*

1. Go to [molstar.org/viewer](https://molstar.org/viewer)
2. Open PDB ID `5X8M`
3. The complex has two chains — color by chain to distinguish them

**What you should see:**
- Two proteins sitting face-to-face
- A relatively flat interface between them (PD-L1 beta-sandwich face touching PD-1's loops)
- The contact surface looks like two puzzle pieces fitting together

---

## Identifying the Interface Residues

The hotspot residues for BindCraft (54, 56, 111, 115, 123 on chain A of PD-L1) were chosen because they:
1. Are located at the PD-L1/PD-1 binding interface
2. Make direct contacts with PD-1
3. Are central to the binding epitope — blocking these should disrupt PD-1 binding

### Finding Each Hotspot in Mol*

Use the sequence panel to highlight each hotspot residue. For each one, observe:
- Is it on the surface or buried?
- What secondary structure element is it in (helix, sheet, loop)?
- Does it appear close to PD-1 chain B?

| Residue | Amino acid | Location | Key contacts |
|---------|-----------|----------|-------------|
| **54** | Gln (Q54) | Loop between strands | H-bond donor to PD-1 |
| **56** | Tyr (Y56) | Beta strand edge | Hydrophobic + H-bond |
| **111** | Asp (D111) | Strand-loop junction | Electrostatic contact |
| **115** | Tyr (Y115) | Beta strand | Key hydrophobic contact |
| **123** | Tyr (Y123) | Surface loop | Stacking interaction |

> **Pattern:** Three of the five hotspot residues are Tyrosine (Y). Tyrosine is special — it has both a hydrophobic aromatic ring AND a polar hydroxyl group. This makes it excellent for bridging hydrophobic cores to polar contacts at interfaces. Many natural protein-protein interfaces are rich in tyrosine.

---

## The Design Approach: Hit the Hotspots

When you set up BindCraft with `target_hotspot_residues = "54,56,111,115,123"`, you're telling it:

> "Design a binder that specifically makes contacts with these five residues on PD-L1."

BindCraft's hallucination loss function rewards designs that:
- Position the binder close to these residues
- Form contacts (especially H-bonds) at the hotspot positions
- Don't clash with the rest of the PD-L1 surface

This ensures the designed binder targets the *same region* as PD-1 — meaning it has the best chance of competing with PD-1 and disrupting the immune checkpoint.

---

## Visualizing a Designed Binder on PD-L1

For comparison, load **8AOK** — this is PD-L1 with an actual designed miniprotein binder already solved by X-ray crystallography.

1. Open `8AOK` in Mol*
2. Color by chain — you'll see PD-L1 (chain A) and the designed binder (chain B)
3. Look at the binder: it's much smaller than PD-L1
4. Rotate to see the interface — notice how the binder sits on the flat beta-sandwich face

**Compare the 8AOK binder's interface residues with the hotspots (54, 56, 111, 115, 123):**
- Use the sequence panel to check where the binder contacts PD-L1
- Are the hotspot residues at the center of the interface?

This is your benchmark: the 8AOK binder shows you what a successful design looks like. Your BindCraft designs should produce similar interfaces.

---

## The Epitope vs. Paratope Distinction

- **Epitope** — the region on the *target* (PD-L1) that is recognized by the binder
- **Paratope** — the region on the *binder* that contacts the target

When you analyze BindCraft outputs, you'll look at:
- Which PD-L1 residues does the binder contact? (Do they include the hotspots?)
- Which binder residues make the contacts? (Are they well-designed for those contacts?)

A good design has a complementary paratope for the PD-L1 epitope centered on the five hotspot residues.

---

## Activity: Interface Mapping

Using 5X8M in Mol*:

1. **Identify the interface.** Switch to Surface representation. Color PD-L1 and PD-1 different colors. Where is the contact area?

2. **Find the hotspots.** Highlight residues 54, 56, 111, 115, 123 using the sequence panel. Are they at the interface?

3. **Measure the interface width.** Use the Distance measurement tool. How wide is the contact region (approximately)?

4. **Compare to 8AOK.** Open 8AOK. Is the designed binder covering the same face of PD-L1 as PD-1 does in 5X8M?

Write a 3–4 sentence description of what you found. You'll reference this observation in your final Design Recommendation Report.

---

> **Key takeaway:** The PD-L1 binding interface is a relatively flat face of the beta-sandwich domain, centered on five key hotspot residues (54, 56, 111, 115, 123). BindCraft designs binders that target exactly this region. Visualizing the natural PD-1/PD-L1 interface gives you an intuition for what a successful design should look like.

**Next:** [Module 4: AlphaFold2 →](m4_01_alphafold2.md)
