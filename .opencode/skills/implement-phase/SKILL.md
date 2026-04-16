---
name: implement-phase
description: Implements the next unimplemented phase from ./plans/ using TDD, one user story at a time, with human-in-the-loop confirmation at each step. Use when user wants to start implementing a plan phase, says "implement next phase", "start phase N", or "let's build the next thing".
---

# Implement Phase

Drives the next unimplemented plan phase to completion via TDD, with the human confirming direction at every decision point.

## Workflow

### 1. Find the next phase

Read every `./plans/*.md` file. The next phase is the first `## Phase N` block that has at least one unchecked `- [ ]` acceptance criterion.

Present to the user:
- Which phase was found and in which plan file
- Its title, user stories, and acceptance criteria
- Ask: "Is this the right phase to work on, or do you want a different one?"

### 2. Explore the codebase

Before writing a line of code, explore what already exists:

- Run `find . -not -path '*/node_modules/*' -not -path '*/.git/*' | head -80` to get the directory tree
- Read any existing files that are directly relevant to this phase
- Identify what already exists vs. what needs to be built

### 3. Identify prerequisites

List everything that must exist before the first user story can be built. Examples:
- Package not yet created
- Missing dependency
- Config file not present
- Service not running (Postgres, Keycloak, etc.)

Present the prerequisite list as a numbered checklist and ask:
- "Do you want to handle these in order, or reorder/skip any?"

Wait for confirmation before proceeding.

### 4. Resolve prerequisites one at a time

For each approved prerequisite:
1. State what you are about to do (one sentence)
2. Do it
3. Verify it works (run a command, show output)
4. Ask: "Good to proceed to the next prerequisite?"

### 5. Implement user stories via TDD

For each user story in the phase (present them as a numbered list first and let the user reorder or skip):

1. **State the story** — one sentence describing what you're about to implement
2. **Load the `tdd` skill** — delegate the red-green-refactor loop to it
3. **Mark criterion done** — once the acceptance criterion is met, update the `./plans/` file: change `- [ ]` to `- [x]`
4. **Confirm** — show the passing test output and ask "Ready for the next story?"

### 6. Phase complete

When all acceptance criteria are checked off:
- Run the full test suite and show the output
- Update the plan file to reflect all criteria checked
- Say: "Phase N complete. The next phase is Phase N+1: [title]."

## Rules

- **One story at a time** — never start the next story until the human confirms the current one is done
- **Never skip a prerequisite silently** — always ask before skipping
- **Always update the plan file** — check off `- [x]` as criteria are met; the plan file is the source of truth for progress
- **Delegate TDD to the `tdd` skill** — do not inline the red-green-refactor loop; load the skill instead
- **No speculative work** — only build what the current acceptance criterion requires
- **Show, don't tell** — always run and show test/command output; never claim something works without evidence
