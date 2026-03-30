---
title: ARC Development — March 2026
date: 2026-03-23
participants:
  - stephen
  - claude-sonnet-4-6
project: arc
source:
  tool: claude-code
  sessionId: e3dbd4da-b6cf-4456-b278-679c2e07dabc
  turns: 180
tags:
  - arc
  - bldrs
  - storyearth
  - nourish
  - worldlink
---

A continuous Claude Code session spanning eight days and five interconnected projects. The session began with client work (StoryEarth, Nourish, GoWorldLink) and evolved into the foundational architecture of ARC as BLDRS Collective's operational hub.

## Projects touched

**StoryEarth** (Mar 23–25)
Astro port of story.earth for WorldLink. Scraped live site, migrated assets to Cloudflare R2, set up Cloudflare Workers deployment, built password-protected /learn section, configured worldlinkorg GitHub organization.

**Nourish 1.0** (Mar 26)
Port of nourishlife.org to Astro. Resolved layout and CSS issues from WordPress→Astro conversion, deployed to Cloudflare Workers as `nourishlife-v1.worldlink.workers.dev`.

**GoWorldLink** (Mar 27–28)
Port of goworldlink.org. Resolved font rendering and mobile menu issues, deployed to Cloudflare Workers.

**BLDRS** (Mar 28–29)
Created `bldrs/bldrs` GitHub repository. Deployed corporate site to `bldrs.co` on Cloudflare. Set up DNS via Cloudflare nameservers. Established `arc.bldrs.co` subdomain as the ARC production URL.

**ARC** (Mar 29–30)
The primary focus of the second half of the session. See artifacts for individual decision records.

## Arc of the ARC work

ARC arrived in a bloated state — an accumulation of Figma Make experiments that had created diverging forks and confused interfaces. The session began by stripping it to a deployable clean version, then systematically rebuilt the core functionality with clarity gained from the process.

The session ended with a complete data architecture design covering environments, Supabase schema, GitHub as source of truth, and a provider extension model that includes Airtable, Google Drive, YouTube, and Cloudflare R2.

## Key insight

The language shift from "user" to "agent" was not merely cosmetic. It surfaced a deeper design intention: agents have sovereignty over their own data and experience; users merely consume a product. This shift cascaded through the auth layer, the onboarding flow, the data schema, and the agent management UI.
