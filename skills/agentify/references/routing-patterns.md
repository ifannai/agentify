# Routing patterns

Load this reference when evaluating or creating `AGENTS.md` or local `CONTEXT.md` files.

## Route; do not preload

Routing files are signposts, not encyclopedias. Keep always-loaded context small and use pointers.

Preferred path:

```text
AGENTS.md
  ↓
STATE.md or equivalent current-work source
  ↓
relevant subsystem CONTEXT.md
  ↓
specific source + tests
```

## Subject first, type second

Place durable context close to the subject it governs:

```text
src/domain/CONTEXT.md
src/data/CONTEXT.md
infra/CONTEXT.md
```

Prefer this over a generic bucket of unrelated context files.

## Context layers

- Layer 0 — entry: `AGENTS.md`; identity, global rules, high-level routing.
- Layer 1 — current work/router loaded after intent is known.
- Layer 2 — local subsystem contract: `CONTEXT.md`.
- Layer 3 — stable references loaded only when pointed to.
- Layer 4 — task-specific outputs, diffs, logs, and evidence.

Load by need, not folder depth.

## AGENTS.md

`AGENTS.md` is the canonical agent entry point. Keep it short and route-first.

A good root file normally contains only:
- project identity/purpose;
- truly global rules;
- compact routing to major subjects;
- pointer to the authoritative current-work source;
- minimal validation guidance.

Route to subjects, not every file.

Example:

```text
Task / subject    Go to          Read next
domain behavior   src/domain/    src/domain/CONTEXT.md
persistence       src/data/      src/data/CONTEXT.md
UI interaction    src/ui/        src/ui/CONTEXT.md
deployment        infra/         infra/CONTEXT.md
```

If routing no longer fits comfortably in a small entry file, move detailed routing to a root `CONTEXT.md` and point to it.

Do not turn `AGENTS.md` into a README duplicate, architecture manual, or historical diary.

## Local CONTEXT.md

Create one only when a subsystem has enough distinct rules to justify it.

Useful sections:
- Job — what this subject owns.
- Contents — important concepts/files only.
- Rules — local invariants.
- Boundaries — what belongs elsewhere.
- References — stable detail to load selectively.
- Verify — checks relevant to this subject.

Prefer pointers over copied explanations.

## Vendor-specific files

Treat vendor-specific instruction files as non-canonical. Do not create or depend on them unless the user explicitly asks.

If one materially conflicts with `AGENTS.md`, report the conflict as drift.
