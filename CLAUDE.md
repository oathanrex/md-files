# Engineering Guidelines (Karpathy-inspired, extended)

Behavioral rules to reduce common LLM coding mistakes. Derived from Andrej Karpathy's
observations on LLM coding pitfalls, extended with an explicit tradeoff dial and
verification specifics.

## 0. Tradeoff Dial (read this first)

These rules bias toward **caution over speed**. That's correct for anything touching
shared code, production paths, or ambiguous requirements. It's *wrong overhead* for
trivial, unambiguous, single-file changes.

Before applying rules 1-4, classify the task:
- **Trivial** (typo fix, one-line config change, obvious bug with one fix) → skip to
  implementation. Don't ask, don't hedge.
- **Non-trivial** (anything with more than one reasonable interpretation, anything
  touching shared/public interfaces, anything you're not 100% sure is correct) →
  apply all rules below.

If you're unsure which bucket you're in, you're in the non-trivial bucket.

## 1. Think Before Coding

- State your assumptions explicitly before writing code.
- If multiple interpretations exist, list them — don't silently pick one.
- If a simpler approach exists than what was asked for, say so before implementing
  the more complex one.
- Push back when a request seems likely to cause a problem downstream.
- If something is unclear, stop and name what's confusing. Ask, don't guess.

## 2. Simplicity First

- Write the minimum code that solves the stated problem. Nothing speculative.
- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- Heuristic: if you wrote 200 lines and it could be 50, rewrite it.
- Self-check: "Would a senior engineer call this overcomplicated?" If yes, simplify.

## 3. Surgical Changes

- Touch only the lines required to accomplish the task.
- Do not reformat, rename, or "clean up" adjacent code you weren't asked to change,
  even if it looks wrong to you — flag it separately instead.
- **Diff-size guardrail:** if your diff touches more files or more lines than the
  task description implies, stop and explain why before proceeding — this is the
  line between "surgical" and "unrequested refactor."
- Example:
  - Task: "Fix the null check in `parseUser()`."
  - Wrong: fixing the null check *and* renaming three variables *and* reformatting
    the whole file.
  - Right: fixing the null check, and a one-line comment noting the other issues
    if they seem worth flagging.

## 4. Goal-Driven Execution

- Define success criteria before you start: what does "done" look like, concretely?
- After implementing, verify — don't just assume it works:
  - Run the project's actual test command (check `package.json` / `Makefile` /
    `pyproject.toml` for the real one — don't guess a generic `npm test`).
  - Run the linter/type-checker if the project has one configured.
  - For behavior changes with no existing test coverage, either add a minimal test
    or explicitly state that you could not verify and why.
- Loop on failures until success criteria are met, or report back with what's
  blocking and why, rather than submitting broken code.
- Example: "Add rate limiting to the /upload endpoint" →
  success criteria = requests beyond N/min return 429, existing tests still pass,
  new test covers the 429 case.

## 5. When Rules 1-4 Conflict With Speed

If the user explicitly says "just do it fast, I'll review" or similar — that's
permission to skip the asking/hedging in Rule 1, but Rules 2-4 (simplicity,
surgical scope, verification) still apply. Speed is not permission for scope creep.
