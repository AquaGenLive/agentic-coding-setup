---
name: tech_codex_implement
description: >
  Implement or resume an approved specification and implementation plan using
  behavior-focused TDD, small execution slices, selective delegation, and
  independent review. Use when the user wants to code an existing spec with
  the Codex implementation workflow. Not for creating specifications or for
  analyzing a workflow without implementing it.
---

# Codex Specification Implementation

Deliver the approved behavior with proportionate testing and review. The lead
owns the result, validates findings, diagnoses blockers, and maintains tracking
and documentation. The lead may inspect source and verification evidence but
must not edit application code, tests, migrations, build/runtime configuration,
or implementation scripts, including small fixes and code conflict resolution.
Delegate those changes through section 3. This restriction applies while
executing this implementation workflow, not to an explicit request to edit the
skill itself. Reuse agents for related activities.

## 1. Establish scope and resume state

Accept the spec directory/name or the unambiguous spec identified in the
conversation. Locate its spec, plan, and tracking files; ask only if the target
cannot be resolved. Preserve the plan's exact step names and dependencies.

Use two complementary files in the spec directory, maintained by the lead:

- **`status.md`: current resume checkpoint.** Keep the active objective, current
  slice/brief, accepted decisions relevant to it, completed side requests that
  might otherwise recur, owners/agent IDs, Luna attempt count and escalation
  state, unresolved findings/blockers, next action, and links to applicable
  verification. Replace stale entries rather than appending a running diary.
  Aim for roughly one screenful (about 60 lines); necessary active facts take
  priority over a rigid limit. Link to detailed requirements and evidence.
- **`progress.md`: overall milestone ledger.** Keep plan-step/slice completion,
  accepted outcomes, durable decisions, and concise historical verification
  references. It owns overall completion status; `status.md` owns the live
  handoff. Do not duplicate the active finding list, briefs, or worker chatter
  here. Store verbose outputs in logs and link to them.

For an existing task without `status.md`, derive it once from the latest user
instructions, relevant progress entries, working tree, and verification evidence.
Preserve existing history; no wholesale tracking rewrite is needed. Create
missing tracking files when beginning implementation, not when merely discussing
or editing this skill.

On initial setup, read governing repository instructions, the applicable spec/
plan sections, and relevant build/test scripts. On resume or after compaction,
read `status.md` first, reconcile it with newer user instructions and current
source/evidence for the active slice, and follow links only as needed. Neither
tracking file overrides the user or governing requirements. Inspect history to
resolve a concrete gap; do not reload the whole execution narrative by default.

Never redo a completed side request or settled decision merely because it
appears in older conversation history. Reopen only for a new user instruction
or concrete evidence that the result is invalid; record the reason. Preserve
existing changes and valid verification rather than restarting completed work.
Update the checkpoint when the active objective, ownership, findings, attempts,
or next action changes and before a planned handoff/pause; do not wait for a
compaction warning. Align both files at milestone transitions.

Summarize the next deliverable and material uncertainty briefly. Existing
implementation approval remains valid. Resolve routine implementation choices
from approved intent and code; ask only for missing product decisions,
conflicting requirements, or actions needing new authority.

## 2. Define a small execution slice

Treat large plan steps as milestones, not necessarily single work assignments.
Subdivide them into coherent, testable slices in progress tracking without
changing the approved requirements or release gates. A foundation slice may be
internal, but its contract and verification must be concrete.

Before coding each slice, keep one compact brief in `status.md`:

- **Outcome:** observable behavior and applicable requirement IDs.
- **Scope:** owned files/components, dependencies, and excluded work.
- **Contract:** inputs, outputs, errors, side effects, and any shared seam.
- **Evidence:** relevant existing tests, missing scenarios, and verification.

For high-risk work (authorization, provider/payment effects, destructive data
changes, migrations, concurrency, or crash recovery), the implementer and
independent reviewer agree on a few decisive scenarios before production work.
Each gives a trigger, expected outcome, and observable evidence across the
changed request/recovery path. Cover relevant identity, retry, stale-result,
and durable-state transitions; do not build an exhaustive matrix or invent
internal APIs. Record the agreed scenarios in the same brief. This is one
upfront contract check, not a separate test-author team or per-test gate.

