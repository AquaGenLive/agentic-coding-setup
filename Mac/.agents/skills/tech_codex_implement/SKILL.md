---
name: tech_codex_implement
description: >
  Implement or resume an approved specification and implementation plan with
  behavioral verification, independent review, and required integration gates.
  Use for implementation with the Codex workflow; exclude specification writing
  and workflow analysis without implementation.
---

# Codex Specification Implementation

Complete the approved scope in one supervised run, following its dependencies and the user's stopping boundaries. The normal path is:

**Agree contracts and ownership → implement coherent batches → freeze an integrated candidate → independent review and required gates → accept and continue.**

Preserve approved acceptance criteria, security requirements, independent review and repository gates. Optimize total time and resources across implementation, repair, verification and handover within that quality bar. A cheaper attempt or faster submission alone is not an efficiency gain.

Read this core once per agent context. Read conditional guidance when it applies; follow-up briefs carry changed scope, findings and evidence rather than the whole skill again.

| Situation | Reference |
|---|---|
| First technical review; risky mechanism; rejected candidate; model escalation; reopened finding | [Review and escalation](references/review-and-escalation.md) |
| Command/browser ownership; tool access; waiting; stalled execution; expensive performance retry | [Execution and recovery](references/execution-and-recovery.md) |

## 1. Scope, roles and authority

The main conversation is the single Lead. It owns decomposition, contracts, dependencies, ownership, communication, acceptance and progress checks. Do not create another coordinator or let workers delegate further.

| Role | Model / configuration | Responsibility |
|---|---|---|
| Lead | Existing main conversation | Coordination; owned integration fixes and verification when efficient |
| Default implementer | `sol_worker`: `gpt-6.1-sol` / `medium` | Implementation needing design, integration judgment or difficult debugging |
| Mechanical implementer | `luna_worker`: `gpt-6-luna` / `max` | Fixed mechanical work using established patterns |
| Independent reviewer | `default`: `gpt-6.1-sol` / `medium` | Read-only review of complete affected behavior and integration |
| Expert reviewer | `default`: `gpt-6-astra` / `high` | Finite diagnosis at the Sol threshold or earlier on concrete evidence |
| Expert implementer | `default`: `gpt-6-astra` / `high` | Direct transfer supported by diagnosis, or after a guided repair fails |

New subagents use `fork_turns: "none"`; specify the table's model/effort for `default` roles. Verify capabilities from exposed tools and their actual results. Missing required roles or genuine access blockers require a fallback decision before substitution; do not silently change models or configuration.

Lead-owned integration, tests, fixtures and browser work require exclusive ownership and independent review, including conflict resolutions. Lead takeover cannot evade an escalated model stage. Reviewers never write production fixes or accept their own code; substantive findings and repository gates remain binding.

A request to implement the approved plan authorizes continuation across its remaining milestones. An explicit range, sequential-order requirement or stopping boundary takes precedence. Parallel work never expands scope or bypasses prerequisites. Commit, push, publication, deployment, paid-setting changes and new user-owned chats need the relevant user authorization.

## 2. Resume and lightweight tracking

Read applicable repository instructions, governing spec/plan requirements, current `status.md` and relevant accepted decisions in `progress.md`. Reconcile the actual checkout with the handover, including uncommitted and untracked files. Retrieve additional history only for a concrete gap.

| Record | Owner / content |
|---|---|
| `status.md` | Lead; current scope/milestone, ready batches, author/reviewer IDs, file/resource ownership, open findings and attempt stages, blockers, next actions and progress deadline. Aim roughly 60 lines; required facts take priority. Replace stale live state. |
| `progress.md` | Lead; accepted milestone/batch outcomes, durable decisions and pause/completion handovers with evidence links. |
| Existing evidence index | One named writer; source, command/directory, result, log/proof link and limitation. Authors keep their own logs; do not append concurrently to a shared file. |
| Requirement coverage in the existing plan or evidence index | Lead; `requirement/section → owner → acceptance evidence → state/blocker`. Reuse existing IDs and tests; distinguish implemented, verified, accepted and externally blocked scope. |

Create missing records only for implementation. Update at decisions, review dispositions, acceptance or handover; record facts once and link them. Replace superseded status instead of copying live-state histories into progress. Keep essential results durably accessible; temporary logs support them. Evidence details belong in the execution reference.

