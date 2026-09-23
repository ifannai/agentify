---
name: agentify
description: Audit, adapt, and maintain software repositories for safe, efficient, agent-agnostic coding-agent work. Use when asked to make a repository agent-friendly, reduce agent context cost, create or review AGENTS.md or CONTEXT.md routing, clarify architectural boundaries, establish current-work context, or check whether agent-facing context has drifted from the codebase.
---

# Agentify

Adapt repositories so coding agents can make safe, narrow changes without reconstructing the whole codebase.

A new agent should be able to answer cheaply:

1. What am I doing?
2. Where does this change belong?
3. What must I not break?
4. What context do I load next?
5. How do I verify completion?

Agentify is agent-agnostic. Treat `AGENTS.md` as the canonical agent entry point. Do not create or depend on vendor-specific instruction files unless the user explicitly asks for them.

# Principles

## Adapt context to the architecture

Understand the repository before proposing context files. Do not redesign application architecture merely to improve agent documentation. Architectural refactoring requires human approval.

## Route; do not preload

Routing files are signposts, not encyclopedias. Keep always-loaded context small and use pointers.

Preferred path:

```text
AGENTS.md
  ↓
current-work source, when relevant
  ↓
relevant subsystem CONTEXT.md
  ↓
specific source + tests
```

Not every repository needs every layer.

## Subject first, type second

Place durable context close to the subject it governs:

```text
src/domain/CONTEXT.md
src/data/CONTEXT.md
infra/CONTEXT.md
```

Prefer this over a generic bucket of unrelated context files. Central references are for genuinely cross-cutting knowledge.

## Load by need, not folder depth

Use this mental model:

- Layer 0 — entry: `AGENTS.md`; identity, global rules, high-level routing.
- Layer 1 — router/current work: optional source loaded after intent is known.
- Layer 2 — local contract: subsystem `CONTEXT.md`.
- Layer 3 — reference: stable detail loaded only when the local contract points to it.
- Layer 4 — working artifacts: task-specific outputs, diffs, logs, evidence.

Start small. Add layers only when real complexity requires them.

## Scripts automate; agents reason

Prefer deterministic tools for formatting, linting, type checks, schema checks, generation, migrations, and import-boundary checks. Use the model for judgment, ambiguity, architecture interpretation, and change planning.

# Modes

- `audit` — read-only assessment.
- `adapt` — apply an approved audit/plan.
- `maintain` — human-invoked drift review.

Natural-language requests such as “audit agent readiness” map to `audit`; “agentify this repo” maps to `adapt`; “check whether agent docs are stale” maps to `maintain`.

If adaptation is requested without a previous audit or approved plan, perform `audit` first and stop for review. If intent is ambiguous and modification may be involved, default to `audit`.

# Context types

## Durable

Stable architecture, invariants, dependency boundaries, security constraints, validation conventions, `AGENTS.md`, and local `CONTEXT.md`.

## Current

Active task, scope, blockers, acceptance criteria, immediate backlog. This may live in `STATE.md`, issues, PRs, project boards, task files, or another authoritative system.

Do not create duplicate current-state systems.

## Historical

Git history, completed PRs, old issues, implementation diaries, superseded plans, and past design discussion. Consult history only to answer a specific unresolved question.

# Progressive inspection

Once this skill is loaded, do not reread `SKILL.md` from disk unless the user asks to inspect/debug the skill itself.

Preferred audit path:

```text
AGENTS.md if present
  ↓
fresh repository snapshot
  ↓
structure + configuration
  ↓
architecture hypothesis
  ↓
targeted evidence
  ↓
conclusion
```

## Repomix snapshot

When Repomix is available, use it as the preferred structural snapshot in `audit` and `maintain`.

1. Read root `AGENTS.md` first if present.
2. Generate a fresh temporary Repomix snapshot of the current working tree.
3. Inspect summary, directory structure, metrics, and compressed representations.
4. Search the snapshot to test specific hypotheses.
5. Read full source directly only for files needed to resolve an unanswered question.

Never dump the entire Repomix output into model context merely because it exists.

Never treat repository-tracked `repomix-output.*` as authoritative unless freshness against current HEAD and working tree is verified. If the user supplies a snapshot, use it as evidence but verify freshness before relying on current paths or behavior.

Prefer temporary or ignored output. Typical CLI shape:

```bash
repomix . --compress --no-git-sort-by-changes \
  --output "${TMPDIR:-/tmp}/agentify-repomix.xml"
```

Do not include Git logs or diffs by default. Add them only to test a specific hypothesis about churn, recent changes, ownership, or drift.

If Repomix is unavailable, fall back to a repository tree, targeted search, and representative reads. Do not add a persistent project dependency merely to run it.

## Expansion rules

Prefer structural evidence before source contents:

- tree and filenames;
- package/build/test/CI configuration;
- imports and dependency direction;
- targeted searches;
- representative files;
- tests expressing intended boundaries.

Do not recursively read the repository for completeness.

Before broadening inspection, identify the unanswered question and read only what can answer it. The goal is to route future work, not memorize the repository.

Treat context consumption as part of audit quality:

```text
structure → hypothesis → targeted evidence → conclusion
```

not:

```text
broad reading → accumulated context → conclusion
```

Do not inspect Git history by default.

# Audit mode

Audit is read-only.

Inspect enough to understand:

- repository structure and entry points;
- `AGENTS.md`, local `CONTEXT.md`, current-work sources, README;
- build/package/test/CI configuration;
- architectural boundaries and dependency direction;
- generated/configuration files;
- security-sensitive boundaries;
- validation commands.

Treat vendor-specific instruction files as non-canonical. Do not create, extend, or depend on them. If an existing one materially conflicts with `AGENTS.md`, report the conflict as drift.

