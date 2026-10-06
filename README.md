# idea-digger

Overnight autonomous idea discovery, in the spirit of [Karpathy's autoresearch](https://github.com/karpathy/autoresearch).

Fill in one brief, go to sleep, wake up to a ranked, novelty-checked idea report.

## How it works

| autoresearch | idea-digger |
|---|---|
| `program.md` (human edits) | `brief.md` — your background, seeds, constraints. The agent never edits it. |
| `train.py` (agent edits) | `dig/` workspace — candidate files, search logs. The agent owns it. |
| `prepare.py` (fixed harness) | `rubric.md` — scoring rubric + kill criteria. The agent never edits it. |
| 5-min wall clock / val_bpb | `DIG_MINUTES` per candidate / rubric score out of 15 |
| keep or discard | `KEEP` (≥ threshold) or `KILL` (one-line reason). No maybes. |
| "do NOT pause to ask the human" | Night mode: log everything, decide alone, report in the morning. |

Plus two Karpathy originals: an **anti-cagy directive** (≥30% of candidates must be high-risk) and an **elegance rule** (an idea that needs a paragraph to explain its novelty is not novel — one sentence or kill).

## Install

Copy into your skills directory (Claude Code / Codex):

```bash
# Claude Code
cp -r . ~/.claude/skills/idea-digger/
```

## Usage

1. Copy `assets/brief-template.md` → `brief.md` in your project dir, fill it in (~1 page: background, seeds, open questions, surveyed literature, constraints).
2. Copy `assets/rubric.md` → `rubric.md` (fixed — don't edit).
3. Say: **"dig ideas overnight"** (override defaults inline, e.g. `DIG_MINUTES=30`).

Wake up to `IDEA_REPORT.md`: ranked survivors (one page each — pitch, gap + citations, method sketch, evaluation plan, risks & fallback, first 2-week milestone), a one-line killed list, and the cheapest kill-or-confirm experiment for the top pick.

Defaults: `NIGHT_BUDGET=8h`, `DIG_MINUTES=20`, `TARGET_KEEPS=5`, `KEEP_THRESHOLD=10/15`.
