# 6.2 Launching the Design Run

**Time:** ~20 minutes reading + hands-on

---

## The Notebook Structure

The BindCraft notebook (Notebook 04) has 12 cells. Each cell does a specific job. You run them **in order from top to bottom**.

| Cell | What it does | How long |
|------|-------------|----------|
| 1 | Mount Google Drive | < 1 min |
| 2 | Install BindCraft + dependencies | ~5 minutes |
| 3 | Settings form (fill in your parameters) | You set this |
| 4 | Advanced settings | Read and leave defaults |
| 5 | Choose filter preset | Use Default |
| 6 | Import functions + load settings | < 1 min |
| 7 | Initialize PyRosetta | ~30 seconds |
| 8 | **Main design loop** | Hours |
| 9 | Consolidate & rank results | < 1 min |
| 10 | Display top 20 designs | < 1 min |
| 11 | Visualize top design | < 1 min |
| 12 | Display animation | < 1 min |

---

## Cell 2: Installation

The first time you run this, Cell 2 installs:
- BindCraft itself
- ColabDesign (AF2 hallucination library)
- PyRosetta
- AlphaFold2 weights (~500 MB)

This takes about 5 minutes. **Don't interrupt it.** Wait for the ✓ green checkmark in the cell margin.

> **Subsequent runs:** Colab environments reset after a few hours. You'll need to re-run Cell 2 each time. The AlphaFold2 weights may be cached by Google — sometimes Cell 2 is faster on repeat runs.

---

## Cell 3: The Settings Form

This appears as a form with text boxes — not code you need to type. Fill in:

```
design_path:              /content/drive/MyDrive/bindcraft/
binder_name:              PDL1
starting_pdb:             /content/bindcraft/example/PDL1.pdb
chains:                   A
target_hotspot_residues:  54,56,111,115,123
lengths:                  65,90
number_of_final_designs:  10
```

Double-check the PDB path — a typo here causes a confusing error later.

---

## Cell 8: The Main Design Loop

This is the heart of the notebook. When you run Cell 8:

1. BindCraft generates a random starting sequence
2. Runs AF2 hallucination (100 gradient steps)
3. Logs the trajectory metrics
4. Passes accepted trajectories to ProteinMPNN
5. Scores MPNN designs with AF2 + PyRosetta
6. Applies filters
7. Saves accepted designs to `Accepted/`
8. Loops until `number_of_final_designs` (10) are collected

**Output you'll see in real time:**
```
Trajectory 1: i_pTM=0.23 [REJECTED - low i_pTM]
Trajectory 2: i_pTM=0.61 [Accepted → MPNN]
  MPNN sequence 1: i_pTM=0.64, ShapeComp=0.62, dG=-14.3 [ACCEPTED]
  MPNN sequence 2: i_pTM=0.59, ShapeComp=0.51, dG=-12.1 [rejected - ShapeComp]
Trajectory 3: i_pTM=0.18 [REJECTED - low i_pTM]
...
```

**Estimated time:** With a Colab T4 GPU and requesting 10 final designs:
- Each trajectory: ~3–5 minutes (including MPNN and AF2 rescoring)
- Total: 1–3 hours depending on acceptance rate
- Acceptance rate: ~20–40% of trajectories pass hallucination; ~50–70% of those pass all filters

---

## Using the Pregenerated Dataset (If No GPU)

If the GPU is unavailable (Colab limits, disconnection), you have **three pregenerated PDL1 datasets** to work with instead of running BindCraft yourself:

| Dataset | Contents |
|---------|---------|
| `PDL1_8aok/` | Full run: 12 accepted designs, all CSV files, animations |
| `PDL1_8aok_trim/` | Partial run: trajectory + MPNN outputs, 1 accepted design |
| `PDL1_5x8m/` | Trajectory outputs only |

These are loaded automatically in Notebooks 05–08 (analysis notebooks). You can complete all the analysis work with these datasets even if you can't run BindCraft live.

> **Good news:** The analysis notebooks are designed to load either live BindCraft output *or* the pregenerated datasets. Just set a flag at the top of each notebook.

---

## Monitoring the Run

**Do not close your browser tab** while Cell 8 is running — Colab will disconnect after a few minutes of inactivity if the tab is closed.

You can leave Colab open in one tab and do other work in another tab. The notebook will keep running.

**Colab GPU time limits:** Free Colab accounts have a usage limit. If you've been running GPU cells for > 3–4 hours, you may be disconnected. This is normal — just reconnect and the output files are saved to Drive.

---

## What to Do While It Runs

While Cell 8 runs:
- Read Module 7 (analyzing results) — you'll need this knowledge when the run finishes
- Examine the pregenerated datasets in Notebooks 05–06 to understand the output format before your own results appear
- Review the PDB files in `PDL1_8aok/Accepted/Ranked/` in Mol* to see what accepted designs look like

---

## Common Errors and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `File not found: PDL1.pdb` | Wrong path in settings | Check the `starting_pdb` field; verify Drive path |
| `CUDA out of memory` | GPU memory exhausted | Restart runtime; reduce batch size (mentor will help) |
| `PyRosetta not initialized` | Skipped Cell 7 | Run Cell 7, then retry Cell 8 |
| `No module named 'colabdesign'` | Skipped Cell 2 | Run Cell 2 to install |
| Runtime disconnected | Colab timeout | Reconnect; saved designs are intact in Drive |

---

> **Key takeaway:** Run the notebook cells in order. Cell 8 runs for 1–3 hours on a GPU, printing progress as trajectories succeed or fail. If GPU is unavailable, the pregenerated datasets are a full substitute for the analysis work. Always keep the browser tab open during a run.

**Next:** [6.3 Understanding the Output Files →](m6_03_outputs.md)
