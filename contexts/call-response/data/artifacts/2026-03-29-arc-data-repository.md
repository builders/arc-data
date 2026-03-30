---
title: arc-data repository established as unified data source
date: 2026-03-29
kind: design
project: arc
conversationId: 2026-03-arc-development
files:
  - contexts/art-of-seeing/context.yaml
  - contexts/bldrs/context.yaml
  - contexts/call-response/context.yaml
  - contexts/system/context.yaml
tags:
  - data-architecture
  - github
  - contexts
---

## Decision

Create `bldrs/arc-data` as a private GitHub repository — the unified source of truth for all ARC context data, replacing the scattered per-context repositories that had accumulated.

## Context

Context switching had created multiple interfaces and methods — Application Toolbar, F16 Universal Context Switcher, sidebar dropdown, ARC Admin toolbar, Settings page Context Manager, Context Builder. Each Figma Make conversation had created diverging forks. There was no single place that captured what a context actually *was*.

The `builders/art-of-seeing-design-data` repository had established a working pattern: Markdown files with YAML frontmatter, organized as `data/[type]/[YYYY]/[MM]/[DD-slug.md]`. The arc-data repo generalises this across all contexts.

## Four-dimension context model

Each context is defined across four dimensions captured in `context.yaml`:

| Dimension | What it controls |
|---|---|
| Application | Routes, navigation, behaviors, permissions |
| Presentation | Layout, visual theme, color palette, typography |
| Structure | Types, fields, relationships |
| Data | Provider, repository, branch, path |

## Structure

```
bldrs/arc-data
└── contexts/
    ├── art-of-seeing/   context.yaml + types/*.yaml + data/
    ├── bldrs/           context.yaml + types/*.yaml + data/
    ├── call-response/   context.yaml + types/*.yaml + data/
    └── system/          context.yaml + types/*.yaml + data/
        data/
            agents/      stephen.yaml
            roles/       super-admin.yaml, admin.yaml, editor.yaml, viewer.yaml, guest.yaml
            access/      stephen-all-contexts.yaml
```

## Note on GitHub organizations

- `builders` = community, public, open source repositories
- `bldrs` = corporate, private, proprietary repositories

`arc-data` is private (contains agent identity records) → `bldrs/arc-data`.
`art-of-seeing-design-data` is public course material → `builders/art-of-seeing-design-data`.
