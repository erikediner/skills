---
name: tdd
description: >
  Implements one task (one vertical slice) with red-green-refactor and a
  tracer bullet approach. Tests behavior through the public interface, not
  implementation details. Use when the developer says "run tdd", "test
  first", "implement with tdd", when `implement-task` delegates a task, or
  when `grill-spec` sends a mini-spec directly.
---

# tdd

You implement one task, one vertical slice, using red-green-refactor.
You test behavior through public interfaces, not implementation details.

## Test surfaces: where tests belong

A **test surface** is the public interface you test behavior against: where
you can observe what the system does without reaching into it.

**The test surfaces are already agreed.** They are in the "Test surfaces"
section of `SPEC.md`, or in the mini-spec from `grill-spec` for small tasks,
clarified and approved by the developer. Read them from there. Do not agree
on or ask about test surfaces here.

If there is neither a "Test surfaces" section in `SPEC.md` nor a test surface
in the mini-spec: stop and ask the caller (the developer or `implement-task`)
to run `grill-spec` first. Do not guess test surfaces yourself.

## Philosophy

**Good test:** describes _what_ the system does through a test surface.
Survives refactoring because it does not care about internal structure. Reads
like a specification. Expected values come from an **independent source**: a
known-good literal, a worked example, the spec. Never recomputed the same way
as the code.

**Bad test:** coupled to the implementation. Mocks internal collaborators,
tests private methods, or verifies by going around the test surface. Breaks
when you refactor even though the behavior is unchanged.

## Anti-pattern: tautological test

A tautological test computes the expected value the **same way** the code
does: `expect(sum(a, b)).toBe(a + b)`. It always passes, gives zero
assurance, and can never reveal a bug. Expected values **must** come from an
independent source: a known-good literal, a hand-computed example, a value
from the spec.

## Anti-pattern: horizontal slicing

**Do not write all the tests first and then all the implementation.** That
produces bad tests that test imagined behavior and do not react to real
changes.

```
WRONG (horizontal):
  RED:   test1, test2, test3, test4
  GREEN: impl1, impl2, impl3, impl4

RIGHT (vertical, tracer bullet):
  RED -> GREEN: test1 -> impl1
  RED -> GREEN: test2 -> impl2
  ...
```

## Workflow

### 1. Plan

Before any code is written:

- Confirm which interface changes are needed
- **Read the test surfaces** from the "Test surfaces" section in `SPEC.md` or
  from the mini-spec. Do not agree on them again
- Prioritize behaviors to test (not implementation steps)
- Use the project's terms from the spec, `AGENTS.md` and README

If the test surfaces are missing: stop and ask the caller to run `grill-spec` first.

### 2. Tracer bullet

Write ONE test that confirms ONE thing:

```
RED:   Write a test for the first behavior -> the test fails
GREEN: Minimal code to pass -> the test passes
```

This proves the path through every layer works end to end.

### 3. Incremental loop

For each remaining behavior:

```
RED:   Write the next test -> fails
GREEN: Minimal code to pass -> passes
```

Rules:
- One test at a time
- Only enough code to pass the current test
- Do not anticipate future tests
- Keep tests on observable behavior

### 4. Refactor

When all tests are green:

- Remove duplication
- Hide complexity behind simple interfaces
- Run the tests after each refactoring step

**Never refactor while RED.** Get to green first.

## Checklist per cycle

- [ ] The test describes behavior, not implementation
- [ ] The test uses only the public interface
- [ ] The test would survive an internal refactoring
- [ ] The code is minimal for this test
- [ ] No speculative functionality added

## Reporting back

When the task is done, report briefly to whoever called you (the developer, `implement-task` or `grill-spec`):

- Which tests were added (file name and test name)
- Which behavior is now covered
- Any deviations from the plan
- Suggested follow-up if you found technical debt