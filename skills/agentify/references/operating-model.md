# Operating model

Load this reference when auditing, adapting, or maintaining project-operation context.

## Default project model

When a repository does not already have an equivalent authoritative operating system:

- `AGENTS.md` — canonical agent entry/router.
- `ROADMAP.md` — human-visible phased plan and progress.
- `STATE.md` — current coding-shift handoff.
- local `CONTEXT.md` — only where subsystem complexity justifies it.

Do not duplicate an existing issue/PR/project-board workflow that already provides the same authoritative roles.

## ROADMAP.md

The roadmap belongs primarily to the human.

It should:
- describe phases or meaningful milestones;
- use checkable `[ ]` / `[x]` items for concrete work;
- distinguish completed from pending work;
- remain higher-level than `STATE.md`;
- avoid implementation transcripts and temporary debugging detail.

Agents may mark work complete only after the relevant acceptance or validation condition is satisfied. Do not silently invent, reorder, or remove human priorities merely to make the roadmap cleaner.

## STATE.md

`STATE.md` is operational, not historical. Rewrite it as work advances rather than accumulating a diary.

Recommended structure:

- **Current Phase**
- **Last Validated State**
- **Blockers**
- **Active Task**
  - Task
  - Scope
  - Target Files
  - Test
  - Acceptance Criteria
- **Immediate Backlog**

### Shift start

1. Read `AGENTS.md`.
2. Read `STATE.md`.
3. Confirm the active task still matches the roadmap.
4. Load only the relevant local context/source/tests.

### Shift end

1. Validate the work.
2. Update completed `ROADMAP.md` checkboxes when warranted.
3. Rewrite `STATE.md` to the exact validated handoff point.
4. Record blockers and the next smallest actionable step.

Do not use `STATE.md` as a chronological diary.

## Drift checks

In maintain mode, check for:
- stale current phase;
- roadmap checkboxes that disagree with validated implementation state;
- roadmap and state describing conflicting active phases;
- `STATE.md` accumulating history;
- missing acceptance criteria or validation state;
- handoff text that no longer points to existing files or work.
