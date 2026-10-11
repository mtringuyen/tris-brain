---
name: rfd
description: Create and maintain RFD (Request for Discussion) documents — lean requirement specs optimized for both human developers and AI coding agents. Use this skill whenever the user wants to plan a feature before coding, design a system, propose architecture changes, write a design doc, define database schemas or API contracts, discuss technical approaches, or update an existing RFD. Triggers on "create RFD", "write design doc", "plan feature", "update RFD", "let's design this first", "how should we architect X", "I want to think through X before building", "write a spec", or any request where structured design discussion should precede implementation.
---

# RFD — Request for Discussion

## Overview

RFDs are lightweight, living design documents that capture architecture decisions, data schemas, and API contracts in a format optimized for both human review and AI agent consumption. Inspired by Oxide Computer's RFD process — timely over polished, rough ideas welcome alongside implementation details.

## When to Use

- Planning a new feature that touches multiple services or needs design alignment
- Proposing cross-cutting architectural changes
- Designing new database schemas or API contracts
- Updating an existing RFD as requirements evolve
- Any work where "build the wrong thing" risk is high enough to warrant 30 minutes of design

**When NOT to use:** Single-endpoint CRUD additions, bug fixes, config changes, or anything where the implementation path is obvious and low-risk. If you can describe the full solution in 2 sentences, you probably don't need an RFD.

## Workflow

### 1. Discover Domain and Sequence

Identify the **domain** and **sequence number** for the RFD:

- **Naming**: `{domain}-{seq}-{slug}.md` (e.g., `billing-001-invoice-system.md`)
- **Discover domains**: Check `ls backend/src/interfaces/` for existing `*_api` packages. Use the domain name without the `_api` suffix (e.g., `billing_api` → `billing`, `clinical_workflow_api` → `clinical-workflow`).
- **Cross-domain RFDs**: If the feature spans multiple domains, use the primary domain for the filename and list all domains in the metadata `domain` field (e.g., `domain: pharmacy, billing, prescription`).
- **Sequence**: Increment within domain. Check existing RFDs: `ls docs/rfds/{domain}-*.md`
- **New domain**: Start at 001

### 2. Create RFD File

1. Ensure `docs/rfds/` exists
2. Read `assets/rfd-template.md` and copy to `docs/rfds/{domain}-{seq}-{slug}.md`
3. Fill in metadata — use today's date for `created` and `updated`
4. **Remove unused diagram sections** — the template includes placeholder flowchart, state, and ER diagrams. Delete any that don't apply to this feature. Add custom diagram sections when they better tell the story (e.g., "Current State" vs "Target State" for fixing broken flows).

### 3. Write RFD Content

Follow the template structure. Load `references/rfd-writing-guide.md` for detailed guidance on each section, including Mermaid diagram examples for every diagram type.

**Key principles:**
- **Lean**: Only include what's needed for implementation decisions
- **Machine-readable schemas**: SQL DDL for migrations, Go type definitions for API contracts
- **Mermaid diagrams**: At least one per RFD — pick the right type for the situation (see writing guide)
- **Acceptance criteria**: Testable statements, not vague descriptions
- **Boundaries**: What AI agents can do autonomously vs. what needs human review
- **Adapt the template**: Not every section is required. Remove sections that add no value for this particular feature. The template is a menu, not a checklist.

### 4. RFD Lifecycle

Update the `status` field in metadata as the RFD progresses:

| Status | Meaning |
|--------|---------|
| `draft` | Initial brain dump, rough ideas |
| `discussion` | Ready for team review and feedback |
| `accepted` | Approved for implementation |
| `partially-implemented` | Some phases shipped, others pending |
| `implemented` | All code merged, RFD is historical record |
| `abandoned` | Decided not to proceed |

### 5. Maintain Existing RFDs

When the user asks to update an existing RFD:

1. Find it: `ls docs/rfds/` and read the relevant file
2. Read current content fully before making changes
3. Update the `updated` date in metadata
4. Add an entry to the `## Changelog` table at the bottom describing what changed
5. If status changes, update the `status` field
6. If the RFD was `accepted` and implementation revealed new decisions, add them to the Architecture Decision section rather than rewriting the original — this preserves the design history

## Template Structure

The RFD template (`assets/rfd-template.md`) contains these sections — remove any that don't apply:

1. **Metadata** — Status, author, dates, domain
2. **Problem & Context** — Why are we doing this?
3. **Proposed Solution** — High-level approach
4. **Diagrams** — Mermaid diagrams (flow, state, ER, sequence) as needed
5. **Architecture Decision** — ADR-style: context, decision, consequences
6. **Data Schema** — SQL DDL, machine-readable
7. **API Contract** — Go interface definitions (for GenTS pipeline)
8. **Implementation Boundaries** — What's auto-approved vs. needs review
9. **Acceptance Criteria** — Testable completion conditions
10. **Open Questions** — Unresolved items
11. **Alternatives Considered** — What was rejected and why
12. **Changelog** — Evolution history

## Resources

### references/rfd-writing-guide.md

Detailed guidance for writing each RFD section, including Mermaid diagram examples for every diagram type (flowchart, state, ER, sequence, architecture). Load when creating or reviewing an RFD for the first time, or when unsure about section content depth.

### assets/rfd-template.md

Copy-ready template. Copy to `docs/rfds/` and fill in for each new RFD. Contains all sections with placeholder comments — remove sections that don't apply.

## Integration with Project Workflow

After an RFD is accepted, these project skills help with implementation:
- **Database changes** → Use `integration-test` skill (includes migration and fixture guidance)
- **E2E testing** → Use `e2e-test` skill for browser-level tests
- **GenTS pipeline** → Run `yarn gen:ts` after modifying `*.api.go` files (see CLAUDE.md)
- **Event modeling** → Use `event-storming` skill for domain process analysis

For steps not covered by skills (migrations, SQL queries, service scaffolding), follow the "Adding New Features" checklist in the project CLAUDE.md.
