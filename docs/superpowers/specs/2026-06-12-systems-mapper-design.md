# Design Spec — `systems-mapper` skill

- **Date:** 2026-06-12
- **Repo:** `tris-brain` (plugin) — new skill alongside `architecture-journal` and `tris-brain`
- **Status:** Locked, ready for implementation planning

## 1. Purpose

A **domain-agnostic** skill that maps and transforms *any* topic or system into a
systems-thinking representation. It runs the same mapping engine in two modes:

- **Understand** — build a structural mental model of a subject to master it.
- **Intervene** — map a real owned system/problem to find leverage points and act.

The engine (stocks/flows/feedback loops/leverage/iceberg) is universal — it applies
to software, organisations, markets, ecosystems, or personal habits equally. The
skill must **never assume a domain** (especially not software).

## 2. Placement & identity

- **Path:** `skills/systems-mapper/`
- **Name:** `systems-mapper`
- **Frontmatter:** `name`, `version: 1.0.0`, and a rich `description` with trigger phrases:
  > Map and transform any topic or system into systems thinking — stocks, flows, feedback
  > loops, leverage points, and the iceberg model. Two modes: Understand (build a structural
  > mental model of a subject to master it) and Intervene (map a real system to find leverage
  > points and act). Use when the user says "map this to systems thinking", "what are the
  > feedback loops in X", "find the leverage points", "systems map of …", "draw a causal loop
  > diagram", "where's the leverage to fix …", "help me understand X structurally", or wants
  > to reason about any topic as an interconnected system. Domain-agnostic.
- **Plugin version bump:** `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
  `1.1.0 → 1.2.0`.

## 3. Two modes, one engine

Both modes run the identical mapping engine (Section 5). Only the closing **tail** differs:

- **Understand tail:** comprehension/retention hooks (retrieval, spacing, a concrete apply step).
- **Intervene tail:** ranked leverage points + concrete actions to take.

The skill announces the chosen mode + tier on a single first line, e.g.:
`Mode: Understand (concept, no owned instance) · Tier: fast` — so the user can correct it
before effort is spent.

## 4. Mode-switch conditions (verbatim into SKILL.md)

**Default = Understand.** Switch to **Intervene** only when *both* an owned concrete
instance **and** a change-signal are present.

| Signal | → Understand | → Intervene |
|---|---|---|
| Object | a subject/concept ("indexing", "Raft", "inflation") | a specific instance the user owns ("*my* pipeline", "*our* checkout", "*this* repo's X") |
| Verb | understand, learn, master, study, explain, "how does X work" | fix, debug, optimise, reduce, unblock, "what should I change" |
| Symptom | none | pain present: slow, flaky, broken, failing, "keeps happening", incident |
| Goal | comprehension / retention | action / change |

**Decision rule:**
1. **Explicit override wins** (`mode=understand|intervene`, or "treat this as a learning exercise").
2. Else: **Intervene** iff `(owned concrete instance) AND (symptom OR change-verb)`. Otherwise **Understand**.
3. **Ambiguous / mixed** (e.g. "I want to understand why *our* DB is slow" — owned instance +
   symptom + understand-verb) → pick **Intervene**, **state the pick + one-line reason, and offer
   to flip**. Never switch silently on a genuine 50/50.

## 5. The mapping engine (shared core, lives in SKILL.md)

A fixed pass producing this **topic-free skeleton** for any input:

1. **Frame** — what system are we drawing? boundary + purpose, *deduced from behaviour, not rhetoric*.
2. **Stocks & flows** — accumulations and the rates that change them.
3. **Feedback loops** — reinforcing (R) vs balancing (B), each named, + a **Mermaid causal-loop diagram**.
4. **Delays & nonlinearity** — where lag causes overshoot/oscillation; thresholds/cliffs.
5. **Leverage points** — ranked weak → strong (Meadows ladder, from `references/leverage-points.md`).
6. **Iceberg** — event → pattern → structure → mental model.
7. **Tail** — mode-dependent (Section 3).

No field-specific vocabulary is baked in anywhere in the engine.

## 6. Tiered knowledge

- **Default (fast):** reason from the **vault** (`obsidian search`/`read` for related notes) +
  model knowledge. Seconds, no web. Mark anything reconstructed from memory as uncertain.
- **`--deep`:** invoke the **deep-research harness** to research + adversarially verify before
  mapping; output carries citations. Recommend `--deep` when stakes are high.

## 7. Output workflow (inline → confirm → save)

1. Detect **mode** + **tier** → announce both on line 1.
2. (deep only) run research.
3. Run the 6-step engine → **render the full map inline** (sections + Mermaid CLD).
4. **Ask: "anything to change?"** — iterate in chat until the user is happy. **Never auto-save.**
5. On approval, **offer to persist** a vault note (Section 9).

**Generic apply-hook** (Understand tail) — domain-neutral wording:
> *Apply hook:* find one concrete instance to trace the loops through — build it, run it, or
> work a real case end-to-end. (Code → run it; an org/market → trace a real episode;
> a habit → instrument one week.)

## 8. File structure (progressive disclosure — matches `architecture-journal`)

```
skills/systems-mapper/
  SKILL.md                  # router + 6-step engine + mode rules + workflow + guardrails
  references/
    leverage-points.md      # Meadows' 12 leverage points, ranked, with cross-domain examples
    archetypes.md           # systems archetypes: limits-to-growth, shifting-the-burden,
                            #   tragedy-of-the-commons, fixes-that-fail, success-to-the-successful
    cld-patterns.md         # Mermaid recipes for reinforcing/balancing loops, stocks, delays
    templates.md            # the two output templates (Understand / Intervene) + vault note format
    examples.md             # 3 worked maps from DIFFERENT domains (anti-anchoring)
