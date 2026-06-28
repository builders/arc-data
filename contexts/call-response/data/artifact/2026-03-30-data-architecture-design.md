---
title: Complete ARC data architecture
date: 2026-03-30
kind: design
project: arc
conversationId: 2026-03-arc-development
tags:
  - data-architecture
  - supabase
  - github
  - airtable
  - environments
  - cloudflare-r2
---

## Problem

Agent, role, and access data was stored only in localStorage. Each deployment (localhost, arc.bldrs.co) had isolated data. No mechanism existed to migrate data between environments or share it across instances.

## Environment model

Three environments, each a distinct origin with isolated data. Promotion is deliberate — never automatic.

```
development   localhost:5173        branch: claude
staging       arc-staging.bldrs.co  branch: staging
production    arc.bldrs.co          branch: main
```

Each environment has its own Supabase project and points to a different branch of `bldrs/arc-data`.

## Provider extension model

```
DataProvider interface:
  read(contextId, typeId, id?) → DataItem[]
  write(contextId, typeId, item) → DataItem
  delete(contextId, typeId, id) → void
  sync(contextId) → SyncResult

Providers:
  LocalStorageProvider    built-in, always active (local cache)
  GitHubProvider          extension: flat files, version control, source of truth
  SupabaseProvider        extension: relational DB, fast reads, backup
  AirtableProvider        extension: editorial CMS for Story.Earth / Nourish
```

## Read strategy (layered, fastest first)

1. localStorage (< 1ms) → return immediately, trigger background refresh
2. Supabase table (50–200ms) → update localStorage, return
3. GitHub YAML/MD (200–500ms) → update Supabase + localStorage, return

## Write strategy (write-through)

1. localStorage → UI updates immediately (optimistic)
2. Supabase table → reliable backup, queryable
3. GitHub YAML via Supabase Edge Function → source of truth, versioned
   (PAT never leaves the server)

## On server-side PAT storage and latency

Storing the GitHub PAT in a Supabase Edge Function adds ~50–150ms per write.
This is acceptable. A PAT in the browser is a serious security exposure.
Reads use localStorage cache and are not affected.

## Supabase schema (key tables)

```sql
agents      -- structured: auth_id, handle, display_name, role, preferences JSONB
data_items  -- flexible: context_id, type_id, environment, data JSONB
migrations  -- tracks applied migrations per environment
sync_log    -- audit trail for GitHub ↔ Supabase operations
```

The `data_items` table mirrors `arc:data` in localStorage, with an `environment` column
to keep dev/staging/production rows isolated in a shared project.

## Asset and media providers (separate from data providers)

```
CloudflareR2Provider    Web-optimized images and assets, served via CDN
GoogleDriveProvider     Original hi-res source files, download-on-demand
YouTubeProvider         Video hosting, embed metadata stored as data items
```

## Airtable integration

Airtable is the editorial CMS for Story.Earth and Nourish. Sync direction:
**Airtable → ARC → rendered site.** ARC reads, does not write back.
Records are cached in Supabase `data_items` with a configurable TTL.

## Build sequence

1. Supabase tables + Edge Function for GitHub writes (server-side PAT)
2. `SupabaseProvider` and `GitHubProvider` implementing DataProvider interface
3. Write-through wired into `storageService.saveDataItem()`
4. Environment detection + per-environment config
5. Testing: Vitest unit/integration, Playwright E2E, migration test suite
6. CI/CD: GitHub Actions per branch → test → build → deploy
7. Editorial providers: Airtable, Google Drive, Cloudflare R2
