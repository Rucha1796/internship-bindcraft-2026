# 9.4 Running BindCraft (Independent Project)

**Time:** ~20 minutes setup + hours of compute

---

## You've Done This Before

By now, you've already run BindCraft on PD-L1 in Week 3–4. Running it on your new target uses the **exact same workflow** — only the settings file changes.

This module is a quick-reference checklist for setting up your independent project run.

---

## Settings File for Your Target

Copy the PD-L1 settings file and update the fields:

```json
{
    "binder_name": "MyTarget",
    "starting_pdb": "/content/drive/MyDrive/bindcraft/MyTarget.pdb",
    "chains": "A",
    "target_hotspot_residues": "X,Y,Z",
    "lengths": [65, 90],
    "number_of_final_designs": 10
}
```

**Fields to change:**
- `binder_name`: Use your target's name (e.g., `"TIGIT"`, `"LAG3"`)
- `starting_pdb`: Path to your cleaned target PDB (prepared in Module 9.2)
- `chains`: The chain you're targeting (check your PDB!)
- `target_hotspot_residues`: Your chosen hotspots from Module 9.3 (comma-separated)

**Fields to keep the same:**
- `lengths`: `[65, 90]` works well for most targets
- `number_of_final_designs`: Start with 10

---

## Pre-Run Checklist (Independent Project)

- [ ] Target summary written and shared with mentor (Module 9.1)
- [ ] Structure downloaded, cleaned, and verified in Mol* (Module 9.2)
- [ ] Hotspot residues identified and documented (Module 9.3)
- [ ] Hotspot residue numbers verified against your specific PDB file's numbering
- [ ] Settings JSON created with correct paths and hotspot residues
- [ ] PDB file uploaded to Google Drive at the path specified in the JSON
- [ ] Colab GPU runtime active (Runtime → Change runtime type → T4 GPU)

**Critical check:** Before running Cell 8 (the main loop), run a quick test by running just the first 10 trajectories and checking if they produce any accepted designs. If acceptance rate = 0%, something may be wrong with your settings.

---

## What's Different for a New Target?

### Acceptance Rate May Differ

Some targets are easier to design against than others. If your acceptance rate after 20 trajectories is:
- **> 30%** → Good; proceed with the full run
- **10–30%** → Typical; may need 50–100 trajectories to get 10 accepted designs
- **< 10%** → Low; consider adjusting hotspot residues or lengths
- **0%** → Something may be wrong; check hotspot numbering and PDB integrity

### Length May Need Adjustment

For larger binding interfaces (> 1000 Ų), slightly longer binders (`[80, 110]` residues) may work better.
For small, tight epitopes, shorter binders (`[50, 70]`) reduce entropy cost.

Ask your mentor for guidance.

### Filter Preset

Start with Default filters. If acceptance rate is very low, ask your mentor about switching to Relaxed filters for a pilot run.

---

## Running in Parallel (Time-Saving)

If your co-mentor has access to multiple Colab accounts or a research cluster, you can run multiple BindCraft jobs simultaneously with:
- Different hotspot sets (to find the best design region)
- Different length ranges
- Different helicity settings (helical vs. sheet vs. mixed)

Each job saves to a different output folder. You can merge the CSVs later for analysis.

---

## Monitoring Progress

While BindCraft runs:

1. Watch the output in Cell 8 — each line tells you whether that trajectory accepted or rejected
2. Periodically check `Trajectory/Plots/` for pLDDT/i_pTM evolution curves
3. After ~20 trajectories, check `trajectory_stats.csv` to see the distribution of i_pTM values

If the run is going well, you'll see i_pTM values reaching 0.55+ in some trajectories within the first hour.

---

## When to Stop

BindCraft stops automatically when `number_of_final_designs` (10) are collected. But you may want to:

- **Stop early** if you already have 5 excellent designs (i_pTM > 0.65) and are running out of GPU time
- **Stop and restart** if something looks wrong (all trajectories failing for the same reason)
- **Let it run longer** if designs look good but i_pTM is consistently 0.55–0.60 — more trajectories will find better ones

You can re-run BindCraft from scratch, and it will add to existing output files (not overwrite).

---

## Saving Your Results

At the end of the run, before the Colab session disconnects:

1. Download `final_design_stats.csv` to your computer
2. Download `failure_csv.csv`
3. Download all PDB files from `Accepted/Ranked/`
4. These should already be saved to Google Drive, but local copies are safer

---

> **Key takeaway:** Running BindCraft on your independent target is the same workflow as PD-L1 — just with an updated settings JSON. Check acceptance rate after 20 trajectories to confirm settings are working. Save all outputs to Drive and local copies before the session ends.

**Next:** [9.5 Analyzing Your Results →](m9_05_analysis.md)
