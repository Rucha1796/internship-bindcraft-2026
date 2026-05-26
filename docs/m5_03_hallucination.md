# 5.3 What Is Hallucination?

**Time:** ~25 minutes reading

---

## The Word "Hallucination" in Protein Design

In everyday language, hallucination means seeing something that isn't real. In protein design, the meaning is almost the opposite: **protein hallucination** means *creating something new* — a sequence-structure pair that doesn't exist in nature — by running a neural network in reverse.

The term comes from the fact that the process "hallucinates" a protein out of noise.

---

## Forward Mode vs. Reverse Mode

**AlphaFold2 in forward mode (normal prediction):**
```
Input: amino acid sequence
  ↓ AlphaFold2
Output: 3D structure + confidence scores
```

You provide the sequence; AF2 predicts the structure.

**AlphaFold2 in reverse mode (hallucination):**
```
Input: random noise (random sequence)
  ↓ gradient descent on AF2 loss
Output: sequence that produces the structure you want
```

You specify what *properties* you want the output to have (high i_pTM, contacts at hotspots), and gradient descent finds a sequence that achieves those properties.

---

## Gradient Descent: The Core Mechanism

Gradient descent is the engine of all modern deep learning. The idea:

1. You have a **loss function** L that measures how bad the current state is (lower = better)
2. You compute the **gradient** ∇L — the direction in which L increases fastest
3. You move in the *opposite* direction (downhill on the loss surface)
4. Repeat until L stops decreasing

```
Start at random sequence
  → measure loss (high = bad interface)
  → compute gradient (which positions should change?)
  → update sequence (move downhill)
  → measure loss again
  → ... repeat for ~100 steps
  → converge on a low-loss sequence (good interface)
```

In BindCraft, the **sequence** is the variable being optimized. The loss function is built from AF2's predictions (i_pTM, pLDDT, pAE, contact count). Each gradient step nudges the sequence toward better AF2 scores.

---

## The Loss Function in Detail

The BindCraft hallucination loss combines several terms:

| Term | What it rewards | Why |
|------|----------------|-----|
| i_pTM | High interface confidence | Most important — we want AF2 to believe they're binding |
| pLDDT (binder) | Binder folds confidently | A binder that AF2 thinks is disordered won't work |
| i_pAE | Low inter-chain PAE | Precise positioning of binder relative to target |
| Contact count | Enough contacts at interface | Ensures the binder actually touches the target |
| Helicity | Controls secondary structure | Usually reward helical designs (stable, easy to express) |
| Radius of gyration | Binder isn't too spread out | Compact binders tend to be more stable |

The loss is a **weighted sum** of these terms. BindCraft has settings to adjust the weights — the default settings work well for most helical binder designs.

---

## What a Trajectory Looks Like

Each hallucination run is called a **trajectory**. Here's what happens:

**Step 0 (initialization):**
- Random amino acid sequence (length 65–90, as specified)
- The sequence is actually represented as a continuous vector of probabilities (soft sequence), not discrete amino acids — this makes gradients possible
- AF2 is run on this random sequence paired with the target → scores start low (high loss)

**Steps 1–100 (optimization):**
- Each step: run AF2, compute loss, compute gradient, update sequence vector
- i_pTM typically starts around 0.1–0.2 (random sequence doesn't bind)
- Over 100 steps, i_pTM gradually increases toward 0.5–0.8
- You can watch this happen in real time (BindCraft outputs plots)

**Final step:**
- Convert the continuous sequence vector to discrete amino acids (softmax → argmax)
- Run AF2 one final time on the discrete sequence
- If i_pTM > threshold: accept trajectory; pass to MPNN
- If not: reject (goes to LowConfidence/ folder)

---

## Why Start Many Trajectories?

Gradient descent finds a **local minimum** of the loss — not necessarily the global minimum. Starting from different random sequences leads to different local minima, i.e., different binder geometries and contact modes.

By running 50–200 trajectories (depending on your time/GPU budget), you explore many different binder shapes. The best ones (highest i_pTM after the full pipeline) become your candidates for wet lab testing.

This is why BindCraft is inherently a **sampling** method — you're sampling from the space of possible binders, guided by the gradient.

---

## Clashing: The Geometry Check

During hallucination, BindCraft checks every few steps for **clashes** — atoms from the binder overlapping with atoms from the target. Physical atoms can't occupy the same space.

If a trajectory produces a clashing geometry, it's immediately rejected (goes to Clashing/ folder) before wasting more compute on it. This is why you'll see Clashing/ trajectories in the output — they failed early in the optimization.

---

## Animation: Watching the Trajectory

BindCraft outputs HTML animations for each trajectory (in Animation/ folders). These show the binder sequence/structure evolving over the 100 gradient descent steps:

- **Early frames:** The binder is disordered, positioned randomly near the target
- **Later frames:** The binder "snaps into" a helical configuration, wrapping around the hotspot residues
- **Final frame:** The accepted geometry for that trajectory

Watching these animations builds intuition for how hallucination works.

---

> **Key takeaway:** Protein hallucination runs AlphaFold2 in reverse — gradient descent on AF2's confidence scores optimizes a random sequence into one that AF2 believes binds the target. Each run (trajectory) finds a local minimum; running many trajectories samples many different binder geometries. The best trajectories are passed to ProteinMPNN in Step 2.

**Next:** [5.4 Filtering Designs →](m5_04_filtering.md)
