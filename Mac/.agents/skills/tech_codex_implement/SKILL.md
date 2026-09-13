---
name: tech_codex_implement
description: >
  Implement or resume an approved specification and implementation plan through
  one supervised assignment at a time, with focused planning, behavior-based
  TDD, bounded implementation attempts, and independent review. Use when the
  user wants to code an existing spec with the Codex implementation workflow.
  Not for creating specifications or analyzing a workflow without implementing it.
---

# Codex Specification Implementation

One approved assignment at a time. Deliver behavior, evidence, and independent acceptance; stop at the user's boundary.

## 1. Roles and authority

| Role | Owns | Spawn configuration |
|---|---|---|
| Root | Scope, overall progress, supervision, user decisions, stop boundary | Current model/effort |
| Assignment lead (Lead) | Dependencies, slicing, coordination, findings, stalls, escalation, acceptance | `default`, `gpt-6-astra`, `medium` |
| Implementer | Plan, tests, code, affected docs, verification | `luna_worker`, fixed `gpt-5.6-luna` / `max` |
| Escalated implementer | Remaining work after two unsuccessful Luna candidates | `default`, `gpt-6-astra`, `high` |
| Independent reviewer | High-risk mechanism/proof and substantive candidate review | `default`, `gpt-6-astra`, `medium` |

- Table configurations list `agent_type`, model, effort. All subagents: `fork_turns: "none"`; default agents use the table's `model` and `reasoning_effort`. Preserve explicit user overrides; do not change root settings. If roles/capacity/delegation are unavailable, report and obtain a fallback choice before substitution.
- Root spawns one active Lead only. Lead spawns at most one active implementer and one independent reviewer; slices run sequentially. No further delegation by workers/reviewer, nested leads, or extra worktrees/databases merely to fill slots.
- Root and Lead may inspect source/evidence within their responsibilities below and edit prose/tracking; **no application/test/migration/build/runtime/script edits**, including small fixes or code conflict resolution. Explicit skill editing is exempt. Reviewer is read-only and never accepts its own implementation.
- Replacement author takes exclusive ownership after the former author stops editing; never overlap Luna/Astra writers.
- While Lead runs, Root handles communication, supervision (§7), and decisions beyond Lead's mandate. No other assignment, parallel investigation, duplicate routine reviews, worker-message relaying, or test supervision. Handle scope changes/real blockers immediately when needed.
- Lead resolves routine technical decisions and validated fixes. Escalate material requirement/scope/authority conflicts to Root, which uses existing approval or asks the user. No commit, publish, deploy, usage reset, or paid-setting change beyond existing authority.

## 2. Resume and tracking

Resolve the spec directory and read applicable repo instructions. Root reads governing scope/acceptance/stop requirements; Lead owns detailed spec, source, and build/test investigation. Ask only for unresolved target/material requirements. Preserve exact step names, dependencies, existing edits, accepted decisions, and valid verification.

| File | Exclusive owner | Contents |
|---|---|---|
| `progress.md` | Root | Milestones, accepted outcomes, durable decisions, historical evidence links, current supervision record (§7). No worker diary or duplicate live findings. |
| `status.md` | Active Lead | Assignment/objective, slice sequence/current brief, decisions, completed side requests, agent IDs/ownership, proof/findings/attempt counts, escalation/blockers, next action, evidence links. Replace stale state; aim ~60 lines, necessary facts take priority. |

Root may bootstrap/recover `status.md` only without an active owner, then explicitly transfer ownership. No simultaneous writers. Create missing tracking files for implementation, not skill discussion/editing; do not rewrite existing history unnecessarily.

On resume/compaction: Root reads supervision + status; Lead reads status. Reconcile with newer user instructions; Lead verifies relevant current source/evidence. Root opens underlying artifacts only for a concrete scope, supervision, or acceptance gap. Tracking never overrides requirements. Follow history only for a concrete gap, not to reload everything or rerun accepted gates. Reopen settled decisions/completed side requests only for new instructions or evidence of invalidity; record why. Owners checkpoint meaningful changes and before planned pause/handoff.

