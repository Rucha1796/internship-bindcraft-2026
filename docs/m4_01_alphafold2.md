# 4.1 What AlphaFold2 Does

**Time:** ~45 minutes reading + video

---

## The Big Picture

AlphaFold2 (AF2) is a deep learning model that takes a protein sequence as input and outputs a 3D structure — along with a confidence score for every part of the structure.

Before AF2, the best computational methods had a median error of ~5–6 Ångströms on hard targets at CASP. AF2 achieved < 1Å on many targets — essentially matching experimental structures. The scientific community called it "a once in a generation advance."

> **Watch:** DeepMind's 5-minute explainer video on AlphaFold2 before reading further. Search "DeepMind AlphaFold explained" on YouTube.

---

## The Core Insight: Evolution as a Teacher

The key innovation in AF2 is recognizing that **evolution has already solved the structure problem — we just need to read the data.**

Here's the logic:

1. If two residues are in physical contact in the folded protein, mutations at one position tend to be compensated by mutations at the other (co-evolution)
2. By collecting thousands of related sequences (a Multiple Sequence Alignment, or MSA) and analyzing which positions co-vary, we can infer which residues are spatially close
3. These inferred contacts, combined with structural templates, give AF2 a detailed picture of the protein's geometry before it even builds the 3D structure

> **Analogy (from the NRC course):** Imagine you're watching a team of people always move together — whenever Alice takes a step left, Bob takes a step right. You'd conclude they're connected somehow (maybe holding hands). Co-evolution is the protein equivalent — if residue 14 is always mutated when residue 87 is mutated, they're probably touching.

---

## How AF2 Works: Three Stages

### Stage 1 — Preprocessing: Build the MSA

AF2 searches protein sequence databases (UniRef90, MGnify) to find all known proteins homologous to the input sequence. These are aligned into a **Multiple Sequence Alignment (MSA)** — a matrix where each row is a related sequence and each column is an aligned position.

From the MSA, AF2 also identifies structural templates (PDB entries with similar sequences) and builds a **pair representation** — a matrix encoding information about every pair of positions (i, j).

### Stage 2 — The Evoformer: Reasoning About Pairs

The **Evoformer** is AF2's core attention architecture. It alternates between:

- **MSA transformer:** Updates the MSA representation by asking "given what I know about evolution, which pairs of positions should be co-evolving?"
- **Pair transformer:** Updates the pair representation based on the MSA transformer's findings

They inform each other iteratively. The pair representation gradually builds up a picture of which residues are close in 3D space — essentially a predicted contact map, but richer and more nuanced than anything previous tools could produce.

> **The attention mechanism:** "Attention" is a neural network operation that asks: *which other elements in the input are most relevant to understanding this element?* For residue i in the MSA, attention looks across all other positions j and weights them by how informative they are for predicting i's environment. This is how AF2 captures long-range dependencies that classical methods missed.

### Stage 3 — The Structure Module: Building 3D Coordinates

The Structure Module takes the pair representation and builds actual 3D atomic coordinates using a process called **IPA (Invariant Point Attention)**. It maintains a "frame" (position + orientation) for each residue and iteratively refines these frames using the pair representation.

The result: a full 3D structure with atomic coordinates for every residue, plus a confidence score for each.

---

## What AF2 Cannot Do

AF2 is remarkable, but it has real limitations:

| Limitation | Why |
|-----------|-----|
| **Intrinsically disordered proteins** | No single stable structure to predict |
| **Multiple conformational states** | Predicts one structure — not the ensemble |
| **Novel folds with no MSA** | Co-evolution signal requires many homologs |
| **Binding-induced conformational change** | Predicts apo structure, not always the bound state |
| **Mutations with no evolutionary precedent** | Can't reliably predict effect of designed mutations |
| **Post-translational modifications** | Glycosylation, phosphorylation not modeled |

> **This is why BindCraft's self-consistency check matters:** designed proteins have no evolutionary history (no MSA), so we can't trust AF2's prediction blindly. The self-consistency check asks: does AF2 predict this *designed sequence* folds into the designed *backbone*? If yes, we trust the design.

---

## AlphaFold2 vs AlphaFold3

| Feature | AF2 (2021) | AF3 (2024) |
|---------|-----------|-----------|
| Proteins | Excellent | Excellent |
| Protein complexes | Good (with Multimer) | Better |
| Antibody-antigen | Moderate | Improved |
| Small molecules | Not supported | Supported |
| DNA/RNA | Not supported | Supported |
| Structure module | IPA | Diffusion |

BindCraft uses AlphaFold2 (via ColabDesign) for hallucination and validation. AF2 is still the standard for most biologics design workflows.

---

## Key Vocabulary

| Term | Plain English |
|------|--------------|
| MSA | A table of related sequences aligned column-by-column |
| Co-evolution | When two positions mutate together, suggesting they're in contact |
| Evoformer | The attention-based neural network that reasons about sequence pairs |
| Pair representation | A matrix encoding spatial relationship information for every residue pair |
| Structure module | The part of AF2 that turns pair representations into 3D coordinates |
| pLDDT | Per-residue confidence score (0–100; >90 = excellent) |
| PAE | Predicted Aligned Error — confidence in the *relative position* of two residues |

---

> **Key takeaway:** AlphaFold2 uses evolutionary co-variation (MSA) and a novel attention architecture (Evoformer) to predict protein structure with near-experimental accuracy. It is the engine behind both ColabFold and BindCraft.

**Next:** [4.2 Reading Confidence Scores →](m4_02_confidence_scores.md)
