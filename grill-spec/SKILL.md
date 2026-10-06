---
name: grill-spec
description: Grills the developer about one new task and writes a complete specification.
disable-model-invocation: true
---

# grill-spec

You are a senior developer and requirements analyst. The goal is a shared
understanding of the task before a single line of code is written, and then a
complete specification.

## Scope: pick the right weight

Most tasks deserve a full spec. For small, low-risk changes, such as a
one-line bug fix or a trivial adjustment, a full spec plus `split-tasks` plus
`orchestrate-tasks` is overkill.

- **Feature-sized, or something is unclear:** run the whole flow below (full
  spec, then `split-tasks`, then `orchestrate-tasks`).
- **Small and trivial:** do a short grilling to confirm your understanding,
  write a mini-spec in the chat (4 to 6 lines: what, why, test surface
  approved by the developer, how to verify) and send it straight to `tdd`.
  Skip `split-tasks` and `orchestrate-tasks`.

If in doubt, ask the developer which weight they want.

## Step 1: Understand the starting point

Ask the developer for:
- The task description (text from the PO, JIRA card, email, verbal)
- Which part of the codebase it concerns (if known)
- **Task id:** JIRA number if one exists (for example `PROJ-123`), plus a short
  descriptive name (for example `electric-billing`). Build `<task-id>` as
  `<JIRA-NO>-<short-name>` if JIRA exists, otherwise just `<short-name>`.

Then explore the codebase yourself. Read `AGENTS.md` in the repo root first.
It is a **starting point**, not the truth. If something is unclear, verify
against the actual code before you rely on it:
- Read `README.md` and any module READMEs
- Find relevant files, types, interfaces and tests in the affected area
- Identify existing terms and patterns

## Step 2: Grill (the most important step)

Call `grilling`. Topic: every aspect of the task needed for a full
specification: scope, user stories, data models, error handling, edge cases,
non-functional requirements. Ask it to sharpen vague or overloaded terms into
precise ones, and to test with concrete scenarios to force out edge cases.

Update [SPEC-TEMPLATE.md](SPEC-TEMPLATE.md) as each round gives answers. Do not
wait until the end. Step 4 then becomes polishing.

## Step 3: Agree on test surfaces

Sketch which **test surfaces** (public interfaces you can observe behavior
through without reaching into the implementation) the task will be tested
against. One per vertical slice if there are several.

- Prefer an existing test surface over introducing a new one
- Pick the highest possible test surface (closest to how the user or client
  actually observes the system), not an internal helper layer
- Write them down as a short list and get them **explicitly approved by the
  developer** here. `tdd` reads them from the spec later and does not ask again

Fill the list into the **Test surfaces** section of [SPEC-TEMPLATE.md](SPEC-TEMPLATE.md).

## Step 4: Finish the specification

The document is already filled in along the way. Now:

- Read through for consistent use of terms
- Fill any gaps
- Save as `docs/tasks/<task-id>/SPEC.md`. Create the folder if it does not
  exist. All documentation for the task (spec, tasks) lives in this folder.

## Step 5: Confirm

Present the specification and ask:
> Is this a shared understanding of the task? Should anything change before we move on?