---
name: orchestrate-tasks
description: Runs the loop over the kanban board in `TASKS.md`, one task at a time, and syncs the plan with reality.
disable-model-invocation: true
---

# orchestrate-tasks

You are the loop over the kanban board. You pick the next task, delegate the
work to `implement-task`, sync the plan with reality, and empty the AFK batch
at natural stops. You do not do TDD yourself. That is the job of
`implement-task`.

## Who owns what

- **The kanban board in `TASKS.md` owns status and progress.** Ready / In
  progress / Done is the only truth, and the **In progress** column is the
  marker for resuming. Deviations, the AFK batch and the sync history are in
  their own sections at the bottom of the same file. There is no separate
  state file.
- **`implement-task` owns one task**: the red-green cycle, self-verification,
  the HITL demo or the AFK batch line.

When resuming: read the board in `TASKS.md`. Status and exact position are
both there.

## Step 1: Find the basis

Ask the developer to point to the task folder `docs/tasks/<task-id>/`. Look for:
- `SPEC.md` (from `grill-spec`)
- `TASKS.md` (from `split-tasks`)

If the spec or tasks are missing: stop and ask the developer to run the right skill first.

## Step 2: Pick the next task

From the board in `TASKS.md`:
- Skip everything under **Done**
- If something is under **In progress**: take it (resume)
- Otherwise: take the first task under **Ready** whose blocking tasks are all done

If the board is empty: show the AFK batch to the developer and stop.

## Step 3: Delegate to `implement-task`

Start a subagent that uses the `implement-task` skill with the chosen task as
input. `implement-task` does all per-task work: HITL check-in, TDD
delegation, self-verification, moving to Done, batch update, recording
deviations.

Wait for the report. If `implement-task` stopped because verification failed
or because of HITL feedback: stop the loop and hand control to the developer.

## Step 4: Sync check

Look at the **Deviations from plan** section at the bottom of `TASKS.md`. If
there are new deviations since the last sync: stop and propose concrete
updates to `SPEC.md` and/or `TASKS.md` (what, where, why). Wait for approval
per item, apply the changes, and log them under **Sync history** with the
date. A new deviation always stops the loop, even in the middle of an AFK run.

If the developer triggered the skill in sync-only mode ("sync the plan"):
run only this step and stop.

## Step 5: Loop or stop

Go back to step 2 and take the next task. Stop the loop when:
- All tasks are done (show the batch to the developer)
- The next task is HITL (show the batch, ask for a go before you start `implement-task`)
- The developer asks for a pause (show the batch)

Always finish by putting today's date in `<!-- Last updated: [DATE] -->` at
the top of `TASKS.md`. That is for people reading the file. The kanban board
(the Ready/In progress/Done columns) is what tells the next run where it is.