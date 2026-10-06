# Scoring Rubric (fixed harness — the agent never edits this file)

Score each candidate 0–5 on three axes. Total out of 15.

## Axes

**Novelty (0–5)**
- 5: no prior work found in 2+ sources (arXiv / Scholar / Semantic Scholar)
- 3: adjacent work exists, clear differentiation recorded
- 1: incremental tweak on published work
- 0: already published → auto-KILL

**Feasibility (0–5)**
- 5: minimal verifiable experiment fits in ≤1 week on the brief's compute
- 3: doable in timeline with identified risks
- 1: needs resources beyond the brief's constraints
- 0: infeasible → auto-KILL

**Fit & leverage (0–5)**
- 5: directly extends the brief's seeds AND exploits the human's background
- 3: connects to surveyed literature or background
- 1: from-zero idea with no connection to the brief
- 0: contradicts the brief's non-goals → auto-KILL

## Kill rules (hard — the val_bpb of idea digging)

1. Total < 10/15 → KILL.
2. Any axis = 0 → KILL, regardless of total.
3. Novelty ≤ 1 → KILL, regardless of total.
4. If the novelty claim needs a paragraph to explain → treat as Novelty ≤ 1.

## Verdict format (one line, grep-able)

```
VERDICT: KEEP 12/15 | 3D-consistent cloak breaks published 3DGS robustness result
VERDICT: KILL | collides with AutoGuard (ICLR'26 under review)
```
