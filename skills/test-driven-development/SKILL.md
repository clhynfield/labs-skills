---
name: test-driven-development
description: Use when implementing any feature or bugfix in code — guides the strict RED-GREEN-REFACTOR cycle: write one failing test, confirm it fails for the right reason, write the minimal code to pass, refactor while the context is fresh.
---

# Test-Driven Development

A disciplined cycle for writing code one behavior at a time: red (failing test) → green (minimal code) → refactor.

## The Iron Law

**No production code without a failing test first.** Not even one line. Not even "I'll just sketch this out." Not even "the test is obvious — I'll add it after."

## The Cycle

### 1. Write the failing test

Write a single test for one behavior. The test should:

- Describe *what* you want, not *how* it's done.
- Fail in a meaningful way (assertion error — not syntax error, not missing import).
- Be small — favor one method's behavior over a whole class.

### 2. Confirm red

Run the test. Confirm it fails for the expected reason.

- Unexpected failure reason: diagnose first; don't paper over with implementation.
- Passes without any implementation: the behavior already exists, or the test isn't testing what you think. Investigate.

### 3. Write the minimal implementation

Write the simplest code that makes the test pass. Resist the urge to add more.

- No "while I'm here" additions.
- No defensive code for cases not yet tested.
- No abstractions for hypothetical future tests.

### 4. Confirm green

Run the test. Confirm it passes. Run the full suite to check for regressions.

### 5. Refactor

Look at the code you just wrote and the code around it:

- Is there duplication to extract?
- Are names clear and intention-revealing?
- Is the code as simple as it can be while remaining clear?
- Any code smells?

If you see an improvement, make it. Re-run tests. If nothing needs refactoring, say so out loud and move on.

## Keep documentation in step

A behavior isn't done when the test goes green — it's done when someone else can find and use it. Once a cycle completes, check whether the change altered the *user-facing shape* of the project: new commands, public functions, parameters, configuration, environment variables, or usage patterns.

If it did, update the README and any affected docs as part of the same change — not as a follow-up. A green test protects the behavior; current docs make it usable. Undocumented behavior that works is behavior nobody can rely on.

This is part of finishing the cycle, not an afterthought. If the shape didn't change — an internal refactor, a private helper — there's nothing to document, and that's fine.

## Running the cycle with subagents

If your environment can spawn subagents, you can run RED, GREEN, and REFACTOR as three separate, sequential subagent turns — one phase each, orchestrated by the main agent. This is optional and aimed at non-trivial work; the single-agent loop above stays the default for small steps.

Why bother: hard phase separation makes the Iron Law structural rather than a matter of willpower. The Green agent *cannot* write code before a failing test exists, because it only starts once the Red agent hands off a confirmed-failing test. Disjoint write scopes keep each phase honest. The hand-off carries the diff and the reasoning forward, so the refactor still happens with the work fresh.

### When to use it

- A new behavior, or a new code path through existing code.
- A bug fix — the Red agent first writes a test that reproduces the bug.
- Any change where the right design isn't obvious yet — let the tests drive it.

For trivial mechanical changes (renaming a symbol, tweaking a string, fixing a typo), don't bother — a single turn is simpler.

### The three turns

The orchestrator runs the turns in order and passes each hand-off to the next:

```
Orchestrator
    │
    ├─► Red agent      — writes the failing test
    │        │ hands off: test diff + confirmed failure output
    │
    ├─► Green agent    — writes the minimal code to pass
    │        │ hands off: implementation diff + confirmed pass output
    │
    └─► Refactor agent — cleans up under green
             │ hands off: refactored diff + confirmed pass output
```

### Red agent — write the failing test

- Touch only the test file(s). Do not write or modify implementation code.
- Add a focused test for one behavior, following the existing test style.
- The test must fail for the right reason — a missing piece, a wrong return value, an unhandled case — not a syntax or import error.
- Run the suite. Confirm the new test fails and the pre-existing tests still pass.
- Hand off: the exact test diff, the failure output, and a one-sentence summary of what the test asserts.

### Green agent — make it pass

- Touch only the implementation. Do not modify the tests.
- Write the minimum code that turns the failing test green — no speculative abstractions, no future-proofing.
- Run the full suite. Confirm it is green.
- Hand off: the exact implementation diff and the passing test output.

### Refactor agent — clean it up under green

- May touch both implementation and tests, but must not change observable behavior. Test assertions stay semantically identical — descriptions may be reworded, never weakened.
- Run the suite before and after; both runs must be green.
- Look for duplication, unclear names, functions doing more than one thing, magic values, and dead code introduced during the green phase.
- If nothing needs improving, say so explicitly. An unnecessary refactor is worse than none.
- Hand off: the diff (or "no changes needed") and the test output confirming green.

Keep each turn to its own phase. A subagent that writes both the test and the implementation in one turn has collapsed the cycle — the separation is the whole point.

## Common Rationalizations

| Excuse | Reality |
|---|---|
| "Test is trivial, I'll skip it" | Trivial tests catch real regressions later. Write it. |
| "I already manually tested it" | Manual tests don't run in CI. Write the automated test. |
| "I'll write the test after" | Tests-after asks "what does this do?" Tests-first asks "what should this do?" Different exercises. |
| "Refactor isn't needed here" | Say so explicitly. Don't silently skip Step 5. |
| "I'll batch the refactor at the end" | Refactor with context fresh, not after you've moved on. |
| "I'll let one subagent do the test and the impl to save a turn" | That collapses the phases the subagent split exists to keep apart. Red hands off a confirmed-failing test before Green starts. |

## Red Flags — STOP

- Code before test.
- "I'll just sketch this out and add the test later."
- Test added after the code already passed manually.
- Implementing more than one behavior per cycle.
- Skipping Step 5 without saying so.
- A single subagent turn that writes both the test and the implementation.

All of these mean: stop, delete the unprotected code, start the cycle properly.