# SKILL 03 — Hook Selection Engine

## Purpose
This skill selects or generates the opening hook for a post. It runs before any writing begins. The hook is chosen based on the nature of the insight, not the writing mode — the mode shapes everything after line one. A great hook is the only thing standing between the post being read and being scrolled past.

The goal: make the reader stop. Not because it's clickbait. Because it feels like someone who actually knows something just said something they shouldn't have to say out loud.

---

## Trigger
Activates after the user selects a theme from Skill 02 output and before the writing mode generates the body. Takes as input: the selected theme's insight extraction (what happened, why it matters, non-obvious angle, tension signal if present).

---

## Hook Archetypes

The system selects from these archetypes — or combines elements of two — based on the insight. Never use the same archetype twice in a row across sessions if memory is enabled.

---

### Archetype 1 — The Precision Signal
A specific, dateable, concrete detail that most people missed or haven't connected yet. Numbers work here only when the number itself is the story — not as decoration.

**When to use:** The non-obvious angle is a specific fact or data point that carries weight on its own.

**Structure:** [Specific detail]. [Why that matters — unstated implication, one line].

**Examples:**
- "Three central banks updated their guidance in the same week. That's never happened before."
- "The product launched quietly on a Friday. By Monday, two incumbents had changed their pricing."
- "One sentence in the regulatory filing changed the entire acquisition math."

---

### Archetype 2 — The Broken Assumption
The industry believes X. Something happened this week that makes X questionable.

**When to use:** The theme challenges a widely held belief, prediction, or narrative.

**Structure:** [State the consensus clearly and fairly]. [Then break it with one line].

**Examples:**
- "The 'AI will replace compliance teams' narrative has been running for two years. One event this week runs directly against it."
- "Everyone agreed the market had stabilised. Then three things happened on the same day."
- "The playbook said do Y. The companies that did Y are now restructuring."

---

### Archetype 3 — The Uncomfortable Question
A question that the reader has probably avoided asking — because the answer is awkward.

**When to use:** The insight reveals a contradiction, irony, or gap between stated intentions and actual behaviour.

**Structure:** Direct question. No preamble.

**Examples:**
- "What if the regulation designed to protect consumers is the thing making them most vulnerable right now?"
- "If this merger makes strategic sense, why are three of their biggest clients asking about exit clauses?"
- "At what point does 'moving fast' become 'not checking'?"

---

### Archetype 4 — The Pattern Nobody Named
Multiple unconnected events this week that form a pattern nobody has explicitly labelled yet.

**When to use:** The cluster contains 3+ signals that seem unrelated but point to the same underlying shift.

**Structure:** [Event 1]. [Event 2]. [Event 3]. [These aren't separate stories.]

**Examples:**
- "A funding round pulled. A VP departure announced. A product timeline slipped. Three different companies. One explanation."
- "Two acquisitions and a partnership announcement, all in the same sub-sector, all in the same week. The market is consolidating around something specific."

---

### Archetype 5 — The Clock
There is a window. It is closing. Most people haven't noticed it yet.

**When to use:** The theme has a time-sensitive strategic implication — a regulatory deadline, a first-mover window, a market gap that closes when incumbents respond.

**Structure:** [What the window is]. [How long it's open]. [Who most people think has time].

**Examples:**
- "There is a gap in this market that exists specifically because the largest player is distracted right now. That gap has a shelf life."
- "The compliance window closes in Q1. The companies that act in the next 60 days get protected status. The rest get reviewed."

---

### Archetype 6 — The Insider Frame
Written as if the author has access to information or context that others don't — because they've been paying close attention.

**When to use:** The non-obvious angle comes from connecting dots across multiple sources in a way that no single article does.

**Structure:** [Statement that signals deep context, not just news awareness].

**Examples:**
- "If you've been watching this sector for more than six months, what happened this week wasn't surprising. It was inevitable."
- "The headline says one thing. The underlying data in the same report says something different. Nobody's talking about the second number."
- "What looks like a product decision is actually a signal about their next fundraise."

---

### Archetype 7 — The Human Scene
Opens on a specific person or team in a specific moment. No names needed — the specificity of the situation is the hook.

**When to use:** The theme has a human consequence that most industry coverage misses. Works especially well for regulatory, workforce, and adoption stories.

**Structure:** [Specific human moment]. [What industry dynamic put them there].

**Examples:**
- "A risk team somewhere rewrote their entire onboarding protocol this month because of one paragraph in a guidance update."
- "There are engineers who built their entire career on a stack that three announcements this week just made legacy."
- "Someone is getting a very difficult call from their board this week because they were early to this market. Being early wasn't the problem."

---

### Archetype 8 — The Reversal
Something that was supposed to protect, enable, or accelerate the industry is now doing the opposite.

**When to use:** The insight reveals irony — a tool, policy, or move backfiring or producing the opposite of its intended effect.

**Structure:** [What it was supposed to do]. [What it's actually doing].

**Examples:**
- "The feature built to increase retention is the reason three enterprise clients are leaving."
- "The compliance framework designed to standardise the industry is now the biggest barrier to the innovation it was meant to protect."

---

### Archetype 9 — The Contrast Frame
Two things that should be comparable are behaving completely differently — and that gap tells you everything.

**When to use:** The theme contains a meaningful contrast between two companies, two regions, two approaches, or two time periods.

**Structure:** [Thing A]. [Thing B]. [The gap between them is the story].

**Examples:**
- "One company in this space raised a down round last month. Its direct competitor just hit a record valuation. Same market. Same product category. The reason for the difference is one decision made 18 months ago."
- "The European version of this regulation is working. The US version isn't. The difference isn't the rule — it's the enforcement mechanism."

---

### Archetype 10 — The Tension Statement
For TENSION SIGNAL clusters only. States the contradiction directly and treats it as intelligence rather than confusion.

**When to use:** Conflict detection flagged this cluster — Tier A and Tier C data contradict each other.

**Structure:** [What the data shows]. [What the market/practitioners are doing or saying]. [One of these is wrong. Or both are right and we're asking the wrong question].

**Examples:**
- "The fundamentals say strong. The sentiment says panic. Both are real. That combination usually means something specific is about to happen."
- "The official numbers show recovery. The people working inside the sector are all saying the same thing privately: it doesn't feel like recovery."

---

## Selection Logic

1. Read the non-obvious angle from Skill 02
2. Check if a TENSION SIGNAL is present → if yes, Archetype 10 is the default candidate (can be overridden)
3. For all other themes, match the nature of the insight to the archetype:
   - Specific overlooked fact → Archetype 1
   - Challenges consensus → Archetype 2
   - Reveals contradiction or irony → Archetype 3 or 8
   - Multiple signals forming a pattern → Archetype 4
   - Time-sensitive window → Archetype 5
   - Cross-source synthesis → Archetype 6
   - Human consequence angle → Archetype 7
   - Two-way comparison → Archetype 9
4. Generate 2 hook options from the best-fit archetype
5. Present both to the writing mode — the mode selects the one that best sets up its framework

---

## Rules

- The hook must work as a standalone sentence in a feed preview
- The hook cannot summarise the news. It must signal that what follows is the layer beneath the news.
- No exclamation points. No em-dash stacking. No "Here's what you need to know."
- If combining two archetypes, the first sentence is one archetype, the second sentence is the other. Maximum two.
- The hook does not need to be a complete thought — it can be a deliberate open question or a statement that demands continuation
- Never use a hook that could apply to any industry or any week. It must be specific to this insight.
