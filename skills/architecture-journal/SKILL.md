---
name: architecture-journal
version: 1.0.0
description: "Capture architecture decisions and run structured design thinking workflows in the tri-obsidian vault, based on Eskil Steenberg's black box principles. Create ADRs, black box specs, design analyses, and project architecture maps. Use when the user mentions architecture decisions, system design, black box analysis, module boundaries, primitives, design review, ADR, or says 'analyze this architecture', 'document this design decision', 'what were my ADRs', 'black box spec', 'think through the architecture', 'design analysis', 'find the primitives', 'architecture review', 'map the architecture', 'should I split this module', 'what's the right abstraction', 'how should I structure this system', 'document why we chose X'. Also trigger when the user is in a design discussion and wants to persist the reasoning to their vault, or when reviewing whether past architecture decisions still hold."
---

# Architecture Journal — Design Thinking in the Vault

Capture architecture decisions and run structured design thinking workflows in the **tri-obsidian** vault, following Eskil Steenberg's black box design principles. Uses the `obsidian` CLI and existing obsidian skills (obsidian-cli, obsidian-markdown) as building blocks.

> **Core philosophy: Find the primitive, draw the black box, design the interface.**

## Always target the vault

```bash
obsidian vault="tri-obsidian" <command> ...
```

## Before taking any action

**Every action in this skill requires a confirmed project.** Architecture and design thinking are always scoped to a project. Before creating any note, running any workflow, or performing any search:

1. **Confirm the project** — Ask which project this is for if not already clear. Use the exact `[[Project Name]]` wikilink that will be set in the `project` property.
2. **Check for existing Architecture Map** — `obsidian vault="tri-obsidian" read file="Architecture: Project Name"`. If one exists, use it as context. If not, offer to create one.

If the request is also ambiguous about **note type**, **analysis depth**, or **create vs update** — present options and ask for confirmation.

Never assume the project. Never create an architecture note without the `project` property set.

---

## The Principles

