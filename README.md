# concept-card

A skill for generating 4-page HTML concept cards from a local wiki knowledge base.

## What it does

When you trigger `/concept <term>`:

1. Searches your local wiki (`~/Documents/knowledge-base/wiki/`) for the concept
2. If found → generates a 4-page HTML card (cover, analogy, comparison, closing) and publishes to Artifact Library
3. If not found → suggests external search commands to supplement the wiki

## Card structure

| Page | Type | Content |
|------|------|---------|
| 1 | cover | Concept name + taxonomy + one-line definition |
| 2 | analogy | "Like what..." with 4 entry points |
| 3 | comparison | This concept vs most similar concept (4 points each) |
| 4 | closing | Significance for iOS indie dev + action items |

## Installation

### Prerequisites

The skill requires these scripts in your PATH:
- `concept-card-gen` — main script (generate → render → publish)
- `concept-to-html` — wiki search

These are installed at `~/.local/bin/` on the author's system. Either install them separately or modify the paths in SKILL.md.

### Install the skill

```bash
# Install via OpenClaw skill install command
openclaw skills add https://github.com/myartings/concept-card

# Or manually download SKILL.md to your skills directory
```

## Trigger

- `/concept <concept>`
- `概念卡片 <concept>`
- `/concept-card <concept>`

## Output

- Local: `~/Sync/docs/concept-<slug>.html`
- Published: Artifact Library URL