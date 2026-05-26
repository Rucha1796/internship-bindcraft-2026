# 1.3 How Proteins Interact

**Time:** ~25 minutes reading

---

## Proteins Don't Work Alone

Almost nothing in a cell happens because of a single protein acting in isolation. Enzymes bind substrates. Antibodies grip pathogens. Receptors catch signals floating past them. Regulatory proteins sit on DNA and control gene expression.

Proteins are social molecules. Their function is built into *how* they touch other molecules — and *where*.

---

## What Is a Binding Interface?

When two proteins bind, they don't fuse together permanently. They come together, spend time in contact, then part ways. The surface where they touch is called the **binding interface**.

A typical protein-protein interface:
- Is **400–2000 Ų** in area (for reference: a thumbnail is ~1 cm² = 100,000,000 Ų)
- Involves **10–30 amino acids** from each protein
- Is often **complementary** — a bump on one protein fits a groove on the other
- Is **not flat** — it has bumps, grooves, and pockets shaped by the protein's 3D structure

> **Key insight:** BindCraft's entire job is to design a new protein whose surface is *complementary* to a specific region of PD-L1's surface.

---

## The Forces That Hold Proteins Together

Protein-protein interactions are held together by **non-covalent bonds** — forces weaker than the covalent bonds that hold atoms together, but working in large numbers to create a stable complex.

### 1. Hydrogen Bonds

A hydrogen bond forms when a hydrogen atom (H) attached to an electronegative atom (nitrogen or oxygen) is attracted to another electronegative atom nearby.

```
N–H ··· O
```

- Energy: ~1–5 kcal/mol each
- Directionality matters: the angle must be right (~120–180°)
- Specificity: hydrogen bonds "prefer" certain partners

At a protein interface, backbone NH and C=O groups, plus side chains of Ser, Thr, Asn, Gln, Lys, Arg, and Asp/Glu, all participate.

### 2. Hydrophobic Effect

Non-polar amino acids (Ile, Leu, Val, Phe, Trp, Met) don't like water. When two proteins come together, they can bury non-polar residues *away* from water — which is energetically favorable.

This is why many protein interfaces have a **hydrophobic core** surrounded by polar contacts at the edges.

> **Intuition:** Imagine drops of oil in water. They coalesce — not because oil "wants" to be together, but because water "wants" to push them out.

### 3. Electrostatic Interactions (Salt Bridges)

Oppositely charged amino acids attract each other:
- Lysine (K) and Arginine (R) are positively charged
- Aspartate (D) and Glutamate (E) are negatively charged

A salt bridge between K and E is like a tiny ionic bond at the interface. It can be very stabilizing — or destabilizing if charges repel.

### 4. Van der Waals Forces

Short-range attractive forces between any two atoms when they are very close but not touching. Each individual van der Waals contact is tiny (~0.1 kcal/mol), but interfaces have *hundreds* of them. In total, they contribute significantly.

This is also why **shape complementarity** matters — surfaces need to be close enough for van der Waals contacts across the entire interface.

---

## Affinity: How Tight Is "Tight"?

We measure binding strength with the **dissociation constant, Kd**.

Kd represents the concentration of protein needed to achieve 50% binding. Low Kd = tight binding.

| Kd value | Classification | Example |
|----------|---------------|---------|
| **< 1 nM** | Very tight | Biotin-streptavidin (0.1 fM) |
| **1–100 nM** | Typical therapeutic | Therapeutic antibodies |
| **100 nM–1 µM** | Moderate | Many hormone-receptor pairs |
| **> 1 µM** | Weak | Transient signaling interactions |

For PD-L1 inhibitors to work therapeutically, the binder needs to compete with PD-1, which binds PD-L1 with Kd ≈ 8 nM. Your designed binder needs Kd in the same range — ideally < 10 nM.

---

## Specificity vs. Affinity

These are related but different concepts.

**Affinity** = how tightly a binder binds its target
**Specificity** = how much more tightly it binds the target vs. everything else in the body

A binder that sticks to PD-L1 with Kd = 1 nM but also sticks to 50 other proteins isn't useful as a drug — it would have terrible off-target effects.

BindCraft designs binders against a specific target site (the hotspot residues). Specificity testing is done experimentally later.

---

## Why Interfaces Are Hard to Design

Designing a new protein to bind a target surface is like carving a key to fit a lock you've never touched — using only a photograph of the lock.

Challenges:
1. **Shape matching**: The binder surface must be geometrically complementary to the target surface
2. **Chemical matching**: Polar atoms on one side must face polar atoms on the other side; hydrophobic patches must be buried
3. **Rigidity vs. flexibility**: The binder must be rigid enough to maintain its designed shape in solution
4. **Thermodynamics**: The entropy cost of bringing two proteins together must be offset by the enthalpy of the contacts formed

AlphaFold2 lets us evaluate these factors computationally before ordering a single experiment.

---

> **Key takeaway:** Proteins bind through a combination of hydrogen bonds, hydrophobic burial, electrostatics, and van der Waals contacts. The quality of the interface — how many contacts, how well the surfaces match — determines binding affinity. BindCraft is designed to optimize all of these simultaneously.

**Next:** [1.4 The Immune System & PD-L1 →](m1_04_immune_system_pdl1.md)
