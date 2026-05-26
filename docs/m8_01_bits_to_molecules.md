# 8.1 From Bits to Molecules

**Time:** ~20 minutes reading

---

## The Translation Problem

At the end of Week 4, you have:
- A CSV file with metrics
- PDB coordinate files
- An amino acid sequence (a string of letters like `MKVIFGLMVGGVIA...`)

What you *don't* have is a physical molecule — atoms, mass, something you can put in a test tube.

This module explains how your computational output becomes a real protein in the laboratory.

---

## What Is a Protein Sequence, Exactly?

Your designed binder sequence is a string of one-letter amino acid codes:

```
MKVIFGLMVGGVIAFVLGISIWPSVKEKGPVDRSIYGKVAKAKFNPEEIVQIFDIGIAKIINNI
```

Each letter maps to a specific amino acid. This sequence encodes everything needed to make the protein, because:

> **Anfinsen's principle:** The amino acid sequence uniquely determines the 3D structure (under physiological conditions).

So this string of 65 letters, when synthesized and placed in water at the right conditions, should *spontaneously fold* into the structure BindCraft designed.

---

## Step 1: Gene Synthesis

Proteins are made from DNA. To express your protein in a cell, you first need to make the **gene** — the DNA sequence encoding your protein.

The process:
1. **Back-translate** — convert your amino acid sequence to a DNA sequence (the genetic code)
2. **Codon optimization** — different organisms use different codons for the same amino acid; optimize for *E. coli* (the bacterium you'll express in)
3. **Order gene synthesis** — companies like Twist Bioscience or Integrated DNA Technologies (IDT) synthesize the exact DNA sequence you specify, for ~$100–200 per gene

**Turnaround time:** 1–3 weeks depending on supplier and gene complexity

The genetic code is redundant: most amino acids can be encoded by 2–6 different codons. Codon optimization chooses codons that *E. coli* uses most efficiently, increasing protein yield.

---

## Step 2: Cloning into an Expression Vector

The synthesized gene is delivered in a small circular piece of DNA (a plasmid). Your co-mentor inserts it into an **expression vector** — a larger plasmid designed specifically for protein production.

The expression vector provides:
- A **promoter** — a DNA signal that tells the cell "start making this protein"
- A **tag sequence** — often a 6×His tag (`HHHHHH`) at the protein's N or C terminus for purification
- **Antibiotic resistance** — allows selection of bacteria that took up the plasmid

**Common vectors for E. coli expression:** pET (T7 promoter system), pGEX (GST fusion), pMal (MBP fusion)

---

## Step 3: Transformation and Expression

1. **Transformation** — introduce the plasmid into *E. coli* cells by heat shock or electroporation
2. **Select colonies** — plate on antibiotic-containing agar; only cells with the plasmid survive
3. **Grow a starter culture** — pick a colony, grow overnight in broth
4. **Induce expression** — add IPTG (a chemical inducer) to tell the T7 promoter to start making your protein
5. **Harvest** — after 4–16 hours of expression, spin down the bacteria in a centrifuge; the pellet contains your protein (inside the cells)
6. **Lyse** — break open the cells with detergent, sonication, or a French press
7. **Clarify** — spin the lysate to remove cell debris; your protein is in the supernatant

**Yield:** Typically 1–50 mg of protein per liter of bacterial culture. Small, soluble designed miniproteins often express very well in *E. coli*.

---

## Step 4: Quality Control — Did It Express?

Before the expensive purification step, a quick check:

**SDS-PAGE gel** — boil the lysate sample, run it on a gel (polyacrylamide), stain with Coomassie blue. You'll see bands at different sizes. Your protein should appear as a new band at the correct molecular weight.

Example: a 70-residue binder ≈ ~8 kDa. Look for a band at 8 kDa (or 9–10 kDa if it has a His tag).

If the band appears in the soluble fraction → great; proceed to purification.  
If the band appears only in the insoluble fraction (inclusion bodies) → the protein didn't fold correctly; may need optimization.

---

## Why This Process Takes Weeks

Even with optimized protocols, each step takes time:

| Step | Time |
|------|------|
| Gene synthesis | 1–2 weeks |
| Cloning + transformation | 3–5 days |
| Expression optimization | 2–5 days |
| Purification | 1–2 days |
| Quality control | 1–2 days |
| **Total** | **2–4 weeks minimum** |

This is why computational pre-filtering is so important — you want to send only the *most promising* designs to the wet lab. Each design costs weeks of work.

---

## What Happens After Your Internship

Your Design Recommendation Report will be submitted at the end of Week 4. Your co-mentor will then:
- Order the gene synthesis for your top 5 designs
- Express and purify each protein (over several weeks)
- Test binding to PD-L1 using ELISA and SPR (Module 8.3)

The wet lab results will come back after your internship ends. But your computational decisions now determine which sequences get tested — that's a real scientific contribution.

---

> **Key takeaway:** Your amino acid sequence becomes a real protein through gene synthesis → bacterial expression → purification. Each design costs weeks and significant cost. This is why rigorous computational filtering (Modules 6–7) is so important — you want to send only the best designs forward.

**Next:** [8.2 What Gets Ordered & Why →](m8_02_what_gets_ordered.md)