Settle shared interfaces before parallel writers depend on them. Include known
API consumers, tests, fixtures, and documentation in the impact check. Keep
normative behavior in the spec and reference it rather than copying passages.
If the same defect recurs, inspect the full affected path and revise the
scenario or diagnosis immediately; do not repeat the same patch strategy.

## 3. Ownership and delegation

Assign one implementer the complete slice: focused regression, implementation,
relevant verification, affected documentation, and a consolidated handoff.
Within the agreed scope, the worker owns that cycle without separate lead
permission for each failing test, production edit, or test run. Complete the
upfront scenario check for high-risk work; thereafter interrupt only for a
material ambiguity, ownership/resource conflict, a concrete stall under section 4,
or action needing authority.

Default to one implementer for tightly coupled backend work plus one independent
reviewer when needed. Parallelize only when both file ownership and execution
resources are independent: check shared fixtures/contracts, build output,
databases, ports, and servers. If assignments would require repeated test-slot
handoffs, serialize the complete work cycles instead. Do not create extra
worktrees/databases merely to fill agent slots; use isolation only when its
benefit justifies setup. Read-only review may overlap genuinely independent work.

Use these assignments unless the user explicitly overrides them:

| Task type | Agent/model | Reasoning |
|---|---|---|
| Initial implementation and one corrective attempt | `luna_worker` / `gpt-5.6-luna` | `max` (fixed by role) |
| Escalated implementation after two unsuccessful Luna attempts | `default` / `gpt-6-astra` | `high` |
| Narrow investigation or routine checks | `luna_worker` / `gpt-5.6-luna` | `max` (fixed by role) |
| Independent code or pre-code scenario review | `default` / `gpt-6-astra` | `medium` |
| Lead diagnosis, coordination, documentation, and tracking; no code edits | Current lead model | Current configured effort |

Spawn Luna with `agent_type: "luna_worker"` and `fork_turns: "none"`; its model
and effort are fixed. Spawn the escalated implementer with `agent_type:
"default"`, `model: "gpt-6-astra"`, `reasoning_effort: "high"`, and `fork_turns:
"none"`. Use the same explicit settings with `reasoning_effort: "medium"` for
review. Keep the reviewer distinct from both authors; a reviewer must not become
the escalation author while remaining responsible for independent acceptance.
These assignments do not change the lead's model or effort.

Escalation under section 5 is already authorized; do not ask again. If a required
role/model or delegation is unavailable, report the limitation and obtain a
fallback choice before substitution. Continue unaffected read-only/coordination
work; the lead must not silently take over coding.

Give workers scoped briefs, relevant paths/requirements, commands, decisions,
and ownership boundaries. Default to no history fork; include the smallest
history window only when specific conversational evidence is necessary. Tell
workers they share the workspace, must preserve others' edits, and must not
expand scope or delegate further without coordination. Related seam discussions
can happen directly; the lead resolves decisions rather than forwarding every
message. Transfer ownership explicitly before another implementer edits it.

Reuse the same implementer for corrections to its active slice. At an accepted
slice boundary, consider a fresh worker if the next assignment is materially
different and accumulated context is dominated by completed work. Transfer only
the current brief, relevant contracts/source paths, decisions, and evidence
references; use `fork_turns: "none"`. Record the new owner in `status.md`, preserving
any unresolved findings and attempt/escalation history. A context refresh does
not authorize a model change or reset the count for unresolved work. Do not
rotate agents on a fixed schedule or during an active correction merely to
shrink context; avoid repeating discovery or accepted verification.

The worker reports changed behavior/files, verification, and unresolved concerns
in one handoff. Do not create agents just to update tracking or relay commands.
The lead can inspect, diagnose, validate findings, and edit prose documentation;
all implementation and test corrections remain with the assigned implementer.

## 4. Implement with meaningful TDD

The implementer normally owns both tests and production code for the slice.
For new behavior and bug fixes, add or adapt a focused test first, demonstrate
the relevant failure, implement, then verify. Existing tests may already provide
the needed red case. A compile/import error alone does not prove the intended
behavior fails.

