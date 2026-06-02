---
name: user-persona-discovery
description: Use when the user asks to brainstorm personas, define target users, create user profiles, or plan cultural probes for design research. Generates persona clusters, persona hypothesis cards (name, goal statement, behavioral summary, constraints, design implications), and probe design briefs through a structured conversational workflow. Produces living hypotheses grounded in observed evidence rather than invented demographics. Trigger phrases: "create personas", "define target users", "user profiles", "who are my users", "user research", "cultural probes", "research through design", "persona discovery".
metadata:
  references:
    - references/user-persona-examples.md
    - references/persona-design-principles.md
---

# User Persona Discovery

Generate evidence-grounded persona hypotheses through a structured conversation. Personas produced by this skill are behavioral archetypes — not demographic stereotypes — formatted as actionable design artifacts.

## Resources

| File | Contents |
|------|----------|
| `references/user-persona-examples.md` | 20 filled-in example personas covering common research software archetypes, ready to use as starting points or templates |
| `references/persona-design-principles.md` | Core principles for evidence-based, goal-oriented, behavior-centered persona design |

---

## Persona Output Format

Every persona produced by this skill follows this structure:

```
**Name:** [Functional label, e.g. "The Batch Job Runner"]
**Goal:** [One sentence — what they are trying to accomplish]
**Behaviors:** [3–5 bullet points of observable actions and strategies]
**Constraints:** [What limits or pressures shape their behavior]
**Tension:** [The core tradeoff they live with, e.g. "speed vs. correctness"]
**Design implications:** [What the system must do to support or not obstruct them]
**Confidence:** [High / Medium / Exploratory] — based on evidence quality
```

See `references/user-persona-examples.md` for 20 fully worked examples.

---

## Conversational Workflow

Run phases in order. Each phase ends with a checkpoint — confirm before advancing.

### Phase 1: Align on Purpose

**Ask:**
- What design decisions are you currently blocked on?
- What would a better understanding of your users allow you to try or stop trying?

**Output:** One sentence — *"These personas exist to help us decide ______."*

**Checkpoint:** Confirm the decision scope before gathering evidence.

---

### Phase 2: Surface Evidence

**Ask:**
- What user interactions, data, interviews, or observations are you drawing from?
- What surprised you? What felt inconsistent?

**Rules:** No interpretations, no persona names, no solutions yet — observations only.

**Output:** A raw list of observed behaviors, expressed goals, constraints, and tensions.

**Checkpoint:** Confirm the evidence list is complete before clustering.

---

### Phase 3: Cluster Patterns

Group observations by behavior and goal, ignoring demographics unless they directly affect behavior.

**Ask:** If we designed specifically for this cluster, what would change?

**Output:** 2–4 candidate clusters, each with: core goal · dominant behaviors · primary constraints.

**Checkpoint:** Confirm clusters are meaningfully distinct before drafting personas. If clusters overlap, merge or split before proceeding.

---

### Phase 4: Draft Persona Hypotheses

For each cluster, collaboratively answer:
- What is this persona trying to accomplish?
- What do they optimize for (speed, safety, accuracy, autonomy)?
- What breaks when the system fails to support them?
- What internal conflict does this persona live with?

**Output:** One persona card per cluster using the format above. Assign a confidence level.

**Checkpoint:** Review each persona card — does it feel *inevitable* given its constraints? If a card contains contradictions that aren't explained by competing incentives, revise before continuing.

---

### Phase 5: Stress-Test with Scenarios

Run 2–3 realistic use scenarios per persona:
- How would this persona use the current design?
- Where would they struggle or work around it?

**Critical question:** If this persona disappeared, would the design change? If not, revise or discard.

**Output:** Annotated scenario walkthroughs with friction points identified.

**Checkpoint:** Any persona that survives all scenarios without forcing a design change should be discarded.

---

### Phase 6: Make Assumptions Explicit

For each surviving persona, identify:
- Evidence sources used
- Weak or missing data
- Assumptions carried forward

Assign or revise confidence labels (High / Medium / Exploratory).

**Output:** Final persona cards with explicit confidence and assumption annotations.

---

### Phase 7: Reflect

- How did building these personas reshape the design problem?
- What design questions became sharper?
- Which personas need more fieldwork to validate?

**Output:** A brief reflection note capturing how the persona set changed the team's understanding.
