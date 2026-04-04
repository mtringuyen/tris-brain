---
name: tris-brain
version: 1.0.1
description: "Manage Tri's second brain in Obsidian (tri-obsidian vault) following Steph Ango's file-over-app system. Create journal entries, evergreen notes, references, clippings. Search and explore the knowledge graph. Run weekly/monthly/yearly review routines. Audit vault health. Use when the user mentions their brain, vault, personal notes, journal, clippings, references, or says 'save to vault', 'what do I know about', 'search my brain', 'random revisit', 'weekly review', 'show connections', 'audit vault'."
---

# Tri's Brain — Obsidian Vault System

Manage Tri's personal knowledge base in the **tri-obsidian** vault, following Steph Ango's system. Uses the `obsidian` CLI and the existing obsidian skills (obsidian-cli, obsidian-markdown, obsidian-bases) as building blocks.

> **Core philosophy: File over app.** Plain text Markdown files you control. Portable, durable, yours forever.

## Always target the vault

```bash
obsidian vault="tri-obsidian" <command> ...
```

## Before taking any action

If the request is ambiguous about **note type**, **location**, **template**, or **create vs append** — present options and ask for confirmation.

---

## The Rules

### 1. One vault, minimal folders

| Folder | What goes here |
|--------|---------------|
| **Root (`/`)** | Personal notes — journal, essays, evergreen notes, weekly to-dos, reviews |
| **References/** | External things — books, movies, places, people. Named by title: `Book Title.md` |
| **Clippings/** | Content by others — saved articles, essays, web content |
| **Attachments/** | Images, audio, videos, PDFs |
| **Daily/** | Daily notes (`YYYY-MM-DD.md`) — exist solely to be linked to, never write directly |
| **Templates/** | Note templates (do not create notes here) |
| **Categories/** | Base views by category (do not create notes here) |

Never create new folders.

### 2. Always pluralize tags and categories

Tags: `#books`, `#movies`, `#podcasts`. Categories: `[[Books]]`, `[[Movies]]`.
Check existing: `obsidian vault="tri-obsidian" tags sort=count counts`

### 3. YYYY-MM-DD dates everywhere

Date properties, daily notes (`YYYY-MM-DD.md`), unique notes (`YYYY-MM-DD HHmm Title.md`).

### 4. Link profusely

Use `[[wikilinks]]` for every first mention of a person, place, thing, concept. Unresolved links are breadcrumbs for future connections — that's a feature.
Check before creating duplicates: `obsidian vault="tri-obsidian" search query="note name"`

### 5. 7-point rating scale

| 7 | Perfect | 6 | Excellent | 5 | Good | 4 | Passable | 3 | Bad | 2 | Atrocious | 1 | Evil |

### 6. Properties: reusable, short, list-type by default

Reuse across categories (`genre` for books, movies, games). Short names (`start`, `loc`). Default to `list` type.
Check existing: `obsidian vault="tri-obsidian" properties sort=count counts`

### 7. Every note starts from a template

```bash
obsidian vault="tri-obsidian" templates                              # list all
obsidian vault="tri-obsidian" template:read name="Book Template" resolve  # preview
```

### 8. Navigate by links, not folders

Organize through `categories` property, internal `[[links]]`, and tags — not file tree.

---

## Creating Notes

### Journal Entry → Root

```bash
obsidian vault="tri-obsidian" create name="2026-04-04 1430 Daily reflection" \
  template="Journal Template" silent
obsidian vault="tri-obsidian" append file="2026-04-04 1430 Daily reflection" \
  content="Went to see [[Movie]] with [[Person]]. The quote [[Next time is next time]] stuck with me..."
```

### Evergreen Note → Root

Title IS the idea, as a sentence. Can be very short.

```bash
obsidian vault="tri-obsidian" create name="Ideas are objects you can manipulate" \
  template="Evergreen Template" silent
obsidian vault="tri-obsidian" append file="Ideas are objects you can manipulate" \
  content="By turning ideas into objects you can stack them, combine them. You don't need to hold everything in your head."
```

### Reference Note → References/

Books, movies, people, places, podcasts, games, companies.

```bash
obsidian vault="tri-obsidian" create path="References/Book Title.md" \
  template="Book Template" silent
obsidian vault="tri-obsidian" property:set name="rating" value="6" path="References/Book Title.md"
obsidian vault="tri-obsidian" property:set name="author" value='["[[Author Name]]"]' type=list path="References/Book Title.md"
```

Templates: Book, Movie, Show, Podcast, People, Place, Restaurant, Album, Video Game, Company, etc.

### Clipping → Clippings/

Content written by someone else.

```bash
obsidian vault="tri-obsidian" create path="Clippings/Article Title.md" \
  template="Clipping Template" silent
obsidian vault="tri-obsidian" property:set name="url" value="https://..." path="Clippings/Article Title.md"
obsidian vault="tri-obsidian" property:set name="author" value='["[[Author Name]]"]' type=list path="Clippings/Article Title.md"
```

### Quick Thought Capture → Root

```bash
obsidian vault="tri-obsidian" unique name="Brief title" \
  content="The thought with [[links]] to related ideas..."
```

Creates `YYYY-MM-DD HHmm Brief title.md` in root. Note: to read back later, use `search` then `path=` (the `file=` resolver won't partial-match timestamp prefixes).

### Decision Guide

| Signal | Type | Location |
|--------|------|----------|
| Tri wrote/thought it | Journal or Evergreen | Root |
| Idea distilled to one sentence | Evergreen | Root |
| Book, movie, person, place, thing | Reference | References/ |
| Something someone else wrote | Clipping | Clippings/ |
| Tasks for the week | Weekly To-Do | Root |
| Quick thought right now | Unique Note | Root |
| Meeting or event | Meeting/Event template | Root |

If unclear, **ask Tri**.

---

## Exploring & Retrieving Knowledge

### "What do I know about X?"

Layered approach — cast wide, then narrow:

```bash
# 1. Text search with context
obsidian vault="tri-obsidian" search:context query="X" limit=20

# 2. Check for a dedicated note
obsidian vault="tri-obsidian" read file="X"

# 3. What links TO this concept? (backlinks)
obsidian vault="tri-obsidian" backlinks file="X" counts

# 4. Unresolved mentions (breadcrumbs)
obsidian vault="tri-obsidian" unresolved counts
```

Present as a **knowledge map**: dedicated notes → mentions in other notes → outgoing links → unresolved breadcrumbs.

### Browse by category (Bases)

Bases are the power tool for structured browsing:

```bash
obsidian vault="tri-obsidian" bases                                           # list all
obsidian vault="tri-obsidian" base:query path="Templates/Bases/Books.base" format=md
obsidian vault="tri-obsidian" base:query path="Templates/Bases/Ratings.base" format=md
obsidian vault="tri-obsidian" base:query path="Templates/Bases/Everything.base" format=md
```

Available bases: Albums, Books, Board games, Clippings, Companies, Daily, Events, Evergreen, Everything, Games, Genre, Journal, Map, Meetings, Movies, People, Places, Podcast episodes, Podcasts, Posts, Products, Projects, Ratings, Recipes, Related, Shows, Templates, Trips.

### Search by property (bracket syntax)

Obsidian uses bracket syntax `[property:value]` for property searches:

```bash
obsidian vault="tri-obsidian" search query="[rating:7]" limit=20
obsidian vault="tri-obsidian" search:context query="[author:Kevin Kelly]" limit=10
obsidian vault="tri-obsidian" search:context query="[genre:Sci-fi]" limit=10
obsidian vault="tri-obsidian" search query="[created:2026-04]" limit=20
```

### Search by tag

```bash
obsidian vault="tri-obsidian" tags sort=count counts         # overview
obsidian vault="tri-obsidian" search query="tag:#podcasts"   # find by tag
```

### Follow the link graph

```bash
obsidian vault="tri-obsidian" read file="Note Name"              # read the note
obsidian vault="tri-obsidian" backlinks file="Note Name" counts  # what links here
```

Follow chains of backlinks to discover emergent connections.

### Find weak spots in the knowledge graph

```bash
obsidian vault="tri-obsidian" orphans     # no incoming links — disconnected
obsidian vault="tri-obsidian" deadends    # no outgoing links — dead ends
obsidian vault="tri-obsidian" unresolved counts  # mentioned but never created
```

**Orphans**: suggest which notes should link to them.
**Dead-ends**: read and suggest outgoing links.
**Unresolved**: breadcrumbs. If one has many mentions, suggest creating a proper note.

### Recent activity

```bash
obsidian vault="tri-obsidian" recents
```

### Random exploration

```bash
obsidian vault="tri-obsidian" random:read
obsidian vault="tri-obsidian" random:read folder="References"
```

For each note: summarize, suggest missing links, flag property gaps, check style rules.

---

## Vault Health Audit

When asked to audit, run these checks:

```bash
obsidian vault="tri-obsidian" tags sort=count counts          # singular tags?
obsidian vault="tri-obsidian" orphans                         # disconnected notes?
obsidian vault="tri-obsidian" deadends                        # dead-end notes?
obsidian vault="tri-obsidian" unresolved counts               # unresolved links
obsidian vault="tri-obsidian" folders                         # unexpected folders?
obsidian vault="tri-obsidian" properties sort=count counts    # unused properties?
```

Report: singular tags, notes in wrong folders, orphans, dead-ends, missing `categories`, unused properties (count=0), nested subfolders beyond Templates/Bases.

---

## Review Routines & Reminders

Steph's system uses **fractal journaling** — reviews at increasing time scales, each compiling the one below. The process of reviewing builds understanding — don't delegate it.

### The Review Tracker

A note called **Review Tracker** in the vault root tracks when each review level was last completed. Whenever this skill triggers for review-related requests, **always check the tracker first**:

```bash
obsidian vault="tri-obsidian" read file="Review Tracker"
```

If the note doesn't exist (command returns error), create it:

```bash
obsidian vault="tri-obsidian" create name="Review Tracker" \
  content="---\ncategories:\n  - \"[[Projects]]\"\ntags:\n  - reviews\n  - routines\ncreated: 2026-04-04\n---\n\n# Review Tracker\n\n| Review Level | Cadence | Last Completed | Next Due |\n|-------------|---------|----------------|----------|\n| Thought Compilation | Every few days | — | — |\n| Weekly To-Dos | Weekly (Monday) | — | — |\n| Monthly Review | Monthly (1st) | — | — |\n| Random Revisit | Every 2-3 months | — | — |\n| Yearly Review | December | — | — |\n| Decade Review | Every 10 years | — | — |\n\n## Log\n" silent
```

Flag any overdue reviews before proceeding. After completing a review, update the tracker:

```bash
obsidian vault="tri-obsidian" append file="Review Tracker" \
  content="- 2026-04-04: Completed weekly to-dos (2026-W14)"
```

And update the table's "Last Completed" and "Next Due" dates using `property:set` or by editing the note content.

### Cadence Overview

| Level | Cadence | What to do |
|-------|---------|------------|
| **Thought Capture** | Daily | `unique` command — get it down, link generously |
| **Compilation** | Every few days | Review recent unique notes, compile salient thoughts into a journal entry |
| **Weekly To-Dos** | Monday | Create `YYYY-Wnn To-Dos` in root from scratch — don't look at last week |
| **Monthly Review** | 1st of month | Review compilations + weekly to-dos, extract themes, create monthly note |
| **Random Revisit** | Every 2-3 months | Read 5-10 random notes, fix links, fill gaps, strengthen connections |
| **Yearly Review** | December | 40 Questions (see `references/40-questions-yearly.md`) |
| **Decade Review** | Every 10 years | 40 philosophical questions (see `references/40-questions-decade.md`) |

### Compilation workflow

```bash
# Find recent notes
obsidian vault="tri-obsidian" recents

# Read each, then create compilation
obsidian vault="tri-obsidian" create name="2026-04-04 Weekly Thoughts" \
  template="Journal Template" silent
obsidian vault="tri-obsidian" append file="2026-04-04 Weekly Thoughts" \
  content="Compiled from recent notes:\n\n- [[2026-04-01 1430 Some Thought]]: key insight with [[links]]...\n- [[2026-04-02 0900 Another Idea]]: connection to [[topic]]..."
```

### Weekly To-Dos

```bash
obsidian vault="tri-obsidian" create name="2026-W14 To-Dos" \
  content="---\ncreated: 2026-04-04\ntags:\n  - to-dos\n---\n\n- [ ] Task one\n- [ ] Task two" silent
```

Write from scratch. If you can't remember it, it wasn't important.

### Monthly Review

```bash
obsidian vault="tri-obsidian" search query="[created:2026-03]" limit=30
obsidian vault="tri-obsidian" create name="2026-03 Monthly Review" \
  template="Monthly Note Template" silent
```

Compile themes, patterns, key moments. Link back to source notes.

### Yearly Review

Work through the 40 questions. Load the question list from this skill's bundled file at `references/40-questions-yearly.md` (use the Read tool on the file relative to this skill's directory). For decade reviews, load `references/40-questions-decade.md`.

```bash
obsidian vault="tri-obsidian" create name="2026 Yearly Review" \
  content="---\ncreated: 2026-12-31\ntags:\n  - reviews\n  - yearly\n---\n\n# 2026 Yearly Review\n\nBased on [[40 questions to ask yourself every year]]\n\n## 1. What did you do this year that you'd never done before?\n\n..." silent
```

The value is in the **trends** — compare with previous years.

### Random Revisit Session

```bash
obsidian vault="tri-obsidian" random:read
```

For each note: (1) read it, (2) add missing `[[wikilinks]]`, (3) fix formatting/tags, (4) fill empty properties, (5) suggest connections to other notes.

---

## Common Workflows

| User says | Action |
|-----------|--------|
| "Save this article" | Create in Clippings/ with Clipping Template, set author/url/topics |
| "I just watched/read/played X" | Create in References/ with appropriate template, set rating/genre/year |
| "I had this thought..." | `unique` for quick capture, or Journal entry if longer |
| "What should I work on?" | Create weekly to-do from scratch |
| "What do I know about X?" | Layered search → backlinks → bases → knowledge map |
| "Show me my favorites" | Query Ratings base, filter by category |
| "Let's do a random revisit" | Read 5 random notes, suggest links/fixes per note |
| "Time for my [level] review" | Check Review Tracker, gather source material, help compile |
| "What ideas keep coming up?" | Find notes with most backlinks, check frequent tags, present themes |
| "Find connections between X and Y" | Read both, check shared backlinks/tags, suggest linking text |
| "Audit my vault" | Run health checks, report against the rules |
