# SKILL 05 — Post Quality Control & Anti-Patterns

## Purpose
This skill runs as a final pass on every generated post before it is returned to the user. It checks for quality failures, anti-patterns, and the core test: does this post feel like news, or does it feel like someone who lives inside this industry?

It also checks mobile formatting, hook strength, and close quality. If failures are found, it flags them or auto-corrects before output.

---

## Trigger
Activates automatically after Skill 04 generates a post draft. Runs silently — the user does not see this check unless a significant issue requires flagging.

---

## Test 1 — The News Test

Read the post and ask: could someone have written this by reading the same headlines without any additional analysis?

**Pass:** The post adds an angle, implication, or perspective that is NOT present in any single source article.

**Fail:** The post is a rewrite or assembly of what the articles already say.

If FAIL: return to Skill 04. Identify the non-obvious angle from Skill 02 and rebuild the body around it. The news is context. The non-obvious angle is the post.

---

## Test 2 — The Authority Test

Read the post and ask: does this feel like someone who has been watching this industry closely, or someone who read about it today?

Indicators of PASS:
- Contains a connection between this event and something broader that an outsider wouldn't make
- Uses precise, industry-appropriate language without over-explaining it
- Does not treat the audience as if they need the basic context explained
- Has a point of view — not just information delivery

Indicators of FAIL:
- Reads like a briefing document
- Explains things the intended audience already knows
- Takes no position, makes no claim, adds no angle
- Could have been written by anyone with access to a news aggregator

If FAIL: identify the weakest paragraph and replace it with the most specific, non-obvious insight from Skill 02's extraction.

---

## Test 3 — Hook Strength Check

Read the first line. Ask: would a professional scrolling their feed stop here?

**Pass conditions (need ONE of):**
- Contains a specific, concrete detail that creates immediate curiosity
- States something the reader hasn't heard framed this way before
- Opens a tension or question that demands resolution
- Signals that the author has access to a layer the reader hasn't seen

**Fail conditions (any of):**
- First line is a context-setter ("This week in [industry]...")
- First line starts with "I" followed by something about the author's opinion
- First line summarises what happened rather than creating a reason to read further
- First line could apply to any week or any industry

If FAIL: return to Skill 03. Select the next-best hook archetype and regenerate the opening.

---

## Test 4 — Close Quality Check

Read the final line. Ask: does this end on forward motion, or does it trail off into summary?

**Pass:** Ends with a question, an implication, a tension left open, a direct challenge, or a resonant observation.

**Fail:** Ends with a summary of what was just said, a call to like/comment/share, or a generic platitude.

Prohibited closings:
- "What do you think? Let me know in the comments."
- "The future of [industry] depends on how we respond."
- "These are exciting times for [industry]."
- "Follow me for more insights like this."
- Any sentence that begins with "In conclusion" or "To summarise"

If FAIL: rewrite the final line only. Keep the penultimate paragraph. The new close should feel like a door left intentionally ajar.

---

## Test 5 — Fact Integrity Check

For every specific claim in the post:
- Confirm it traces to a Tier A or Tier B source from Skill 02's collection
- Confirm no numbers, names, dates, or company facts were generated without a source
- Confirm future-oriented statements use appropriate uncertainty language ("if," "likely," "could," "the evidence suggests")

**Pass:** All claims sourced. No fabricated specifics. Predictions framed as scenarios.

**Fail:** Any claim that cannot be traced to a collected source.

If FAIL: remove the unsourced claim and replace with a sourced one, or remove the line entirely. Do not keep unsourced specifics even if they seem plausible. Flag to user if significant content must be removed.

---

## Test 6 — Mobile Format Check

Apply to the full post:

| Rule | Check |
|---|---|
| No paragraph longer than 3 lines on mobile (~60 chars/line) | Scan for dense blocks |
| First line of each paragraph carries weight independently | Read each opener in isolation |
| No two consecutive lines start with the same word | Line-by-line check |
| White space is intentional — used at thought transitions | Check spacing placement |
| Post ends on action or curiosity, not summary | Final line check |

If any mobile rule fails: reformat the offending paragraph. Split dense blocks. Vary sentence openers.

---

## Test 7 — Anti-Pattern Scan

Scan for prohibited patterns. Automatic flag or removal:

### Prohibited phrases (auto-remove or rewrite):
- "In today's fast-paced world"
- "I'm excited/thrilled/proud to share"
- "Let that sink in"
- "This is a game-changer"
- "The future is now"
- "Now more than ever"
- "At the end of the day"
- "It's no secret that"
- "In conclusion" / "To summarise"
- "Thoughts?" as a standalone closer
- "Drop a comment below"
- "If you found this valuable, please share"
- Any use of "synergy," "disruption" (as noun without specific context), "ecosystem" (without specific context), "leverage" (as verb)

### Structural anti-patterns (flag for review):
- Post opens and closes with questions (weakens authority — open OR close with a question, not both)
- More than one rhetorical question in the body (dilutes impact)
- Three or more consecutive one-line paragraphs (fragmented — at least one paragraph needs 2–3 lines to build weight)
- Every paragraph ends with a declarative statement (no rhythm variation — mix with implications and questions)

---

## Test 8 — Cliché Detection for Industry-Specific Language

Check for industry clichés that feel hollow even when technically accurate:

Examples by domain (not exhaustive):
- Fintech: "democratising finance," "the unbanked," "seamless experience"
- AI/Tech: "at scale," "unlock value," "harness the power of"
- Healthcare: "patient-centric," "transforming care," "the future of medicine"
- Climate/Energy: "net zero journey," "the energy transition," "sustainable future"

These phrases are not prohibited outright — they are flagged for review. If they appear, the system checks: is this phrase carrying real meaning in context, or is it filler? If filler, replace with the specific reality it was meant to describe.

---

## Output After QC

If all 8 tests pass: return the post to user as final output.

If 1–2 tests fail and are auto-correctable: correct silently, return final post.

If 3+ tests fail: rebuild the post body using the correct framework from Skill 04 before returning. Do not return a post that fails multiple core tests.

Flag to user only if:
- Significant factual content had to be removed due to no source
- The hook had to be replaced entirely
- The theme's non-obvious angle could not be preserved after corrections

---

## The Final Reader Test

Before returning the post, run this mental check:

"If someone in this industry read this, would they think: *this person knows something* — or would they think: *I could have found this on Google*?"

If the answer is the second option, the post is not ready. Return to Skill 04.
