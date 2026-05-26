# 1.4 The Immune System & PD-L1

**Time:** ~45 minutes reading + activity

---

## Your Immune System as Border Control

Think of your immune system as an army that patrols your body looking for invaders — viruses, bacteria, abnormal cells. One of the most important soldiers in this army are **T cells** (T lymphocytes).

T cells are remarkably sophisticated. They can recognize specific molecular "flags" on the surface of cells and decide: *is this cell healthy, or should it be destroyed?*

---

## The PD-1 / PD-L1 Checkpoint

The immune system needs off-switches. Without them, T cells would attack healthy tissue — this is what happens in autoimmune diseases. One of the most important off-switches is the **PD-1 / PD-L1 checkpoint**.

Here's how it works in a healthy body:

```
T cell
  │
  ├── PD-1 receptor (on T cell surface)
  │        │
  │        └── binds to PD-L1 (on healthy cells)
  │
  └── Signal: "This cell is healthy — don't attack"
```

**PD-1** (Programmed Death receptor 1) sits on the surface of T cells. When it binds to **PD-L1** (Programmed Death Ligand 1), it sends a "stand down" signal — the T cell becomes less aggressive.

PD-L1 is expressed on many healthy tissues to protect them from immune attack. This is completely normal.

---

## How Tumors Hijack This System

Cancer cells are clever. Many tumors have evolved to express **high levels of PD-L1** on their surface. By mimicking a healthy tissue signal, they trick T cells into standing down:

```
Tumor cell
  │
  ├── Overexpresses PD-L1
  │        │
  │        └── binds PD-1 on nearby T cells
  │
  └── Signal: "Nothing to see here — don't attack me"
```

This is called **immune evasion** — the tumor hides from the immune system using the body's own off-switch.

> **Real numbers:** PD-L1 overexpression is found in ~30–50% of non-small cell lung cancers, ~20–30% of melanomas, and many other tumor types.

---

## Immunotherapy: Releasing the Brakes

The breakthrough insight was: *what if we block the PD-1/PD-L1 interaction?* If we prevent the "stand down" signal, T cells stay activated and attack the tumor.

This is the basis of **checkpoint immunotherapy** — one of the biggest advances in cancer treatment in decades.

**Pembrolizumab (Keytruda)** — approved by the FDA in 2014 — is an antibody that binds to PD-1 and blocks it from interacting with PD-L1. This keeps T cells active against the tumor.

> **Clinical impact:** Keytruda has shown durable responses (some patients in remission for 10+ years) in melanoma, lung cancer, and many other cancers. It generated ~$25 billion in revenue in 2024 — the best-selling drug in the world.

---

## Why We Design Binders Against PD-L1

Keytruda (and similar drugs like Opdivo) are antibodies — large, complex molecules that are expensive to manufacture and can only be given by injection.

A computationally designed binder against PD-L1 could potentially be:
- **Smaller** — easier to manufacture, possibly oral administration
- **More specific** — designed to target exactly the right site
- **Faster to develop** — designed in silico rather than through animal immunization

In this internship, you'll use BindCraft to design proteins that bind the same site on PD-L1 that PD-1 naturally binds — and that Keytruda is designed to protect.

---

## The PD-L1 Structure

PD-L1 is a type I transmembrane protein. The part we care about is its **extracellular domain** — the piece that sticks outside the cell and interacts with PD-1.

The extracellular domain has an **IgV-like fold** (similar to antibody variable domains) — a beta-sandwich structure with two layers of beta strands.

The binding interface with PD-1 is relatively flat and hydrophobic — which is actually favorable for designing protein binders (BindCraft works better on hydrophobic surfaces).

**Key PDB structures:**
- **5C3T** — PD-L1 extracellular domain with small molecule inhibitor BMS-202 in the binding site (useful for seeing the exact binding groove)
- **4ZQK** — PD-1:PD-L1 complex (shows the natural interface)
- **8AOK** — Used in the BindCraft pregenerated dataset for this internship

---

## The Interface Residues We Target

The hotspot residues for BindCraft in this internship are:

| Residue | Position | Role |
|---------|----------|------|
| Tyr56 | 56 | Core hydrophobic contact |
| Glu54 | 54 | H-bond donor |
| Ile54 | 111 | Hydrophobic core |
| Met115 | 115 | Hydrophobic packing |
| Tyr123 | 123 | Aromatic stacking |

These are the residues that PD-1 makes the closest contacts with — and the ones we instruct BindCraft to design against.

---

## Activity: Draw the Circuit

On paper, draw the PD-1/PD-L1 immune circuit in two scenarios:

**Scenario 1 — Normal tissue:**
T cell → PD-1 → binds PD-L1 on healthy cell → T cell stands down → healthy cell survives

**Scenario 2 — Tumor evasion:**
T cell → PD-1 → binds PD-L1 on tumor cell → T cell stands down → tumor survives

**Scenario 3 — With a designed binder:**
Designed binder → blocks PD-L1 → PD-1 can't bind → T cell stays active → tumor attacked

Label which step your designed protein intervenes in Scenario 3.

---

> **Key takeaway:** PD-L1 is a protein expressed by cancer cells to shut off T cells. Blocking the PD-1/PD-L1 interaction reactivates the immune system against the tumor. In this internship, you design proteins that do exactly that.

**Next:** [1.5 Antibodies vs. Designed Binders →](m1_05_antibodies_vs_binders.md)
