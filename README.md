# tris-brain

A Claude Code plugin for managing Tri's second brain in Obsidian, following [Steph Ango's vault system](https://stephango.com/vault).

## What it does

- **Create notes** — journal entries, evergreen notes, references, clippings, weekly to-dos
- **Explore knowledge** — search, browse bases, follow backlinks, find connections
- **Review routines** — fractal journaling with daily/weekly/monthly/yearly review cycles
- **Vault health** — audit against Steph Ango's rules, fix orphans and dead-ends

## Install

```bash
claude plugin marketplace add mtringuyen/tris-brain
claude plugin install tris-brain@tris-brain
```

> Private repo — installing requires `gh`/git credentials with read access to `mtringuyen/tris-brain`.

## Vault

Targets the **tri-obsidian** vault using the `obsidian` CLI.

## Dependencies

- [Obsidian](https://obsidian.md/) with the CLI enabled
- [obsidian-skills](https://github.com/kepano/obsidian-skills) plugin installed (provides `obsidian:obsidian-cli`)
