# Schedule

A week-by-week schedule for the 4-week (core) and 6-week (extended) internship tracks.

---

## Track Overview

| Track | Duration | Who | Deliverable |
|-------|---------|-----|------------|
| Core | 4 weeks | Both students | PD-L1 Design Recommendation Report |
| Extended | +2 weeks (weeks 5–6) | One student | Independent target design + final presentation |

Weeks 1–4 are identical for both students. The extended student continues through week 6.

---

## Week 1 — Biology Foundations

**Goal:** Understand proteins, the immune system, and why PD-L1 matters.

| Day | Morning | Afternoon | Assignment |
|-----|---------|-----------|-----------|
| Mon | [1.1 What Is a Protein?](m1_01_what_is_a_protein.md) | [1.2 Protein Structure](m1_02_protein_structure.md) | Write: "Explain protein folding to a 10-year-old" |
| Tue | [1.3 How Proteins Interact](m1_03_protein_interactions.md) | [1.4 Immune System & PD-L1](m1_04_immune_system_pdl1.md) | Draw the PD-1/PD-L1 circuit from memory |
| Wed | [1.5 Antibodies vs. Designed Binders](m1_05_antibodies_vs_binders.md) | [2.1 Timeline](m2_01_timeline.md) + [2.2 The PDB](m2_02_pdb.md) | Find PD-L1 on the PDB; record PDB ID and resolution |
| Thu | [2.3 Homology to Deep Learning](m2_03_homology_to_dl.md) | Mol* hands-on with mentor | Explore 5C3T, 5X8M, 8AOK in Mol* |
| Fri | [3.1 Tour of Mol*](m3_01_molstar_tour.md) + [3.2 Reading a Structure](m3_02_reading_structure.md) | [3.3 PD-L1 Interface](m3_03_pdl1_interface.md) | Interface mapping activity (3.3 worksheet) |

**Week 1 deliverable:** Completed Interface Mapping Worksheet from Module 3.3

---

## Week 2 — AlphaFold2 and De Novo Design

**Goal:** Understand how AF2 works and what BindCraft does.

| Day | Morning | Afternoon | Assignment |
|-----|---------|-----------|-----------|
| Mon | [4.1 What AlphaFold2 Does](m4_01_alphafold2.md) | [4.2 Reading Confidence Scores](m4_02_confidence_scores.md) | PAE matrix interpretation activity |
| Tue | **Notebook 02:** Run ColabFold on PD-L1 | [4.3 Running ColabFold](m4_03_colabfold.md) | Record pTM score; compare to experimental structure |
| Wed | **Notebook 03:** AF2 with vs. without MSA | [4.4 MSAs and Evolution](m4_04_msa.md) | Compare confidence scores for PD-L1 vs. random sequence |
| Thu | [5.1 The Design Challenge](m5_01_design_challenge.md) | [5.2 The BindCraft Pipeline](m5_02_bindcraft_pipeline.md) | Walk Through a Trajectory activity (5.2 worksheet) |
| Fri | [5.3 What Is Hallucination?](m5_03_hallucination.md) | [5.4 Filtering Designs](m5_04_filtering.md) | Quiz: name all 8 filter criteria from memory |

**Week 2 deliverable:** Short written summary comparing AlphaFold2 prediction vs. BindCraft hallucination (3–5 sentences each, your own words)

---

## Week 3 — Running BindCraft on PD-L1

**Goal:** Set up and launch BindCraft; understand the outputs while it runs.

| Day | Morning | Afternoon | Assignment |
|-----|---------|-----------|-----------|
| Mon | [6.1 Setting Up](m6_01_setup.md) | Prepare settings JSON for PD-L1 run | Pre-run checklist complete |
| Tue | **Notebook 04:** Launch BindCraft on PD-L1 | [6.2 Launching the Run](m6_02_running.md) | BindCraft running; read Module 7 while it runs |
| Wed | Monitor BindCraft run | **Notebook 05:** Explore pregenerated PDL1_8aok data | Describe what the failure_csv tells you |
| Thu | [6.3 Understanding Output Files](m6_03_outputs.md) | **Notebook 06:** Visualize top designs from pregenerated data | Screenshot top 3 designs for your notes |
| Fri | [6.4 Visualizing Your Designs](m6_04_visualization.md) | Analyze your own BindCraft output (if complete) | Side-by-side comparison: your outputs vs. pregenerated |

**Week 3 deliverable:** Annotated screenshot gallery — top 5 designs from the pregenerated dataset with 1-sentence caption per design

---

## Week 4 — Analysis and Design Recommendation Report

**Goal:** Rank designs, apply traffic light system, write your Design Recommendation Report.

