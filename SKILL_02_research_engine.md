# SKILL 02 — Research Phase: Filter, Cluster & Insight Extraction

## Purpose
This skill takes the raw collected data from Skill 01 and transforms it into structured, ranked intelligence. It filters noise, detects staleness, groups data into meaningful themes, extracts non-obvious insights, runs conflict detection, and outputs a ranked topic list ready for user selection.

---

## Trigger
Activates after Skill 01 completes. Input is the collection brief + raw data from the tiered source pull.

---

## Step 1 — Deduplication

Before any filtering:
- Remove exact duplicate URLs
- Remove semantic duplicates: articles covering the same event with no added perspective
- Keep the Tier A version if duplicates exist across tiers. If both are Tier B, keep the more detailed one.

---

## Step 2 — Staleness Detection

Tag every collected item with a staleness status. This is NOT purely based on publish date — it checks whether the underlying event or story is still developing.

### Status definitions:

**FRESH**
- Published within 48 hours AND
- The underlying story is still unresolved, developing, or recently surfaced
- Examples: ongoing regulatory review, earnings just released, product just launched, merger rumour just surfaced

**AGING**
- Published 2–5 days ago OR
- Published recently but the underlying event is resolved or fully covered
- Action: flag in output. Can be used if it introduces a unique angle not covered elsewhere.

**DEAD**
- Published 5+ days ago AND underlying event is closed/resolved
- OR: a story that was widely covered 2+ days ago with no new development
- Action: exclude from all analysis and post generation. Do not surface to user.

### Delta check rule:
Ask: "Is there anything still happening with this story right now?" If yes → FRESH or AGING. If no → DEAD regardless of publish date.

---

## Step 3 — Relevance Filter

Remove items that are:
- Not meaningfully connected to the stated industry or sub-focus
- Press release boilerplate with no signal content
- Pure product marketing with no industry implication
- Tier C items that don't reflect genuine discussion volume or sentiment

Keep items that:
- Contain a fact, shift, decision, or development with industry implications
- Surface a disagreement, reversal, or anomaly
- Reference a named entity from the user's key entities list
- Represent a regulatory, funding, or structural change

---

## Step 4 — Source Diversity Enforcement

Before a theme cluster is formed:
- Minimum 2 Tier A sources AND 1 Tier B source must contribute
- If a potential cluster has only 1 Tier A source, it is held as a WEAK SIGNAL — surfaced separately at the bottom of the output, not as a primary theme
- If a cluster has zero Tier A sources, it is excluded entirely

---

## Step 5 — Topic Clustering

Group surviving items into 5–15 theme clusters. Common cluster types (not exhaustive):

| Cluster type | Description |
|---|---|
| Market shift | Pricing, consolidation, volume changes |
| Technology adoption | New tool, platform, or method gaining traction |
| Regulatory change | New rules, enforcement action, policy shift |
| Industry disruption | A new entrant or model threatening incumbents |
| Talent / org signal | Leadership change, mass hiring, layoffs, restructure |
| Funding signal | Large raise, acquisition, or shutdown with implications |
| Narrative shift | How the industry is talking about itself differently |
| Conflict signal | When Tier A and Tier C data contradict each other |

Each cluster gets:
- A working title (4–6 words, not a sentence)
- A 2-line summary of what happened
- List of contributing sources with tier labels
- Staleness tag (FRESH / AGING — no DEAD items reach this stage)

---

## Step 6 — Conflict Detection

For each cluster, check: does the Tier A/B factual picture contradict what Tier C sentiment shows?

Examples:
- Tier A reports strong earnings. Tier C shows widespread negative sentiment among users/practitioners.
- Tier A shows regulatory approval. Tier C shows industry insiders questioning the decision.
- Tier B reports strong growth. Tier C shows confusion or disbelief.

If contradiction is detected:
- Flag the cluster with: `⚡ TENSION SIGNAL`
- Note: what the data says vs what the sentiment shows
- This cluster becomes a strong candidate for the Tension Report writing mode

---

## Step 7 — Insight Extraction

For each cluster, answer these questions before scoring:

1. **What happened?** — The factual event, precisely stated
2. **Why does it matter?** — The implication for the industry, not just the company
3. **Who is affected?** — Specific actors: incumbents, startups, consumers, regulators, investors
4. **What is changing?** — What will be different going forward
5. **What is non-obvious?** — The angle most people reading the news won't see
6. **What tension exists?** — Is there a contradiction, irony, or unexpected consequence

The answer to question 5 (non-obvious angle) is the most important input for the writing system. If no non-obvious angle exists, the cluster scores lower.

---

## Step 8 — Ranking

Score each cluster from 1–10 across five dimensions:

| Dimension | Description |
|---|---|
| Freshness | How live and developing is the story |
| Impact level | How broad and significant is the industry implication |
| Industry relevance | How directly connected to the stated sub-focus |
| Non-obvious angle | How much does the insight exceed what's already known |
| Discussion momentum | Tier C signal: is this being actively talked about |

Apply a +0.5 bonus for clusters with a confirmed TENSION SIGNAL.

Final score = weighted average. Freshness and Non-obvious angle carry highest weight.

---

## Output Format

Return to user:

```
RESEARCH OUTPUT — [Industry] / [Sub-focus]
Time window: [value] | Themes found: [n]
────────────────────────────────────────────

THEME 1 — [Working title]                          Score: [X.X/10]
Status: FRESH | Sources: 3×Tier A, 2×Tier B

What happened:   [1 sentence]
Why it matters:  [1 sentence]
Who's affected:  [list]
Non-obvious:     [1 sentence — the angle to write from]
Tier C sentiment: [1 line — tone + volume]
[⚡ TENSION SIGNAL: [what contradicts what] — if applicable]

THEME 2 — ...

────────────────────────────────────────────
WEAK SIGNALS (single-source, surfaced for awareness only):
[list]

────────────────────────────────────────────
→ Select a theme number to begin post generation.
   System will suggest a writing mode. You can override.
```

---

## Rules

- Never surface a cluster with zero Tier A sources as a primary theme
- Never mix Tier C data into factual content — it appears only in the sentiment line
- The non-obvious angle must be genuinely non-obvious — not just a restatement of the headline
- If fewer than 3 scoreable themes exist, report this honestly rather than padding with weak clusters
- Conflict clusters are always surfaced, even if their overall score is lower than non-conflict themes