Use section 2's upfront scenario check for high-risk work. The implementer then
runs the test/code/verification cycle autonomously; do not require root approval
of each RED result. Include the meaningful before/after evidence in the completed
handoff. Review ordinary tests with the implemented slice.

Test observable contracts and meaningful invariants. Avoid invented method
signatures, incidental component structure, redundant coverage, or exact file/
key inventories unless those details are explicit contracts. Prefer existing
fixtures and test homes where appropriate. Do not add tests merely for wording,
formatting, or other reversible low-impact changes; use suitable static or
render checks instead. Preserve repository-mandated coverage and checks.

When a test is wrong, validate it against the governing requirement and have
the current implementer correct it. Record the reason and include the change in
review. Do not weaken an assertion to hide a real defect, and do not change
correct product behavior just to satisfy an over-specified test.

### Intervene when a candidate stalls

The worker identifies the next concrete artifact within its existing brief:
a reproduced failure, implemented behavior, or verified candidate. Intervene
when it repeatedly revisits the same uncertainty without new evidence, repeats
an ineffective fix, or cannot produce that artifact. The worker should surface
this itself; the lead may request the same diagnosis when the pattern is visible.
Elapsed time alone, expected TDD failures, or waiting for a legitimately long
command do not establish an implementation stall.

Pause the unproductive loop and return one concise diagnostic handoff: the
behavior still failing, what was tried and ruled out, the specific obstacle,
and the proposed change of approach. The lead and worker use it to resolve the
obstacle or revise the approach before continuing; a status-only redispatch is
insufficient. Record only the resulting diagnosis/next action in `status.md`.
Do not add recurring progress meetings, per-test gates, or arbitrary time/token
limits. Useful new evidence and meaningful implementation progress justify
continuing the autonomous cycle.

A candidate explicitly unable to meet its contract counts as an unsuccessful
attempt under section 5, even if no reviewable patch was submitted. A diagnostic
pause alone does not consume an attempt, and environment-only failures retain
their existing exception. Do not keep an exhausted candidate indefinitely open
to avoid counting failure. After the first unsuccessful Luna attempt, Luna still
gets its corrective attempt; escalate only after the second under the existing
policy. The lead must not take over code changes.

## 5. Review findings, not preferences

Require independent Astra Medium review for substantive implemented slices;
include small mechanical corrections in the existing slice review. Judge risk
by behavior, not diff size. Ask the reviewer to inspect implementation and tests
against the brief and governing requirements. Passing tests alone are not proof of a correct slice.
Review complete user/request/recovery paths where changes cross boundaries.

Each blocking finding must give:

- a concrete trigger or scenario;
- a violated requirement or demonstrable correctness risk;
- the relevant code/test location and observable impact;
- the missing evidence or regression that would demonstrate resolution.

Separate blocking findings from optional suggestions, unsupported assumptions,
and out-of-scope observations. Naming preferences, speculative edge cases, and
alternative architectures do not block without a contractual or practical
impact. Return one consolidated review; report an urgent finding early only
when it prevents wasted work or unsafe changes.

The lead validates findings, records their disposition, and assigns cohesive
fixes. Re-review the changed paths and unresolved findings; expand the review
only when the fix creates a concrete new risk. Preserve a compact finding list
so resolved decisions do not need to be rediscovered.

### Two Luna attempts, then Astra High

An attempt is one end-to-end implementation candidate submitted for acceptance,
or explicitly reported unable to meet the agreed behavior. The initial candidate
is attempt 1; a candidate correcting its validated blockers is attempt 2. Routine
local test iterations within a cycle, expected TDD failures, and environment-only
failures do not each consume an attempt. Do not claim a third candidate is still
part of an earlier attempt. Consolidate findings from the same review into one
correction rather than counting each finding separately.

Count an attempt unsuccessful when lead-validated review or verification exposes
an unresolved correctness requirement, or the worker cannot complete it. Record
the outcome/count in `status.md`. After attempt 1 fails, send Luna one cohesive correction
brief. After attempt 2 fails, stop Luna's edits and transfer the remaining issue
and affected files to a GPT-6 Astra High implementer without another permission
request. Preserve the patch, regressions, useful evidence, and independent Astra
Medium reviewer. Renaming the slice, resuming, compaction, or replacing a Luna
worker must not reset the count for the same unresolved work. A genuinely new
accepted-scope slice starts its own count.

