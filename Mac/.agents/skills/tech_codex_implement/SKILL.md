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

Deliver the approved behavior with proportionate planning, testing, and review.
Follow the role assigned in your brief; only the root selects and spawns an
assignment lead. An assignment lead executes one bounded assignment through
workers, not through further assignment leads.

## 1. Roles, scope, and authority

| Role | Responsibility | Agent/model and effort |
|---|---|---|
| Root | User scope, one assignment at a time, supervision, overall progress, stopping boundary | Current root model/effort |
| Assignment lead | Inspect dependencies, decompose, coordinate plans/implementation, validate findings, diagnose stalls, enforce escalation, obtain acceptance | `default`, `gpt-6-astra`, `medium` |
| Implementer | Concrete plan, tests, code, affected docs, verification | `luna_worker`, `gpt-5.6-luna`, fixed `max` |
| Escalated implementer | Resolve work after two unsuccessful Luna candidates | `default`, `gpt-6-astra`, `high` |
| Independent reviewer | High-risk mechanism/scenario review and substantive candidate review | `default`, `gpt-6-astra`, `medium` |

Both managerial roles may inspect source/evidence and edit prose/tracking, but
must not edit application code, tests, migrations, build/runtime configuration,
or implementation scripts, including small fixes and code conflict resolution.
This restriction applies to implementation, not an explicit request to edit this
skill. The reviewer remains read-only and distinct from both implementation authors.

The root spawns only one active assignment lead. While it runs, the root handles
user communication, supervision, and decisions outside the assignment's authority;
it starts no other assignment, implementation, or parallel investigation. It does
not repeat routine plan/code reviews, relay worker findings, or supervise test runs.
Supervisory investigation under section 7 is allowed. User scope changes and real
blockers may be handled immediately; they need not wait for a scheduled check.

Within the assignment, permit one active implementer and one independent reviewer.
Slices execute sequentially. This fits four active roles including the root.
Only the assignment lead delegates implementation/review; workers and reviewers
must not spawn additional agents or managers. A replacement author takes exclusive
ownership after the former author stops editing. Do not overlap Luna and Astra
High as writers or create extra worktrees/databases merely to fill agent slots.

The assignment lead resolves routine technical choices and validated corrections
within its mandate. Escalate material scope/requirement conflicts or unavailable
authority to the root, which resolves them within existing approval or asks the
user. Existing implementation approval remains valid. Do not commit, publish,
deploy, consume a usage reset, or change paid settings beyond existing authority.

## 2. Resume and tracking ownership

Resolve the spec directory and read applicable repository instructions, governing
spec/plan sections, and relevant build/test scripts. Ask only if the target or a
material requirement cannot be resolved. Preserve the plan's exact step names,
dependencies, existing edits, accepted decisions, and valid verification.

Use two complementary files in the spec directory:

- **Root owns `progress.md`:** overall milestone ledger, accepted outcomes,
  durable decisions, historical verification references, and one compact current
  supervision record (section 7). Do not copy the live slice brief/finding list or
  append worker chatter. Update overall acceptance from assignment handoffs.
- **Assignment lead owns `status.md` while active:** assignment ID/objective,
  slice sequence/current brief, relevant decisions, completed side requests that
  might recur, agent IDs/ownership, unresolved behavior and attempt counts,
  escalation state, blockers, next action, and evidence links. Replace stale
  entries rather than append a diary. Aim for about 60 lines; necessary active
  facts take priority. Link to detailed requirements and evidence.

No simultaneous writers to either file. The root may bootstrap/recover `status.md`
when no assignment lead is active, then transfers ownership explicitly. Existing
history need not be rewritten. Create missing tracking files for implementation,
not when merely discussing or editing this skill.

On resume/compaction, the root reads its supervision record and current status;
the assignment lead reads `status.md`. Reconcile with newer user instructions and
current source/evidence relevant to the active work. Tracking never overrides
the user or governing requirements. Follow historical links for a concrete gap,
not to reload the whole narrative or rerun accepted gates by default.

Never redo a completed side request or reopen a settled decision merely because
it appears in older history. Reopen only for new instructions or concrete evidence
of invalidity and record why. Each owner updates its checkpoint at meaningful
state changes and before planned pauses/handoffs, not only at compaction.

## 3. Dispatch and decompose one bounded assignment

The root selects one approved milestone or explicit outcome, not the whole
remaining specification by default. Its brief states the outcome/requirements,
accepted dependencies, bounded scope, exclusions, acceptance conditions, and stop
boundary. Send the assignment lead the spec/skill paths and focused context.
The root does not perform detailed decomposition or require routine approval of it.

