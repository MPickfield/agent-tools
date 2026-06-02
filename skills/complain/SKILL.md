---
name: complain
description: After a task, complain with evidence about code, docs, and tooling that made the work harder, so the codebase gets more legible to future agents.
---

# /complain

You just finished a task. Before the friction fades, **complain — bluntly — about everything that made it harder than it should have been**: code, docs, comments, conventions, tooling, skills. You're reporting *lived friction*, not running a linter. Cut praise and diplomacy.

## Hard rule

**Every complaint cites evidence from THIS session — no evidence, drop it.** That's what separates this from `/review`: a generic smell you didn't actually trip on is noise here. Evidence = "expected A (its name said A), got B" / "opened N files to grok one flow" / "grepped K times to find X" / "assumed Z, got corrected" / "comment lied vs the code".

## What counts (lead with what only you, who did the work, would know)

- **Confusing / misleading** — code, names, or interfaces that surprised you or implied the wrong thing.
- **Hard to find** — scattered logic, no entry point, excessive grepping, missing orientation doc.
- **Bad comments/docs** — wrong, stale, or verbose comments and CLAUDE.md notes. (A lying comment is worse than none.)
- **Hard to test** — couldn't isolate the change; had to stand up the world to check one thing.
- **Process/tooling** — a skill told you to do the forbidden, a generated file (openapi.json, types.gen.ts) went stale after a git op, a convention lived only in the user's head until they corrected you.

**Assumed something wrong and got corrected?** Don't just file it as `[my-fault]` — ask what in the code, naming, or docs *invited* the assumption. That root is usually a `[code]`/`[doc]` complaint (often your highest-signal one).

Generic smells (SOLID/DRY, big files, indirection) count **only if you tripped on them this session** — else that's `/review`'s job.

## Group by root cause

Merge symptoms that share one cause into a single complaint. Duplicate entries are the main way this turns into noise.

## Output

Per complaint:
```
[tag] [code|doc|my-fault] `path:line`
WHAT HAPPENED: <concrete friction from this session>
EXPECTED vs FOUND: <assumption vs reality>
FIX: <smallest change that helps the next agent>
```
- `[tag]` — short freeform label for the kind (`misleading-name`, `hard-to-find`, `stale-comment`, `process`…).
- `[my-fault]` — your own mistake, not the code's. **Keep and flag it, don't omit.** If a doc/check would stop the next agent repeating it, it's really `[doc]`; otherwise label it honestly so you don't pad the list by blaming the code.

End with **Top 3**, ranked by friction × how often future work hits it. Don't refactor — hand the list to the user to decide.
