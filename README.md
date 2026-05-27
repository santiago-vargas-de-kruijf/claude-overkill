# claude-overkill

A Claude skill that takes the current problem a step beyond its pragmatic answer. Invoke it with `/overkill` (or just say "overkill this") and Claude will surface advanced alternatives — data structures, distributed-systems algorithms, niche frameworks, design patterns, and frontier tooling — each rated on a calibrated complexity scale, with the concrete future-scale scenario in which it becomes the correct choice.

This skill is for explorers and engineers who already know the simple answer and want to map what lies past it: the techniques, trade-offs, and frontier ideas they would otherwise never encounter. It is **not** a default recommendation engine — the suggestions are deliberately maximalist, and the complexity score is the honest signal of cost.

## Install

### Option 1 — As a plugin via the marketplace (recommended)

Inside any Claude Code session:

```
/plugin marketplace add santiago-vargas-de-kruijf/claude-overkill
/plugin install overkill@overkill-marketplace
```

### Option 2 — Manual install (no marketplace)

Personal (available in every project):

```bash
git clone <this-repo> /tmp/claude-overkill
cp -r /tmp/claude-overkill/plugins/overkill/skills/overkill ~/.claude/skills/
```

Project-local (shared with collaborators via git):

```bash
mkdir -p .claude/skills
cp -r path/to/this-repo/plugins/overkill/skills/overkill .claude/skills/
```

## Usage

In any Claude Code conversation about a concrete problem:

- `overkill` — propose advanced alternatives for the current discussion
- `overkill this` / `/overkill` — same thing
- `overkill --max` — restrict output to the highest-complexity options (🔥 7+)
- `overkill --advanced` — switch the comparison table to operator-focused columns (ops burden, hiring difficulty, time to first commit)
- `overkill --current` — use web search to verify references and check project health (adds latency)

Flags can be combined freely: `overkill --max --advanced --current`.

Claude will return ranked alternatives with complexity scores, a comparison table, and the concrete scale or condition under which each option starts to earn its cost.

## Repository layout

```
claude-overkill/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   └── overkill/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           └── overkill/
│               └── SKILL.md
└── README.md
```
