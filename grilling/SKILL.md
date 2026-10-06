---
name: grilling
description: >
  Interview primitive in rounds: asks the whole frontier of questions that can
  be answered now, numbered with a recommended answer, and waits for answers
  before the next round. Called by `grill-spec` and `split-tasks` when they
  need to clarify something with the developer. Do NOT use directly when the
  developer wants a specification. Tell them to type `/grill-spec` with a
  slash instead.
---

# grilling

You interview the developer about a topic another skill has given you.
Finding facts is your job. Explore code, documentation and context yourself
before you ask. Decisions are the user's job. Do not guess them.

## Input from the caller

The caller gives the **topic** of the interview (what needs clarifying) and
optionally where to write the answers (for example a section in a template).
Explore what you can clarify without asking before you write questions.

## Rounds, not one question at a time

Build a **frontier**: all questions that can be answered **now**, independent
of each other. Ask them together, numbered, each with a recommended answer.
Wait for answers to the whole round before you move on.

A question that depends on the answer to another, unanswered question does
**not** belong in this round. It comes in a later round, once the question
it depends on is settled.

## Loop

1. Explore code, documentation and context for what can already be answered without asking
2. Build the frontier: questions with no unsettled dependencies, numbered, with a recommended answer
3. Ask the whole round together, wait for answers
4. Update the basis with the answers, sharpen vague or overloaded words into precise terms
5. Build the next frontier from what is now settled
6. Repeat until the frontier is empty

## Done

The frontier is empty when no more questions can be asked without something
new coming up. Report briefly to the caller what was settled, and write the
result where the caller asked.