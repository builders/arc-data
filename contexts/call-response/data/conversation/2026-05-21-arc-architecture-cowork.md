---
title: ARC Architecture — Cowork Session, May 2026
date: 2026-05-21
participants:
  - stephen
  - claude-sonnet-4-6
project: arc
source:
  tool: claude-cowork
  sessionId: f7b1470d-ec38-4220-97b8-906c62f96ed0
  turns: 24
tags:
  - arc
  - bldrs
  - worldlink
  - qualis
  - architecture
  - memory
  - sovereignty
---

A foundational Cowork session establishing the philosophical and architectural
ground for ARC as it transitions from design and development into production.
The session covered the full stack — from physical workspace setup through
data memory architecture, multi-instance deployment, and the philosophical
frame (Qualis) that gives the technical work its reason.

## Context

Stephen's first substantive Cowork session after syncing the dev environment
between the MacBook Pro (BLDRS/personal account) and the Mac Studio (WorldLink
team account). The MacBook Pro + Cowork is established as the ARC oversight
layer; the Mac Studio + Claude Code is the WorldLink build layer.

The local ARC instance at `arc.bldrs.co` is currently disconnected from GitHub
data storage with sparse localStorage — a known starting point, not a blocker.
The session was diagnostic and architectural rather than operational.

## Arc of the session

**Data memory architecture.** Opened with a review of ARC's layered storage
model and the Services table in the BLDRS Media Library Airtable base. The
hierarchy was confirmed: filesystem seeds → Supabase auth → localStorage →
GitHub substrate → Airtable as optional editorial surface. The critical
discipline: write-through to Airtable never reverses — GitHub is always the
canonical record.

**Ecosystem inventory.** The WorldLink Client Sites and BLDRS Sites tables
from Airtable were reviewed and committed to memory. The dual-tier pattern
(WordPress production + Astro/React v1 rebuilds) is consistent across all
three WorldLink sites. Holobiont is the intended design system for WorldLink
but currently isolated. The Qualis cluster (`qualis.life`) is the BLDRS
equivalent — also without a design system yet.

**Philosophical ground.** ARC's origin in Symphony CMS (PHP/XSLT) was
recorded: a desire to rebuild it with a modern stack via Figma Make, which
grew into ecosystem orchestration. The Qualis philosophy was articulated in
full: qualia + quanta as the whole; life as centre, love as the whole;
Fuller's tetrahedral model (four entities, six relationships); the Universal
Coordinate System as the geometric expression of in-ness and out-ness;
chronos and kairos as integrated time.

**Multi-instance deployment.** The architecture for deploying ARC as
`arc.bldrs.co` (operator view, cross-ecosystem) and `arc.goworldlink.org`
(WorldLink-scoped) was worked through. Each instance is the same codebase
pointed at a different `system` context; per-client data repos enforce the
boundary. WorldLink gets auth sovereignty via its own Supabase project.

**Sessions infrastructure.** The existing `log-session.py` Stop hook was
reviewed — it captures Claude Code sessions as Markdown and pushes to
`bldrs/claude-sessions` on GitHub, already working. Gap identified: Cowork
sessions are not captured by this mechanism. Call + Response is the intended
capture layer for architecturally significant exchanges from both tools.

**This session as Call + Response.** The question of what to capture and how
led to a review of the existing Call + Response context and data. The format
was found to be well-suited. This session is the first Cowork entry.

## Key decisions

- This conversation (MacBook Pro, Cowork, BLDRS account) is the right place
  for ARC architecture decisions. WorldLink build work stays on Mac Studio.
- Call + Response captures selectively — only what is architecturally or
  philosophically significant. The sessions logger captures everything.
- Sessions are source material; Call + Response is the distilled record.
- `claude-cowork` should be added as a first-class `source.tool` option in
  `conversation.yaml`.

## Open issues surfaced

- `worldlink/arc-data/` does not yet exist — needed before `arc.goworldlink.org`
  can be deployed.
- No Qualis design system equivalent to Holobiont.
- `docs.bldrs.co` is stale — out of scope until explicitly reopened.
- Local ARC instance disconnected from GitHub; sparse localStorage.
  Fresh-start assessment deferred.
