# Project Prompts

Two prompts for applying my practices to any project. Copy-paste ready.

---

## Part 1 — Project Overhaul Prompt

Paste this into Claude (or any AI) to audit and overhaul an existing Next.js project to match my standards.

---

```
Audit this codebase and overhaul it to match the following standards. Work through each category. For each issue found, fix it — don't just list problems.

---

## TypeScript & Next.js

- TypeScript everywhere. No plain JS files. No `any` unless absolutely unavoidable. Strict mode on in tsconfig.
- App Router only. If Pages Router is used, migrate it.
- Server Components by default. Add `"use client"` only for browser APIs, event handlers, or state.
- Co-locate page-specific logic in the route segment. Move shared logic to `lib/`.
- Wrap async auth checks in `<Suspense>`.

## Project Structure

Ensure structure matches:
```
app/                    # Next.js App Router
  layout.tsx
  page.tsx              # Landing (public)
  dashboard/page.tsx    # Protected
  integrations/page.tsx
  alerts/page.tsx
  settings/page.tsx
  pricing/page.tsx
  login/page.tsx
  signup/page.tsx
  auth/callback/route.ts
  api/
    integrations/route.ts
    integrations/[id]/route.ts
    usage/route.ts
    alerts/route.ts
    billing/route.ts
    webhooks/route.ts
lib/
  supabase.ts
  encryption.ts
  tiers.ts
worker/                 # if applicable
```

## Styling

- Tailwind CSS for all styling. Remove inline styles and CSS modules unless forced.
- Mobile-first. All touch targets minimum 48×48px.
- `next/image` for all images. Add `priority` prop on hero images.
- `font-display: swap` for custom fonts. Preload critical fonts.

## API Design

- Every route returns `{ data: T }` on success, `{ error: string }` on failure. No bare arrays, no mixed shapes.
- Correct HTTP methods: GET reads, POST creates, PATCH updates, DELETE removes. No mutations on GET.
- kebab-case paths (`/api/alert-configs`). Plural nouns for collections.
- All list endpoints paginated (default 20–50, return total count).
- Correct status codes: 200/201/204 success; 400 bad request; 401 unauth; 403 forbidden; 404 not found; 422 business logic; 429 rate limit; 500 server error. Never return 200 with an error body.
- Validate request body with Zod on every POST/PATCH. Return field-level errors on failure.
- Strip fields the caller isn't authorized to see. Never return encrypted keys, tokens, or passwords.

## Security (non-negotiable)

- Never log raw API keys, tokens, or passwords.
- Encrypt user-supplied credentials before storing (AES-256). Key in env var, never in code.
- Nothing sensitive in `NEXT_PUBLIC_*` env vars.
- RLS enabled on every Supabase table. `auth.uid() = user_id` policies.
- Service role key server-only. `anon` key on client.
- Validate session server-side on every protected route. Never trust user-supplied `user_id` in request bodies.
- Validate all user input at API boundaries with Zod. Reject unknown fields.
- HTTPS everywhere. HSTS header: `Strict-Transport-Security: max-age=31536000; includeSubDomains`.
- CORS: whitelist known origins only. Never `*` in production.
- Check resource ownership on every mutation — not just "is logged in", but "owns this resource".
- Verify webhook signatures before processing any payment event.

## Error Handling

- `error.tsx` on every major route segment. Error boundaries must show a recovery action (retry/back).
- Wrap every third-party call in try/catch. Log full error server-side. Return generic message to client.
- In polling workers: catch per-integration, log, continue — never re-throw from the loop.
- Loading states on all async operations (skeleton or spinner).
- User-facing messages are actionable: "GitHub token expired — reconnect your account", not "Something went wrong".
- Never expose stack traces to the client.

## Middleware

- `middleware.ts` must have an explicit `config.matcher`.
- Exclude static assets from the matcher:
```ts
export const config = {
  matcher: [
    '/((?!_next/static|_next/image|favicon\\.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp|ico|css|js)$).*)',
  ],
};
```
- Or use an explicit route list (preferred — auditable):
```ts
export const config = {
  matcher: ['/dashboard/:path*', '/settings/:path*', '/integrations/:path*', '/alerts/:path*'],
};
```
- Middleware does token checks and redirects only. No DB queries, no external API calls.

## Code Style

- No unnecessary dependencies. If it's 10 lines, don't add a package.
- No speculative abstractions. Three similar lines > a premature utility.
- Comments only where logic is non-obvious. No narrating obvious code.
- No backwards-compatibility hacks for code being removed — delete it cleanly.

## Forms

- Use `react-hook-form` + `zod` + `@hookform/resolvers`. Not hand-rolled validation.

## Payments (if applicable)

- Webhook endpoint verifies signature before processing anything.
- Subscription tier read from DB, not from client request body.
- Source of truth is your DB (`subscriptions` table), synced via webhooks.
- Downgrade at period end, not immediately on cancellation.
- Grace period (3–7 days) on payment failure before downgrading.
- Enforce tier limits in both the API and worker.

## Worker (if applicable)

- Stateless — no in-memory state between runs. All state lives in the DB.
- Idempotent polling — running twice produces the same DB state (upsert).
- Never throw from the top-level loop. Catch per-integration, mark error in DB, continue.
- No in-memory caches. Read fresh from DB each cycle.
- Tier-based polling intervals: Free 15min, Pro 5min, Team 1min.
- Alert deduplication via `alert_history` table. Don't re-fire until metric recovers and crosses again.

## Git

- `.env*` gitignored. Build artifacts gitignored. `node_modules` gitignored.
- Commit messages: present-tense imperative, area prefix: `"worker: fix rate limit handling"`.
- Branches: `feat/`, `fix/`, `chore/` prefixes.

---

Start with a summary of everything you found that doesn't comply, then fix it all.
```