## 3. Dispatch and slicing

Root assigns one approved milestone/outcome with requirements, dependencies, scope/exclusions, acceptance/stop conditions, spec/skill paths, and focused context. Detailed decomposition belongs to Lead; no routine Root approval.

Lead inspects actual code/dependencies; keep coupled transaction/contract changes together, split independent verifiable outcomes. A numbered milestone need not be one worker assignment. Do not split merely by file count/layer. Foundations must prove a necessary contract and name the consuming integration; no scaffolding-only slice, scope expansion, skipped gates, or next-milestone work.

**Focused handoffs (all roles):** Supply assigned outcome/requirement IDs with exact spec sections; owned components/files and shared resources; dependencies/exclusions; inputs/outputs/errors/effects and shared contracts; relevant evidence, missing scenarios, unresolved findings/repair guidance, next artifact, verification, and remaining integration. Link source documents with section/function/test locations; quote only essential constraints. Do not paste entire specs, project handbooks, conversations, or completed finding histories. Recipients read applicable instructions and referenced requirements; retrieve more source/context for concrete dependencies or uncertainties. Preserve required workflow rules below, acceptance criteria, attempt counts, and ownership/restoration obligations. No arbitrary context cap or separate handoff document. Corrections to the same agent carry changed scope/source, unresolved findings, new evidence, and plan delta; fresh replacements receive the complete relevant assignment state.

Assign the full cycle:

`plan → early proof if high-risk (§4) → implementation/docs → focused checks → candidate review → broad gates → handoff`

Outside proof/candidate checkpoints, workers act autonomously; no permission per edit/test/RED/run. Serialize conflicting fixtures, build outputs, databases, ports, and servers without repeated test-slot transfers.

Every brief states role, ownership, governing paths, shared workspace/preserve-others rule, and applicable workflow. Lead reads this skill. Workers receive plan/proof, repair-guidance use, review-before-broad-gates, attempt, pause, communication route below, and §6 execution ownership/stall rules (notice, autonomous handle resumes, five-minute check, one recovery) explicitly. Reviewer briefs include §5's repair guidance and finite-request/return lifecycle. All briefs include §6's concise-output practice: retain verbose logs, return result/evidence links and relevant failure excerpts, retrieve further output for a concrete gap. **Every Lead/worker/reviewer/escalation brief includes §6's waiting rule verbatim**; paths alone do not transmit rules to fresh contexts.

**Root–Lead reporting:** Lead sends Root accepted milestones/assignment completion, blockers or decisions needing Root, and a concise assessment when scheduled supervision needs one (§7). Report only the change: outcome, blocker/decision if any, next action, evidence link. Keep routine plans, proof exchanges, findings, retries, test results, and execution tracking within Lead/worker/reviewer and `status.md`; do not copy their discussions to Root. Root uses these reports for user communication/progress, without acknowledgment-only replies, re-summarizing technical feedback, or requesting a second report of the same state. Required user updates use known state, not extra team inspections.

**Communication route:** At first dispatch, establish the recipient's actually exposed native messaging capability once, using tool definitions and the first necessary plan/notice; no extra handshake. Separately exposed collaboration APIs need not appear in a nested tool registry; registry absence alone does not prove unavailability. Use returned agent IDs. Lead records the working route or finite-handoff fallback alongside ownership in status and includes it in follow-ups. Recheck only after an actual capability failure or changed runtime, not each turn/compaction. Do not repeatedly search for tools, read parent task history, or use separate user-owned task messaging to communicate. Missing messaging uses §6's fallback; missing required delegation/reactivation follows §1. Workers/reviewer may discuss agreed contracts directly where supported; Lead resolves decisions. Reuse workers during corrections. Consider fresh worker context at an accepted, materially different slice boundary when old context is dominated by completed work; transfer relevant contracts/evidence and preserve counts. No scheduled rotation.

