---
name: implement-task
description: >
  Runs one task from the kanban board in `TASKS.md` with TDD as a subagent,
  verifies the diff and tests itself, runs `review-diff`, and updates the
  board. Stops after one task. Use when the developer says "implement the
  next task" or when `orchestrate-tasks` delegates a task. NOT for running
  several tasks in a row or starting orchestration. Ask the developer to type
  `/orchestrate-tasks` themselves. For the red-green cycle, see `tdd`. For the
  review itself, see `review-diff`.
---

# implement-task

You take **one** task from the kanban board, delegate it to `tdd` as a
subagent, verify the result, and update the board. Then you stop.

## Step 1: Pick the task

Read `SPEC.md` and `TASKS.md` in `docs/tasks/<task-id>/`. If either is
missing: stop, ask the developer to run `grill-spec` or `split-tasks`.

If the caller named a task: use it. Otherwise, from the board in `TASKS.md`:
- Skip **Done**
- Take what is under **In progress** (resume)
- Otherwise take the first task under **Ready** with no unfinished blockers

Move the task to **In progress**. Read the HITL/AFK mark on the task.

If this is the first time the task moves to **In progress** (no **Start
point** line under it yet): run `git rev-parse HEAD` and write the result into
the task block as `- **Start point:** <SHA>`. If you resume a task that
already has a **Start point** line: use that SHA. Do not take a new one.

## Step 2: Check in before (HITL only)

AFK: skip to step 3. HITL: show the developer the chosen task, the thin
vertical slice, the first red test case, the test surfaces, and anything you
need clarified. Wait for an explicit go.

## Step 3: Delegate to tdd (always)

Start `tdd` as a subagent for the slice: red test, green implementation,
refactor. Thin but complete through every layer.

## Step 4: Verify yourself (always)

Read the diff and run the tests yourself. Do not blindly trust the
subagent's report. If verification fails: treat it as HITL, stop for the
developer.

## Step 5: Code review (always)

Call `review-diff` with the **Start point** SHA from the task block in
`TASKS.md` as the "before" point. Read the last line of the report,
`Total blocking: <sum>`. If the sum is greater than 0: treat it as HITL, stop
for the developer before you go on to step 6. If the sum is 0: go on to step
6, and include the **Consider** findings in the report or the demo. If the
report has findings marked **Human**: treat the task as HITL in step 6, even
if it is marked AFK.

You may commit along the way. The Start point SHA makes the review see every
change since the task started, including committed ones. Do not push before
the review has given `Total blocking: 0`.

## Step 6: After the implementation

HITL: demo what was built, the tests added, deviations from the plan, and
findings from `review-diff`. Ask "Does this look right?". Yes: move to
**Done**. No: note the feedback and iterate within the same task.

AFK: move to **Done**, add one line (task, what was built, tests added) to
**AFK batch** at the bottom of `TASKS.md`. Do not interrupt the developer.

## Step 7: Record deviations

If something does not match `SPEC.md` or `TASKS.md`: write one line under
**Deviations from plan** at the bottom of `TASKS.md`. Do not update the spec
or tasks yourself. `orchestrate-tasks` does that.

## Step 8: Report and stop

Update `<!-- Last updated: [DATE] -->`. Report to the caller: which task was
finished, tests added, deviations recorded, and any reason for stopping.
Stop.