```

**Reference pull-triggers** (explicit, so the model does not guess):
- Ranking leverage points (engine step 5, **either mode**) → read `leverage-points.md`.
- A recurring pattern smells like an archetype → read `archetypes.md`.
- Rendering a diagram → read `cld-patterns.md`.
- Producing the final output → read `templates.md`.

## 9. Vault integration

- Reuses the `obsidian` CLI like sibling skills; always targets `vault="tri-obsidian"`.
- Saved map note: `<topic> — systems map.md`, with frontmatter
  `created`, `tags: [systems-thinking]`, `topics:` (wikilinks), `via: "[[systems-mapper]]"`,
  and a backlink to **[[Systems thinking]]** plus relevant stubs (`[[Retrieval practice]]` etc.).
- Every saved map therefore compounds into the existing **[[Systems thinking]]** hub.

## 10. Guardrails (baked into SKILL.md)

- **Anti-anchoring:** "This engine is domain-agnostic. Do not assume software; map the system
  the user actually named." Reinforced by the cross-domain `examples.md`.
- **Anti-overclaim:** systems thinking explains *complexity & leverage*, **not** memory/retention.
  Flag that inference; never sell systems thinking as a learning hack. Carries two "do not assert"
  notes (the claims refuted in the 2026-06-12 research: system-dynamics "generic structures" →
  universal transfer; and system dynamics simultaneously boosting retention *and* covering
  curriculum faster).
- **Anti-procrastination:** in Understand mode the tail must end by pointing back to
  retrieval + real application — "the map is the 20%, not the work."
- **Uncertainty honesty:** in fast tier, mark reconstructed-from-memory claims; recommend
  `--deep` when stakes are high.

## 11. Cross-domain examples (`references/examples.md`)

Three deliberately different domains so the engine is seen to generalise:
1. **Software** — e.g. a CI/CD pipeline or DB indexing.
2. **Human / organisational** — e.g. team burnout.
3. **Natural / economic** — e.g. overfishing (tragedy-of-the-commons) or inflation.

## 12. Non-goals (YAGNI)

- Not a slash-command plugin (no `/systems-map`); a skill only.
- Not a full system-dynamics simulator; no numeric modelling/equations.
- No automatic vault writes; persistence is always user-confirmed.
- Does not teach the subject matter itself — it maps structure and points to the user's own
  learning loop.

## 13. Acceptance criteria

- Invoking on a **non-software** topic (e.g. "map inflation to systems thinking") produces a
  correct map with **zero software assumptions**.
- Mode auto-detection follows Section 4; ambiguous inputs trigger a stated pick + flip offer.
- Default tier hits no web; `--deep` triggers research + citations.
- Output renders inline with a valid Mermaid CLD, waits for "anything to change?", and only
  writes to the vault on explicit approval, linked to `[[Systems thinking]]`.
- `SKILL.md` stays lean; deep content is in `references/` and pulled on the stated triggers.

## 14. Open questions

None blocking. Naming (`systems-mapper`) and the 3 example domains are confirmed defaults and
can be revised during implementation without changing the architecture.
