---
name: tech_brainstorming
description: Interactive brainstorming workflow for planning new software projects, features, or technical initiatives. Guides you through structured multi-round Q&A to explore architecture, UI/UX, data models, and integration decisions, then produces a formal specification and step-by-step implementation plan. Use this skill whenever the user wants to brainstorm, plan, spec out, or think through a new feature, project, module, or technical design -- even if they don't explicitly say "brainstorm" or "spec". Also triggers for requests like "let's figure out how to build X", "I need to plan a new Y", "help me design Z", or "what would it take to add W".
argument-hint: <spec-name>
---

# Specification Brainstorming & Writing Workflow

You are running an interactive specification workflow. The user wants to define a new feature or project through structured brainstorming, then produce a formal spec and implementation plan.

## Input

The spec topic/name is: **$ARGUMENTS**

If no argument was provided, ask the user what they want to spec out before proceeding.

## Setup

1. **Determine the spec number.** Look in `spec/` for existing directories and files. Find the highest `{nr}` prefix (e.g., `01`, `02`, ...) and increment by 1. Pad to two digits.
2. **Derive the file slug** from the spec name argument: lowercase, hyphenated (e.g., "User Authentication" becomes `user-authentication`).
3. **Create the spec folder** at `spec/{nr}-{slug}/` -- all output files for this brainstorming session go into this folder.
4. **Create the brainstorming file** at `spec/{nr}-{slug}/brainstorming.md` with a heading `# {Spec Name} - Brainstorming`.
5. **Read existing project context** -- check `CLAUDE.md`, `package.json`, `pom.xml`, and other `spec/{nr}-{slug}/*` folders and files to understand the project's current state, tech stack, and conventions. Use this context to inform your questions and recommendations.

## Brainstorming Phase

This is an iterative, conversational brainstorming process. Each round builds on what you learned from the previous round's answers. You are not pre-planning all the questions -- you are discovering what to ask next based on the user's decisions.

### One round at a time

Write **one round** of 2-5 questions per iteration. After the user answers, **re-read the entire brainstorming file**, analyze their choices, and then design the next round's questions based on what you now know. Each round should go deeper into the areas that matter most given the user's previous answers.

For example:
- Round 1 might ask about core scope and high-level architecture
- If the user chose a REST API in Round 1, Round 2 might drill into endpoint design, auth strategy, and data model
- If they chose a CLI tool instead, Round 2 would ask about command structure, output formats, and configuration
- Later rounds get increasingly specific -- error handling for the chosen architecture, edge cases for the chosen workflow, seed data for the chosen data model

Do NOT pre-plan all rounds upfront. Each round is a response to the previous one.

### Question format

Every question uses this exact format:

```markdown
### Q{n}: {Question text}

{Optional: ASCII visualization, diagram, or comparison table if it helps clarify the options}

- [ ] A) **{Option}** -- {Brief explanation}
- [x] B) **{Option}** **(recommended)** -- {Brief explanation}
- [ ] C) **{Option}** -- {Brief explanation}
```

Rules:
- Always pre-select the recommended option with `[x]` -- the user can change it, but having a default reduces friction and shows you've thought about it
- Mark the recommended option with `**(recommended)**`
- Use `--` (not `---`) between option and explanation to avoid YAML confusion
- Include 2-5 options per question (A, B, C, ...)
- Use ASCII diagrams/visualizations when comparing layouts, architectures, or flows -- a picture is worth a thousand words, especially for spatial or structural decisions
- Group questions into themed rounds with a `## Round {n}: {Topic}` heading and a `---` separator between rounds

### What to cover (as a compass, not a checklist)

Use these categories as a mental map for what might need exploring. Don't march through them in order -- let the user's answers guide which areas need attention:

1. **Core concept & scope** -- purpose, target audience, boundaries. What's in, what's out?
2. **Architecture & data model** -- storage, schemas, relationships. Where does state live?
3. **UI/UX** -- component library, layout, interactions, visual style. What does the user see and touch?
4. **Workflow & logic** -- user flows, state machines, business rules. What happens when?
5. **Integration points** -- APIs, external services, dependencies. What does this connect to?
6. **Content & seed data** -- defaults, templates, sample data. What ships on day one?

Skip topics that don't apply. Add domain-specific topics as needed. A CLI tool doesn't need UI/UX rounds; a pure backend service doesn't need frontend design questions.

### Between rounds

After writing each round's questions to the brainstorming file:
1. Tell the user which file to edit and what to do ("Edit `spec/{nr}-{slug}/brainstorming.md` and mark your choices with `[x]`")
2. **Stop and wait** for the user to respond (they will edit the file directly or reply in chat)
3. **Re-read the entire brainstorming file** to see all accumulated answers
4. Analyze the answers: What decisions have been made? What new questions arise from those decisions? What areas are still unexplored?
5. Write the next round, targeting the most important open questions given what you now know
6. **Only append** to the brainstorming file -- never modify existing questions or answers. This preserves the decision history.

### When to stop

