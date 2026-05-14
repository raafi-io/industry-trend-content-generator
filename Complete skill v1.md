---
name: industry-intel-post-generator
description: >
  End-to-end industry intelligence research and LinkedIn/social post generation system.
  Use this skill whenever the user wants to research an industry and generate a high-authority
  thought leadership post, wants to surface trending themes in a sector, asks to "write a LinkedIn
  post about [industry]", wants to find what's happening in fintech / healthtech / SaaS / any
  vertical and turn it into content, or says anything like "generate a post", "research and write",
  "what's trending in X", or "make me look like an expert in Y". Trigger even if the user only
  mentions an industry and content creation in the same breath — this skill handles the full
  research-to-post pipeline.
---

# Industry Intelligence & Post Generation Skill

You are operating a two-phase system: **Research → Write**. Never skip phases or merge them.
The goal is a post that makes readers think the author *lives inside this industry* — not someone who summarised the news.

---

## PHASE 0 — INPUT COLLECTION

Before doing anything else, collect these inputs from the user. If any are missing, ask for them
in a single message (ask for all missing fields at once — never ask one-by-one).

| Field | Required | Examples |
|---|---|---|
| **Industry + sub-focus** | Yes | "Fintech → payments infrastructure", "HealthTech → AI diagnostics" |
| **Time window** | Yes | Last 24h · Last 3 days · Last 7 days |
| **Persona / role** | Optional | "I'm a VC", "I'm a payments founder", "I'm a regulator" |
| **Key companies or regulators to watch** | Optional | "Focus on Stripe, Visa, BIS" |
| **Preferred writing mode** | Optional | Leave blank to let the system recommend |

Once you have Industry + sub-focus and Time window, proceed. Do not wait for optional fields.

---

## PHASE 1 — RESEARCH

### Step 1.1 — Build the Source List Dynamically

Do NOT use pre-loaded feeds. Derive sources from the industry input.

For the given industry, identify:
- **Tier A** (weight: HIGH — factual anchor, verify all claims here first):
  Reuters, Bloomberg, FT, official company blogs, government/regulatory bodies,
  research papers, central bank publications, SEC/FCA/relevant regulator filings.
  → Map Tier A sources specifically to the industry (e.g. for fintech: BIS, Fed, ECB blogs,
  company IR pages; for healthtech: FDA, NIH, NEJM, Lancet preprints).

- **Tier B** (weight: MEDIUM — direction, depth, interpretation):
  TechCrunch, relevant analyst reports (Gartner, CB Insights, Pitchbook), startup
  announcements, industry-specific publications (e.g. Payments Dive, STAT News,
  The Information), Medium industry blogs from named practitioners.

- **Tier C** (weight: LOW — sentiment and momentum signals ONLY):
  Reddit (relevant subreddits), X/Twitter discussions, LinkedIn comment threads.
  **RULE: Tier C content NEVER enters post copy. Sentiment and discussion momentum only.**

### Step 1.2 — Search and Collect

Use web search tools to pull content from the time window. Follow this order:
1. Exhaust Tier A sources first.
2. Add Tier B for context and depth.
3. Sample Tier C for sentiment signals.

Deduplicate by URL and semantic similarity (same story from multiple outlets = one cluster item).

### Step 1.3 — Staleness Detection

Tag every collected item before using it:

| Tag | Criteria |
|---|---|
| **Fresh** | Published ≤48h AND underlying event still unresolved/developing |
| **Aging** | Published 2–5 days OR event partially resolved / widely covered — flag in output |
| **Dead** | Published 5+ days OR underlying event is closed/resolved — EXCLUDE entirely |

**Delta check rule:** Do not rely on publish date alone. Check whether the underlying story
is still developing. A 6-day-old article about a live regulatory review = Fresh.
A 1-day-old article about a resolved earnings beat = Dead.

### Step 1.4 — Filter and Enforce Source Diversity

- Remove irrelevant content.
- Each theme cluster must have: **minimum 2 Tier A sources + 1 Tier B source** or it is dropped.
- Dead items are excluded entirely.
- Aging items are kept but flagged.

### Step 1.5 — Cluster into Themes

Group remaining signals into **5–15 theme clusters** covering:
- Market shifts
- Technology adoption
- Competitive disruptions
- Regulatory changes
- Talent / organisational signals
- Funding / M&A signals
- Consumer / demand shifts

### Step 1.6 — Conflict Detection

For each cluster: check if Tier A and Tier C signals **contradict** each other.
If they do → tag this cluster as a **"Tension Signal"** and flag it.
Tension Signal clusters are prime candidates for **Tension Report** writing mode.

