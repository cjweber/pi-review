# pi-review

`pi-review` adds a practical code review workflow to Pi via `/review` and `/end-review`
that is used by us at Earendil.

## Install

```bash
pi install git:github.com/earendil-works/pi-review
```

## What It Does

- Review **uncommitted changes**
- Review changes against a **base branch**
- Review a specific **commit**
- Review a GitHub **pull request** (checks it out locally via `gh`)
- Review one or more **folders/files** as a snapshot (not a diff)
- Produce prioritized findings with a clear verdict and actionable follow-ups
- It separates feedback to the agent from human callouts

It also supports custom shared instructions that are loaded from `REVIEW_GUIDELINES.md`.

## Quick usage

```bash
/review
/review uncommitted
/review branch main
/review commit abc123
/review pr 123
/review pr https://github.com/owner/repo/pull/123
/review folder src docs
/review branch main --extra "focus on performance and error handling"
/review pr 123 --loop 3
```

When a review session is active, finish it with:

```bash
/end-review
```

You can then return only, return + summarize, or return + queue fixing work.

### Loop mode (`--loop N`)

`--loop N` runs up to `N` rounds of **review → fix findings → commit → push → re-review** in one go, without you executing each step:

- The review runs in the current session (no review branch, no `/end-review` involved).
- When the review reports findings, the agent fixes them, commits with a message like "Address review findings (round 2)", pushes, and the change is re-reviewed automatically.
- The loop stops early when a review comes back clean (verdict "correct"), or after `N` rounds with findings remaining so you can take over.
- Don't interleave manual prompts while a loop is active; if it ends mid-way, findings are in the session above.
