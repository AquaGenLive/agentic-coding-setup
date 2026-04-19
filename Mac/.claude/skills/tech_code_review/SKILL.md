---
name: tech_code_review
description: >
  Review implemented code against its specification after tech_implement has
  finished. Manually invoked via the /tech_code_review slash command with a
  spec folder as argument. Verifies that every requirement in specification.md
  and implementation-plan.md is actually present in the code, flags drift or
  gaps with exact file:line references, and confirms the relevant type-check
  and spec-specific tests pass. Read-only: the skill reports findings and
  never modifies code. Do NOT auto-trigger on generic phrases like "review my
  code" or "check the implementation" — only run when the user explicitly
  invokes /tech_code_review.
argument-hint: <spec-folder-name> (e.g., "13-i18n" or just "13")
---

# Code Review Against Specification

You are reviewing a completed implementation against its spec. The `tech_implement` workflow has just finished (or the user is in a post-implementation state), and now your job is to check whether the code faithfully matches what the spec requires.

The core principle: **the spec is the source of truth**. For every concrete requirement in `specification.md` and `implementation-plan.md`, the actual code must implement it correctly. Your job is to find drift — things missing, things done differently, things that look done but don't actually run.

You are read-only. Produce a report. Do not modify any code. If the user wants fixes afterwards, they will ask.

## Input

The spec folder identifier is: **$ARGUMENTS**

If no argument was provided, list the `spec/` directory and ask the user which spec to review.

## Phase 1: Load the spec

1. **Resolve the spec folder.** The argument may be a full folder name (e.g., `13-i18n`), a number (`13`), or a partial name. Look under `spec/` to find the matching directory. If multiple match, ask the user which one.

2. **Read these files** from the resolved folder:
   - `specification.md` — the authoritative requirements (must exist)
   - `implementation-plan.md` — ordered steps and files touched (must exist)
   - `progress.md` if present — which steps were marked complete
   - `brainstorming.md` only if you hit an ambiguity in the spec and need the original decision context

   If `specification.md` or `implementation-plan.md` is missing, tell the user and stop — there is nothing authoritative to review against.

3. **Read `CLAUDE.md`** for project conventions (tech stack, directory layout, test commands, build commands).

4. **One-line summary to the user**: which spec, how many steps, whether `progress.md` marks all done or only a subset. If only a subset is marked done, review only those completed steps; flag the rest as "not yet implemented" (expected, not a defect).

## Phase 2: Detect the change set

The goal is to know which files make up the implementation of this spec.

1. **Start with uncommitted changes.** Run `git status` and `git diff --stat` to see modified/new files. In most cases the implementation just finished and is not yet committed.

2. **Heuristic: is the uncommitted change set plausibly the whole spec?** Cross-reference the files listed against the `implementation-plan.md` (which enumerates files to create/modify per step). Rough checks:
   - Does the uncommitted set include the key files the plan names (migrations, config classes, new modules)?
   - If the plan says "14 namespaces" or "4 email templates" etc., does the uncommitted set contain the expected number?
   - If `progress.md` marks all steps done but the uncommitted set is clearly too small (e.g., plan says ~30 files, only 3 are uncommitted), the earlier steps were likely committed already.

3. **Extend to recent commits if needed.** When the uncommitted set under-covers the spec:
   - Find recent commits that look related: `git log --oneline -20` and scan for spec-related messages (the folder name, the feature name, conventional-commit prefixes mentioned in `CLAUDE.md`).
   - Collect their files: `git log --name-only <commit>..HEAD -- .` or `git show --stat <commit>`.
   - Merge those files with the uncommitted set.
   - Be explicit to the user that you expanded the review scope and which commits you pulled in.

4. **Tell the user** what you're reviewing: uncommitted changes only, or uncommitted + N recent commits. One sentence.

## Phase 3: Build a verification checklist

Read the specification section by section and extract every concrete requirement. Typical categories:

- **Files that must exist** — new modules, configs, migrations, templates, tests, docs
- **Schema / data model** — tables, columns, enum values, constraints, defaults
- **API contract** — endpoints, request/response shapes, validation rules, error codes
- **Behavior** — side effects, ordering rules, fallback rules, error handling, security checks
- **UI surface** — new pages, components, form fields, translations, routing
- **Config** — env vars, feature flags, profiles, cache settings, dependencies added
- **Tests** — test files the spec explicitly calls out
- **Docs** — documentation the spec says should be added or updated

Group these into 3–5 review **scopes** aligned with the project layout. Typical split for a full-stack spec:

- Backend code (models, services, controllers, DTOs, config)
- Frontend code (components, pages, hooks, services, types)
- Database / migrations
- Tests (backend + frontend + e2e)
- Docs (under `doc/` and `CLAUDE.md`)

Adapt the split to the actual spec. A pure-frontend spec doesn't need a backend scope.

## Phase 4: Delegate verification to parallel agents

Spawn one `Explore` agent per scope — **all in a single message** so they run in parallel. Each agent gets:

