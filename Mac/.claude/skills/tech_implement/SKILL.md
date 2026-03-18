---
name: tech_implement
description: >
  Use this skill to implement a feature from an existing specification using
  test-driven development (TDD) with a coordinated team of agents. This is the
  go-to skill when the user has a completed spec or implementation plan and
  wants to move to coding — especially when they mention TDD, tests-first,
  quality assurance, or team-based parallel development. The workflow writes
  and reviews tests before any production code, then delegates implementation
  to coding agents who work against those tests. Trigger this skill for
  requests like "implement the spec", "build from the plan", "start development
  on X", "code the feature", "implement with TDD", or any variation of going
  from an existing specification to working, tested code. Do NOT trigger for
  planning, brainstorming, or creating specs (use tech_brainstorming instead),
  and do NOT trigger for simple bug fixes, refactors, or one-off code changes
  that don't reference a spec.
argument-hint: <spec-name>
---

# TDD Team Implementation Workflow

You are running a team-based development workflow that implements a specification using Test-Driven Development. You will read the spec, create a task list, orchestrate test-writing agents, and then hand off to coding agents — all coordinated by you as the team lead.

The core principle: **tests come before code**. Every implementation step goes through a TDD cycle where tests are written and reviewed before a single line of production code is written. This catches spec misunderstandings early, when they're cheap to fix, rather than after implementation when they're expensive.

## Input

The arguments are: **$ARGUMENTS**

Parse the arguments:
- The spec name (required): identifies which spec to implement (matches files in `spec/`)

If no spec name is provided, list available specs from `spec/` and ask the user to pick one.

## Phase 1: Load the Specification

1. **Find the spec files.** Search `spec/` for files matching the spec name argument (look for `*-{spec-name}*` patterns). You need:
   - The specification file (e.g., `{nr}-{slug}.md` or `{nr}-{slug}-specification.md`)
   - The implementation plan file (e.g., `{nr}-{slug}-implementation-plan.md`)
   - If only a brainstorming file exists, tell the user to run `/spec` first to produce a specification and implementation plan.

2. **Also read:**
   - `CLAUDE.md` for project conventions
   - `package.json` for current dependencies and scripts
   - The spec's `progress.md` if it exists (located at `spec/{nr}-{slug}/progress.md`, where `{nr}-{slug}` is derived from the spec files found in step 1, e.g., `spec/06-recurring/progress.md` for spec files named `06-recurring-*.md`)

3. **Summarize** what you found: spec name, number of implementation steps, key decisions, and any existing progress.

## Phase 2: Create the Task List

Use TaskCreate to track all work. Read the implementation plan and create one task per implementation step. Each task should include:
- A clear title matching the step name (e.g., "Step 1: Database Migrations")
- A description with the step's goal, files to create/modify, and verification command
- Dependencies on prior tasks where the implementation plan specifies them

After creating all tasks, display the full task list to the user and confirm before proceeding.

## Phase 3: Team Setup

**You are the team lead. You do not write code, tests, or read source files yourself. You delegate everything to teammates.**

### Team lead responsibilities (only these):
- Create and manage the task list
- Spawn and coordinate teammates
- Review teammate plans and approve/reject them
- Synthesize status updates for the user
- Manage the TDD review cycle (described in Phase 4)
- Escalate to the user when the TDD review loop fails after 5 attempts
- Clean up the team when done

### Team lead restrictions (never do these):
- Read source code files (only read spec/plan files for delegation context)
- Write or edit any source code or test code
- Run build, lint, or test commands
- Run verification commands
- Research or explore the codebase
- Debug errors directly — delegate debugging to a teammate

### Deciding the team structure

Analyze the implementation plan and decide:
- How many teammates to spawn (consider parallelizable work from the dependency graph)
- What role/focus each teammate should have
- Which tasks to assign to which teammates

In addition to coding teammates, you need two specialized roles:
- **Test Automation Agent(s):** Write TDD-style tests before implementation begins for each step
- **Test Review Agent(s):** Review the tests against the spec/implementation plan to ensure correctness

