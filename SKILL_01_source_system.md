# SKILL 01 — Industry Configuration & Source System

## Purpose
This skill handles the entry point of the entire system. It receives the user's industry input, constructs a targeted source list across three tiers, and prepares a structured data collection payload for the Research Phase. It does NOT summarise or analyse — it only gathers, structures, and weights.

---

## Trigger
This skill activates when:
- A user provides an industry name or sub-focus for intelligence gathering
- A session begins and no prior industry configuration exists
- A user requests a fresh research run on a new or updated industry

---

## Step 1 — Receive & Parse Industry Input

Accept free-form input. Extract:

| Field | Example |
|---|---|
| Industry | Fintech |
| Sub-focus (optional) | Payments infrastructure |
| Time window | 24h / 3 days / 7 days (default: 3 days) |
| Key entities (optional) | Company names, regulators, competitors |
| Persona context (optional) | "I'm a VP of Product at a B2B SaaS company" |

If sub-focus is not provided, infer the most relevant sub-focus from the industry name before proceeding.

If time window is not provided, default to **3 days**.

---

## Step 2 — Construct Tiered Source List

Dynamically build a source list for the given industry. Do not use pre-loaded static feeds. Generate the list from the industry input.

### Tier A — Primary Truth Sources
**Weight: HIGH. Used for factual accuracy. All claims must be anchored here.**

Identify which of the following apply to the stated industry:

- Major financial/business wire services (Reuters, Bloomberg, Financial Times, WSJ, AP)
- Official company blogs / investor relations pages (for named entities)
- Government and regulatory body publications (e.g. SEC, FCA, RBI, FDA — match to industry)
- Central bank communications (if financial sector)
- Peer-reviewed research / whitepapers (if tech or science sector)
- Standards bodies and trade associations

Output: list of 5–10 Tier A sources specific to the industry.

### Tier B — Industry Context Sources
**Weight: MEDIUM. Used for direction, depth, interpretation. Never primary.**

Identify relevant:

- Vertical trade publications (e.g. Finextra for fintech, MedCity for healthtech, The Information for tech)
- Analyst reports and intelligence firms (e.g. CB Insights, Gartner, Forrester — match to industry)
- Reputable startup/ecosystem trackers
- Company announcements and press releases
- LinkedIn thought leader publications (high-engagement, verified authors only)
- Industry-specific newsletters with editorial standards

Output: list of 5–12 Tier B sources specific to the industry.

### Tier C — Social Signal Sources
**Weight: LOW. Sentiment and discussion trend detection only. NEVER used as factual source. NEVER cited in posts.**

Always include:
- Reddit (identify the 2–3 most relevant subreddits for the industry)
- X / Twitter (identify relevant hashtags and account clusters)
- LinkedIn public discussions (trending posts, comment threads on major topics)

Output: list of Tier C signal channels with specific subreddit/hashtag targets.

---

## Step 3 — Build Collection Parameters

Output a structured collection brief:

```
INDUSTRY CONFIGURATION
─────────────────────
Industry:         [value]
Sub-focus:        [value]
Time window:      [value]
Key entities:     [list or "none"]
Persona context:  [value or "not provided"]

TIER A SOURCES (HIGH weight)
[source 1]
[source 2]
...

TIER B SOURCES (MEDIUM weight)
[source 1]
[source 2]
...

TIER C CHANNELS (LOW weight — sentiment only)
[channel 1]
[channel 2]
...

COLLECTION READY → pass to SKILL 02
```

---

## Rules

- Source list is always built dynamically from the industry input. Never use a generic default list.
- Tier A must contain at least 3 sources before collection proceeds. If fewer than 3 exist for the industry, flag this before continuing.
- Tier A is always searched and exhausted before Tier B is consulted.
- Tier C data is collected in parallel but tagged separately and never mingles with factual content.
- If the user provides a persona context, note it here — it will affect relevance scoring in Skill 02 and tone selection in Skill 05.
- Do not begin analysis, clustering, or any writing at this stage. This skill outputs a collection brief only.
