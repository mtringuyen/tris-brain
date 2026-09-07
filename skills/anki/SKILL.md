---
name: anki
version: 1.0.0
description: "Summarize articles into durable knowledge and generate Anki spaced-repetition cards. Use when the user shares a blog post or article URL, pastes content to learn, says 'teach me about [topic]', 'summarize this', 'make Anki cards from this', 'turn this into flashcards', or wants to remember what they read. Produces an exhaustive summary with elaborative interrogation, then atomic cards added via the anki-mcp server."
---

# Anki — High-Signal Knowledge Synthesizer

Transform content → durable knowledge. Combat fluency illusion.

**Core principle:** Understand first, memorize second. Anki retains — it doesn't teach.

## Input
- Blog/article URL or pasted content
- OR "teach me about [topic]"

---

## Output 1: Summary

**EXHAUSTIVE.** Capture EVERY concept, framework, mental model, and key claim. Kill fluff, never kill substance. If source has 20 concepts, summary has 20 concepts.

### Elaborative Interrogation (Required)

For each concept extracted, answer: **"Why is this true?"**

This step is critical — [Dunlosky et al.](https://www.aft.org/ae/fall2013/dunlosky) found elaborative interrogation significantly more effective than passive summarization. Don't just extract facts; interrogate them.

### Structure

```
## [Title]

**Thesis:** [1 sentence core argument]

### Key Concepts
- **[Term]** — definition + why it matters + WHY is this true?
- **[Term]** — definition + why it matters + WHY is this true?
[Scale to content: 3 concepts = 3 bullets, 30 = 30. Miss NOTHING.]

### Relationships
- [Concept A] → [Concept B]: [how they connect]
- [Concept X] vs [Concept Y]: [key distinction]
[Map ALL connections between concepts]

### Connections to Prior Knowledge
- **Builds on:** [existing knowledge this extends]
- **Contrasts with:** [similar ideas to distinguish]
- **Enables:** [what understanding this unlocks]

### Why It Matters
[1-2 sentences — so what? why care?]
```

### Summary Rules
1. **Exhaustive extraction** — every distinct idea gets a bullet
2. **Preserve nuance** — include caveats, conditions, exceptions
3. **Map relationships** — how concepts connect to each other
4. **Connect outward** — link to prior knowledge
5. **No orphan concepts** — everything connects to something
6. **Interrogate each concept** — ask "why is this true?" and include the answer

---

## Output 2: Anki Cards

### Philosophy

**Research-backed principles (qualitative — attach specific numbers only when verified against the source):**
- Retrieval practice substantially outperforms re-reading (Karpicke & Roediger, 2008)
- Atomic cards outperform complex cards (Wozniak's minimum information principle)
- Self-explanation and "why" questions improve transfer (Chi et al.)
- Target roughly 85-90% retention — retrieval should mostly succeed, not always (desirable difficulties, Bjork)
- Cluster cards — isolated single cards decay fast
- Far transfer is rare — explicitly practice applying concepts in varied contexts

### Pre-Card Gate: Self-Explanation Test

**STOP. Before creating ANY cards, do this for each concept:**

1. Look away from source material
2. Explain the concept aloud in your own words
3. Include WHY it's true, not just WHAT it is

**If any check fails → re-read the source once, then retry the explanation.**

This operationalizes understanding. [Chi et al.](https://onlinelibrary.wiley.com/doi/10.1207/s15516709cog1803_3) showed high self-explainers all achieved correct mental models; low self-explainers did not.

Then verify FOR EACH CONCEPT:
- [ ] Can I explain this without looking? (understanding)
- [ ] Do I know WHY, not just WHAT? (depth)
- [ ] Does this connect to existing knowledge? (integration)

**If NO to any → flag in output, still create cards but note gap.**

---

### Required Card Types

**For every concept in summary, create at minimum Definition + Why. Full concepts (not bare terms/facts) also get Application + Context Transfer:**

| Type | Model | Purpose | Format | Scope |
|------|-------|---------|--------|-------|
| **Definition** | `1.  Basic (and reversed card)` | Term ↔ meaning (bidirectional) | "What is X?" / "Term for [definition]?" | Every concept |
| **Why** | `3. Basic` | Causal understanding | "Why does X happen/matter?" | Every concept |
| **Application** | `3. Basic` | When/how to use | "When would you apply X?" | Full concepts |
| **Context Transfer** | `3. Basic` | Test in different domain (fights transfer problem) | "How would X apply in [different context]?" | Full concepts |

**For connected concepts, also create:**

| Type | Model | Purpose | Format |
|------|-------|---------|--------|
| **Relationship** | `3. Basic` | How concepts connect | "How does X relate to Y?" |
| **Discrimination** | `3. Basic` | Distinguish similar concepts | "X vs Y: key difference?" |

**Optional (use sparingly):**

| Type | Model | When | Format |
|------|-------|------|--------|
| **Cloze** | `2.  Cloze` | Syntax, quotes, fixed phrases ONLY | `{{c1::answer}}` |
| **Example** | `3. Basic` | Abstract concepts needing varied contexts | "Give an example of X in [different domain]" |
| **Boundary** | `3. Basic` | When concept fails/doesn't apply | "When would X NOT apply?" |

---

### Card Rules (Evidence-Based)

**DO:**
1. **One fact per card** — atomic, answerable in <5 seconds
2. **Require production** — free recall, not recognition
3. **Add context hints** — `(re: [topic])` combats interference
4. **Cluster cards** — Definition + Why minimum per concept; full concepts add Application and Context Transfer
5. **Encode relationships** — how concepts connect matters as much as concepts themselves
6. **Terse causal links** — include WHY, drop filler words. Sacrifice grammar, keep causation.
   - Verbose: "This is because teams naturally create communication interfaces where they need to coordinate"
   - Terse: "Teams create interfaces at coordination points"
7. **Vary phrasing across cards** — same fact, different linguistic framing strengthens memory (Wozniak Rule #17: redundancy). Examples:
   - Card A: "What causes X?" → "Y leads to X because..."
   - Card B: "X results from what?" → "X is caused by Y — mechanism is..."
   - Card C: "Why does Y produce X?" → "Y → X because..."

**DO NOT:**
- Single orphan cards (isolated facts decay rapidly)
- Yes/no questions (too little retrieval effort)
- Lists in one card (split into multiple)
- Cloze for conceptual understanding (use for syntax/quotes only)
- Cards for content you don't understand
- Pattern-matchable answers (rephrase to require real recall)
- Same phrasing across related cards (vary wording)

---

### Card Quantity Guide

| Concept Complexity | Minimum Cards |
|-------------------|---------------|
| Simple term/fact | 2 (Definition + Why) |
| Core concept | 4-5 (Definition + Why + Application + Context Transfer + Relationship) |
| Complex framework | 6+ (above + Discrimination + Boundary + Examples) |
| Central thesis | 5-6 (multiple angles, connections, varied phrasing) |

**Heuristic:** N concepts → roughly 3-4× N cards. A guide for scale, not a quota — the card-type rules above decide what to create.

---

### Anki Integration

**Configuration:**
- Deck: `All`
- Models:
  - `1.  Basic (and reversed card)` — definitions (bidirectional)
  - `2.  Cloze` — syntax/quotes only
  - `3. Basic` — all other cards (why, application, relationship, discrimination, context transfer)

**Workflow:**
1. Generate summary (display to user)
2. Generate cards (display to user for review)
3. **STOP. Ask user: "Add these cards to Anki?"**
4. **Only proceed if user explicitly confirms (e.g., "yes", "add them", "go ahead")**
5. **Verify targets:** confirm deck `All` and the model names below exist in the collection (query via the anki-mcp server, e.g. `deckNames`/`modelNames`); use the collection's actual names if they differ. If this source was processed before, ask whether to re-add — `addNote` does not deduplicate.
6. `mcp__anki-mcp__sync` — sync before adding
7. `mcp__anki-mcp__addNote` for each card
8. `mcp__anki-mcp__sync` — sync after adding
9. Report: cards added, any failures

**CRITICAL: Never add cards without explicit user permission.**

**Card Formats:**

Definition (bidirectional):
```json
{
  "deckName": "All",
  "modelName": "1.  Basic (and reversed card)",
  "fields": {
    "Front": "What is [concept]? (re: [source/topic])",
    "Back": "[definition — one atomic fact]"
  }
}
```

Why/Application/Relationship/Discrimination/Context Transfer:
```json
{
  "deckName": "All",
  "modelName": "3. Basic",
  "fields": {
    "Front": "[Question requiring production] (re: [source/topic])",
    "Back": "[Answer with 'because' — explains, not just states]"
  }
}
```

Cloze (syntax/quotes only):
```json
{
  "deckName": "All",
  "modelName": "2.  Cloze",
  "fields": {
    "Text": "[Context with {{c1::answer}} for key term]"
  }
}
```

---

## Example

**Input:** Article about Conway's Law

### Summary

```
## Conway's Law

**Thesis:** Organizations ship systems that mirror their communication structure.

### Key Concepts
- **Conway's Law** — system architecture reflects org chart; teams build interfaces where they coordinate. WHY: coordination requires communication → communication paths become integration points → integration points become APIs
- **Inverse Conway Maneuver** — intentionally restructure teams to achieve desired architecture. WHY: if org structure determines architecture, changing org changes architecture
- **Communication overhead** — more team boundaries = more API boundaries = more friction. WHY: every handoff requires translation, documentation, meetings
- **Team autonomy** — independent teams produce loosely coupled systems. WHY: no coordination needed = no shared interfaces = natural decoupling

### Relationships
- Conway's Law → Inverse Conway: observation vs intentional application
- Team boundaries → API boundaries: direct causal link
- Microservices ↔ autonomous teams: require each other

### Connections to Prior Knowledge
- **Builds on:** Org design, systems thinking
- **Contrasts with:** "Architecture is purely technical" belief
- **Enables:** Understanding why reorgs affect codebase

### Why It Matters
Architecture debates are often org design debates in disguise. Can't fix system without fixing team structure.
```

### Cards (18 cards for 4 concepts)

**Conway's Law (5 cards):**

1. Definition (Basic+Reversed):
   - Front: `What is Conway's Law?`
   - Back: `Orgs ship systems mirroring communication structure`

2. Why:
   - Front: `Why does Conway's Law occur? (re: org design)`
   - Back: `Teams build interfaces at coordination points → communication paths = API boundaries`

3. Application:
   - Front: `When consider Conway's Law? (re: architecture)`
   - Back: `Architecture decisions, reorgs, integration friction — org structure constrains tech options`

4. Context Transfer:
   - Front: `How would Conway's Law apply to a hospital? (re: org design)`
   - Back: `Hospital departments (ER, radiology, billing) = separate systems with handoff interfaces. Cross-dept communication friction → system integration friction`

5. Relationship:
   - Front: `Conway's Law → API design? (re: org design)`
   - Back: `Team boundaries = API boundaries. More teams = more interfaces = more overhead`

**Inverse Conway Maneuver (4 cards):**

6. Definition (Basic+Reversed):
   - Front: `What is Inverse Conway Maneuver?`
   - Back: `Restructure teams intentionally → get desired architecture`

7. Why:
   - Front: `Why does Inverse Conway work? (re: Conway's Law)`
   - Back: `Org structure determines architecture → change org, change architecture`

8. Why (varied phrasing):
   - Front: `Inverse Conway exploits what insight? (re: org design)`
   - Back: `Architecture follows communication — so design communication to get desired architecture`

9. Discrimination:
   - Front: `Conway's Law vs Inverse Conway? (re: org design)`
   - Back: `Conway: observe effect. Inverse: exploit it — restructure teams first`

**Team autonomy & microservices (5 cards):**

10. Definition:
    - Front: `Team autonomy in Conway context?`
    - Back: `Teams decide + ship independently, no cross-team coordination`

11. Relationship:
    - Front: `Microservices ↔ team autonomy? (re: Conway's Law)`
    - Back: `Require each other. Dependent teams → coupled systems`

12. Why:
    - Front: `Why microservices need autonomous teams? (re: architecture)`
    - Back: `Cross-team coordination → coupling. Independent teams = independent services`

13. Application:
    - Front: `When "monolith + many teams" fails? (re: Conway's Law)`
    - Back: `Always — team boundaries create friction inside monolith. Worst of both.`

14. Context Transfer:
    - Front: `How does team autonomy apply to open source projects? (re: Conway's Law)`
    - Back: `Loosely coordinated contributors → modular plugin architectures. Tight core team → cohesive core with clear boundaries`

**Communication overhead (4 cards):**

15. Definition:
    - Front: `Communication overhead in Conway's Law?`
    - Back: `Coordination cost at team boundaries. More boundaries = more APIs = more friction`

16. Why:
    - Front: `Why communication overhead matters? (re: Conway's Law)`
    - Back: `Every team boundary → system boundary. Overhead compounds as teams multiply`

17. Relationship:
    - Front: `Team boundaries → API boundaries? (re: Conway's Law)`
    - Back: `Direct link — teams build interfaces at coordination points`

18. Boundary:
    - Front: `When does communication overhead NOT predict architecture? (re: Conway's Law)`
    - Back: `Highly disciplined orgs with strong architecture governance can resist — but rare and requires constant effort`

---

**After displaying cards, ask: "Add these cards to Anki?"**

---

## Quality Checklist

### Summary
- [ ] **EXHAUSTIVE** — every concept from source captured
- [ ] **WHY answered** — each concept includes "why is this true?"
- [ ] Relationships mapped between concepts
- [ ] Connected to prior knowledge
- [ ] Why it matters answers "so what?"

### Cards
- [ ] **Self-explanation test passed** — can explain each concept without looking
- [ ] **EXHAUSTIVE** — every concept covered; full concepts carry ~4 cards
- [ ] Every concept has Definition + Why; full concepts add Application + Context Transfer
- [ ] Similar concepts have Discrimination cards
- [ ] Core concepts have Boundary cards (when doesn't it apply?)
- [ ] Each card = ONE atomic fact
- [ ] All cards require production (free recall)
- [ ] Context hints on all cards `(re: topic)`
- [ ] "Because" in answers where applicable
- [ ] No orphan cards
- [ ] No yes/no questions
- [ ] Cloze used only for syntax/quotes
- [ ] **Phrasing varied** — related cards use different wording for same fact
- [ ] **User permission obtained** before adding to Anki

### Final Verification
Re-read source. Ask: "Did I miss any concept worth remembering?" If yes, add it.
Count: [N concepts] × ~4 = [expected cards]. Actual cards should be close.