Identify validation commands, but do not run the complete suite automatically. Run checks only when requested, when repository health affects a finding, or when a claim cannot otherwise be verified.

## Assess

### Navigation and routing

Can an agent locate the correct subject without reading most of the repository?

A good root `AGENTS.md` normally contains only:

- project identity/purpose;
- truly global rules;
- a compact map of major subjects;
- pointers to what to read next;
- the authoritative current-work pointer, if one exists;
- minimal routing/validation guidance.

It should not be a complete architecture manual, README duplicate, historical diary, or giant source-file index.

If the routing table itself becomes large, introduce a dedicated root `CONTEXT.md` router and point to it from `AGENTS.md`.

### Boundaries

Can the agent explain what each major area owns and what belongs elsewhere? Look for unclear or accidental cross-boundary imports.

### Current state

Can the next agent determine what is active and what comes next? Reuse existing issues, PRs, boards, task files, plans, or state files. Do not recommend `STATE.md` by default.

### Validation

Can an agent determine exactly how to check tests, lint, type checking, build, formatting, architecture rules, and migrations where relevant?

### Invariants

Identify dangerous non-obvious rules. Determine whether each important invariant is mechanically enforced, documented, or implicit only.

### Context cost

Ask: how much unrelated context must an agent consume for one safe narrow change?

Do not use line count alone. A context hotspot may show:

- unrelated features repeatedly touching one file;
- narrow changes requiring most of a file;
- concentrated conflicts;
- multiple independently describable responsibilities;
- unrelated test setup;
- ownership that is hard to explain;
- changes requiring distant code knowledge.

### Coupling and duplication

Look for mixed responsibilities, entry-point hotspots, hidden dependency direction, generic modules accumulating behavior, and duplicate route maps/rules/current state.

Do not recommend refactoring merely because coupling exists. Explain the agent cost and whether a real upcoming change justifies intervention.

### Generated/configuration/security boundaries

Identify generated files, migrations, environment files, lockfiles, secrets, auth, authorization, persistence boundaries, user-generated content, client/server boundaries, shell execution, and deployment credentials.

Prefer explicit local constraints over generic prose.

### Documentation quality

Check whether agent-facing context is current, path-valid, non-duplicative, scoped, actionable, and worth loading. A stale instruction file is worse than no instruction file.

# Audit output

Report before changing anything.

Classify findings:

- `Good` — supports low-context safe work.
- `Watch` — workable but likely to create context/drift cost.
- `Problem` — likely to misroute an agent, hide a critical invariant, or force broad reading.

For each meaningful `Watch` or `Problem`, include evidence, agent impact, and the smallest useful correction. End with a prioritized adaptation plan. Do not recommend files merely to fill a template.

# Adapt mode

Apply only an approved audit/plan. Prefer the smallest context system that solves demonstrated problems.

## AGENTS.md

`AGENTS.md` is the canonical entry point. Keep it short and route-first.

A compact table is appropriate for a small number of stable subjects:

```text
Task / subject    Go to          Read next
domain behavior   src/domain/    src/domain/CONTEXT.md
persistence       src/data/      src/data/CONTEXT.md
UI interaction    src/ui/        src/ui/CONTEXT.md
deployment        infra/         infra/CONTEXT.md
```

Route to subjects, not every file. If routing no longer fits comfortably in a small entry file, move detailed routing to root `CONTEXT.md`.

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

## Current work

Reuse the existing authoritative system. Create `STATE.md` only when active work otherwise lacks a durable handoff source and the user approves it.

## ROADMAP.md

Do not add by default. Use only when the repository genuinely benefits from a durable phased plan.

## README

Treat README as human-facing onboarding. Do not duplicate the agent routing system into it; link to `AGENTS.md` where useful.

## Mechanical enforcement

When a critical invariant is cheap to enforce, prefer a deterministic check such as an import-boundary rule, architecture test, schema validation, or generated-file check. Do not add a toolchain for a minor convention.

# Maintain mode

Check for:

- broken/deleted paths;
- routes pointing to moved responsibilities;
- root context duplicating local context;
- obsolete current-work state;
- historical plans mistaken for current requirements;
- new subjects with no route;
- changed invariants;
- new context hotspots;
- vendor-specific instructions conflicting with `AGENTS.md`;
- stale Repomix outputs being treated as truth.

Prefer deleting stale context or replacing it with a pointer over adding another layer.

# Refactoring policy

Agentify may recommend code refactoring when repository structure creates substantial agent cost, but never refactor application architecture solely to improve an audit score.

Refactoring may be justified by repeated unrelated churn, unclear ownership causing mis-edits, broad context required for narrow changes, boundaries that cannot be described honestly, or an imminent feature that will worsen a hotspot.

Require human approval.

# Human approval boundaries

Audit is read-only.

Before repository changes, present intended changes unless the user already approved a specific plan.

Never make these incidentally without explicit approval:

- architectural refactors;
- moving application code across boundaries;
- deleting historical material;
- replacing the current-work system;
- adding persistent dependencies/infrastructure;
- changing database schemas or deployment behavior;
- introducing vendor-specific agent configuration.

# Validation after adaptation

Validate in proportion to the change.

Always verify:

- routed paths exist;
- named commands are real;
- current-work pointers resolve;
- generated/historical files are labelled correctly;
- new context does not duplicate another authoritative source.

If code/configuration/build behavior changed, run relevant lint, typecheck, tests, and build commands.

For documentation-only changes, do not run an expensive full suite merely for ceremony unless requested or required to verify a claim.

Report what was and was not run.

# Success criterion

A repository is sufficiently agent-ready when a capable coding agent can start from a small canonical entry file, follow explicit pointers to the relevant subject, load only local rules and necessary implementation evidence, respect critical invariants, and validate the result without reconstructing the whole repository.