Stop brainstorming when:
- All major aspects of the spec have been covered (given the specific shape the project has taken based on the user's answers)
- You have enough information to write a complete specification
- The user explicitly says they're done

Before stopping, confirm with the user: "All key areas are covered. Ready to move on to the tech stack?"

## Tech Stack Decision Phase

After brainstorming is complete but before writing the specification, settle on the tech stack. This happens in the brainstorming file as a final round.

### Step 1: Propose a tech stack

Based on everything learned during brainstorming (the chosen architecture, data model, UI approach, integrations, etc.) and the existing project context from `CLAUDE.md` / `package.json` / `pom.xml`, propose a concrete tech stack. Append it to the brainstorming file as a new section:

```markdown
## Tech Stack Proposal

Based on the decisions above, here is the recommended tech stack:

| Layer        | Technology              | Rationale                        |
|--------------|-------------------------|----------------------------------|
| Backend      | ...                     | ...                              |
| Frontend     | ...                     | ...                              |
| Database     | ...                     | ...                              |
| ...          | ...                     | ...                              |

**Do you agree with this stack, or would you like to use something different?**
If you want a different stack, describe it below or edit the table.
```

Include all relevant layers: language/runtime, framework, database, messaging, frontend framework, styling, build tools, deployment, etc. Only include layers that apply.

If an existing project is detected (via `CLAUDE.md`, `pom.xml`, `package.json`), the recommendation should align with what's already in place unless the brainstorming decisions clearly call for something different.

### Step 2: Wait for the user's response

The user will either:
- Accept the proposed stack (explicitly or by not changing anything)
- Modify the table or describe a different stack in chat

### Step 3: Evaluate the choice (once, max)

If the user changed the stack or proposed their own:
1. **Acknowledge their choice** -- don't dismiss it
2. **Evaluate honestly** -- compare their choice against the brainstorming decisions. If there are real downsides (e.g., chose a NoSQL database but the brainstorming revealed heavy relational queries, or chose a framework the existing codebase doesn't use), clearly state the trade-offs
3. **Suggest an alternative only if there are concrete downsides** -- not just preference. Frame it as: "Here's what might bite you: [specific issue]. Consider [alternative] instead. But it's your call."
4. **Accept the user's final decision** -- if they stick with their choice after seeing the trade-offs, move on. Do not ask again or push back further. One round of feedback, maximum.

If the user accepted the proposed stack, skip the evaluation -- no need to second-guess your own recommendation.

Append the final agreed-upon tech stack to the brainstorming file as `## Final Tech Stack` before proceeding to the specification phase.

## Specification Phase

Once brainstorming is complete, write a formal specification to `spec/{nr}-{slug}/specification.md`.

The specification consolidates all decisions from the brainstorming into a clear, implementable document. It should include:
- **Overview** -- what this feature/project is and why it exists
- **Requirements** -- functional and non-functional, derived from brainstorming answers
- **Data model** -- schemas, relationships, storage decisions
- **API / Routes** -- endpoints, request/response shapes, authentication
- **UI pages / components** -- layouts, interactions, states
- Any other sections relevant to the specific project

Reference the brainstorming file for decision rationale. Match the style and depth of any existing `spec.md` in the project.

## Frontend Design Phase (optional)

After writing the specification, check whether the project includes frontend work (UI pages, components, layouts, visual design). Look at the brainstorming answers and the specification -- if any section describes user-facing interfaces, this phase applies.

If frontend work is present:
1. **Ask the user**: "This spec includes frontend work. Want me to spawn a design agent to create visual designs / prototypes for the UI?" -- only proceed if they confirm.
2. **Spawn a subagent** using the `frontend-design` skill. Provide it with:
   - The full specification file (`spec/{nr}-{slug}/specification.md`)
   - The brainstorming file (`spec/{nr}-{slug}/brainstorming.md`) for additional context on UI/UX decisions
   - The existing codebase context (tech stack, component library, styling conventions from `CLAUDE.md`, `package.json`, and any existing frontend code)
   - A clear brief: which pages/components to design, the visual style and interactions described in the spec
3. The design agent produces frontend code (components, pages, styles). Its output serves as a **visual reference and starting point** for the implementation phase -- not the final implementation.
4. Note the design output location in the specification file so the implementation plan can reference it.

If the project has no frontend work, skip this phase entirely.

## Implementation Plan Phase

After the specification, create `spec/{nr}-{slug}/implementation-plan.md`.

The implementation plan breaks the work into concrete, ordered steps:
- **Numbered steps** (Step 1, Step 2, ...) each with a clear goal
- **Files to create/modify** listed per step
- **Verification check** per step -- how to confirm the step is done correctly
- **Dependency ordering** -- foundation first, then features that build on it
- **Dependency graph** showing which steps can run in parallel
- **Checklist** -- each step has `- [ ]` sub-tasks
- **Decisions summary table** at the top for quick reference

## Important rules

- **Never start implementation.** This skill only produces documents: brainstorming, specification, and implementation plan.
- **All output files go in `spec/{nr}-{slug}/`** -- each brainstorming session gets its own folder containing brainstorming.md, specification.md, and implementation-plan.md.
- **Append-only brainstorming** -- never edit existing content in the brainstorming file. Only add new rounds.
- **Use the project context** -- reference existing tech stack, conventions, and patterns from the codebase. Recommendations should fit the project, not be generic.
- **Be opinionated** -- always recommend an option. The user hired you for your judgment, not to present a menu without commentary. But respect their choices when they override you.