The assignment lead inspects actual code and dependencies, then keeps the
assignment intact or divides it into coherent, verifiable slices in `status.md`.
A numbered milestone is not automatically one worker assignment. Split independent
outcomes; keep coupled transaction/contract changes together. Do not divide solely
by file count or architectural layer. An internal foundation must demonstrate a
necessary contract, not just scaffolding, and name its consuming integration.
Decomposition must not expand scope, skip release gates, or start the next milestone.

Keep one compact brief for each active slice:

- Observable outcome and applicable requirement IDs.
- Owned files/components, dependencies, exclusions, and shared resources.
- Inputs/outputs/errors, side effects, and relevant contracts.
- Existing evidence, missing scenarios, next concrete artifact, verification,
  and any remaining integration dependency.

Assign a complete cycle: plan, early behavioral proof where required (section 4),
implementation and affected documentation, focused verification, independent
candidate review, required broad verification, and consolidated handoff. Outside
the bounded proof/candidate checkpoints, workers act autonomously; do not require
manager permission for each test, RED result, code edit, or verification run.
Check shared fixtures/contracts, build output, databases, ports, and servers;
serialize conflicting operations without repeatedly transferring test slots.

Spawn assignment lead and reviewer with `agent_type: "default"`,
`model: "gpt-6-astra"`, `reasoning_effort: "medium"`, `fork_turns: "none"`.
Spawn Luna with `agent_type: "luna_worker"`, `fork_turns: "none"`; its model and
effort are fixed. Spawn the escalation author as `default`, `gpt-6-astra`, `high`,
with `fork_turns: "none"`. These assignments do not change the root's settings.
If roles, capacity, or delegation are unavailable, report the limitation and
obtain a fallback choice before changing this model/role structure. Managers do
not silently take over coding. Preserve user-provided model overrides.

Give each agent its role, owned scope, governing paths, and relevant workflow
rules explicitly. Assignment leads read this skill; worker briefs include planning,
the early-proof checkpoint, review-before-broad-verification order, attempt
accounting, and stop-on-pause rules. Include the waiting rule from section 6
verbatim in every assignment-lead, worker, reviewer, and escalation brief; a skill
path alone is insufficient. Do not assume no-history agents inherited instructions.
Use native agent communication for the team and valid returned agent identifiers; do not create separate user-owned tasks for
internal subtasks. Tell agents they share a workspace and must preserve others'
edits. The assignment lead resolves decisions; workers/reviewer may discuss an
agreed contract directly without the root relaying messages.

Reuse an implementer during active corrections. At an accepted slice boundary,
consider fresh worker context if the next slice is materially different and old
context is dominated by completed work. Transfer only relevant contracts/evidence
and preserve unresolved behavior counts; do not rotate on a fixed schedule.

## 4. Plan the mechanism and prove the critical path

### Plan before editing

For every Luna coding assignment, including corrections, inspect relevant source,
tests, and requirements, then publish a concrete plan to the assignment lead
before editing code/tests/migrations/configuration. Read-only investigation and
baseline verification may come first. Private reasoning alone is not the plan.
A few grounded bullets suffice for a small change. Cover approach and affected
files/classes/functions, reused helpers, change order, critical tests, and material
uncertainties. The escalation author likewise states its revised approach.

For stateful/asynchronous/concurrent work, explain the actual mechanism:
operation identity and durable target association; facts surviving restart,
expiry, or deletion; how later work finds and authorizes the operation; what
commits together; and rollback, pending, replay, and late-result behavior. Name
existing fields/queries/boundaries or the necessary new mechanism. "Handle
recovery" is insufficient. Do not require this analysis for unrelated simple edits.

The assignment lead checks scope, dependencies, and feasibility without duplicating
technical review. For high-risk work (authorization, provider/payment effects,
destructive changes, migrations, concurrency, recovery), the independent Astra
Medium reviewer traces a few decisive scenarios through the proposed mechanism.
The worker identifies one or two early-proof scenarios: real entry point, observable
state and side effects (persisted where relevant), decisive failure/replay/race
condition, and proposed test. The assignment lead includes them in this existing
review; the reviewer checks whether they would expose a broken mechanism.
Identify concrete missing facts/lookups/authority/atomic boundaries. Combine
feedback into one pre-code exchange; resolve correctness blockers before production
changes. No extra review team, document, or per-test approval stage. Ordinary work
proceeds after plan publication without waiting for an acknowledgment.

