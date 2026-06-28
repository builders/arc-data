---
title: ARC multi-instance deployment model
date: 2026-05-21
kind: design
project: arc
conversationId: 2026-05-arc-architecture-cowork
tags:
  - deployment
  - sovereignty
  - worldlink
  - supabase
  - multi-tenancy
  - system-context
---

## The goal

Multiple ARC instances with different data scopes, each sovereign within
its own boundary. The operator instance has cross-ecosystem visibility;
client instances see only their own data.

## Instances

```
arc.bldrs.co          — operator view (BLDRS account)
                        sees: bldrs/arc-data/ + worldlink/arc-data/ + future clients

arc.goworldlink.org   — WorldLink client instance
                        sees: worldlink/arc-data/ only
```

Future clients (e.g. Design Science Studio) each get a peer instance with
the same scoping model.

## How the boundary is enforced

Each deployment is the **same codebase** pointed at a different `system`
context configuration. The WorldLink instance's `system/context.yaml`
simply does not reference `bldrs/arc-data/` — the boundary is at the
configuration layer, not via access-control rules on a shared database.

This is sovereignty enforced architecturally, not by permission.

## Per-client data repos

```
bldrs/arc-data/        — BLDRS contexts (art-of-seeing, bldrs, call-response, system)
worldlink/arc-data/    — WorldLink contexts (nourish, story-earth, holobiont — to be created)
```

No client's data lives in another entity's repo. Data sovereignty is
enforced at the data layer before it reaches auth or application logic.

## Auth sovereignty

WorldLink gets its own Supabase project, configured via environment variables
in the `@bldrsco/arc-auth-supabase` extension. WorldLink users authenticate
at `arc.goworldlink.org` against a WorldLink-owned Supabase instance. BLDRS
has no access to those sessions; WorldLink could manage it independently.

If the BLDRS–WorldLink relationship changes, WorldLink retains: its data repo,
its auth, its deployment. Nothing is hostage to the operator.

## What doesn't exist yet

- `worldlink/arc-data/` — the per-client data repo for WorldLink. Needs a
  `system` context and initial context definitions for Nourish, Story.Earth,
  and WorldLink. This is the foundational prerequisite.
- WorldLink Supabase project — to be provisioned once the data repo exists.
- Cloudflare Pages deployment for `arc.goworldlink.org` — domain, build
  config, and environment variables pointing at the WorldLink data repo and
  Supabase project.

## Order of operations

1. Create `worldlink/arc-data/` with a minimal `system` context
2. Provision WorldLink Supabase project
3. Configure `@bldrsco/arc-auth-supabase` for per-deployment environment vars
4. Deploy `arc.goworldlink.org` via Cloudflare Pages
5. Verify data scoping: WorldLink instance cannot read `bldrs/arc-data/`

## Proof of concept value

If this pattern holds for WorldLink, it holds for any number of future
clients. Each new client gets: their own `arc-data` repo, their own Supabase
project, their own subdomain deployment. The operator instance gains a new
data source entry in its `system` context. No shared infrastructure is
modified.
