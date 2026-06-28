---
title: Call + Response as ARC's own memory layer
date: 2026-05-21
kind: decision
project: arc
conversationId: 2026-05-arc-architecture-cowork
tags:
  - call-response
  - memory
  - sessions
  - sovereignty
  - cowork
---

## The decision

Call + Response is ARC's sovereign memory layer for architecturally and
philosophically significant exchanges. It sits above the raw sessions log
and below the architecture documentation — the distilled record of how
thinking evolved, not a transcript of every turn.

## Two layers, different purposes

**Sessions log** (`bldrs/claude-sessions`, via `log-session.py`):
- Complete operational transcripts
- Automatic — runs as a Claude Code Stop hook after each turn
- All detail preserved, including mundane exchanges
- Claude Code only (does not capture Cowork sessions)
- Good for: reconstruction, debugging, "what did we actually do"

**Call + Response** (`bldrs/arc-data/contexts/call-response/`):
- Curated record — only what is architecturally or philosophically significant
- Intentional — written by a human or agent after reflection
- Both Claude Code and Cowork sessions as source material
- Good for: vision, future context, ARC reading its own history

## The relationship

Sessions are source material. A significant decision made during a Claude
Code session gets distilled into a Call + Response entry, with the session ID
recorded as provenance (`source.sessionId`). The full detail lives in the
sessions log; the meaning lives in Call + Response.

## What to capture

The eight-category sidebar of the Call + Response app provides the filter:

- **Agents** — who is working, with which tool, at what scope
- **Prompts** — the calls that initiated something significant
- **Responses** — the answers that produced something durable
- **Commits** — decisions locked in to the architecture
- **Builds** — what was built, what shipped
- **Milestones** — significant transitions in the project arc
- **Issues** — open questions and blockers surfaced
- **Timeline** — the chronological through-line

Forgetting is a useful function. Not every turn is a call; not every reply
is a response. Only record what matters for the overall vision and
architecture.

## The Cowork gap

`log-session.py` is a Claude Code Stop hook — it does not capture Cowork
sessions. Architecturally significant Cowork exchanges (like this one) must
be captured manually into Call + Response. This is by design: Cowork is the
architecture/planning layer; its significant outputs should be curated, not
logged automatically.

A future enhancement: a Cowork-aware variant of `log-session.py` that can
be triggered manually at the end of a session to commit the transcript to
`bldrs/claude-sessions`, making Cowork sessions available as source material
alongside Claude Code sessions.

## Schema note

The `source.tool` field in `conversation.yaml` should add `claude-cowork`
as a first-class option alongside `claude-code`, `figma-make`, and
`claude-web`. This session is recorded with `tool: claude-cowork` pending
that schema update.