| Day | Morning | Afternoon | Assignment |
|-----|---------|-----------|-----------|
| Mon | [7.1 The Metrics Explained](m7_01_metrics.md) | **Notebook 07:** Failure analysis + filter distributions | Identify the most commonly failed filter |
| Tue | [7.2 Ranking & Filtering Designs](m7_02_ranking.md) | **Notebook 08:** Interactive design selection | Preliminary top 5 list |
| Wed | [7.3 Good vs. Bad Designs](m7_03_good_vs_bad.md) | [7.4 PDL1 Case Study](m7_04_case_study.md) | Draft Design Recommendation Report |
| Thu | [8.1 From Bits to Molecules](m8_01_bits_to_molecules.md) + [8.2 What Gets Ordered](m8_02_what_gets_ordered.md) | [8.3 How We Test Binders](m8_03_testing.md) + [8.4 The DMTA Cycle](m8_04_dmta.md) | Finalize Design Recommendation Report |
| Fri | **Design Recommendation Report due** | Present top 5 to mentor and co-mentor | Receive feedback; discuss what gets ordered |

**Week 4 deliverable:** Completed Design Recommendation Report with top 5 PD-L1 binder candidates, justifications, and synthesis recommendations

---

*Students completing the 4-week track finish here. Extended-track students continue to Weeks 5–6.*

---

## Week 5 — Independent Target Discovery

**Goal:** Research a new immunooncology target and set up BindCraft.

| Day | Morning | Afternoon | Assignment |
|-----|---------|-----------|-----------|
| Mon | [9.1 Your New Target](m9_01_new_target.md) — biology background | Literature search: PubMed + PDB for your target | Target Summary (see 9.1 deliverable) |
| Tue | [9.2 Finding the Structure](m9_02_finding_structure.md) | Download, clean, and verify target PDB | Share cleaned PDB with mentor for review |
| Wed | [9.3 Identifying Hotspot Residues](m9_03_hotspots.md) | Interface analysis in Mol* | Hotspot Selection document (see 9.3 template) |
| Thu | Discuss hotspot choices with mentor | Finalize settings JSON | Pre-run checklist complete |
| Fri | [9.4 Running BindCraft](m9_04_running.md) | Launch BindCraft on independent target | BindCraft running overnight |

**Week 5 deliverable:** Target Summary + Hotspot Selection document + BindCraft launched

---

## Week 6 — Analysis and Final Presentation

**Goal:** Analyze independent project results and present your science.

| Day | Morning | Afternoon | Assignment |
|-----|---------|-----------|-----------|
| Mon | [9.5 Analyzing Your Results](m9_05_analysis.md) | Load and explore independent project outputs | Draft metric distribution plots |
| Tue | Rank designs, visual inspection in Mol* | Comparison to PD-L1 run | Preliminary top 5 selections |
| Wed | Finalize top 5 selections with written justifications | Begin presentation slides | Slide draft: slides 1–6 |
| Thu | Complete presentation slides | Practice presentation with mentor | Slide draft: slides 7–12 |
| Fri | **Final Presentation** (20 minutes + Q&A) | Wrap-up discussion with mentors | Celebration! |

**Week 6 deliverable:** Final Presentation (20 min) covering both PD-L1 and independent target results

---

## Checkpoints and Mentorship

Your mentor will check in with you at these key moments:

| Checkpoint | When | What we review |
|-----------|------|----------------|
| Biology check | End of Week 1 | Can you explain PD-L1's role to someone with no science background? |
| Pipeline check | End of Week 2 | Can you walk through the 4 BindCraft steps without notes? |
| Settings review | Before launching BindCraft (Week 3 Mon) | Are the settings correct? |
| Preliminary results | Mid Week 3 | Is the run producing accepted designs? |
| Report draft | Week 4 Wed | Are the top 5 well-justified? |
| Hotspot review | Week 5 Thu | Are the hotspot choices well-reasoned? |
| Analysis review | Week 6 Wed | Are the independent results interpreted correctly? |

---

## If You Fall Behind

The schedule is a guide, not a strict requirement. Science rarely goes exactly as planned. If you:
- **Fall behind by 1–2 days:** Skip the bonus activities (the worksheet exercises) and focus on core reading + notebooks
- **Miss a BindCraft run:** Use the pregenerated datasets — they contain everything you need for analysis
- **Have questions:** Ask your mentor! That's what they're there for.

The most important deliverable is the **Design Recommendation Report** at the end of Week 4. Everything else is in service of that.

---

> **Remember:** You're doing real science. BindCraft designs you select may actually be synthesized and tested. The skills you're building — reading structures, understanding confidence metrics, making evidence-based decisions — are the same skills professional computational biologists use every day.
