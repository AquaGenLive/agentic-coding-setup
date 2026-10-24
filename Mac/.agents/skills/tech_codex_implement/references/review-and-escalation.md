# Review and escalation

Read before the first technical review, a high-risk mechanism checkpoint, a corrective candidate/model transfer or disposition of an integration defect. Apply with the core workflow; keep one independent ordinary reviewer where supported.

## Consolidated independent review

Review complete affected request, state, recovery and authorization paths for the coherent candidate, using the actual source and executable assertions. Mechanical integrations can be reviewed together. Findings need a concrete trigger, violated requirement, location, impact and closure evidence; preferences and speculative redesign are not blockers.

Return one finite disposition:

`accepted / changes required / not assessable | source and reviewed scope | findings and closure conditions | repair guidance or next needed evidence`

On changes required, distinguish confirmed cause from hypothesis and propose the smallest credible repair direction with specific assertions. An unknown cause calls for one bounded discriminating check. Reviewer remains read-only and ends the turn instead of waiting for future artifacts. Lead consolidates feedback and routes it to the current author; the author publishes only the correction delta and continues ordinary in-scope work without acknowledgment.

Reactivate the same reviewer for a ready correction, new integrated interaction, materially changed mechanism or substantive failure. Reassess changes and unresolved findings, expanding only for a concrete introduced risk. Missing evidence means state what is missing and return; no timer-driven review, speculative queue, replacement merely because a turn ended or automatic unchanged final-package review.

Review and full gates can run concurrently on one frozen candidate as the core permits. An accepted review does not waive failed or missing gates. If another author changes reviewed/gated inputs, disclose it and renew affected evidence before acceptance. Do not infer equivalence from passing test names/counts.

## High-risk mechanism checkpoint

Use for new or materially changed financial effects, authorization, destructive operations, migrations, concurrency or recovery mechanisms. Routine use of an already accepted mechanism does not automatically qualify.

Author identifies operation/target identity, ownership and later lookups, facts surviving restart/expiry/deletion, atomic commit/rollback, pending/replay/late-result behavior, and one or two decisive scenarios. Trace actual fields, queries and entry points. Resolve material authority or transaction uncertainty before risky production edits; obtain a finite pre-code reviewer assessment if that uncertainty cannot be settled from established contracts.

Implement the smallest complete path needed for the decisive proof and submit its source, actual assertions and command/results. Normally consolidate mechanism assessment and executable proof in one early review rather than requiring a separate plan/proof ceremony for every consumer. When a pre-code assessment was necessary, the proof review checks its closure conditions. Do not broaden the dependent mechanism until proof is independently cleared; unrelated ready work may continue.

Cross the real application/persistence boundary relevant to the risk. Include the smallest existing compatibility slice for the changed contract: affected producers/consumers, architecture constraints or legacy persisted behavior as applicable. A migration's decisive proof must execute through the actual application/database transport. Select checks from the change's risks; do not add a universal checklist or a second review ceremony. Mocked interfaces, helper-only reflection or source-text checks cannot establish durable commit, ownership or rollback.

Reuse fixtures and valid accepted evidence; disclose synthetic HTTP, provider, restart or assistive-technology limitations. Clear the checkpoint only with executable boundary and compatibility results. Reopen it for material changes, invalidated evidence or demonstrated unmet requirements. Normal consolidated review still covers the complete implementation, and required full suites remain binding.

## Causal behavior accounting

A batch is an assignment; a behavior is a stable acceptance target; a candidate is a submitted attempt against that target. Keep one compact record in the existing status/outcome:

`behavior + target | author/model/stage | last candidate/outcome | unsuccessful Luna/Sol/Astra counts + guided outcome | open findings | accepted source/evidence`

One batch can contain several behavior records. A finding belongs to its causal behavior, not a new retry budget. Several findings in one candidate for one behavior count at most once. A shared-mechanism defect discovered in several consumers does not multiply the count merely because several consumers expose it. Rename, split, replacement, compaction or resume never resets unresolved counts or model stage.

A submitted acceptance candidate, proof repair submitted as satisfying an agreed closure target, or explicit inability becomes unsuccessful when a validated requirement remains unmet. Missing required behavioral evidence at Lead completeness review can make it unsuccessful before broad gates. Count each candidate once, including later invalidation of provisional acceptance. Local test iterations, expected RED, plan feedback, exploratory diagnosis and environment-only failures are not separate attempts. Withdrawal cannot erase a known failed submission. Do not leave an exhausted repair indefinitely awaiting proof to avoid its count.

