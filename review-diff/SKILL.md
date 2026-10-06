---
name: review-diff
description: >
  Reviews a bounded diff from a fixed point in the git history with two
  independent reviewers, each in its own context: correctness and tests, and
  requirements. Reports findings with file and line, and ends with
  `Total blocking: N`. Use when the developer says "code review", "review the
  diff", "check the code against the spec", when `implement-task` calls it
  before a task moves to Done, or before push and merge. NOT needed before
  every commit. NOT for threat modeling or architecture review. NOT for
  writing commit messages or summarizing changes.
---

# review-diff

You lead a review of a diff. Two reviewers read the diff, each in its own
context, without the conversation that wrote the code. You check their
findings and report back. You change no files and do not commit.

## Step 1: Fix the diff

The caller gives the "before" point: a commit SHA, tag or branch. Run
`git rev-parse --verify <point>^{commit}` and
`git merge-base --is-ancestor <point> HEAD`. If either fails: stop and ask for
a point that is in the history of HEAD.

The diff is `git diff <point>`. It includes commits after the point and all
changes in the working tree, staged and unstaged. Do not change the index.

Run `git diff --stat <point>` first. Leave these out of the review by adding
the pathspec after `-- .` in every `git diff` and in
`git ls-files --others --exclude-standard`:

```
:(glob,exclude)**/pnpm-lock.yaml
:(glob,exclude)**/*.Designer.cs
:(glob,exclude)**/*ModelSnapshot.cs
:(glob,exclude)**/.pnpm-store/**
:(glob,exclude)**/node_modules/**
:(glob,exclude)**/dist/**
:(glob,exclude)**/bin/**
:(glob,exclude)**/obj/**
```

The filtered diff is `git diff --diff-filter=d <point> -- . <pathspec>`.
Quote each pathspec the way the shell needs, for example with single quotes
in PowerShell and bash. Get untracked files with
`git ls-files --others --exclude-standard -- . <pathspec>`.

`--diff-filter=d` keeps deleted files out. List them by name with
`git diff --name-only --diff-filter=D <point>` and the same pathspec. Reviewer
A still reviews deleted test files (`*.test.ts`, `*.spec.ts`, `*Tests.cs`).
Binary files are not read.

If the filtered diff and the list of untracked files are empty: stop and
report that there is nothing to review. If the filtered diff has more than
2000 changed lines: ask the caller to split it or confirm that it should be
reviewed as one.

Find the requirements. They are `SPEC.md` and the current task in `TASKS.md`
under `docs/tasks/<task-id>/`, or a mini-spec the caller sends along.

## Step 2: Start the reviewers in parallel

Always start A. Start B at the same time only if there are requirements. If
there are none: do not start B, and write "Not reviewed: no requirements"
under Requirements.

Give each reviewer the before point, the pathspec from step 1, the list of
untracked and deleted files and the instructions below. Also give B the
requirements. Do not give them the conversation that wrote the code.

The reviewers run `git diff` themselves and can read any file in the repo for
context, such as `AGENTS.md`, callers and tests. They change no files, do not
change the index and do not run tests. `implement-task` has already run them.

### Reviewer A: Correctness and tests

Ask: does the code do what it seems meant to do, and do the tests prove it?
Skip everything lint, type checking and Prettier catch.

**Blocking:**
- Code that gives a wrong result or crashes: null or undefined, wrong
  condition, wrong edge case, missing `await`, wrong order.
- Swallowed errors: empty catch, catch that logs and continues, fallback that
  hides that something failed.
- Input from a system boundary that is used or stored without validation.
  This covers HTTP endpoints, Rebus handlers and file parsers.
- Tokens, secrets or personal data in logs or code.
- New or changed behavior with no test covering it. Does not apply to changes
  with no runnable code, such as Markdown, skills, pipeline YAML and lint
  setup.
- A test that proves nothing:
  - The expected value is computed the same way as the code.
  - The test reads a source file as text and checks its contents.
  - The test mocks internal collaborators instead of testing through the
    test surface.
  - The diff weakens an assertion, skips a test or deletes a test.
- In Markdown, skills and configuration: a command, path, link or section
  heading that other files depend on is wrong or broken.
- A breach of "Don't" in `AGENTS.md`.

**Human:**
- A change to authentication, access control or cryptography. Always flag
  it, even when it looks right. It does not stop the flow, but the caller
  must show it to the developer.

**Consider:**
- Duplicated logic with small variations.
- A function or class that does more than one thing.
- Nesting deeper than three levels.
- A magic number or string without a named constant.
- A name that does not describe intent.
- A comment that explains what the code does, not why.
- Dead code or commented-out code.
- An abstraction with no current need.
- A deviation from the pattern in the same file or from the conventions in `AGENTS.md`.

### Reviewer B: Requirements

Ask: does the diff do what the requirements asked for, and no more?

**Blocking:**
- A missing requirement or acceptance criterion.
- A requirement that looks implemented but is wrong: wrong condition, wrong
  data source, wrong edge case.
- An acceptance criterion checked off in `TASKS.md` that the diff does not
  meet.
- Something built without being asked for that changes behavior the user
  sees, a public API or the data model.

**Consider:**
- Other things built without being asked for, such as helper functions or
  refactoring in touched files.

### Finding format

Each finding is one line:

```
[Blocking] path/to/file.ts:42 | what is wrong | how it fails or which requirement it breaks
[Human] path/to/Auth.cs:10 | what changes | why the developer must see it
[Consider] path/to/File.cs:17 | what | why
```

- Report only findings you can point to with file and line.
- If you are unsure, mark the finding **Consider** and write what needs checking.
- No praise, no summary, no findings about files outside the diff.
- End the report with exactly one line: `Blocking: <count>`.

## Step 3: Check the findings

Read the code behind each **Blocking** finding yourself before you pass it
on. If the finding is wrong, move it to "Rejected" with one line on why. Do
not add findings of your own, and do not change **Consider** to **Blocking**.

If a reviewer has the wrong format or the wrong count, count the checked
`[Blocking]` lines yourself and use that number. If the report is missing
entirely, run the reviewer again once. If it is still missing, write that
under "Not reviewed" and count it as one blocking finding. A review that did
not happen must never give `Total blocking: 0`.

## Step 4: Report back

Show the reports separately. Do not merge them and do not rank findings
across them.

```
## Correctness and tests
<findings from A>
Blocking: <count after checking>

## Requirements
<findings from B, or "Not reviewed: no requirements">
Blocking: <count after checking>

## Rejected
<findings that did not hold up when checked>

## Not reviewed
<excluded files, deleted files, binary files>

Total blocking: <sum>
```

The last line is always `Total blocking: <sum>`. The caller reads that line
to decide whether the flow stops. If the sum is greater than 0, the flow
stops. `implement-task` then does not move the task to Done, and nobody
pushes or merges. Findings marked **Human** do not stop the flow, but they
make the task HITL in `implement-task`. **Consider** stops nothing, but is
shown to the caller.