These principles guide all analysis and documentation this skill produces. Load the full reference from `references/eskil-principles.md` (relative to this skill's directory) when running deep analysis.

1. **Black Box Interfaces** — Expose what a module does, hide how
2. **Replaceable Components** — Rewritable from the interface alone
3. **Single Responsibility** — One module = one person can build it
4. **Primitive-First** — Find the core data type; everything follows
5. **Simple Interfaces** — One good way, implementable, think forward
6. **Wrap What You Don't Own** — Never depend directly on external code
7. **Constant Velocity** — Five lines today beats one line plus future edits

---

## Note Types

All architecture notes go in **Root** (personal thinking, not external references). All use `[[wikilinks]]` profusely — link to projects, people, concepts, other architecture notes.

### Architecture Decision Record (ADR) → Root

A specific decision: what, why, alternatives, consequences.

```bash
obsidian vault="tri-obsidian" create name="ADR: Use event sourcing for audit trail" \
  content="---
categories:
  - \"[[Architecture]]\"
  - \"[[Design Decisions]]\"
tags:
  - adrs
project: \"[[Project Name]]\"
status: accepted
created: 2026-04-06
---

## Context

What is the issue motivating this decision?

## Decision

What change are we making?

## Primitives Affected

What core data types does this touch?

## Consequences

What becomes easier or harder?

## Alternatives Considered

What else was evaluated and why was it rejected?
" silent
```

**Properties:**

| Property | Type | Values |
|----------|------|--------|
| `project` | list | `[[Project Name]]` |
| `status` | text | `proposed` / `accepted` / `deprecated` / `superseded` |
| `categories` | list | Always `[[Architecture]]` + `[[Design Decisions]]` |
| `tags` | list | `#adrs` |

### Black Box Spec → Root

A module's interface, responsibilities, and boundaries. Complete enough that someone could rewrite the module from this spec alone.

```bash
obsidian vault="tri-obsidian" create name="Black Box: Payment Gateway Wrapper" \
  content="---
categories:
  - \"[[Architecture]]\"
  - \"[[Black Box Specs]]\"
tags:
  - black-boxes
project: \"[[Project Name]]\"
status: active
created: 2026-04-06
---

## Purpose

One sentence: what does this module DO? (Not how.)

## Interface

The public API — functions, inputs, outputs, contracts.

## Primitives

What core data types flow in and out?

## Guarantees

What invariants does this module enforce? What can callers rely on?

## Dependencies

What external things does this module wrap? What internal modules does it use?

## Replacement Test

Could someone rewrite this from the interface alone? What would they need?

## Red Flags

Known leaky abstractions, coupling, or violations.
" silent
```

**Properties:**

| Property | Type | Values |
|----------|------|--------|
| `project` | list | `[[Project Name]]` |
| `status` | text | `active` / `planned` / `deprecated` |
| `categories` | list | Always `[[Architecture]]` + `[[Black Box Specs]]` |
| `tags` | list | `#black-boxes` |

### Design Analysis → Root

The output of a full Design Thinking Workflow session (see below).

```bash
obsidian vault="tri-obsidian" create name="Design Analysis: Chat Application" \
  content="---
categories:
  - \"[[Architecture]]\"
  - \"[[Design Analyses]]\"
tags:
  - design-analyses
project: \"[[Project Name]]\"
created: 2026-04-06
---

## System Overview

What is this system? What must it do?

## Primitive

The core data type, why it was chosen, what trade-offs it implies.

## Black Box Map

| Module | Purpose (one sentence) | Status |
|--------|----------------------|--------|
| [[Black Box: X]] | What it does | planned |

## Key Interfaces

How modules connect. What flows between them. Kept as simple as possible.

## Risk Profile

What's most likely to change? Platforms, languages, hardware, people, priorities.

## Wrap List

External dependencies that must be wrapped and why.

## Tooling Plan

Recorders, simulators, test apps, loggers needed.

## Implementation Order

What to build first. What gets stubbed behind a forward-looking API.
" silent
```

**Properties:**

| Property | Type | Values |
|----------|------|--------|
| `project` | list | `[[Project Name]]` |
| `categories` | list | Always `[[Architecture]]` + `[[Design Analyses]]` |
| `tags` | list | `#design-analyses` |

### Project Architecture Map → Root

A living overview of a project's architecture. Updated over time as the system evolves.

```bash
obsidian vault="tri-obsidian" create name="Architecture: Project Name" \
  content="---
categories:
  - \"[[Architecture]]\"
  - \"[[Projects]]\"
tags:
  - architecture-maps
project: \"[[Project Name]]\"
primitives:
  - \"type 1\"
  - \"type 2\"
created: 2026-04-06
updated: 2026-04-06
---

## Primitive

The core data type: what, why, trade-offs.

## Module Map

| Module | Purpose | Owner | Status | Spec |
|--------|---------|-------|--------|------|
| [[Black Box: X]] | Does Y | Person | active | [[Black Box: X]] |

## Key Interfaces

How modules connect. Diagrams or descriptions.

## Evolution Log

- 2026-04-06: Initial architecture defined
" silent
```

**Properties:**

| Property | Type | Values |
|----------|------|--------|
| `project` | list | `[[Project Name]]` |
| `primitives` | list | Core data types |
| `updated` | date | Last revision |
| `categories` | list | Always `[[Architecture]]` + `[[Projects]]` |
| `tags` | list | `#architecture-maps` |

### Decision Guide

| Signal | Note Type | Naming Pattern |
|--------|-----------|----------------|
| Chose between alternatives | ADR | `ADR: <decision summary>` |
| Documenting a module's contract | Black Box Spec | `Black Box: <module name>` |
| Ran a design thinking session | Design Analysis | `Design Analysis: <system>` |
| Starting or mapping a project | Architecture Map | `Architecture: <project>` |
| General architecture insight | Evergreen Note | Sentence as title (use tris-brain) |
| Talk or article about architecture | Reference or Clipping | Use tris-brain |

If unclear, **ask Tri**.

---

## Design Thinking Workflow

The core workflow. Triggered by "help me think through the architecture of X" or "design analysis for X."

### Step 0: Confirm the Project

Before anything else, confirm which project this analysis is for. Establish the exact `[[Project Name]]` wikilink. Then:

1. Check if an Architecture Map exists: `obsidian vault="tri-obsidian" read file="Architecture: Project Name"`
2. Check for existing ADRs/specs: `obsidian vault="tri-obsidian" search:context query="[project:Project Name]" limit=10`
3. Load the full Eskil principles: Read `references/eskil-principles.md` (relative to this skill's directory)

Use any existing notes as starting context for the session.

### Step 1: Find the Primitive

Ask: **"What is the core data type that flows through this system?"**

Guide the thinking with Eskil's examples:
- Video editor → clips on a timeline (not "video")
- Healthcare system → events (not "medical journals")
- Jet fighter → state of the world (sensor readings, capabilities)
- Unix → text files

Push for specificity. Challenge vague answers. The primitive choice defines everything downstream.

**Record**: The primitive and why it was chosen over alternatives.

### Step 2: Draw the Black Box Boundaries

Ask: **"What are the major modules, and what does each one DO?"**

Test each proposed module:
- Can one person build and maintain it? If no → split it
- Can you describe its purpose in one sentence? If no → it's doing too much
- Does the description say what, not how? If not → you're leaking implementation

**Record**: Each module with its one-sentence purpose.

### Step 3: Design the Interfaces

Ask: **"How do the modules talk to each other? What's the simplest possible API?"**

Apply Eskil's format design rules:
- One good way, not multiple options
- Implementable — if it's too complex, nobody gets it right
- Think forward: design the API for what you'll need in 2 years, even if today's implementation is simple (the text-rendering pattern)
- Semantics over structure — define what things mean

**Record**: Key interfaces between modules.

### Step 4: Identify What to Wrap

External dependencies are the most common source of forced rewrites — a platform changes its API, a library drops backward compatibility, or a service you rely on pivots. Wrapping them means your interfaces survive even when the world under them shifts.

Ask: **"What don't you own? What could change under you?"**

Walk through the risk categories:
- **Platforms** — APIs, terms of service, user trends
- **Languages** — breaking changes (Python 2→3), compiler availability
- **Hardware** — new capabilities, deprecated interfaces
- **Priorities** — features needed in the future you can't predict
- **People** — what if someone leaves?

Everything here needs a wrapper. Never let external code leak into your interfaces.

**Record**: External dependencies and wrapping strategy.

### Step 5: Plan the Tooling

Ask: **"What tools do you need to test and debug these black boxes in isolation?"**

Eskil's insight: tooling that never ships is often the most valuable code.
- **Test apps** — minimal apps exercising the API
- **Recorders** — capture data flowing through interfaces
- **Playback** — replay recorded data
- **Loggers** — observe module behavior
- **Simulators** — mock one module to test another
- **Visualizers** — see system state

**Record**: Tooling to build alongside the system.

### Step 6: Plan Implementation Order

Ask: **"What do you build first? What can be stubbed?"**

Apply the text-rendering pattern:
- Build a simple implementation behind the *full* API
- Others start building against it immediately
- Upgrade internals later — no interface changes
- Never a "good enough for now" API. Only "good enough for now" implementations.

**Record**: Build sequence, what gets stubbed, what blocks what.

### Step 7: Save to Vault

Compile steps 1-6 into a **Design Analysis** note:

```bash
obsidian vault="tri-obsidian" create name="Design Analysis: <System Name>" \
  content="<compiled output from steps 1-6>" silent
```

Then ask:
1. Create individual **Black Box Specs** for each identified module?
2. Create or update the **Architecture Map** for this project?
3. Any decisions made that warrant their own **ADR**?

Link everything together with `[[wikilinks]]`.

---

## Exploring Architecture Knowledge

### "What were my decisions for project X?"

```bash
obsidian vault="tri-obsidian" search:context query="[project:Project Name]" limit=20
obsidian vault="tri-obsidian" search query="tag:#adrs" limit=20
```

### "Show me all black box specs"

```bash
obsidian vault="tri-obsidian" search query="tag:#black-boxes" limit=20
```

### "What design analyses have I done?"

```bash
obsidian vault="tri-obsidian" search query="tag:#design-analyses" limit=20
```

### "Show me the architecture of project X"

```bash
obsidian vault="tri-obsidian" read file="Architecture: Project Name"
```

Follow links to individual Black Box Specs and ADRs.

### "What architecture patterns keep coming up?"

```bash
obsidian vault="tri-obsidian" search:context query="[categories:Architecture]" limit=30
```

Follow backlinks across projects to find recurring patterns. Present as themes.

### "What modules have I deprecated?"

```bash
obsidian vault="tri-obsidian" search:context query="[status:deprecated]" limit=20
```

---

## Architecture Review Routine

A periodic check on whether a project's architecture still holds. Run quarterly for active projects, or when triggered by pain.

### When to trigger

- Before starting a major new feature
- After a painful bug that suggests architectural issues
- Quarterly for active projects
- When onboarding someone new to a project

### The Review

Read the project's Architecture Map, then check each principle:

| # | Question | What to look for |
|---|----------|-----------------|
| 1 | **Primitives still right?** | Has the core data type drifted? Multiple primitives where there should be one? |
| 2 | **Black boxes still black?** | Any module leaking implementation details? Can each still be replaced from its interface? |
| 3 | **Interfaces still simple?** | Has API complexity crept up? Multiple ways to do the same thing? |
| 4 | **Wrappers holding?** | External dependencies still wrapped? Any direct calls bypassing the wrapper? |
| 5 | **Single responsibility?** | Any module grown too large for one person? |
| 6 | **Velocity constant?** | Is development getting slower as the project grows? Where? |
| 7 | **Tooling sufficient?** | Can modules be tested in isolation? Are there gaps? |

For each issue found, create an **ADR** documenting the proposed change. Update the **Architecture Map** with findings.

After review, append to the Architecture Map's Evolution Log:

```bash
obsidian vault="tri-obsidian" append file="Architecture: Project Name" \
  content="\n- 2026-04-06: Quarterly review — [summary of findings]"
```

---

## Updating Existing Notes

Architecture notes are living documents. When the user wants to update status, revise an interface, or mark something deprecated:

1. **Read the existing note** — `obsidian vault="tri-obsidian" read file="<note name>"`
2. **Update via the obsidian CLI** — use `patch` to update properties or `write` to replace content
3. **Cross-update linked notes** — if an ADR supersedes another, update the old ADR's status to `superseded` and link to the new one. If a Black Box Spec changes, check the Architecture Map's module table.

Common updates:
- ADR status: `proposed` → `accepted` → `deprecated` / `superseded`
- Black Box Spec status: `planned` → `active` → `deprecated`
- Architecture Map: append to Evolution Log, update Module Map table

---

## Common Workflows

| User says | Action |
|-----------|--------|
| "Help me think through the architecture of X" | Run full Design Thinking Workflow (Steps 1-7) |
| "Document this design decision" | Create ADR, link to project |
| "Spec out this module" | Create Black Box Spec |
| "Map the architecture of project X" | Create or update Architecture Map |
| "What were my ADRs for X?" | Search by project + `#adrs` tag |
| "Review the architecture of X" | Run Architecture Review checklist |
| "What's the primitive for X?" | Read Architecture Map, or run Step 1 interactively |
| "This module is getting too big" | Run Steps 2-3 to find split points, create new specs |
| "We need to replace this dependency" | Create ADR, update the wrapping Black Box Spec |
| "We accepted that ADR" / "Mark as deprecated" | Update note status, cross-link if superseded |
| "What patterns do I keep using?" | Search all `[[Architecture]]` notes, find cross-project themes |
| "What's the state of project X?" | Read Architecture Map, list active/planned/deprecated modules |