---

## Part 2 — CLAUDE.md Section

Paste this block into any project's `CLAUDE.md` to make Claude follow these standards on every change.

---

```markdown
## Development Standards

Follow these rules on every change — new files, edits, refactors, and fixes.

### TypeScript & Next.js
- TypeScript everywhere. No `any`. Strict mode on.
- App Router only. Server Components by default — `"use client"` only for browser APIs, events, or state.
- Co-locate route logic in the segment. Shared logic in `lib/`.

### API Design
- All routes return `{ data: T }` on success, `{ error: string }` on failure. No exceptions.
- Correct HTTP methods. Correct status codes (never `200` with an error body).
- Validate request body with Zod on every POST/PATCH. Reject unknown fields. Field-level errors on failure.
- Paginate all list endpoints. Never return unbounded arrays.
- Never return sensitive fields (tokens, encrypted keys, passwords) in responses.

### Security
- Never log API keys, tokens, or passwords.
- Validate session server-side on every protected route. Never trust user-supplied `user_id`.
- Check resource ownership on every mutation — not just authenticated, but authorized.
- Verify webhook signatures before processing any payment event.
- RLS enabled on all DB tables. Service role key server-only.
- Input validated at every system boundary (API routes, server actions).

### Error Handling
- `error.tsx` on every major route segment with a recovery action.
- Third-party calls wrapped in try/catch. Log server-side, return generic message to client.
- Never expose stack traces to the client.
- Loading states on all async operations.
- User-facing errors are actionable and specific.

### Middleware
- Always set `config.matcher`. Never run middleware on `_next/static`, `_next/image`, or static assets.
- Middleware does token checks and redirects only — no DB or external API calls.

### Code Style
- No unnecessary dependencies.
- No speculative abstractions.
- Comments only where the WHY is non-obvious.
- Delete removed code cleanly — no backwards-compat hacks.

### Styling
- Tailwind only. No inline styles. Mobile-first. Touch targets ≥ 48×48px.
- `next/image` for all images.

### Forms
- `react-hook-form` + `zod` + `@hookform/resolvers` for all forms.
```
