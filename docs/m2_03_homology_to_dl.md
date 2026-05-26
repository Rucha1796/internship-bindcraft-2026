# 2.3 From Homology Modeling to Deep Learning

**Time:** ~30 minutes reading

---

## Homology Modeling: Using Relatives to Build Models

The core idea is simple: **proteins with similar sequences tend to have similar structures.** If you know the structure of protein A, and protein B has 50% identical sequence, you can build a reasonable model of B using A as a template.

This is analogous to guessing what a word sounds like in French if you already know how a similar Spanish word sounds.

### The Rule of Thumb

| Sequence identity to template | Expected model quality |
|-------------------------------|----------------------|
| > 50% | Good — backbone reliable, side chains mostly correct |
| 30–50% | Moderate — overall fold correct, some errors |
| < 30% | Poor — high risk of wrong fold |
| < 20% ("twilight zone") | Essentially guessing |

### The Homology Modeling Workflow

*(From the NRC ML for Biologics course, Gaudreault & Wei)*

```
Sequence of unknown protein
        ↓
1. Find Template
   Use BLAST/FASTA to search the PDB for sequences with high similarity
        ↓
2. Generate Backbone
   Start with template structure; substitute amino acids to match target sequence
        ↓
3. Build Side Chains
   Search rotamer libraries for the lowest-energy side chain conformations
   (a 56-residue protein has over 10^99 possible rotamer combinations — need clever algorithms)
        ↓
4. Build Loops (Disordered Regions)
   Insertions and deletions relative to the template must be rebuilt from scratch
   Loops ≤ 8 residues: manageable. Loops ≥ 9 residues: still challenging.
        ↓
5. Optimize & Validate
   Energy minimization to remove clashes
   Ramachandran analysis to check backbone geometry
   MolProbity / ProCheck scores
        ↓
Homology Model
```

### Key Limitations

- **Template required** — no template, no model
- **CDR loops are hard** — antibody H3 loops are 10–20 residues and highly variable; homology modeling struggles
- **Accuracy plateaus** — even with a perfect template, models have errors in side chains and flexible regions
- **No dynamics** — a static snapshot doesn't capture conformational flexibility

---

## Rosetta: Physics-Based Design

David Baker's Rosetta energy function (1997) represented a different approach: instead of copying a template, *calculate which structure has the lowest energy*.

Rosetta uses a combination of:
- **Van der Waals interactions** (atomic packing)
- **Hydrogen bond geometry** (directionality matters)
- **Electrostatics** (charge-charge interactions)
- **Solvation** (energy cost of burying polar groups)
- **Statistical potentials** (frequencies of amino acid pairs observed in crystal structures)

Rosetta was transformative for **protein design** — it could evaluate whether a designed sequence was likely to fold into a target structure. It is still used today inside BindCraft (as PyRosetta) for energy minimization and interface scoring after AlphaFold2 designs the backbone.

---

## The Deep Learning Revolution

For both homology modeling and Rosetta, a fundamental limitation remained: **they couldn't capture long-range dependencies in protein sequences well.** An amino acid at position 10 might contact an amino acid at position 200 — but classical methods couldn't easily learn these relationships.

Deep learning changed this by:

### Co-evolutionary Information as the Key Signal

Evolution gives us a massive hint: if two positions in a protein are in contact, mutations at one position tend to be compensated by mutations at the other — **co-evolution**. By analyzing thousands of homologous sequences (a **Multiple Sequence Alignment, or MSA**), we can infer which residues are co-evolving and therefore likely to be in spatial contact.

> **Analogy:** If you notice that every time a restaurant adds a new appetizer, they also add a new dessert — you'd conclude that appetizers and desserts are somehow connected (maybe they're from the same supplier). Co-evolution is the protein equivalent of this pattern recognition.

### The Attention Mechanism

AlphaFold2 uses **attention** — a neural network mechanism that explicitly asks: *which other positions in the sequence are most relevant to understanding this position?*

For each pair of amino acids (i, j):
1. The MSA transformer notices a correlation between positions i and j
2. It hypothesizes that i and j are close in 3D space
3. The pair representation updates based on this hypothesis
4. Structural module uses the updated pair representation to place atoms

This is fundamentally different from Rosetta: instead of computing energy terms from first principles, AF2 *learned* the relationship between sequence and structure from the entire PDB.

### ESMFold: No MSA Needed

Meta's **ESMFold** (2022) took this further: instead of an MSA transformer, it uses a **protein language model** (ESM) trained on hundreds of millions of sequences. The language model implicitly encodes co-evolutionary information without explicitly building an MSA.

Result: ESMFold is ~60× faster than AlphaFold2, with slightly lower accuracy on difficult targets but excellent performance on proteins with abundant sequence data.

---

## Comparison of Approaches

| Approach | Requires template? | Requires MSA? | Typical accuracy | Speed |
|----------|-------------------|---------------|-----------------|-------|
| Homology modeling | Yes (>30% identity) | No | Moderate–good | Fast |
| Rosetta | No | No | Low–moderate | Slow |
| AlphaFold2 | No | Yes | Excellent | Minutes |
| ESMFold | No | No | Good | Seconds |
| ColabFold (AF2 + MMseqs2) | No | Yes (auto) | Excellent | Minutes |

---

> **Key takeaway:** Homology modeling uses known relatives; Rosetta uses physics; AlphaFold2 uses evolutionary co-variation learned by deep learning. Each approach unlocked new capabilities — and each limitation drove the next breakthrough.

**Next:** [3.1 Tour of Mol* →](m3_01_molstar_tour.md)