Store the current plan summary/reference in `status.md`. Before corrections,
update only affected parts and explain the failed approach. Reuse a valid plan
on resume; revise it when evidence changes the mechanism. A material high-risk
mechanism change returns to the same reviewer. Do not duplicate the spec, create
a plan file for every small fix, or reset attempts by replanning.

### Implement and verify autonomously

For new behavior/bug fixes, demonstrate a meaningful focused failure, implement,
and verify. Existing tests may supply the RED case. Compilation/setup failures
alone do not prove the intended behavior fails. Test observable contracts rather
than invented signatures, incidental structure, or redundant inventories.

For the high-risk mechanism identified above, implement only the smallest complete
path needed to execute its agreed early-proof scenarios. Before expanding surrounding
implementation, submit the focused proof through the assignment lead to the same
reviewer: tested source state, test location, command/result, and behavior actually
asserted. Keep the proof scope fixed during this check. The reviewer inspects the
small executable artifact and its evidence, not just its name or passing count.
It must cross the real application/persistence boundary relevant to the risk;
reflection into a private validator, method-existence/source-text checks, or mocks
bypassing that boundary cannot substitute. Reuse existing fixtures and valid proof.

The assignment lead records proof acceptance or the missing scenario in `status.md`.
Only after reviewer acceptance may the worker expand the implementation autonomously.
This is one bounded checkpoint per risky mechanism, not root approval, a full
candidate review, or permission for every test/edit. Resolve proof gaps within this
checkpoint rather than adding review rounds or documents. Reopen it only for a
material mechanism change or invalidated evidence; simple low-risk work skips it.
An initial proof gap follows the existing stall/attempt rules, not automatic failure
per test; an explicitly unable or submitted acceptance candidate still counts under
section 5. Do not label an exhausted attempt an unfinished proof indefinitely.

Correct faulty tests against governing requirements and include the reason in
review. Never weaken a valid assertion or change correct behavior to satisfy an
invented contract. Avoid tests merely for wording or low-impact formatting while
preserving repository-required coverage and checks.

### Detect stalls within the assignment

A worker surfaces repeated investigation without new evidence, ineffective fixes,
or inability to produce its next concrete artifact. The assignment lead also
watches for planning/delegation/review loops producing no concrete result. Pause
the unproductive pattern and give one diagnostic handoff: failing outcome, what
was tried/ruled out, obstacle, and proposed change. Consolidate ownership, clarify
the mechanism, or narrow the slice before continuing. Status-only redispatch is
not a changed approach. Escalate obstacles beyond the mandate to the root.

Elapsed time alone, expected TDD failures, and legitimately long tests are not
implementation stalls. Do not add per-test gates or perpetual progress meetings.
An explicitly unable candidate counts as unsuccessful under section 5; a diagnostic
pause or environment-only failure alone does not. Do not indefinitely label an
exhausted candidate "in progress." Root supervision is separate under section 7.

## 5. Stable independent review and implementation escalation

Require independent Astra Medium review for substantive slices; include mechanical
corrections in the existing review. Judge risk by behavior, not diff size. Inspect
complete affected request/recovery paths against the requirements. Each blocker
must state a concrete trigger, violated requirement/risk, code/test location,
observable impact, and evidence that would demonstrate resolution. Preferences,
unsupported assumptions, and speculative redesigns do not block. Consolidate
findings; report urgent blockers early only when they prevent wasted/unsafe work.

For each blocker, the reviewer defines an observable closure condition. The
assignment lead keeps a compact finding entry in `status.md`: ID, required outcome,
focused evidence/test and source reference, review state (open/ready/closed), and
attempt count. Link details instead of copying reports. The worker supplies evidence
against each condition; a passing suite name alone does not close a finding.

Submit initial and corrective candidates after focused/neighboring checks and
before the next broad verification cycle. The assignment lead checks evidence
completeness; the independent reviewer decides whether the scenarios and code
resolve the findings. Return blockers together through the existing correction
flow. Once substantive review clears the candidate, run required broad gates;
review clearance is provisional until those gates pass. If a broad gate reveals
a defect, correct it, re-review affected findings/paths, and renew invalidated
verification. Preserve any explicit repository-mandated earlier check, but do not
rerun broad suites merely to request review.