General guidelines:
- Group related tasks that share context into the same teammate's workload
- Identify tasks that can run in parallel (e.g., backend services vs frontend UI)
- 3-6 teammates is typical; adjust based on the plan's parallelism
- Give each teammate a name of a Marvel Avenger hero
- The test automation agent and test review agent should be different teammates (separation of concerns — the writer shouldn't review their own work)

### If tasks cannot be broken down

For very small specs with only 1-2 simple tasks that don't benefit from parallelism, you may use a streamlined approach: spawn a single test agent and a single review agent for the TDD cycle, then a single coding agent. The TDD review gate still applies — this is non-negotiable regardless of team size.

## Phase 4: The TDD Cycle (per implementation step)

This is the heart of the workflow. For each implementation step, before any production code is written, the following cycle runs:

### Step 1: Test Automation Agent writes tests

Spawn (or instruct) the Test Automation Agent with:
- The specific implementation step from the plan
- Relevant spec sections describing expected behavior
- Project conventions from CLAUDE.md (testing framework, file naming, patterns)
- File paths and data shapes from the spec
- Any dependencies on previously completed steps

The Test Automation Agent writes TDD-style tests that:
- Cover the happy path and key logic for this implementation step
- Follow the project's existing test patterns and conventions
- Are runnable (they will fail since production code doesn't exist yet, but they should compile/parse)
- Test behavior described in the spec, not implementation details
- Include clear test names that describe what they verify

### Step 2: Test Review Agent reviews the tests

Once the Test Automation Agent finishes, spawn (or instruct) the Test Review Agent. Provide:
- The tests that were just written
- The same spec sections and implementation plan step
- Project testing conventions

The Test Review Agent evaluates:
- **Spec alignment:** Do the tests actually verify what the spec describes? Are important behaviors missing?
- **Correctness:** Are assertions correct based on the spec? Are expected values right?
- **Completeness:** Are the happy path and important logic branches covered?
- **Quality:** Do tests follow project conventions? Are they maintainable and clear?
- **No over-testing:** Tests shouldn't test implementation details or trivially obvious things

The Test Review Agent produces a verdict: **PASS** or **FAIL** with specific feedback.

### Step 3: Handle the review result

**If PASS:** The tests are approved. Move to Phase 5 (coding) for this step.

**If FAIL:** Send the review feedback back to the Test Automation Agent. The Test Automation Agent must address every piece of feedback and produce updated tests. Then the Test Review Agent reviews again.

**Track the attempt count.** This review loop can repeat up to 5 times. If the 5th review still results in FAIL:
- Stop the TDD cycle for this step
- Summarize the issue clearly: what the Test Automation Agent keeps getting wrong, what the Test Review Agent keeps flagging, and where the disconnect seems to be
- Present this summary to the user and ask for guidance on how to proceed (e.g., clarify the spec, adjust expectations, override the review, or skip tests for this step)
- Wait for the user's response before continuing

### Parallelizing TDD cycles

If multiple implementation steps are independent (no dependencies between them), you can run their TDD cycles in parallel — each with its own Test Automation Agent + Test Review Agent pair. This significantly speeds up the workflow for specs with high parallelism.

## Phase 5: Implementation (coding)

Once tests pass review for a step, assign the coding task to the appropriate coding teammate. Provide:
- The approved tests (so they know exactly what to make pass)
- The spec section for this step
- Project conventions from CLAUDE.md
- File paths and data shapes
- Dependencies on other completed steps

The coding teammate should:
1. Read and understand the pre-written tests
2. Implement the production code to make the tests pass
3. Run the tests and verify they pass
4. Run any additional verification commands from the implementation plan
5. Report back with results

### Plan mode enforcement

For any task that involves creating more than 2-3 files or has complex logic, require plan approval before the teammate starts implementing. Tell the teammate to work in plan mode. Review their plan and either approve or reject it with feedback.

Simple tasks (e.g., creating a single config file, adding a migration) do not need plan approval.

### Handling test failures during implementation

If the coding teammate cannot make the tests pass and believes a test is incorrect:
- The coding teammate reports the specific test and why they think it's wrong
- You (team lead) send this feedback to the Test Review Agent for a second opinion
- If the Test Review Agent agrees the test is flawed, send it back to the Test Automation Agent for a fix
- If the Test Review Agent disagrees, the coding teammate needs to adjust their implementation
- Use your judgment — if this becomes a back-and-forth, escalate to the user

## Phase 6: Coordination and Progress

### Workflow per implementation step
1. Claim the task — update status to `in_progress`
2. Run the TDD cycle (Phase 4) for this step
3. Once tests pass review, assign coding (Phase 5)
4. When coding is complete and tests pass, mark task `completed`
5. Update the spec's `progress.md` (at `spec/{nr}-{slug}/progress.md`) by delegating the update to a teammate
6. Assign the next available task

### Communication
- When teammates send messages, respond with clear direction
- If two teammates need to coordinate, tell them to message each other directly
- Broadcast only for team-wide announcements (e.g., "all backend tasks complete, frontend can now integrate")

### When a teammate is blocked
- Help coordinate with the teammate they depend on
- If blocked on a spec ambiguity, escalate to the user immediately — don't guess

### Final verification
When all tasks are complete, delegate a final end-to-end verification to a teammate:
- Run the full test suite
- Run any integration or e2e tests specified in the plan
- Verify the build succeeds
- Report results back to you

## Key Rules

- **Tests before code** — the TDD cycle is non-negotiable for every implementation step
- **Separate test writing from test review** — different agents, because the writer shouldn't grade their own work
- **Follow the spec exactly** — file paths, data shapes, API routes, field names
- **Use the task list** — create tasks before starting, update status as you go
- **Update progress tracking** — keep the spec's `progress.md` (at `spec/{nr}-{slug}/progress.md`) in sync with completed work. Create the subdirectory if it doesn't exist. Write the name of the implementation plan and the task list to the progress.md file. After each step is finished, check the step on the tasklist inside the progress.md
- **Don't over-abstract** — keep it simple, follow existing project patterns
- **Each step should compile** before moving to the next
- **Ask before proceeding** if something in the spec is ambiguous or contradictory
- **5-attempt limit on test review** — escalate to user after 5 failed reviews, don't loop forever
