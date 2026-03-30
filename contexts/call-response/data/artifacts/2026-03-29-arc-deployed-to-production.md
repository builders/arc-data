---
title: ARC deployed to arc.bldrs.co
date: 2026-03-29
kind: decision
project: arc
conversationId: 2026-03-arc-development
tags:
  - deployment
  - cloudflare
  - bldrs
---

## Decision

Strip ARC of accumulated Figma Make bloat and deploy a clean, working instance to `arc.bldrs.co` as the production URL for BLDRS Collective's operational hub.

## Context

ARC had accumulated multiple conflicting implementations from iterative Figma Make experiments. Context switching had four diverging implementations. The agent/user management page had been deleted. The system was functional but incoherent.

The decision was to copy the current state to `arc_fm` as a reference archive, strip the working `arc` directory to only what was necessary for deployment, and treat the deployed instance as the clean foundation for further development.

## What was done

- Copied `/arc` to `/arc_fm` as legacy reference
- Removed unused components, duplicate implementations, dead code
- Deployed to `arc.bldrs.co` via Cloudflare Workers (`npx wrangler deploy`)
- Established `bldrs/arc` as the GitHub repository

## Outcome

`arc.bldrs.co` resolves and loads. The docs directory was absent but otherwise the core system was functional. This became the foundation for all subsequent ARC development in this session.