## 4. Plan, prove, implement

**Before editing:** Luna inspects relevant source/tests/requirements and publishes a concrete plan to Lead for every assignment/correction. Read-only investigation/baselines may precede it; private reasoning is insufficient. Include approach, affected files/functions, reuse, change order, tests, uncertainties. Small changes need only a few bullets. Astra likewise publishes its revised approach. Store plan summary/link in status. For corrections, worker checks §5's repair guidance against source and publishes only the plan delta: adopted approach, evidence-backed deviations/uncertainties, changed scope/order/tests, and why the failed approach changes. Do not independently recreate the reviewer's diagnosis or full plan; investigate concrete gaps. Worker retains local implementation judgment. Reuse valid plans; no duplicated specs or per-fix plan files. Lead checks scope/dependencies/feasibility for every plan without duplicating technical review or requiring ordinary workers to wait for acknowledgment.

**Shared-contract test selection:** When changing a shared contract, include a short map in the existing worker plan: changed field/version/enum/key set or behavior → affected producers/consumers → existing compatibility/contract tests. Cover relevant API/UI, persistence, readiness/job inventories, localization/docs, and E2E boundaries, not every category by default. For global counts/readiness queries, identify fixture producers and their owned cleanup; run producer/consumer tests together where isolation or ordering matters. Locate actual tests rather than copying stale plan class names. Record gaps; add meaningful coverage only where missing. No extra planner, document, or permission gate.

**Mechanism:** For stateful/async/concurrent work, name operation identity, durable target association, surviving facts after restart/expiry/deletion, later lookup/authorization, atomic commits, rollback/pending/replay/late-result behavior. Identify actual fields/queries/boundaries or necessary additions; “handle recovery” is insufficient.

**High-risk checkpoint** applies to authorization, provider/payment effects, destructive changes, migrations, concurrency, recovery:

1. Reviewer traces decisive scenarios. Worker proposes 1–2 early proofs: real entry point, observable state/effects (persisted where relevant), decisive failure/replay/race condition, test. In the existing pre-code exchange, reviewer checks missing facts/lookups/authority/atomic boundaries and whether scenarios expose broken mechanisms. Consolidate feedback; resolve correctness blockers before production edits.
2. Worker implements the smallest complete path for those scenarios, then submits source marker, test location, command/result, and actual assertions through Lead to the same reviewer. Freeze that proof scope during review; **do not expand surrounding implementation yet**.
3. Reviewer inspects executable proof/evidence, not test names/counts. Cross the real application/persistence boundary relevant to risk. Private-validator reflection, method-existence/source-text checks, or mocks bypassing that boundary do not substitute. Reuse fixtures and valid evidence.
4. Lead records acceptance/missing scenario in status. Expand implementation only after reviewer acceptance; then continue autonomously. One bounded checkpoint per risky mechanism, not full candidate review, Root approval, extra agents/documents, or per-test permission. Reopen only for material mechanism change or invalid evidence, using the same reviewer.

Ordinary low-risk work proceeds after plan publication without acknowledgment or this checkpoint. For new behavior/bugs: meaningful focused RED → implementation → verification; existing tests may supply RED. Compile/setup failures alone are not behavioral proof. Test observable contracts, not invented signatures/structure/wording. Correct faulty tests against requirements with reasons; never weaken valid assertions or alter correct behavior for invented contracts. Preserve repo-required checks.

**Local stalls:** Worker surfaces repeated investigation without new evidence, ineffective fixes, or inability to deliver the next artifact. Lead watches planning/review loops too. Interrupt the unproductive pattern with one diagnostic handoff: failing outcome, tried/ruled-out approaches, obstacle, proposed change. Clarify mechanism, narrow scope, or consolidate ownership; status-only redispatch is not progress. Escalate beyond-mandate obstacles to Root. Unresolved execution/approval calls use §6's earlier capability diagnostic, not the scheduled progress-stall counter.

