# Best Practices & Stack Reference

**Important:** The `me/` folder is at the repository root (`./me/`). All file paths below are relative to the workspace root. Always read the actual `.md` file in `me/` — do not rely on memory or assumptions.

When the user requests work, consult the relevant practices and stack files. If a file has changed since your knowledge cutoff, re-read it.

---

## Practices

Coding rules and conventions that apply to every project in this repo. Always enforce the rules in these files when writing or reviewing relevant code. If uncertain, re-read the file.

| File | Coverage |
|------|----------|
| `me/practices/coding-standards.md` | TypeScript, App Router, Server Components, code style, UI frameworks, Tailwind, Radix, naming conventions |
| `me/practices/security-rules.md` | API keys, encryption, database RLS, authentication, input validation, CORS, HSTS, OWASP |
| `me/practices/api-design-rules.md` | HTTP semantics, URL naming, response envelope, pagination, status codes, validation, error handling |
| `me/practices/error-handling-rules.md` | Error boundaries, graceful degradation, user-facing messages, monitoring, logging |
| `me/practices/worker-patterns.md` | Stateless workers, per-service error isolation, polling loops, tier enforcement, alert deduplication |
| `me/practices/git-conventions.md` | .gitignore patterns, commit message style, branch naming, multi-AI file sync |
| `me/practices/frontend-design.md` | Anti-AI-slop design principles, typography, whitespace, color, hierarchy, patterns |
| `me/practices/vercel-middleware.md` | Middleware matcher config, exclusions, cost gotcha, checklist |

**When to use:** Before writing any code, check if a practice applies. Before submitting work, verify compliance.

---

## Audits

Copy-paste checklists for pre-launch and post-change reviews. When the user asks for an audit or before launching, open the corresponding file and go through every point systematically.

| File | Covers |
|------|--------|
| `me/audits/api-security-checklist.md` | OWASP API Top 10 — auth, injection, rate limiting, sensitive data, secrets |
| `me/audits/api-design-checklist.md` | HTTP semantics, naming consistency, response shapes, pagination, authorization |
| `me/audits/performance-checklist.md` | Core Web Vitals (LCP, INP, CLS), images, fonts, JavaScript bundles |
| `me/audits/mobile-checklist.md` | Touch targets, viewport, breakpoints, iOS Safari, orientation handling |
| `me/audits/error-handling-checklist.md` | Error boundaries, monitoring, user messages, empty states, timeouts |
| `me/audits/billing-checklist.md` | PCI compliance, webhook verification, subscription enforcement, dunning |
| `me/audits/onboarding-ux-checklist.md` | Signup flow, first-run experience, activation metrics, feature discovery |
| `me/audits/seo-checklist.md` | Structured data, crawlability, E-E-A-T signals, OG tags, AI search (GEO) |
| `me/audits/pre-launch-checklist.md` | Infrastructure, security, monitoring, legal, QA, polish — final gate before launch |

---

## Stack

Technology choices, architecture, and library recommendations. When generating code, use only the libraries and patterns documented in these files. If a deviation is necessary, ask the user first before proceeding.

| File | Documents |
|------|-----------|
| `me/stack/saas-stack.md` | Next.js + Supabase + Railway + Vercel + Resend + Dodo — rationale for each choice |
| `me/stack/project-structure.md` | Folder layout for Next.js SaaS + worker (app/, worker/, lib/, public/) |
| `me/stack/frontend-libraries.md` | Animation, video, UI components, forms, icons, data fetching, dates, utilities, toasts, charts, carousels |
| `me/stack/dodo-payments.md` | Dodo Payments integration — checkout flow, webhooks, signature verification, tier enforcement, UI patterns |
| `me/stack/fumadocs.md` | Fumadocs docs framework setup, MDX compilation, next.config.mjs, `.source/` generation, gotchas |

**When to use:** When scaffolding new code or evaluating dependencies, check these files first. Don't add libraries without justification from the stack.

---

## Templates

Starter files to copy into new projects. When scaffolding, use the templates in `me/templates/` as the starting point, then customize as needed.

| File | Use for |
|------|---------|
| `me/templates/claude-md-template.md` | Blank CLAUDE.md scaffold — copy, customize, save as `CLAUDE.md` in project root |
| `me/templates/gitignore-nextjs.md` | Base .gitignore for Next.js + worker projects — copy as `.gitignore` |
| `me/templates/worker-env-template.md` | All environment variables a polling worker needs — reference when setting up `.env.local` |

---

## Tools

Utilities and workflows for using AI, graphs, and code review. These are meta-tools for working with the codebase and AI.

| File | Purpose |
|------|---------|
| `me/tools/graphify-workflow.md` | When/how to run graphify, hook setup, reading outputs, Windows path fix |
| `me/tools/code-review-graph.md` | Graphify MCP tools reference, query patterns, when to use vs. file tools |
| `me/tools/ai-collaboration.md` | Multi-AI sync (Claude/Cursor/Windsurf/Gemini), workflow by task type |
| `me/tools/prompt.md` | **Project overhaul prompt** (copy-paste into Claude for existing project audit) + **CLAUDE.md standards block** (paste into any new project) |

---

## Root Files

| File | Purpose |
|------|---------|
| `me/INDEX.md` | Table of contents and how-to guide for using this knowledge base |
| `me/README.md` | Overview of the knowledge base, how it's organized, and when to use each section |

---

## Workflow: Using These Files

### Starting a New Project
1. Read `me/practices/coding-standards.md` for language and framework rules
2. Use `me/stack/project-structure.md` to scaffold the folder layout
3. Copy `me/templates/claude-md-template.md` → `CLAUDE.md` in the new project root
4. Copy the CLAUDE.md block from `me/tools/prompt.md` → append to project's `CLAUDE.md`
5. Copy `me/templates/gitignore-nextjs.md` → `.gitignore`
6. When ready, run `/graphify` to build a knowledge graph of the new project

### Overhauling an Existing Project
1. Paste **Part 1** from `me/tools/prompt.md` into Claude
2. Claude audits the codebase against all practices and fixes issues
3. After changes, run relevant audits from `me/audits/` before launch

### During Development
- When writing API routes, enforce rules from `me/practices/api-design-rules.md`
- When handling errors, check `me/practices/error-handling-rules.md`
- When adding frontend features, reference `me/practices/frontend-design.md`
- When adding dependencies, verify they're in `me/stack/frontend-libraries.md` or `me/stack/saas-stack.md`

### Before Launch
- Run `me/audits/pre-launch-checklist.md` (covers all other audits)
- For specific concerns, run the targeted audit (security, performance, mobile, etc.)
- Verify every critical item is checked off

---

## Quick Lookup

**"How should I handle errors?"** → `me/practices/error-handling-rules.md` + `me/audits/error-handling-checklist.md`

**"What database should I use?"** → `me/stack/saas-stack.md`

**"Is this API route correct?"** → `me/practices/api-design-rules.md` + `me/audits/api-design-checklist.md`

**"What libraries can I add?"** → `me/stack/frontend-libraries.md` + `me/stack/saas-stack.md`

**"What are the security rules?"** → `me/practices/security-rules.md` + `me/audits/api-security-checklist.md`

**"How do I structure the project?"** → `me/stack/project-structure.md`

**"I need to overhaul a project"** → `me/tools/prompt.md` (Part 1)

**"What should I put in CLAUDE.md?"** → `me/templates/claude-md-template.md` + `me/tools/prompt.md` (Part 2)

---

## Remember

- All files are in `me/` at the workspace root.
- These are the source of truth. If they've changed, re-read them.
- When unsure, check the file instead of relying on memory.
- These practices are applied to every project using this template.
