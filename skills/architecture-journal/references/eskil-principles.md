# Eskil Steenberg's Black Box Design Principles

Source: Eskil Steenberg — "How to structure large software projects" (YouTube talk)

## Core Rule

> It's faster to write five lines of code today than to write one line today and then have to edit it in the future.

Finish your code. Write it so it never breaks. If that means typing more and being more explicit, good — you're thinking about it now instead of debugging it five years later.

## Optimize For

- **Dependability** — software that lasts forever and never breaks
- **Extendability** — new features and capabilities can be added
- **Team scalability** — one person per module, many small projects instead of one big project
- **Constant velocity** — development speed stays the same regardless of project size
- **Risk reduction** — insulate against platforms changing, languages breaking, hardware evolving, people leaving

## The Seven Principles

### 1. Black Box Interfaces

Every module is a black box. What it does is exposed through APIs. How it does it is hidden.

- Document the interface, not the implementation
- Users of a module should never need to look at its source code
- Like header files in C: "here are the functions you're allowed to use"

### 2. Replaceable Components

Any module should be rewritable from scratch using only its interface.

- If a person leaves, you can rewrite their module from the API spec
- Gradually migrate from old implementation to new
- Both can run simultaneously during transition (the healthcare system pattern)

### 3. Single Responsibility / One Person Per Module

Each module should be small enough for one person to build and maintain.

- Multiple people can design an API, but one person implements it
- Senior developers get hard modules, junior developers get easy ones
- Avoid multiple people working on the same module — that's where coordination overhead kills velocity

### 4. Primitive-First Design

Find the core data type that flows through your system. Everything follows from this choice.

**Examples:**
- Video editor → clips on a timeline (not "video")
- Healthcare system → events (not "medical journals")
- Jet fighter → state of the world (current sensor readings, capabilities, contacts)
- Unix → text files
- Houdini / Notch → nodes with parameters
- Photoshop → layers of bitmaps
- Teardown → voxels

**Trade-offs:**
- Polygons: simple, universal, but no curves
- NURBS: mathematically precise curves, but complex
- Voxels: easy collision/destruction, but no smooth surfaces
- Supporting multiple primitives adds enormous complexity (Maya problem)

**The best primitive**: consistent, works for everything, simple enough that all tools can operate on it.

### 5. Format / Interface Design

Formats are the core of software design. APIs, files, protocols, languages — all formats.

**Key concepts:**
- **Semantics vs. structure**: Metric system = pure semantics (what it means). JSON = pure structure (how it's stored). Most formats need both.
- **Simple structure, rich semantics**: a JSON loader can load any JSON regardless of meaning — that's the power of separating structure from semantics
- **Implementability**: if a format is too complex, nobody will implement it correctly. Smaller formats get better implementations.
- **One good way**: if two groups want different primitives (polygons vs. NURBS), pick one. Both groups implementing one thing < both groups implementing two things, even if one is unhappy.
- **Think forward**: design the API for what you'll need in 2 years, even if today's implementation is simple. (Text rendering: build the Korean-ready API now, implement Korean later.)

### 6. Wrap What You Don't Own

Never depend directly on code you don't control.

- Platforms (SDL, Windows, Android) — wrap them
- Libraries (even open-source, well-maintained ones) — wrap them
- External services (Amazon, databases) — wrap them
- You don't know their development direction, whether they'll accept your patches, or when they'll break backward compatibility

**The healthcare system pattern**: don't expose SQL to users of your API. If your API leaks "send SQL queries," you're locked into SQL forever. Make your own interface; how you store data is your business.

### 7. Build Tooling Around Black Boxes

Tooling that never ships is often the most valuable code you write.

**Essential tools:**
- **Test applications** — minimal apps that exercise the API (the betray.h test app pattern)
- **Recorders** — capture data flowing through interfaces (black box recorder)
- **Playback** — replay recorded data for testing
- **Loggers** — observe what's happening
- **Simulators** — mock one module to test another
- **Visualizers** — see system state
- **Language bindings** — C API → Python API → others, so different teams can use their preferred language

## Plugin Architecture

When a core would need to know about everything (every video format, every effect, every sensor), use plugins instead.

- Plugins describe what they can do (capabilities, parameters, defaults)
- The core stays small; plugins provide all domain-specific functionality
- Plugins live in separate DLLs — built independently, added one by one
- Plugin developers only need the API spec, not the core source

**Prefer "get plugged in" over "plug yourself in"**: don't live in someone else's world. Accept others into your world through your interfaces. If a module can be standalone rather than a plugin, that's better — but plugins are often the right pragmatic choice.

## The Pattern Across All Three Systems

Whether building a video editor, healthcare system, or jet fighter, the approach is identical:

1. Find the primitive
2. Build a core black box to store/pass that primitive
3. Design a clean API for the core
4. Add plugins/extensions for domain-specific functionality
5. Build tooling to test and debug
6. Layer UIs and applications on top

The core is never domain-specific. It's a data management / pass-through layer. The domain knowledge lives in the endpoints (plugins, UIs, applications).