Evidence stale solely because of a later authorized change is not by itself a failed candidate; renew it. For a demonstrated integration defect:

- Original requirement never met: reopen that behavior at its existing counts/stage.
- Later change broke previously valid behavior: charge the causal later change's existing behavior record.
- Attribution uncertain: obtain one bounded source/evidence diagnosis before assigning the repair; do not reset or guess.

The discovering author does not inherit repair ownership automatically. Preserve scope and unaffected evidence. A replacement needs exclusive ownership and the required model/stage, not overlapping writers or a fresh budget. A model escalation belongs to the unresolved target, not every later edit in the same file.

## Bounded model transfers

### Luna: initial candidate plus one correction

Allow **two unsuccessful Luna candidates** for the same unresolved target: initial submission and one corrective submission. After the first failure, use consolidated reviewer guidance with the same Luna author when the task is still mechanical. After the second, safely transfer to `sol_worker` (`gpt-6.1-sol` / `medium`) with source, evidence, counts and open findings. Transfer earlier when scope needs broader judgment or difficult diagnosis; exhausting cheap retries is not a goal.

When resuming historical work, preserve all recorded failures. A Luna target already at or above two failures transfers at the next safe handoff; current commands and cleanup still need an owner. Sol-first targets start directly at Sol with no preliminary Luna budget. An escalated unresolved behavior never returns to Luna.

### Sol threshold and earlier expert diagnosis

After **two unsuccessful Sol candidates** against the same target, freeze further Sol correction attempts at a safe handoff and obtain expert diagnosis. This also covers proof/integration failures and work transferred from Luna; improved phases or different symptoms do not reset the target. Diagnose earlier when concrete source/proof evidence shows a substantial reasoning or implementation gap. A risk label alone does not require Astra; environment/access failures do not justify model escalation.

Use a fresh `default` / `gpt-6-astra` / `high` in the independent reviewer role. The ordinary reviewer ends its active turn and remains available; do not create parallel reviewers for the same target. Supply relevant contracts, source/delta, original failures, attempted repairs, counts and accepted reusable evidence.

Expert diagnosis is finite and read-only: confirm causes versus hypotheses, assess the complete remaining path and feasibility, then return an ordered minimal repair plan with preserved invariants and closure assertions. If cause is unknown, prescribe one discriminating check and decision rule rather than an open-ended investigation. Unresolved material feasibility/scope/resource choices go to the user; independent work can continue.

### Diagnosis selects the repair owner

After diagnosis, Lead records the repair choice and its evidence in the existing behavior record:

- **One guided Sol candidate:** cause and correction are understood, and the repair fits the current author's demonstrated capability. Keep the same Sol author; Astra remains independent reviewer. Preserve the guided outcome for later same-target findings.
- **Direct expert implementation:** diagnosis shows a substantial reasoning/implementation gap, or Sol cannot carry out the repair. Transfer directly without a mandatory guided attempt to a fresh `default` / `gpt-6-astra` / `high` implementer. Preserve counts and mark any bypassed guided stage explicitly.

A failed guided candidate transfers to expert implementation without another Sol repair budget. Finish/transfer commands and cleanup before replacement. For expert-authored work, restore the ordinary independent Sol reviewer or a fresh equivalent if genuinely unavailable; an expert who authors code cannot accept it. Reuse valid evidence. Never return an unresolved escalated target to a fresh Sol/Luna budget, overlap repair writers or continue model loops through a material human decision.

## Capacity and migration

Reserve actual capacity for these roles before launching additional parallel authors. A completed turn may still occupy capacity; do not invent retirement or silently substitute another model. Finish independent ownership where useful and use supported lifecycle controls. If the required role still cannot run, checkpoint the affected target and obtain a fallback decision.

For Lead-authored integration, maintain an actual author/source record and independent review. Lead ownership cannot erase an existing behavior's required model stage or supply an uncounted repair. Preserve historical failures and accepted outcomes when grouping old slices into larger batches. Scope/performance observations do not authorize weakening requirements or acceptance assertions.