At submission, identify scope, tested source state, commands/results, and concerns.
The author stops editing the reviewed scope until consolidated feedback or an
explicit withdrawal. Use a lightweight revision marker/manifest appropriate to
the workspace, not mandatory whole-repository hashing for every handoff. Disclose
late changes and renew affected verification. The assignment lead validates and
assigns cohesive fixes; re-review changed paths/unresolved findings, expanding
only for concrete risks introduced by the fix. The root does not repeat review.

### Two Luna attempts, then Astra High

An attempt is an implementation candidate submitted for acceptance or explicitly
unable to meet the agreed behavior. Initial candidate is attempt 1; corrective
candidate is attempt 2. Local test iterations, expected RED, plan feedback, and
environment-only failures do not each consume an attempt. Count failure when
validated review/verification leaves a correctness requirement unresolved or the
worker cannot complete it. Consolidate one review's findings into one correction.
A candidate submitted with unresolved requirements is unsuccessful even before
broad gates run, including missing required behavioral evidence identified by the
lead's completeness check. Calling submission a "readiness check" must not create
uncounted attempts. Track the same candidate through focused review and broad gates;
do not count one rejection twice or withdraw it merely to erase a known failure.

The assignment lead records counts against the same unresolved behavior in
`status.md`, with owner and finding references. After failure 1, Luna gets one
cohesive correction. After failure 2, stop Luna's edits and transfer the remaining
behavior/files to Astra High automatically, preserving patch, tests, evidence,
and the independent Astra Medium reviewer. No renewed user permission is needed.
The reviewer must not become the correction author and still accept its own work.

Renaming, splitting, resuming, or changing workers/assignment leads must not reset
counts for unresolved behavior. Genuinely independent new behavior starts its
own count. Before escalation, the assignment lead diagnoses the full failing path:
contract ambiguity, faulty test, incomplete mechanism, or oversized scope. Astra
uses that diagnosis and decisive scenarios to change the approach. Neither manager
makes the fix itself. If Astra still fails, retain its ownership and diagnose
before further correction; do not cycle back to fresh Luna attempts. Independent
review and required verification are never waived by escalation.

## 6. Verification and waiting discipline

During implementation/correction, run focused and neighboring checks; clear the
candidate's substantive review before broad verification as specified in section 5.
At acceptance, satisfy repository/plan-required tests, compilation/type checks,
builds, integration/E2E,
docs, and derived-artifact updates. Record command, outcome, and tested code state.
Relevant subsequent source/fixture/config/schema edits invalidate corresponding
evidence. Reuse valid results; do not repeat broad suites merely for handoffs.
Synchronize required graph/derived artifacts at the prescribed stable boundary,
not repeatedly per worker. Keep verbose output in logs with concise evidence links.

Every dispatch must include this waiting rule, which also binds the root:

> For non-interactive work, use completion notifications or waits of 30–60 seconds
> where supported. When a command returns a running handle, resume that handle
> with a substantial wait. Do not replace waiting with repeated log reads or
> process-status checks. A timeout alone does not justify messaging another agent.

Shorter waits require a concrete reason, such as interactive input, an imminent
cancellation deadline, or a tool-enforced limit. State that reason once for the
operation; "checking whether it finished" is insufficient. Use supported wait
arguments explicitly rather than relying on a short default. Inspect output for
a concrete failure/stall. Required user updates must not make workers poll faster.

The assignment lead checks the implementer's initial waiting pattern and checks
again after transfer to Astra High. If short polling appears without a valid reason,
send one correction and verify the next wait complies; diagnose continued violations
under the existing coordination/stall rules. No per-command approval or extra monitor
agent. The root follows the same rule and checks team compliance during scheduled
supervision. These checks must not themselves become frequent polling.

The root remains available without starting unrelated work. Live waits do not
create recurring automations or guarantee wakeups while the task is inactive.

At acceptance/escalation, record outcomes, attempt counts, and readily available
usage/time concisely. Distinguish cached input, uncached input, and output; do not
invent usage or billing estimates or poll usage continuously.

## 7. Root supervision: 60 minutes, then every 30 minutes

At dispatch, the root reads the actual clock and records in a small supervision
block in `progress.md`: assignment/agent ID, start time, next check (start +60m),
last check/assessment, and a compact per-blocker record of identity, progress
evidence, and consecutive-stall count. Use timestamps with timezone. Preserve this record across
continuation/compaction; changing a worker, slice, or assignment-lead context for
the same assignment does not restart its timer or erase a stall.

