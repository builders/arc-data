# arc-data

Unified data repository for ARC (Application Resource Center). This repository serves as the source of truth for all context data, type schemas, and media assets across ARC contexts.

## Structure

```
arc-data/
├── contexts/
│   ├── art-of-seeing/       # The Art of Seeing — visual culture course
│   │   ├── context.yaml     # Context definition (all four dimensions)
│   │   ├── types/           # Type schemas
│   │   │   ├── gallery.yaml
│   │   │   ├── exhibition.yaml
│   │   │   ├── observation.yaml
│   │   │   ├── activity.yaml
│   │   │   ├── project.yaml
│   │   │   ├── journal-entry.yaml
│   │   │   └── presentation.yaml
│   │   └── data/            # Content entries (Markdown + YAML frontmatter)
│   │       └── [type]/[YYYY]/[MM]/[DD-slug.md]
│   ├── bldrs/               # BLDRS Collective — studio practice and projects
│   │   ├── context.yaml
│   │   ├── types/
│   │   └── data/
│   ├── call-response/       # Call + Response — collaborative design
│   │   ├── context.yaml
│   │   ├── types/
│   │   └── data/
│   └── system/              # ARC System — meta-layer
│       ├── context.yaml
│       └── types/
└── media/
    └── images/              # Shared media assets
```

## Context Model

Each context is defined across four dimensions:

| Dimension | What it controls |
|---|---|
| **Application** | Routes, navigation, behaviors, permissions |
| **Presentation** | Layout, visual theme, color palette, typography |
| **Structure** | Types, fields, relationships |
| **Data** | Provider, repository, branch, path |

These four dimensions are captured in each context's `context.yaml` file.

## Data Format

Content entries use Markdown with YAML frontmatter:

```markdown
---
title: Entry Title
date: 2026-03-29
type: gallery
published: true
tags:
  - tag-one
  - tag-two
---

Entry body content...
```

Files are organized as `data/[type]/[YYYY]/[MM]/[DD-slug.md]`.

## GitHub Sync

ARC's `context-github-sync` service reads context configuration and syncs manifests to this repository. The `data.provider` field in `context.yaml` points ARC to the correct owner/repo/branch/path for each context.

## Related Repositories

- `builders/art-of-seeing-design-data` — original Art of Seeing data (source for migration into this repo)
- `builders/arc` — ARC application source code
