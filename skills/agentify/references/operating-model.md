# Operating model

Load this reference when auditing, adapting, or maintaining project-operation context.

## Default project model

The default repository-local operating model is:

- `AGENTS.md` — canonical agent entry/router.
- `ROADMAP.md` — human-visible phased plan and progress.
- `STATE.md` — current coding-shift handoff.
- local `CONTEXT.md` — only where subsystem complexity justifies it.

Use `ROADMAP.md` and `STATE.md` by default regardless of repository size. Repository size may justify omitting local `CONTEXT.md` files, but it is not a reason to replace the operating model with an arbitrary planning file.

An existing file such as `features.md`, `TODO.md`, notes, or a vendor-specific planning file does **not** count as an equivalent operating system merely because it contains current work.

Preserve another system instead only when one of these is true:

1. the human has explicitly chosen it as the authoritative project operating model; or
2. the repository has a clearly established external workflow, such as GitHub Issues/Projects/PRs, that reliably provides both human-visible planning and exact shift/task handoff.

When neither condition applies, normalize repository-local planning to `ROADMAP.md` + `STATE.md`.

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


## Migration during adapt

When adopting the default operating model:

1. Create `STATE.md` if it does not exist.
2. Create `ROADMAP.md` if it does not exist.
3. Migrate useful active-task, validation, blocker, and handoff information from legacy files such as `features.md` into `STATE.md`.
4. Migrate durable planned work or milestones into `ROADMAP.md`.
5. Update `AGENTS.md` to route to `STATE.md` and `ROADMAP.md` appropriately.
6. Remove or deprecate the superseded planning file when safe and when doing so does not violate a human approval boundary.

Do not retain a legacy planning file solely to avoid creating `STATE.md` or `ROADMAP.md`.
