# 9.1 Your New Target

**Time:** ~20 minutes reading + discussion with mentor

---

## Weeks 5–6: The Independent Project

In the first four weeks, you learned protein biology, mastered the tools, and ran BindCraft on a well-understood teaching target (PD-L1). Now you're ready to do this independently on a **new immunooncology target**.

This module is the starting point for your independent project.

---

## Your Target Protein

Your mentor will assign you an immunooncology target for weeks 5–6. It will be a protein involved in cancer immune evasion — similar in concept to PD-L1, but a different structure and binding interface.

Common immunooncology targets in this space:
- **TIGIT** — T cell immunoreceptor with Ig and ITIM domains
- **LAG-3** — Lymphocyte activation gene 3
- **TIM-3** — T cell immunoglobulin and mucin domain 3
- **CD47** — "Don't eat me" signal; expressed on many tumors
- **VISTA** — V-domain Ig suppressor of T cell activation
- **CD80/CD86** — B7 ligands that interact with CTLA-4

Your mentor will tell you:
1. The protein name and gene
2. Why it's an interesting target
3. Whether there's an experimental structure available

---

## First Questions to Ask

When you receive your new target, answer these questions (you'll need them in the coming modules):

**Biology questions:**
- What does this protein do normally? What's its function?
- How does it contribute to cancer immune evasion?
- What is its natural binding partner? (The equivalent of PD-1 for PD-L1)
- Are there any approved drugs or clinical candidates against this target?

**Structure questions:**
- Is there an experimental structure in the PDB? (X-ray or cryo-EM)
- If yes: what's the PDB ID and resolution?
- If no: can you use an AlphaFold prediction from the AF Database?
- Which chain in the structure is the domain you want to target?

**Interface questions:**
- Where does the natural binding partner bind?
- Which residues make the key contacts? (These will become your hotspots)
- Is there a published complex structure (target + natural partner) to identify the interface?

---

## The Literature Search

For a thorough background on your target, search:

**PubMed** (pubmed.ncbi.nlm.nih.gov):
- Search: `[target name] structure`
- Search: `[target name] binding interface`
- Search: `[target name] immunotherapy`

**RCSB PDB** (rcsb.org):
- Search the protein name
- Look for: complex structures (target + natural partner), high-resolution structures (< 3 Å)

**UniProt** (uniprot.org):
- Search the protein name
- Provides sequence, domain structure, and links to relevant structures

**AlphaFold Database** (alphafold.ebi.ac.uk):
- If no experimental structure exists, look here for an AF2 prediction
- Every known human protein has been predicted

---

## Comparing to PD-L1

As you research your new target, compare it to PD-L1:

| Feature | PD-L1 | Your Target |
|---------|-------|-------------|
| Protein family | Immunoglobulin superfamily | |
| Domain size | ~220 residues (extracellular) | |
| Fold type | Beta-sandwich | |
| Interface character | Flat, beta-strand-rich | |
| Natural ligand | PD-1 | |
| Approved drugs | Keytruda, Opdivo, etc. | |
| Number of PDB structures | > 100 | |

Most immunooncology checkpoints are immunoglobulin superfamily members with similar beta-sheet folds. But the specific binding interfaces vary — this is why different drugs are needed.

---

## Deliverable: Target Summary

By the end of Module 9.1 (day 1 of week 5), write a short target summary:

**Target Summary — [Protein Name]**

1. **Biology** (3–4 sentences): What does this protein do? How does it suppress immune responses?

2. **Structure** (2–3 sentences): PDB ID, resolution, which chain, fold type.

3. **Interface** (2–3 sentences): Where does the natural binding partner bind? What type of contacts?

4. **Therapeutic context** (2–3 sentences): Are there drugs/trials against this target? Why is it still interesting?

5. **Your plan** (1–2 sentences): What hotspot residues will you use? How is this similar to or different from PD-L1?

Share this with your mentor before proceeding to Module 9.2.

---

> **Key takeaway:** Your independent project starts with understanding the biology and structure of a new target. Use the same skills from Modules 1–3 (structure visualization, interface identification, literature reading) — just applied to a new protein. The goal is to understand your target well enough to make intelligent choices about hotspot residues and design parameters.

**Next:** [9.2 Finding the Structure →](m9_02_finding_structure.md)
