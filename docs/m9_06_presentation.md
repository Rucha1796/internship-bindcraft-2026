# 9.6 Final Presentation Guide

**Time:** ~1 hour preparation (spread across Week 6)

---

## The Capstone

Your internship ends with a **20-minute presentation** to your mentor, co-mentor, and any other interested lab members. This is your opportunity to show everything you've learned and the science you've done.

This is not a test — it's a scientific discussion. You're presenting real data and real design decisions to real scientists. Ask for their feedback; they may have ideas for next steps.

---

## Presentation Structure

### Suggested Outline (20 minutes total)

| Section | Time | Content |
|---------|------|---------|
| Introduction | 3 min | The biology: why your target matters |
| Methods | 5 min | The BindCraft pipeline |
| PD-L1 Results | 4 min | What you found on the teaching target |
| Independent Target | 6 min | Your new target: hotspot choice + BindCraft results + top 5 designs |
| Discussion | 2 min | What you'd do next; reflection |

---

## Slide-by-Slide Guide

### Slide 1: Title

```
Computational Design of Protein Binders Against [Your Target]

[Your Name]
Internship 2026
[Date]
Mentor: [Mentor name] | Co-Mentor: [Co-mentor name]
```

---

### Slide 2: The Biology — Why This Target?

- What does your target protein do normally?
- How does it suppress immune responses against cancer?
- What is the natural binding partner you're trying to block?

**Visual:** Include a simple diagram of the checkpoint circuit (like the PD-1/PD-L1 circuit from Module 1.4 — but for your target). Draw it yourself or adapt from a published paper.

---

### Slide 3: The Design Challenge

- How many possible protein sequences exist (sequence space size)
- Why computational design is necessary
- One-sentence summary of the BindCraft approach

**Visual:** A simple schematic of the BindCraft pipeline (4 steps: hallucination → MPNN → filtering → self-consistency)

---

### Slide 4: Teaching Target — PD-L1 Results

- The target PDB structure (screenshot from Mol*)
- The 5 hotspot residues highlighted
- Key numbers: how many trajectories ran, how many accepted designs, acceptance rate
- i_pTM distribution plot (from Notebook 05)

---

### Slide 5: Top PD-L1 Designs

- Table of top 3 designs with key metrics (i_pTM, ShapeComp, Binder RMSD, dG)
- Screenshot of the #1 design in Mol* (PD-L1 in blue, binder in rose)
- 1–2 sentences on what makes this the top design

---

### Slide 6: Your Independent Target — Background

- Target protein name and function
- The PDB structure you used (ID, resolution)
- The natural binding interface (screenshot from Mol* showing the complex, if available)

---

### Slide 7: Hotspot Selection

- Your chosen hotspot residues and why you chose them
- Table: residue number | amino acid | justification
- Screenshot showing hotspots highlighted on the target structure

---

### Slide 8: BindCraft Results

- Run statistics: trajectories, acceptance rate, designs
- Comparison to PD-L1 run (easier or harder target?)
- Metric distribution plots (i_pTM, ShapeComp histograms)

---

### Slide 9: Your Top 5 Designs

For each top design, a brief row:
| Rank | i_pTM | ShapeComp | Binder RMSD | Structural feature | Why selected |
|------|-------|-----------|-------------|-------------------|-------------|

Plus: screenshot of your #1 design in Mol*

---

### Slide 10: What Would Happen Next?

- How your sequences would be expressed (gene synthesis → E. coli → purification)
- Which assay you'd run first and why (ELISA, then SPR)
- What a "success" would look like (ELISA hit, Kd < 100 nM in SPR)
- If one design fails, what you'd learn and try next (DMTA loop)

---

### Slide 11: Reflection — What You Learned

Personal reflection (be honest — good science requires honesty about limitations):

- What was harder than expected?
- What worked really well computationally?
- What would you do differently if you did this again?
- What questions do you have that the experiments will answer?

---

### Slide 12: Acknowledgments

Thank your mentor, co-mentor, any other team members, and any tools/papers you relied on.

```
AlphaFold2 (Jumper et al., Nature 2021)
BindCraft (Pacesa, Nickel et al., Nature 2025)
ColabFold (Mirdita et al., Nature Methods 2022)
ProteinMPNN (Dauparas et al., Science 2022)
```

---

## Presentation Tips

**Do:**
- Know your data. If someone asks "what does i_pTM mean?", you should be able to explain it clearly.
- Show actual structures. Visualizations are more compelling than numbers alone.
- Acknowledge uncertainty. "This design has a borderline Binder_RMSD — it might not fold on its own, but the interface looks really good" is better than pretending everything is perfect.
- Make eye contact. You're presenting to people who want to hear from you.

**Don't:**
- Put more text on slides than you can read in 5 seconds
- Read from your slides
- Skip the biology motivation — it's what makes the computational work matter
- Forget to label axes on plots

---

## Common Questions from the Audience

**"Why did you choose those hotspot residues?"**
→ Describe your structure analysis and any literature support.

**"What happens if none of your designs bind?"**
→ That's still a result — it tells us something about BindCraft's predictions for this target and what to try next.

**"How is this different from just using an antibody?"**
→ Review Module 1.5 — size, cost, design speed, potential for harder-to-access epitopes.

**"Could you improve these designs?"**
→ Yes — partial diffusion, additional MPNN sequences, different hotspots, experimental feedback into the next round.

---

## After the Presentation

Your mentor will give you feedback and may have suggestions for next steps. This conversation is part of the science — take notes.

If any of your designed proteins end up being expressed and tested, you'll be notified of the results even after the internship ends. This is your science — you deserve to know how it turns out.

---

> **Congratulations on completing the internship.** You went from knowing no protein biology to designing de novo protein binders against a cancer target using state-of-the-art AI tools. That's real computational biology, and you did it in 6 weeks.

**← [Return to Home](index.md)**