Elapsed time, expected RED, or long tests alone are not stalls. Environment-only failures do not consume attempts, but persistent environment blockers still require diagnosis/supervision. Proof feedback is not one attempt per test; explicit inability/submitted acceptance candidates follow §5. Never keep an exhausted candidate indefinitely “in progress” or “awaiting proof.” Root's scheduled stall counter is separate (§7).

## 5. Candidate review and escalation

Independent reviewer examines complete affected request/recovery paths for every substantive slice; judge risk by behavior, not diff size. Mechanical corrections stay in existing review. Each blocker: concrete trigger, violated requirement/risk, location, observable impact, closure evidence. No preference/speculative-redesign blockers. Consolidate feedback; flag urgent issues early only to prevent wasted/unsafe work.

**Actionable repair guidance:** With changes-required feedback, reviewer supplies evidence-supported diagnosis (confirmed cause vs hypothesis), smallest credible repair direction, and specific closure assertions. Name relevant functions/fields/boundaries where established. Combine related findings into one short ordered correction plan; mechanical fixes may need only one sentence. Unknown cause → state the uncertainty and next targeted diagnostic, not an invented fix or prolonged investigation. For missing evidence, specify the proof needed rather than assuming production changes. Guidance uses already inspected artifacts; no extra planner, mandatory upfront consultation, full replacement implementation, or per-fix document. Preserve §4's high-risk pre-code/proof checkpoints.

Reviewer remains read-only; accept requirement-correct alternatives, not just compliance with its suggested repair. Lead passes the consolidated guidance to the same worker, resolving scope/dependency conflicts without rewriting the technical plan. Store summary/link with findings; do not duplicate it across tracking files.

Lead tracks each finding in status: `ID | required outcome | focused test/evidence + source | open/ready/closed | attempt count`. Worker supplies evidence for the closure condition; a passing suite name is insufficient.

**Reviewer lifecycle (all review stages):**

- Lead dispatches only a concrete ready review: target (mechanism/proof/candidate/correction), source/evidence links, questions, findings to reassess. No "monitor until finished" assignments.
- Reviewer inspects available artifacts, returns a final answer, and ends its turn: `verdict (accepted/changes required/not assessable) | reviewed source/scope | findings + closure conditions | repair guidance when changes required | next needed artifact or none`. Missing evidence → name it and return; never wait for worker changes, future evidence, or broad gates. Missing required evidence on a submitted candidate still follows attempt accounting below.
- Lead stores the returned reviewer ID/path and current findings in status. Once required evidence is ready, call `collaboration.followup_task` on that SAME reviewer with source marker, changed scope, relevant evidence, and unresolved findings. This starts a new turn for a completed/idle agent. Use `collaboration.send_message` for corrections during an active review; it alone does not restart an idle agent. Do not queue speculative reviews, spawn a replacement merely because review completed, or reload unrelated history.
- Return preserves the reviewer for follow-up; it does not delete/close the agent or guarantee a freed capacity slot. If the required follow-up capability is unavailable, report through the existing fallback rule; do not invent a tool or switch to user-owned task messaging.
- Reactivate for ready plan/proof/candidate/correction or invalidated review evidence, not timers/progress chatter. During broad gates Lead checks results; reviewer remains idle unless a defect/source change needs reassessment. No extra review diary or final no-change review round.


- Initial/corrective candidate: focused + neighboring checks, including affected shared-contract/fixture checks from §4, **before the next broad cycle**. After corrections, renew the affected map/checks and reuse valid evidence; update outdated test expectations only against the governing contract, preserving required historical compatibility. Lead checks evidence completeness; reviewer decides substantive closure. Review clearance remains provisional until broad gates pass. Preserve explicitly required earlier repo checks; no broad reruns merely to request review.
- Submission includes scope, tested source marker, commands/results, concerns. Freeze reviewed scope until consolidated feedback or explicit withdrawal. Use a lightweight revision/manifest, not mandatory whole-repo hashing. Disclose later changes; renew affected evidence.
- Return blockers as one cohesive correction. Re-review changes/unresolved findings; expand only for concrete risks introduced by fixes. If broad gates reveal a defect, correct it and renew affected review/verification. Root does not repeat review.