Resume preserves valid evidence, decisions, causal attempt counts/model stages, command ownership and restoration obligations. Apply workflow/model transfers at a safe assignment boundary with a handover; do not interrupt commands merely to change process. Preserve exhausted stages. A supervised pause needs explicit resume.

## 3. Coherent batches and bounded parallelism

Size work by a shared outcome/mechanism and meaningful acceptance boundary. A batch may include several consumers, files or related behaviors. Group repetitive integrations of an established contract instead of assigning a fresh author and full acceptance cycle to each widget. Keep a transaction with its atomic effects, rollback and authorization. Split genuinely independent mechanisms or incompatible dependencies, not merely to fill agent slots or use Luna.

Before parallel implementation agree the shared interfaces, invariants and acceptance assertions. Lead resolves routine contract choices from the approved requirements and existing patterns; only unresolved material requirement, scope, authority or feasibility conflicts need the user. Establish and test a new shared contract before broad consumer rollout. New contract uncertainty blocks dependent work; independently ready work may continue.

Choose the smallest team that can usefully advance current dependencies, retaining independent review. Add an author when work can proceed independently and the expected completion-time benefit exceeds coordination, integration and shared-resource costs; record that reason in the brief. Keep coupled mechanisms with one owner while context remains useful. Reserve reviewer/escalation capacity. Extra reviewers need distinct scopes and a concrete critical-path benefit; returned agents do not necessarily free slots.

Briefs name outcome/spec sections, owned files, contracts/dependencies, decisive checks, resources/cleanup and next handoff. Tell authors to preserve others' edits. Each file has one writer; sequence shared locales, exports and fixtures. Independence also requires compatible schema/API changes, generated output and shared state.

Prefer the existing checkout for disjoint ownership. Use worktrees only when concrete isolation needs justify them and the environment supports preserving the actual source, including uncommitted work. Do not create one per worker or lose edits by starting from an older commit. Serialize incompatible database fixtures, build outputs, servers/ports, persistent browser profiles and global E2E runs. Shared-resource waits block only their dependent operations; authors can continue safe owned work.

Reuse authors and reviewers across related corrections while context is useful. Fresh contexts need an unrelated scope, required model transfer or evidenced context problem. Replacement requires a checkpoint and settled/transferred commands and cleanup; preserve attempt history. Do not rotate per consumer or claim an in-place context reset. Report capacity limits.

### Worker selection

Assess the whole batch before selecting an author. Use Luna when criteria/approach are fixed, existing patterns cover the work, and no design/API/behavior uncertainty or difficult diagnosis remains. Presentation, localization, repetitive wiring, routine tests and browser proof can qualify.

Use Sol for new mechanisms, financial/authorization or async/focus lifecycle decisions, nontrivial integration or uncertain causes. Record `worker + model/effort | concrete reason`. Transfer Luna early if broader judgment emerges; no model quota or need to exhaust cheap retries. Evidence-supported expert diagnosis/transfers follow the review reference, preserving corrective stages.

## 4. Implement and verify autonomously

The author inspects relevant requirements/source and publishes a short concrete approach: affected contracts/files, reuse, order, tests and material uncertainties. Ordinary plans and correction deltas are informational; continue without acknowledgment. Resolve material scope/contract conflicts before dependent changes. Do not turn every routine decision into another Lead/reviewer handoff or add discovered robustness work without a governing requirement or demonstrated preservation defect.

For new logic or a bug, use meaningful behavioral regression tests, including expected RED where required. A setup/import/compile failure alone does not establish a behavioral failure. Mechanical presentation changes use appropriate existing tests and rendered evidence; do not manufacture retrospective RED artifacts for bookkeeping. Repository TDD/test instructions still take precedence. Never weaken valid assertions, invent API requirements or change correct behavior just to satisfy a faulty test.

For new or materially changed financial, authorization, destructive, migration or concurrency/recovery mechanisms, use one early proof checkpoint from the review reference. Before dependent rollout, prove the real application boundary and the smallest relevant existing compatibility checks, including affected legacy behavior. Routine consumers reuse valid mechanism proof.

During work run focused and affected-neighbor checks after meaningful changes/corrections. Map changed shared contracts to actual producers, consumers and compatibility tests; avoid relying on stale file inventories. Author owns execution/results/cleanup. Two fixes leaving the same underlying failure without useful new evidence trigger a bounded discriminating diagnosis, not speculative edits or automatic full reruns; unresolved diagnosis can go to the existing reviewer.

