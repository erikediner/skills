---
name: debugging
description: >
  Debugs one concrete bug: builds a red feedback loop that reproduces exactly
  this bug before forming any hypothesis, then minimizes, instruments, fixes,
  and writes a regression test. Use when the developer says "debug", "find
  the bug", or when something throws, fails or is unexpectedly slow. Do NOT
  use when a test fails as expected in the red phase of `tdd`, or when the
  caller is `tdd` or `implement-task`.
---

# debugging

You debug one concrete bug. The goal is a loop that reliably goes red on
exactly this symptom, before you guess at the cause. No fix without a
reproduction that proves the fix changes something.

## Security: applies throughout the debugging

Hide secrets (tokens, passwords, API keys, personal data) in everything you
show: logs, error messages, HTTP requests, screenshots. Build loops that read
secrets from environment variables (`$env:`, `.env`, secret manager). Never
paste a real key into a command, file or message to the developer.

## Phase 1: Build the red loop (the main part)

Before you form a single hypothesis, build a feedback loop that reproduces
the bug reliably and fast. You use this loop for the rest of the debugging.
It must run again in seconds, not minutes.

Rank the methods in this order and use the first one that works:

1. **Failing test at the nearest test surface.** Is there already a failing
   test, or can you quickly write one against the public interface closest
   to the bug? This is the fastest to iterate on, and it becomes the
   regression test in phase 6.
2. **HTTP call against a running dev server.** `curl` or `Invoke-RestMethod`
   against the endpoint with input that triggers the bug. Use this when the
   bug sits in an API layer and a test needs too much setup.
3. **CLI run against fixed input.** Run the command or script directly with a
   fixed, minimal input set that triggers the bug.
4. **Headless browser script.** For bugs that only occur in the UI or DOM.
   Navigate to the page, perform the action, catch the error.
5. **Replay of a saved request.** Last resort: a HAR file, a request dump or
   a logged event from production or staging, replayed locally.

Confirm that the loop actually goes **red** on the symptom before you move
on. If it does not go red, you have not reproduced the bug yet. Stay on this
step. Do not guess your way forward without a reproduction.

## Phase 2: Minimize

Remove everything from the reproduction that is not needed for it to go red:
irrelevant input fields, unrelated code paths, unneeded setup. A minimal
example makes the next step precise instead of vague.

## Phase 3: Hypothesis

Form one concrete hypothesis about the cause, based on the minimized example
and the code you have read. One hypothesis at a time, not a list of guesses.

## Phase 4: Instrument

Confirm or reject the hypothesis with a log point, a debugger, or an assert
placed where the hypothesis says the problem is. Do not fix before you have
confirmed. An unconfirmed hypothesis is still a guess.

## Phase 5: Fix

Make the smallest change that fixes the confirmed cause. Run the red loop
from phase 1 again. It must now go green.

## Phase 6: Write a regression test

Turn the reproduction from phase 1 into a permanent test in the test suite,
if it was not a test already. The test must fail against the code as it was
before the fix, and pass after.

## Report back

Brief, to the developer: what the bug was (file and line), what caused it,
what the fix was, and which regression test you added.