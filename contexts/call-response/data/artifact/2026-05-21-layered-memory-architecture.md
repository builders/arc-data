---
title: ARC layered memory architecture
date: 2026-05-21
kind: design
project: arc
conversationId: 2026-05-arc-architecture-cowork
tags:
  - data-architecture
  - memory
  - sovereignty
  - airtable
  - github
  - supabase
---

## The problem

ARC needs multiple memory layers for different performance and sovereignty
requirements. Each layer must be independently swappable without losing
data sovereignty — no vendor should hold the data hostage.

## The hierarchy

```
Filesystem (seed data)
  ↓ bootstraps
Supabase (auth + session state)
  ↓ governs access to
localStorage (low-latency read cache)
  ↓ reflects
GitHub (sovereign substrate — source of truth)
  ↑ write-through from
Airtable (optional editorial surface)
```

## Layer responsibilities

**Filesystem seeds** — the bootstrap layer. No network, no vendor dependency.
The one thing that always works. Seed data initialises a fresh ARC instance.

**Supabase** — auth and session state only. Not the data store. Chosen for its
Postgres + GoTrue foundation, which is self-hostable and replaceable. The
`@bldrsco/arc-auth-supabase` extension isolates this dependency.

**localStorage** — ephemeral UI state and read cache (filter preferences,
last-viewed entry, scroll position). Survives a page reload, not a device
change. Never canonical. Risk: data loss, not data lock-in.

**GitHub** — the sovereign substrate. Markdown with YAML frontmatter, filed
as `data/[type]/[YYYY]/[MM]/[DD-slug].md`. Version-controlled, portable,
owned by whoever controls the repo. This is the source of truth.

**Airtable** — opt-in editorial surface for contexts that need it (editor
familiarity, external collaborators). Write-through means Airtable edits
flow back to GitHub as proposed commits; they do not become the canonical
record. Airtable is a lens, not a vault.

## The critical discipline

Write direction is always: **editor → Airtable → GitHub**. Never the reverse.
"Source of truth for the ecosystem" means source of truth for *editorial
convenience*, not for the data itself. GitHub is the canonical record.
This distinction is load-bearing for sovereignty.

## Swappability

Each layer is independently replaceable:
- Supabase → Clerk, Auth0, or self-hosted GoTrue
- Airtable → Notion, a bespoke admin UI, or nothing
- GitHub → any git host (Gitea, GitLab, self-hosted)
- localStorage → sessionStorage, IndexedDB, or an in-memory cache

None of them hold the data hostage because GitHub (or its replacement) is
always the authoritative store — and Markdown files are format-sovereign.