Prepare a stable integrated candidate for consolidated review and required broad gates. One review can cover a coherent batch of mechanical or ordinary changes. Give the reviewer affected paths, requirements, source marker, tests/results, known limits and any prior findings. **Review and gates may run concurrently only against the same frozen source** with resource ownership agreed. In a shared checkout, stop writes to any source/config/fixtures the gate reads; other work needs genuine isolation. Reviewed scope is frozen until feedback or explicit withdrawal.

Preserve every repository/plan-required test, compile/typecheck, build, E2E, documentation and derived-artifact gate. Consolidate broad suites at stable batch/milestone boundaries where those instructions allow it; earlier required checks remain required. One owner reports each result. Reuse unaffected valid evidence; later source changes renew affected evidence/review and every gate invalidated by that change. Do not repeat green suites merely to repackage evidence, but do not claim an old pass covers changed source. Required sequential E2E projects stay sequential.

At integration, review new interactions and changes lacking valid review; avoid unchanged component/package reviews. Gate defects return to their causal author/stage; review closure is provisional until gates pass. Reconcile approved requirements with current assertions/results and identify remaining scope, gaps, skips and limits. Aggregate test counts alone cannot establish coverage or completion.

## 5. Communication, waiting and blockers

Use native agent messaging for material scope/contract decisions, urgent blockers, ready review material and completed handoffs. Establish the actual route once; recheck only after a capability change. Do not use other user-owned chats to coordinate subagents. Missing native messaging uses a finite return/reactivation handoff, not fabricated tools.

Authors return at a ready candidate, required early checkpoint, completed verification or genuine blocker: `outcome | source/evidence | open findings/attempts | cleanup/active obligations | next action/owner`, then end their turn. Finish or explicitly transfer active command ownership; do not abandon handles. Reviewers return a finite disposition and end their turn. Reuse returned agents via the supported reactivation tool for related follow-up; no acknowledgment-only turns or waiting for future assignments. A blocking request explicitly names its next owner and needed decision; Lead routes it before waiting.

Use direct native event-aware waits, bounded by tool limits and actual deadlines: Lead up to 600 seconds; subagents awaiting a decision up to 120 seconds. Workers own command-handle waits. The execution reference governs timeouts, missing responses and recovery.

Record start time/timezone and a Lead assessment at +60 minutes, then +30 minutes after each actual assessment. Preserve deadlines across compaction, handoffs and milestones. At existing assessments check accepted/remaining scope, repeat repairs, coordination cost, blockers and readily available usage; follow resource guidance in the execution reference. Record only the delta and next deadline, without an extra assessor, usage polling or history reload.

For a first stalled assessment, change the unproductive approach once. The same blocker without meaningful progress at two consecutive scheduled assessments requires a safe checkpoint and human decision for that workstream. Capability failures use the earlier execution diagnostic. Continue independent authorized work; pause the whole run when no safe independent work remains, a prerequisite blocks all ready work, or the user requests it. A whole-run pause stops new assignments and needs explicit resume. Unfinished work or long but progressing tests alone are not stalls. Never fabricate missed checks or infer approval from silence.

## 6. Accept, continue and finish

Acceptance requires working behavior, resolved substantive findings, independent review, all applicable current-source gates, matching docs and cleanup. The Lead checks evidence completeness and acceptance without duplicating technical review or waiving blockers.

At each milestone checkpoint the accepted source/outcomes, evidence/limits, causal attempt history and cleanup. Continue to the next authorized milestone with satisfied dependencies, preserving the progress deadline; do not ask permission again or end the run solely at a milestone boundary. Cross-milestone overlap requires the approved plan and user scope to allow it, satisfied prerequisites and explicit ownership; never bypass an explicit sequential limit.

At completion or a requested boundary, obtain final owner handoffs, settle commands/restoration, release owned browser/server/fixture resources, and confirm known child turns ended using completion events or one exposed state check. Recheck only children acted on. A stopped turn does not prove its process stopped; returned agents are not claimed retired or capacity freed. Report unresolved cleanup honestly. Checkpoint accepted/current/pending work, clear the active deadline only at safe shutdown, and report outcome, validation, limits and remaining work. No unattended continuation or new scope after the run ends.