While the assignment lead runs, the root waits within runtime limits, handles
user messages and exceptions, and checks the clock when due. It need not inspect
agents or send status requests at every short tool-wait return. If the assignment
finishes before a scheduled check, handle the handoff instead. If the root resumes
overdue, perform one real check promptly; do not fabricate missed assessments or
count two catch-up checks as consecutive observations. After each actual check,
schedule the next for +30m. Steering does not reset that schedule. Off-schedule
user updates or blocker discussions do not advance/reset scheduled observation
counts or change deadlines; retain their evidence for the next scheduled assessment.
They may still resolve a material decision or satisfy a user-directed stop.

At each check, read `status.md` and recent evidence to assess concrete progress,
advancing implementation/verification, repeated failures or coordination loops,
scope compliance, and compliance with the waiting rule. Inspect only what resolves
uncertainty; do not duplicate technical review or demand lengthy reports already
covered by evidence.

- Meaningful progress: let work continue and clear any resolved/advancing stall.
- Recoverable issue: give one focused steering message; let work continue.
- Human decision needed: notify the user in plain language and continue unaffected
  authorized work where possible. Do not guess the decision or do dependent work.
- Same blocker with no meaningful progress at two consecutive scheduled checks:
  pause the assignment and escalate to the user, even if no implementation
  candidate has been submitted. This check count is separate from Luna attempts.

At the first scheduled check, poor progress alone must not stop the assignment
lead. For example, stuck on A at 60m means steer/continue; still stuck on A without
meaningful progress at 90m means pause/escalate. Unfinished work or a long test
alone is not a stall. Track the underlying unresolved behavior, not its label:
renaming it, switching authors, or promising a fix is not progress. New evidence
counts only if it materially advances diagnosis/resolution of that blocker;
unrelated accomplishments cannot clear it. Multiple blockers retain their own
consecutive observations so a persistent one is not hidden by a new one.

For a two-check stall, request a coordinated pause through the assignment lead:
stop starting edits, attempts, and verification; notify the implementer/reviewer;
preserve patch/evidence/counts and checkpoint active processes. Allow running
operations to reach a safe stopping point where needed; do not kill them blindly
or treat this as permission for indefinite new work. The owning worker and
assignment lead monitor existing operations through safe stop and record results;
the root recovers that responsibility only after exclusive ownership transfer.
Confirm child acknowledgment
or interrupt an unresponsive child when safe. Do not start another assignment or
resume automatically; wait for the user's direction.

Send a concise, self-contained escalation:

> Paused: [plain-language issue].
> Progress: [completed outcome].
> Blocker: [same obstacle observed at both check times].
> Tried: [steering/fixes and their result].
> Recommendation: [next action and specific decision needed].

Record pause state in the root supervision block; assignment lead checkpoints
`status.md`. If it is unresponsive, the root recovers the file only after exclusive
ownership is established. On explicit user-authorized resume, preserve all work
and implementation failures, record the changed direction and a fresh supervision
baseline with active consecutive-stall counts at zero, and schedule the next
check in 30m. Retain prior assessments as history; this reset never clears Luna
failures or occurs without user direction. Do not invent runtime support for
unattended timers; if supervision cannot continue, disclose it and checkpoint.

## 8. Accept, hand off, and end the assignment

A slice is accepted when behavior works, substantive findings are resolved,
required verification passes, and docs match code. Assignment lead advances
sequential slices and completes assignment-level integration/release gates before
claiming the assignment accepted. Test counts/files changed alone are not completion.

The assignment lead returns one accepted handoff: outcome and bounded scope,
tested source state, independent review disposition, verification/limitations,
relevant later dependencies, and resource cleanup. It updates `status.md` to a
completed checkpoint and stops its workers and its own work. The root checks
scope, required evidence, independent acceptance, and the user stop boundary;
it does not re-review patches or rerun valid gates. It records acceptance in
`progress.md` and clears the active supervision record while retaining any
material decisions. A blocker handoff must never be recorded as acceptance.

For another authorized assignment, create a fresh Astra Medium assignment lead
with `fork_turns: "none"` and focused persistent context. Do not reuse the finished
manager or its workers for unrelated assignments. This is a fresh working context,
not deletion of stored history. The same interrupted assignment may resume with
the same manager or a replacement after ownership transfer, preserving failures,
decisions, and supervision history/deadlines, except for the explicit authorized
resume baseline in section 7. A root-supervised pause requires user direction.

When the user's stopping boundary is reached, report accepted outcomes, evidence,
and limitations, leave a completed checkpoint, and stop. Do not begin later steps
or expand scope without authorization.