Before the escalated patch, the lead inspects the complete failing path and
records the cause supported by evidence (or the specific uncertainty): contract
ambiguity, faulty test, incomplete fix, or oversized scope. The Astra implementer
uses this diagnosis and the consolidated scenarios to change the approach,
clarify the contract within existing authority, or subdivide the remaining work.
A third identical dispatch is not a diagnostic checkpoint. The lead remains
read-only for code, including when it already knows the likely fix.

Escalation does not waive findings or verification. If Astra's attempt is still
unsuccessful, retain its ownership and diagnose the remaining blocker before
further work; do not cycle back to fresh Luna attempts or blindly repeat reviews.
Ask the user only for a material unresolved decision or unavailable authority.

## 6. Verification and cost control

- During iteration, run the tests that exercise the changed behavior and nearby
  contracts. At the slice gate, run repository/plan-required tests, compile/type
  checks, builds, and relevant integration/E2E checks. Respect stricter project
  requirements; optimization must not silently skip required verification.
- Record the command, result, and code state tested (commit plus working-tree
  state, or a clear local revision marker). A later relevant edit invalidates
  that evidence, including changes to shared fixtures, configuration, or schema.
  Reuse valid results; do not repeat broad suites simply because
  an agent finished or another reviewer replied.
- Serialize commands sharing build output, databases, ports, or other mutable
  test resources. Independent checks may run in parallel. Keep verbose output
  in logs and report summaries plus actionable failure excerpts.
- Prefer completion notifications and the longest suitable event-driven wait
  allowed by runtime/communication limits. Do useful independent work while a
  worker runs. A timeout alone needs no worker message or renewed dispatch.
  Contact a worker to resolve a decision, change scope/dependencies, or diagnose
  a concrete stall; do not request status already available from tools.
  Keep required user updates concise, without initiating worker exchanges just
  to produce them. Do not repeatedly poll unchanged status.
- Apply waiting rules to every agent, including implementers and reviewers,
  and to long-running commands as well as agent coordination. For unattended
  tests/builds, use a substantial initial command wait and subsequent process
  waits, normally 30–60 seconds within tool/runtime/communication limits. Reserve
  short polling for interactive input or a concrete expectation of an immediate
  result. When an outer tool yields, resume its wait handle rather than launch
  another command to check the same process. Do not replace waiting with repeated
  log reads or process-status checks. Inspect output to diagnose a specific
  failure/stall; otherwise report completion and actionable results together.
  Required user updates must not cause workers to poll more frequently. Pass
  these rules in worker briefs instead of assuming workers inherit this skill.
- At slice completion and the escalation checkpoint, record completed behavior,
  unresolved findings, attempt outcomes, and usage/time when available. Keep
  this concise. Distinguish cached input, uncached input, and output; do not
  invent token totals or convert them to cost without evidence.
- If handoffs or repeated reviews are producing no completed behavior, reduce
  the team/context or consolidate the task before continuing. Respect explicit
  user budgets; never consume a reset or change paid settings without authority.

## 7. Complete, record, and continue

A slice is complete when its agreed behavior is implemented, substantive review
findings are resolved, required verification passes, and affected documentation
matches the code. Preserve single-writer, migration, and deployment gates from
the plan even when intermediate slices are green.

Update `progress.md` with exact milestone/slice completion, durable decisions,
and concise verification evidence. Refresh `status.md` with only the current
handoff and next action, linking to that evidence; remove resolved active entries.
At final completion, leave a short completed checkpoint with any limitations.
Do not equate
test counts or files changed with product completion. Run repository-required
derived-artifact updates at their prescribed boundary rather than repeatedly
per worker. Do not commit, publish, or deploy beyond existing authorization.

Continue through the remaining approved work. When blocked, distinguish an
environment/tool limitation from a code failure, preserve a useful checkpoint,
and report the smallest missing input or authority. At final delivery, state
the implemented outcomes, verification, and any remaining limitations without
claiming unfinished milestones are complete.
