---
name: competitor-sweep
description: "Run a 12-point competitive intelligence sweep for any company. Checks homepage, meta tags, product pages, pricing, blog, case studies, careers, LinkedIn, G2/Capterra, news, integrations, and team pages for each competitor. Outputs structured markdown report with threat levels. Use when user says /competitor-sweep, 'check competitors', 'competitive sweep', 'competitor update', 'what are competitors doing', or 'weekly sweep'."
---

# Competitor Sweep

12-point competitive intelligence check per competitor. Produces a structured markdown report with threat levels and strategic implications.

## Input

If the user doesn't provide enough context, gather it interactively:

1. **Company name** — Who are we analyzing competitors for?
2. **Company website** — Their homepage URL
3. **Brief positioning** — What do they do, who do they serve? (1-2 sentences)
4. **Competitor list** — Names and website URLs of competitors to check. Minimum 2, recommend 3-5.
5. **Previous sweep?** — If the user has a prior sweep report (file path or pasted), load it for [NEW] change detection.

**Arguments:**
- `--competitors <list>` — Comma-separated competitor names/URLs (skip the interactive prompt)
- `--quick` — Skip checks 8-12, only run homepage through careers (checks 1-7)
- `--diff <filepath>` — Compare against a previous sweep file and highlight changes with [NEW] tags

## Workflow

### Step 1: Gather Context

If the user provides a client context file or pastes company info, use that. Otherwise ask the 5 questions above. You need enough context to:
- Understand what market the company operates in
- Know which competitors to check
- Write meaningful threat assessments (not just descriptions)

### Step 2: Load Previous Sweep (if provided)

If the user provides a previous sweep via `--diff` or by pasting/referencing a file, read it to establish a baseline for [NEW] change detection.

### Step 3: Run 12-Point Check Per Competitor

For EACH competitor, run these checks. Use Firecrawl (`firecrawl_scrape`) as primary web fetcher, WebFetch as fallback. Be efficient — skip checks that return nothing notable.

#### The 12-Point Checklist

1. **Homepage messaging** — Fetch homepage. Note hero headline, subheadline, primary CTA, social proof bar (logos, stats). Flag positioning shifts vs previous sweep.

2. **Meta tags** — Extract `<title>` and `<meta name="description">`. These change before visible content — early positioning signal.

3. **Product/feature pages** — Check main product or features page. Note new feature sections, removed pages, navigation structure changes.

4. **Pricing/packaging** — Fetch /pricing or equivalent. Note tiers, pricing model (per seat, per usage, flat), free trial availability, new add-ons.

5. **Blog/resources** — Fetch blog or resources page. List last 5 published posts (title + date). Note content themes and publishing cadence.

6. **Case studies/logos** — Check customers or case studies page. Flag new customer logos, new case study titles, social proof changes.

7. **Careers page** — Check /careers or equivalent. Count open roles. List strategic hires: leadership, marketing, product, engineering. Flag aggressive hiring (10+ roles) or specific GTM hires (e.g., ABM Manager = going upmarket).

8. **LinkedIn company page** — Web search for their LinkedIn. Note last 3-5 post topics and content formats. Flag high-engagement posts.

9. **G2/Capterra** — Search "[company] G2 reviews" or "[company] Capterra reviews". Note overall rating, review count, sentiment from recent reviews.

10. **Recent news** — Web search "[company name] news" limited to past 30 days. Flag funding, partnerships, product launches, acquisitions.

11. **Integration/partner page** — Check /integrations or /partners. Note listed integrations, partner logos, ecosystem plays.

12. **About/team page** — Check /about or /team. Flag leadership changes, new exec hires, team size claims.

### Step 4: Assess Threat Levels

For each competitor, assign a threat level based on findings:
- **HIGH** — Active product launches, aggressive hiring, new customer wins in our segment, positioning shifts toward our company
- **MEDIUM** — Steady activity, some growth signals, but not directly threatening
- **LOW** — Quiet, no major changes, maintenance mode

### Step 5: Write Strategic Implications

After all competitors are analyzed:
1. **Threats** — What competitive moves require a response?
2. **Opportunities** — What gaps or weaknesses can our company exploit?
3. **Recommended Actions** — 3-5 specific, prioritized next steps

### Step 6: Generate Report

Write the report as a markdown file. Save to the user's preferred location, or default to the current working directory as `competitor-sweep-{YYYY-MM-DD}.md`.

Use the output format from `references/output-format.md`.

## Parallelization

When sweeping 3+ competitors, use sub-agents to check competitors in parallel (max 3 concurrent). Each agent gets one competitor + the 12-point checklist + the company context for threat assessment.

## Tools Used

- **Firecrawl** (`firecrawl_scrape`) — Primary web content extraction
- **WebFetch** — Fallback for pages Firecrawl can't reach
- **WebSearch** — For LinkedIn, G2/Capterra, and recent news checks
- **Read/Write** — Context files and report output
