---
title: "Language refactor: user → agent"
date: 2026-03-30
kind: refactor
project: arc
conversationId: 2026-03-arc-development
files:
  - src/app/services/auth-service.ts
  - src/app/services/supabase.ts
  - src/app/contexts/AuthContext.tsx
  - src/app/contexts/ConfigContext.tsx
  - src/app/components/SyncStatus.tsx
  - src/app/pages/SettingsPage.tsx
  - src/app/pages/BlogPage.tsx
  - src/app/pages/BlogPostPage.tsx
  - src/app/types/art-gallery.ts
  - src/app/pages/system/AgentsManagementPage.tsx
tags:
  - refactor
  - language
  - auth
  - agents
---

## Decision

Rename "user" to "agent" throughout ARC's domain language. An agent has sovereignty over their data and experience; a user merely consumes a product. The language shift encodes this design intention in the codebase.

## The boundary rule

Supabase speaks `User`. ARC speaks `Agent`. The auth service is the adapter.

```
Supabase session.user  →  [mapSupabaseUserToAgent()]  →  ARC ArcAgent
     (untouched)                 (boundary)                (renamed)
```

Every reference to "user" that lives outside the auth service layer — in components, contexts, state, UI copy — is ARC domain language and was renamed. References inside Supabase SDK calls were left untouched.

## New type introduced

```typescript
// AuthContext.tsx
export interface ArcAgent {
  id: string;        // Supabase user.id — stable identity anchor
  email: string;
  displayName?: string;
  avatarUrl?: string;
}
```

## What changed (Pass 1 — auth layer)

- `auth-service.ts`: `AuthUser` → `AuthAgent`, `AuthSession.user` → `.agent`, `getUser()` → `getAgent()`
- `supabase.ts`: `getCurrentUser()` → `getCurrentAgent()`
- `AuthContext.tsx`: introduced `ArcAgent` + `mapSupabaseUserToAgent()`, state renamed `user` → `agent`, context exposes `agent` not `user`
- Downstream consumers: `ConfigContext`, `SyncStatus`, `SettingsPage`, `BlogPage`, `BlogPostPage` — all updated to destructure `agent` from `useAuth()`

## What changed (Pass 2 — data model + UI)

- `art-gallery.ts`: `UserPreferences` → `AgentPreferences`
- `AgentsManagementPage`: `Agent` interface updated — `first_name/last_name/password` replaced with `handle/displayName/auth.provider/auth.userId`. Password removed entirely; auth provider owns credentials.
- `AgentSetup` component: new onboarding step collecting handle, display name, avatar strategy
- `InstallPage`: three-step flow (WelcomeGuide → GitHubSetup → AgentSetup)

## Zero TypeScript errors

The compiler flagged every missed reference automatically. All errors resolved before commit.
