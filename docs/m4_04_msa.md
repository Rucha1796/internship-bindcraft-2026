# 4.4 MSAs and Evolution

**Time:** ~25 minutes reading

---

## Why Evolution Matters for Structure Prediction

AlphaFold2 doesn't predict protein structure from a single sequence. It uses **evolutionary information** — specifically, patterns from thousands of related sequences — to figure out which residues must be close in 3D space.

This page explains why, and what happens when that evolutionary information is missing.

---

## What Is an MSA?

A **Multiple Sequence Alignment (MSA)** is a table of related protein sequences, aligned column by column so that corresponding positions are in the same column.

Example (simplified):
```
Sequence 1 (human):    MKVIFG--LMVGGVIA...
Sequence 2 (mouse):    MKVIFG--LMVGGVIA...
Sequence 3 (zebrafish): MKITFG--LMVGGVVA...
Sequence 4 (fly):      MKITFGGGLMVGGVIA...
Sequence 5 (worm):     MKVTFG--LMVAGVVA...
...
(thousands more)
```

Each column = one position in the protein sequence
Each row = one homologous protein from a different organism

---

## Co-Evolution: The Signal AF2 Uses

Over millions of years of evolution, mutations happen randomly. Most mutations to the buried core of a protein are harmful — they disrupt the fold and the protein stops working. Organisms with those mutations die. This is **purifying selection** — it "purifies" the gene by removing damaging mutations.

**But here's the key insight:** If a mutation in position 47 is compensated by a mutation in position 81 — if both mutations together keep the protein folded — then both can survive. We call this **co-evolution**.

```
If position 47 changes from Asp (D) to Asn (N)...
...and position 81 changes from Arg (R) to Lys (K) at the same time...
...the salt bridge is preserved (D→N doesn't destroy it if R→K compensates)
```

When this happens thousands of times across evolution, we see a **statistical correlation** between positions 47 and 81 in the MSA. AlphaFold2's Evoformer learned to read these correlations.

> **Key insight:** Co-evolving residues are almost always spatially close in the protein structure. The MSA is essentially a map of "which residues must be neighbors in 3D space."

---

## How AlphaFold2 Builds the MSA

AF2 (and ColabFold) use the following process:

1. **Take the query sequence** (your protein)
2. **Search databases** — UniRef90, BFD, Mgnify (hundreds of millions of sequences)
3. **Find homologs** — sequences with > ~30% identity that are clearly related
4. **Align them** — put corresponding positions in the same column
5. **Pass the MSA to Evoformer** — the neural network reads the co-variation patterns

MMseqs2 (used by ColabFold) does step 2–3 in seconds. JackHMMER (original AF2) takes hours for some proteins.

---

## What Happens With No MSA?

AF2 can predict structure with a **single-sequence** input (no MSA). But accuracy drops significantly for most proteins.

This is an important concept because:
- **Designed binders have no natural homologs.** BindCraft hallucinated sequences don't exist in any database.
- When AF2 predicts a designed binder's structure (self-consistency check), it often uses a single-sequence or shallow-MSA mode.
- This makes i_pTM and pLDDT especially important — they are AF2's confidence given *limited evolutionary information*.

> **Notebook 03** (AF2 with vs. without MSA) will let you see this directly: predict PD-L1 with a full MSA, then predict a short designed sequence with no MSA, and compare the confidence scores.

---

## Depth: How Many Sequences Is Enough?

MSA "depth" = number of sequences in the alignment.

| Depth | Effect on AF2 |
|-------|-------------|
| < 30 sequences | Significant accuracy drop |
| 30–300 | Moderate; some uncertainty in loops |
| > 300 | Good signal; predictions generally accurate |
| > 5000 | Very good; diminishing returns above this |

**PD-L1 is a well-studied human protein** — ColabFold will find thousands of homologs. High depth → high confidence prediction → high pLDDT.

**A newly hallucinated binder sequence** — no natural homologs in any database. MSA depth = 1 (just itself). AF2 has to rely entirely on learned folding patterns, not co-evolution. This makes high i_pTM from a single-sequence prediction especially meaningful — it means AF2's internal folding knowledge is confident about the interface.

---

## The Evoformer Insight

The Evoformer processes two representations simultaneously:
1. **MSA representation** — rows are sequences, columns are positions; captures co-evolution
2. **Pair representation** — a 2D matrix of residue-residue relationship features

These two are updated jointly through "triangle attention" — attention that considers triangles of residue relationships (if A-B are close and B-C are close, A-C are likely close too, by the triangle inequality in 3D space).

This geometric reasoning is why AF2 is so much better than previous methods.

---

## Activity: Compare MSA Depths

In Notebook 03:

1. Run ColabFold on PD-L1 (full MSA, ~thousands of sequences)
2. Run ColabFold on a short random designed sequence (no natural homologs, depth ~1)
3. Compare:
   - pLDDT for each
   - PAE matrix
   - pTM score
   - The 3D structure — does the designed sequence produce a confident fold?

**Expected result:** PD-L1 gets very high scores; the random designed sequence likely gets mediocre scores and a less clear structure. This illustrates *why* BindCraft's own filtering (using AF2 in single-sequence mode on each designed binder) is so important — designs that score well even without evolutionary information are likely genuinely stable.

---

> **Key takeaway:** MSAs give AF2 evolutionary co-variation signals that reveal which residues must be spatially close. Deep MSAs → better predictions. Designed binders have no MSAs — so when AF2 is still confident about a designed binder's structure, that confidence comes from its learned knowledge of protein physics, not from evolutionary data. This makes high i_pTM especially significant for BindCraft designs.

**Next:** [Module 5: De Novo Design →](m5_01_design_challenge.md)