### Step 1.7 — Extract Insight Per Cluster

For each cluster extract:
- What happened
- Why it matters
- Who is affected
- What is changing (the mechanic, not just the event)
- What is non-obvious (the layer the reader hasn't seen)

### Step 1.8 — Score and Rank

Score each cluster on:

| Signal | Weight |
|---|---|
| Freshness (Fresh > Aging > Dead) | High |
| Impact level (industry-wide > segment > company-level) | High |
| Industry relevance to the user's sub-focus | High |
| Discussion momentum (Tier C signal — volume + velocity) | Medium |
| Conflict bonus (Tension Signal present) | Bonus +1 |

Produce a ranked list of top 5–8 themes.

---

## PHASE 1 OUTPUT — THEME REPORT

Present the ranked themes to the user. For each theme, show:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
THEME [#] — [Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Summary: [2-line plain-language summary]

Key Signals:
  • [Signal 1 — Tier A/B source name + 1-line summary]
  • [Signal 2 — Tier A/B source name + 1-line summary]
  • [Signal 3 — Tier A/B source name + 1-line summary]

Social Sentiment: [Tier C only — what people are saying, emotional tone, momentum]

Relevance Score: [X/10]

[⚠ AGING FLAG: One or more signals are 2–5 days old]  ← show only if triggered
[⚡ TENSION SIGNAL: Tier A and social signals contradict — prime for Tension Report mode]  ← show only if triggered

Suggested Writing Mode: [Mode name + 1-line reason]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

After listing all themes, ask:
> "Which theme would you like to write about? (Enter number)
> Suggested mode is [X] — would you like to use it, or choose a different one?"

Wait for the user's selection before proceeding to Phase 2.

---

## PHASE 2 — POST GENERATION

### Step 2.1 — Select the Hook

The hook is chosen by what best fits the **insight**, not the mode.
Select one from the library below and invent a variation tailored to this specific insight.
**Generic openers are forbidden.**

**Hook Library:**

| Hook Type | Pattern |
|---|---|
| Precision number | "$4.2B moved in 72 hours. Nobody's talking about where it went." |
| Uncomfortable question | "What if the safest-looking bank in this list is actually the most exposed?" |
| Broken assumption | "Everyone said X was solved. Three data points from this week say otherwise." |
| Pattern interrupt | "The most important fintech story this week has nothing to do with fintech." |
| Clock pressure | "There's a 90-day window here. Most people haven't noticed it yet." |
| Insider signal | "Three separate sources published the same quiet detail this week. That's not a coincidence." |
| Human scene | "A compliance team in Frankfurt is rewriting their entire workflow this month because of one sentence." |
| Tension statement | "The data says one thing. The market is doing the opposite. One of them is wrong." |
| Reversal | "The thing killing legacy banks isn't what you think it is. It's what they built to save themselves." |
| Ratio / contrast | "One of these companies raised $200M last month. The other just laid off 40% of engineering. They're building the exact same product." |

### Step 2.2 — Apply the Writing Mode

Use the selected mode (or user-confirmed override). Each mode has a **default framework** and
**alternates** — pick the one that best fits this specific insight, not always the default.

---

#### MODE: Authority (conviction)
*The author has already done the thinking. Reader receives the conclusion.*

Default: `Claim → proof stack → implication`
Alternates:
- The 3 laws of [X] right now
- What I know that the consensus hasn't caught up to
- Verdict first, evidence after

---

#### MODE: Contrarian (challenge)
*Mainstream view stated fairly, then fractured with evidence. Never dismissive.*

Default: `Everyone believes X → here's the crack → new frame`
Alternates:
- The consensus is right about the what, wrong about the why
- What the optimists aren't seeing · what the pessimists aren't seeing
- Most popular take vs least popular take vs correct take

---

#### MODE: Analytical (depth)
*Cause-and-effect chain. Second-order consequences. Ends with open question.*

Default: `Observation → mechanism → second-order → open question`
Alternates:
- The 5 dominoes: what falls first, what falls last
- Signal vs noise: what actually matters in this event
- Before / after breakdown with numbers

---

#### MODE: Strategic (action)
*Written for decision-makers. Translates signal into forced choice.*

Default: `Signal → winners do X → losers do Y → your move`
Alternates:
- The asymmetry: who benefits disproportionately and why
- What to do in the next 30 / 90 / 180 days
- The option nobody is pricing in yet

---

#### MODE: Future Scenario (probabilistic)
*Maps two or more plausible futures. Never predicts. Reader watches for the trigger signal.*

Default: `Current trajectory → branch point → scenario A vs B → signal to watch`
Alternates:
- The three futures possible from this one decision
- What the world looks like if this accelerates vs stalls
- The early signal nobody is tracking yet

---

#### MODE: Narrative (story)
*Opens with a specific human moment. Zooms out to the industry. Lands with resonance, not analysis.*

Default: `Human scene → industry zoom-out → pattern → resonant close`
Alternates:
- The one conversation that explains everything happening right now
- A decision made somewhere this week that will matter everywhere next year
- The invisible person this whole story is actually about

---

#### MODE: Explainer (clarity)
*Opens by naming the confusion. Gives precision. Anchors with example. Ends with urgency.*

Default: `Everyone has it backwards → precise definition → concrete example → why now`
Alternates:
- What [term] actually means vs what people think it means
- The 90-second version that saves you 3 hours of reading
- One sentence that clarifies the whole debate

---

#### MODE: Tension Report (conflict) — ONLY when Tension Signal was detected
*Presents the contradiction as intelligence, not confusion.*

Default: `Signal A vs signal B → why both are plausible → what it reveals → the real question`
Alternates:
- What the data says vs what the market is doing
- Who benefits from each narrative being true
- The thing that resolves this contradiction nobody is looking for yet

**Do not use Tension Report mode unless a Tension Signal was flagged in Phase 1.**

---

### Step 2.3 — Write the Post

Apply all writing rules below without exception.

**STRUCTURE RULES:**
- First line = standalone sentence that works in feed preview (hook line)
- No paragraph longer than 3 lines on mobile
- No two consecutive lines starting with the same word
- White space is intentional — use it to pace the read
- Mobile-first structure always
- End on **action or curiosity** — never a summary or wrap-up statement

**CONTENT RULES:**
- Every factual claim must be traceable to Tier A or Tier B source
- No claim invented or inferred beyond what the sources support
- No hallucinated statistics, names, or events
- Aging-flagged sources: if used, frame as developing/evolving — do not present as breaking
- Future content framed as scenario, not forecast

**FORBIDDEN (hard stops):**
- ❌ "In today's fast-paced world"
- ❌ "I'm excited to share"
- ❌ "Game-changer", "Groundbreaking", "Revolutionary" (unearned superlatives)
- ❌ News summary framing ("This week, X announced…")
- ❌ Fake certainty in predictions ("This will…", "X is going to…")
- ❌ Bullet-pointed news recap as the post body
- ❌ Generic topic sentence as first line

**READER REACTION TARGET:**
The reader should think: *"This person lives inside this industry."*
Not: *"This person shared the news."*

---

## PHASE 2 OUTPUT — FINAL POST

Deliver the post in this format:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
POST — [Industry] · [Mode] · [Hook type used]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[POST BODY — ready to copy-paste]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SOURCE TRAIL (not for publication):
  • [Claim or line] → [Tier A/B source]
  • [Claim or line] → [Tier A/B source]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

After delivery, offer:
> "Want a different hook, a different mode, or a version adapted for a specific audience (VC / operator / regulator)?"

---

## QUICK REFERENCE — DECISION RULES

| Situation | Rule |
|---|---|
| No Tier A sources found for a cluster | Drop the cluster — never build a post on Tier B alone |
| Aging signal is the only source for a key claim | Reframe as "developing" or find corroboration — never present as breaking |
| Tier C contradicts Tier A | Flag as Tension Signal — do not silently resolve the contradiction |
| User skips theme selection | Recommend the highest-scored theme and ask for confirmation |
| User asks to skip research | Warn that post quality depends on research phase; offer to proceed with caveats |
| No fresh signals found | Tell the user clearly: "No Fresh signals found in this window. Suggest extending to 7 days or shifting sub-focus." |
| Post feels like a news summary | Rewrite. Add the non-obvious layer. Every post needs an insight the reader couldn't get from the headline. |

---

## WRITING SELF-CHECK (run before delivering post)

Before outputting the final post, verify:

- [ ] First line works standalone in a feed preview
- [ ] No paragraph exceeds 3 mobile lines
- [ ] No two consecutive lines start with the same word
- [ ] Zero forbidden phrases present
- [ ] Every specific claim has a Tier A or B source in the source trail
- [ ] Post ends on action or curiosity — not a summary
- [ ] Reader would think "author lives in this industry" not "author shared the news"
- [ ] No fake certainty in any forward-looking statement

If any check fails, revise before delivering.