- **The specific spec sections** that cover this scope. **Paste relevant excerpts** into the prompt — don't just reference section numbers. The agent hasn't seen the spec.
- **The exact files expected** to exist or be modified for this scope, pulled from the implementation plan.
- **Project conventions** (the relevant parts of `CLAUDE.md`).
- **A strict reporting format.** Ask each agent, per requirement, to return one of:
  - **✅ Matched** — implemented as specified
  - **⚠️ Partial / Deviation** — done differently or incompletely. Include: what the spec requires vs what the code does, and a file:line reference.
  - **❌ Missing** — not implemented. Include: the requirement and where it should have been added.
- **A word cap** (typically ≤ 800 words). Ask for terse bullets, no preamble or self-introduction.

### Agent prompt template

```
You are verifying one scope of spec {nr}-{slug} against the actual code.

Spec excerpts (authoritative):
<paste the relevant sections>

Files expected to exist or be modified (from implementation-plan.md):
<list>

Project conventions:
<relevant CLAUDE.md excerpts>

For each requirement above, report exactly one of:
  ✅ Matched — <one line>
  ⚠️ Partial/Deviation — spec says X, code does Y at <file>:<line>
  ❌ Missing — requirement + where it should have been added

Keep the report under 800 words. Use bullets. No summary paragraph.
```

Do not run more than ~5 agents in parallel — beyond that you're just adding coordination overhead.

## Phase 5: Verify at runtime

Requirements can look right in code and still be broken. After the parallel agents report, verify a few runtime claims:

1. **Type-check / compile** — whichever applies to the changes:
   - Frontend TypeScript: `npx tsc --noEmit` (run from the frontend directory)
   - Backend Java: `./mvnw compile` (run from the backend directory)
   - Other stacks: use whatever `CLAUDE.md` specifies.

2. **Spec-specific tests.** The default is **targeted**, not full-suite. Run the tests the spec itself introduces — these are the fastest signal that the feature actually works:
   - Parity tests, migration tests, new integration tests the spec names.
   - A few key unit tests for newly added services or components.
   - If the user asks for a full run (or the targeted runs suggest broader breakage), then expand.
   - If you choose to skip the full suite, **say so explicitly** in the report so the user knows the coverage of your verification.

3. **Spot-check findings.** Trust-but-verify any agent claim that's load-bearing — especially `⚠️` / `❌` items. An agent may misread subtle code (e.g., a value comes via an interceptor rather than being passed explicitly). Open the flagged file and confirm before putting it in the final report.

## Phase 6: Report findings

Produce one concise report for the user. Use this structure:

```markdown
## Spec {nr}-{slug} Review

<one sentence: review scope — uncommitted only, or + N commits>

### ✅ Fully implemented
- <grouped bullets; keep to the high-signal items, don't list every trivial match>

### ⚠️ Partial / Deviations
For each:
- **What**: what the spec requires vs what the code does
- **Where**: `<file>:<line>`
- **Impact**: does the feature still work functionally, or is this a real gap?

### ❌ Missing
For each:
- **What**: requirement (brief quote or paraphrase from the spec)
- **Where**: where it should have been added

### Verification
- Type-check: PASS | FAIL (command used)
- Tests run: <list of test files or commands> — PASS | FAIL
- Full suite: not run (note this explicitly) | PASS | FAIL

### Recommendation
- **Blockers**: what must be fixed before this can ship (typically: anything `❌` or functional `⚠️`)
- **Nice-to-haves**: cosmetic deviations the user may want to address
- **None**: if everything is clean, say so plainly
```

## Guidance

- **Be concrete.** Every finding needs a file path, and a line number when possible. A review without file paths isn't actionable.
- **Distinguish functional from cosmetic deviations.** If the spec shows example code `api.post('/x', { ..., language: i18n.language })` but the axios interceptor already sets `Accept-Language`, the feature still works — flag as a minor deviation, not a blocker. Explain in one sentence *why* it still works.
- **The spec is the authority for requirements.** The implementation plan describes *how* to build the spec; the spec describes *what* should be built. If they disagree, the spec wins unless the user has explicitly noted a change.
- **Respect scope.** Don't flag issues in code that predates this spec or sits outside the spec's concerns. The review is "did this spec get implemented correctly?", not "is this code perfect?".
- **Be fair about `progress.md`.** If it shows some steps as done and others not, review only the done ones. The not-done ones are not defects.
- **Be upfront about what you skipped.** If you didn't run the full test suite, or you only checked 4 of 6 scopes because of time, say so. The user can then decide to ask for more.
- **When everything is clean, keep the report short.** A one-paragraph "everything matches, here's what I verified" is better than a wall of ✅s.
- **Never modify code.** If the user wants fixes, they'll ask for them in a follow-up turn.

## Escalation

Stop and ask the user if:
- The spec folder you resolved doesn't match what they meant.
- The spec is missing required files and you cannot review reliably.
- The change set looks inconsistent (e.g., `progress.md` says done but clearly half the files are missing and you can't find matching commits).
- An agent surfaces a claim you can't verify and you need a decision on whether to trust it.

Don't guess through ambiguity — a short clarifying question is cheaper than a misleading report.