**Two Luna attempts → Astra High:** An attempt is a submitted acceptance candidate or explicit inability to meet agreed behavior. Initial = 1, correction = 2. Validated unresolved requirements, including missing behavioral evidence at Lead's completeness check, make submission unsuccessful even before broad gates. Local iterations, expected RED, plan feedback, and environment-only failures do not each count. No uncounted “readiness checks,” double-counted rejection across gates, or withdrawal to erase known failure.

Track counts against the same unresolved behavior. After failure 1: one consolidated Luna correction using reviewer repair guidance and worker plan delta; guidance adds no attempt or retry-counter reset. After failure 2: stop Luna edits; automatically transfer remaining behavior/files to Astra High with patch/tests/evidence/counts and the same independent reviewer. No renewed permission. Lead diagnoses contract ambiguity, faulty tests, incomplete mechanism, or oversized scope; Astra uses that diagnosis to change approach. Managers/reviewer never become fix authors.

Renaming/splitting/resuming/replacing agents cannot reset unresolved counts; genuinely independent behavior starts its own count. If Astra fails, retain ownership and diagnose before correcting; no return to fresh Luna attempts or waived review/gates.

## 6. Verification and waiting

Focused checks during implementation/correction; substantive candidate clearance before broad gates (§5). Acceptance requires all repo/plan tests, compile/typecheck/build, integration/E2E, consistent docs, and required derived/graph updates. Record command/outcome/tested source. Later source/fixture/config/schema changes invalidate affected evidence only; reuse valid results. Update derived artifacts at prescribed stable boundaries, not per worker.

**Concise tool output:** Before verbose non-interactive runs, retain full output in a log and bound the tool response where supported; preserve the actual command exit status, including through logging/pipelines. Report `command + directory | exit status (or running/blocked/unknown) | concise result | tested source | log/report path`; include failures/errors/skips or coverage limitations when relevant. For failures, add the decisive error excerpt and affected test/operation. Read further log ranges or reports only for a specific diagnostic/review/evidence gap; do not repeatedly load full logs or successful-test listings. Scope searches/file reads to relevant sections, expanding for dependencies; small outputs need no extra log. If truncated output lacks reliable completion or result evidence, retrieve the missing evidence before claiming success. Keep logs available through review/handoff; summaries never replace executable proof or reviewer access to source/tests.

**Waiting rule — include verbatim in every dispatch; Root obeys it too:**

> Prefer event-aware agent waits: Root 300 seconds; Lead 180 seconds. These are
> targets, bounded by tool limits, higher-priority wait/update instructions, and
> time remaining to the next supervision/diagnostic deadline. Honor a 60-second
> runtime cap when present; never claim a skill overrides it. Workers resume
> command handles with substantial supported waits. Handle messages/completion
> promptly when the wait returns early. Do not substitute fixed sleeps, repeated
> log reads, or process-status checks. A timeout alone justifies no status request.

Waiting applies while work is active; a reviewer with no assessable request returns under §5 instead of entering a wait loop. Set supported wait arguments explicitly; cap each wait at the next actual deadline and assess it when due, without resetting clocks. If the tool's minimum wait exceeds the remaining interval, use the shortest supported wait and check promptly on return. Record a runtime-imposed shorter cadence once per assignment; disclose that longer-wait savings are inactive, without repeating that explanation each timeout. Other shorter waits need a concrete reason: interactive input, imminent cancellation, tool limit. “Checking if finished” is insufficient. Required user updates use concise known state; they do not trigger worker polling. Read output for real failures/stalls.

