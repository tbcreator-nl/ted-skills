# Ted Skills

Shared Claude Code skills. Drop any skill folder into your `.claude/skills/` directory to use it.

## Available Skills

| Skill | Command | What it does |
|-------|---------|-------------|
| **competitor-sweep** | `/competitor-sweep` | 12-point competitive intelligence check across all competitors for a company. Checks homepage, meta tags, product pages, pricing, blog, case studies, careers, LinkedIn, G2/Capterra, news, integrations, and team pages. Outputs structured markdown report with threat levels and strategic recommendations. |

## Installation

### Option 1: Clone and symlink (recommended)

```bash
# Clone this repo somewhere on your machine
git clone https://github.com/tbcreator-nl/ted-skills.git ~/ted-skills

# Symlink skills you want into your Claude Code skills directory
ln -s ~/ted-skills/competitor-sweep ~/.claude/skills/competitor-sweep
```

### Option 2: Copy directly

```bash
# Copy a skill into your Claude Code skills directory
cp -r competitor-sweep ~/.claude/skills/competitor-sweep
```

## Usage

Once installed, use the skill in Claude Code:

```
/competitor-sweep
```

The skill will prompt you for:
1. Company name and website
2. Brief positioning (what they do, who they serve)
3. Competitor list (names + URLs)

Optional flags:
- `--quick` — Only run checks 1-7 (skip LinkedIn, G2, news, integrations, team)
- `--diff <filepath>` — Compare against a previous sweep and tag changes with [NEW]
- `--competitors <list>` — Skip the interactive prompt, pass competitors directly

## Output

Each sweep produces a markdown file (`competitor-sweep-YYYY-MM-DD.md`) with:
- TL;DR table with threat levels per competitor
- Per-competitor sections covering all 12 check points
- Market context and your company's own position
- Strategic implications: threats, opportunities, recommended actions

## Contributing

To add a new skill:
1. Create a folder with your skill name
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`)
3. Optionally add `references/`, `scripts/`, or `assets/` subdirectories
4. Open a PR

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed
- Web access tools (Firecrawl MCP recommended, WebFetch as fallback)
