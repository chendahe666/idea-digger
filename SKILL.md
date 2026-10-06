---
name: idea-digger
description: "Overnight autonomous idea discovery in the spirit of Karpathy's autoresearch: time-boxed dig cycles, a hard keep-or-kill metric, three-file separation of concerns, zero interruptions to the sleeping human. Use when the user says 'dig ideas overnight', 'idea digger', '找idea', or wants autonomous thesis/paper idea discovery while they sleep."
---

# Idea Digger (when you sleep)

## Purpose

Turn a research brief into ranked, novelty-checked research ideas overnight — using Karpathy's autoresearch operating philosophy. The human fills in one brief, goes to sleep, and reviews a single report in the morning.

## The Karpathy mapping

| autoresearch | idea-digger |
|---|---|
| `program.md` (human edits) | `brief.md` — background, seeds, constraints. **The agent never edits it.** |
| `train.py` (agent edits) | `dig/` workspace — candidate files, search logs. The agent owns it. |
| `prepare.py` (fixed harness) | `rubric.md` — scoring rubric + kill criteria. **The agent never edits it.** |
| 5-min wall clock / val_bpb | `DIG_MINUTES` per candidate / rubric score out of 15 |
| keep or discard | `KEEP` (≥ threshold) or `KILL` (one-line reason). No maybes. |
| "do NOT pause to ask the human" | Night mode: log everything, decide alone, report in the morning. |

## Workflow

### Phase 0 — Load the brief (once)

1. Read `brief.md` in the project dir. If missing, copy `assets/brief-template.md` → `brief.md` and ask the human to fill it. **This is the one thing that needs them awake.**
2. Read `rubric.md` (copy from `assets/rubric.md` if missing). Never modify either file.

### Phase 1 — Night loop (the Karpathy loop)

Repeat until `NIGHT_BUDGET` (default 8h) is exhausted or `TARGET_KEEPS` (default 5) survivors are banked:

1. **Pick next vein.** Choose an open question from the brief, or a gap exposed by a previous kill's reason. Never re-dig a killed vein — check `dig/kills.log` first.
2. **Time-boxed dig** (`DIG_MINUTES`, default 20). Search literature (arXiv / Scholar / Semantic Scholar), draft ONE candidate. The timer is wall-clock; when it rings, stop and score what you have.
3. **Score against `rubric.md`** → append `VERDICT: KEEP n/15 | one-line reason` to `dig/keeps/` or `VERDICT: KILL | one-line reason` to `dig/kills.log`.
4. **Kill fast, kill honestly.** A candidate that needs a paragraph to explain its novelty is not novel. One sentence or kill.

### Phase 2 — Novelty deep-check (survivors only)

For each KEEP: multi-source search for concurrent work from the last 6 months. Published → move to `kills.log` with the citation. Close work → record the differentiation in the candidate file.

### Phase 3 — Morning report

Write `IDEA_REPORT.md`:
- **Ranked survivors**, one page each: title / one-sentence pitch / gap + 2–3 citations / method sketch / evaluation plan / risks & fallback / first 2-week milestone.
- **Killed list**, one line each (idea + reason). Dead ends are data.
- **Recommended next step**: the single cheapest kill-or-confirm experiment for the top pick.

## Output Contract

- ONE canonical deliverable: `IDEA_REPORT.md`. No scattered files.
- Every "no prior work" claim must cite the searches actually run. Never assert novelty from memory.
- The report states its adaptations honestly (no GPU, no cross-model reviewer, etc.).

## Operating Rules

1. **Never wake the human.** No questions during night mode. Ambiguity → take the bolder interpretation, log the choice, move on.
2. **Never edit `brief.md` or `rubric.md`.** One belongs to the human, one to the harness.
3. **Fixed budgets are hard.** `DIG_MINUTES` per candidate, `NIGHT_BUDGET` total. No "just 5 more minutes."
4. **Anti-cagy directive.** At least 30% of candidates must be high-risk. If every candidate is incremental, you are being cagy: think harder, combine near-misses, try a radical vein.
5. **Context budget.** Per-candidate record ≤ 10 lines. Full text only for survivors. Grep, don't re-read.
6. **Elegance rule.** Prefer the idea expressible in one sentence; tie-break survivors by brevity of pitch.
7. **Morning gate.** Nothing is submitted, emailed, or acted on. The human reviews the report and decides.

## Defaults (override in the invocation, e.g. `dig ideas overnight — DIG_MINUTES=30`)

- `NIGHT_BUDGET=8h`, `DIG_MINUTES=20`, `TARGET_KEEPS=5`, `KEEP_THRESHOLD=10/15`