Lead checks initial implementer waits and again after Astra transfer. Unjustified short polling → one correction → verify next wait; continued violations use existing stall rules. Root checks team compliance during scheduled supervision. The execution-stall check below is a specific diagnostic trigger, not permission per command, an extra monitor, or routine status polling. Live waits provide no recurring automation/inactive-task wakeup guarantee.

**Execution-stall check — Lead owns it; independent of §7:**

- Before elevated execution or another known approval-sensitive operation, worker sends Lead `operation + command/directory | elevated access/reason | expected initial outcome: approval/result/handle | evidence/log location`. Ordinary commands need no notice. Use the established route. Without native messaging, return one necessary pre-execution notice; Lead records it and reactivates the same worker for the complete execution/result cycle. This is tracking, not new user approval; existing execution permissions still apply.
- **Worker owns execution through result:** After notice/necessary reactivation, execute, resume the same running handle, collect the result and checkpoint cleanup autonomously. A handle, ordinary timeout, or ongoing log output is not a final-answer/handoff boundary. Do not require Lead to reactivate each resume. Return for completed evidence, a real blocker/required decision, review checkpoint, or explicit diagnostic; honor pause/interrupt instructions. No duplicate execution or ownership transfer merely to wait.
- Lead records one compact current-operation entry in `status.md`: `operation | start/timezone | state | latest evidence | diagnostic deadline/recovery used`. States: `awaiting tool response` (no result/handle), `running` (handle or concrete process evidence), `blocked` (approval/permission/execution obstacle). Worker sends the initial outcome through native messaging without ending its turn. Without messaging, record the matching operation/start time and initial result/handle in the existing worker-owned evidence location named in the notice, then continue; do not edit Lead-owned status or create a per-command document. This record is inspectable evidence, not an automatic notification. Lead inspects it only for a relevant event or the diagnostic below. Silence or generic agent-running status proves no command execution. Known blockers return/report immediately.
- After **five minutes without an initial response or execution evidence**, Lead checks the named execution evidence and available agent/tool state once at the next supported wait boundary. Read an actual clock; preserve elapsed time across compaction/handoff. This diagnoses an unresolved invocation, not ordinary test duration. A running process with a handle follows the waiting rule. No faster polling, timer promises, or automatic termination of running tests.
- If state remains unclear, obtain one finite worker diagnostic: `exact operation/directory | last tool response | approval outcome or unknown | handle/process evidence | temporary mutation/restoration pending`. Safely interrupt the worker turn if needed, then reactivate for that diagnostic. Interruption/abort does not prove the underlying process stopped or never started. If interruption/diagnosis is unavailable, report that limitation promptly to Root.
- Lead permits **one evidence-based, authorized recovery** addressing an identified cause. Before retrying, resolve whether the prior operation remains active; preserve/restore temporary mutations safely and avoid duplicate execution. Do not bypass approval, infer approval from silence, or cycle through command variants. Unresolved capability with no justified recovery goes directly to Root. If the recovery again lacks an initial response/evidence after five minutes, or encounters the same capability blocker, stop further variants and notify Root immediately. Preserve this recovery count for the same blocker across command/path/agent changes.
- Stop dependent work; preserve source, handles, evidence and restoration obligations. Continue only independent authorized work. Capability failures consume no Luna attempt; do not wait for two §7 check-ins. Root reports the exact operation, elapsed wait, known/unknown execution state, recovery tried, and capability/action needed. Say approval was rejected only when a rejection was returned. Do not promise moving verification to Root solves access without evidence of a working route; existing role/authority rules still apply. This diagnostic does not reset Root's 60/30-minute supervision schedule.

At acceptance/escalation, record outcomes, attempts, readily available time/usage. Distinguish cached input, uncached input, output; no invented totals/billing or continuous usage polling.

## 7. Root supervision: +60m, then +30m

At dispatch, read actual clock. Record in progress: assignment/agent ID, start + timezone, next check = start+60m, last assessment, per-blocker identity/evidence/consecutive-stall count. Preserve across compaction and worker/slice/Lead replacement; no timer/count reset for the same assignment.

Wait within tool limits; handle user input/exceptions. A wait timeout without new information → wait again; no status reads/requests, log inspection, or worker checks merely because the wait returned. Only a meaningful report, user request, scheduled check, or concrete exception triggers Root assessment. Completion before deadline → handoff. Overdue resume → one real check promptly, never fabricated missed checks or two catch-up observations. After each actual check, next = check+30m. Steering/off-schedule messages do not change deadlines or scheduled counts; retain useful evidence. Immediate decisions/user-directed stops remain allowed.

At each check, use current status and received Lead reports to assess concrete implementation/verification progress, repeated failures/coordination loops, scope, and waiting compliance. If evidence is insufficient, ask Lead one targeted question or inspect the specific linked artifact; do not routinely read child histories, raw logs, source, or repeat Lead's diagnosis. Record only the assessment delta and next check; no duplicate technical review or lengthy repeated reports.

- Progress on a blocker → continue; clear its advancing/resolved stall count.
- Recoverable issue / first stalled check → steer once, continue. Poor progress alone at 60m must not stop Lead.
- Human decision needed → explain plainly; continue unaffected authorized work, not dependent work or guessed decisions.
- Same underlying blocker without meaningful progress at **two consecutive scheduled checks** → coordinated pause and human escalation, even before any candidate submission. E.g. stuck A at 60m: steer; still stuck A at 90m: pause. This is separate from Luna attempts.

Track blockers separately. Renaming, author replacement, promises, unrelated accomplishments, unfinished work, or long tests do not establish blocker progress. New evidence must materially advance its diagnosis/resolution; another improving blocker cannot hide it.

**Pause:** Root instructs Lead to stop new edits/attempts/verification, notify worker/reviewer, preserve patch/evidence/counts, checkpoint active processes. Existing operations may reach safe stop, not run indefinitely or be killed blindly. Worker/Lead retain monitoring and record results until exclusive ownership transfers to Root. Confirm child acknowledgment or safely interrupt an unresponsive child. No new assignment or automatic resume.

Tell the user: `Paused: plain issue; Progress: completed outcome; Blocker: same issue at both check times; Tried: steering/fixes/results; Recommendation: next action + decision needed.`

Root records pause in progress; Lead checkpoints status. Recover ownership from an unresponsive Lead before writing its file. Resume only on explicit user direction: preserve work/failures/history, record changed direction, reset active scheduled-stall counts to 0, next check +30m. Never reset Luna failures. If supervision cannot continue, disclose/checkpoint; do not invent unattended timer support.

## 8. Accept and stop

Acceptance = working behavior + resolved substantive findings + required gates + matching docs. Lead accepts slices sequentially and completes assignment-level integration/release gates. Test counts/file counts alone are insufficient; blocker handoff is never acceptance.

Lead's consolidated handoff: outcome/scope, tested source, independent disposition, verification/limitations, later dependencies, cleanup. Complete status; stop workers and own work. Root checks the handoff for scope, evidence completeness, independent acceptance, and stop boundary; open underlying artifacts only for a concrete inconsistency/gap. No repeated technical acceptance review or valid-gate reruns. Record acceptance in progress, clear active supervision, retain durable decisions.

Next authorized assignment: fresh Astra Medium Lead with no history fork and focused persistent context; do not reuse finished managers/workers for unrelated work. Fresh context does not delete stored history. Same interrupted assignment may resume with its manager or a replacement after ownership transfer, preserving failures/decisions/supervision except the explicit §7 resume reset. A supervised pause still requires user direction.

At the requested stopping boundary, report outcomes/evidence/limitations, leave completed checkpoints, and stop. No later steps or expanded scope without authorization.
