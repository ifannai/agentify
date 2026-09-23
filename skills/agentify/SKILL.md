---
name: agentify
description: Audit, adapt, and maintain software repositories for safe, efficient, agent-agnostic coding-agent work. Use when asked to make a repository agent-friendly, reduce agent context cost, improve AGENTS.md routing, establish ROADMAP.md/STATE.md handoff, clarify boundaries, or check agent-facing context drift.
---

# Agentify

Adapt repositories so coding agents can make safe, narrow changes without reconstructing the whole codebase.

A new agent should be able to answer cheaply:

1. What am I doing?
2. Where does this change belong?
3. What must I not break?
4. What context do I load next?
5. How do I verify completion?

Agentify is agent-agnostic. Treat `AGENTS.md` as the canonical agent entry point.

# Core principles

- Adapt context to the existing architecture first.
- Route; do not preload.
- Prefer deterministic scripts for mechanical checks and agents for judgment.
- Do not redesign application architecture merely to improve agent documentation.
- Avoid redundant reads of guidance already loaded in the current run.

# Reference loading

Load detailed guidance only when needed:

- `references/operating-model.md` — ROADMAP.md, STATE.md, and shift lifecycle.
- `references/routing-patterns.md` — AGENTS.md, CONTEXT.md, layered routing.
- `references/repomix-audit.md` — Repomix-based progressive inspection.

# Modes

## Audit

Read-only assessment. Use for “audit agent readiness”, “is this repo agent-friendly?”, or equivalent.

Load:
- `routing-patterns.md`
- `operating-model.md`
- `repomix-audit.md` when Repomix is available

Inspect enough to understand:
- repository structure and entry points;
- agent/current-work documentation;
- package/build/test/CI configuration;
- architectural and dependency boundaries;
- generated/configuration/security-sensitive files;
- validation commands.

Do not run the full validation suite unless requested or needed to verify a finding.

Assess:
- navigation/routing;
- architectural boundaries;
- project operation and handoff;
- validation;
- invariants;
- context cost;
- coupling/duplication;
- generated/configuration/security boundaries;
- documentation drift.

### Audit output

Classify findings:
- `Good` — supports low-context safe work.
- `Watch` — workable but likely to create context/drift cost.
- `Problem` — likely to misroute an agent, hide a critical invariant, or force broad reading.

For each meaningful `Watch` or `Problem`, include evidence, agent impact, and the smallest useful correction. End with a prioritized adaptation plan.

## Adapt

Apply only an approved audit/plan. Prefer the smallest context system that solves demonstrated problems.

Load:
- `routing-patterns.md`
- `operating-model.md`

Create or update only the context files justified by the audit.

Do not duplicate an existing authoritative issue/PR/project workflow.

## Maintain

Human-invoked drift review. Do not enter automatically during unrelated implementation work.

Load the same references relevant to the area being checked.

Reuse the audit taxonomy: `Good`, `Watch`, `Problem`.

For each drift finding report:
- Status
- Drift
- Evidence
- Agent impact
- Smallest correction

Also state whether the canonical/current state changed since the last known good structure.

# Context types

- Durable — architecture, invariants, boundaries, security constraints, validation conventions.
- Current — active task and exact handoff.
- Planned — human-visible milestones and sequencing.
- Historical — old plans, completed PRs/issues, Git history, implementation diaries.

Historical context is consulted only to answer a specific unresolved question.

# Progressive inspection

Prefer low-cost structural evidence before broad source contents:
- tree and filenames;
- package/build/test/CI configuration;
- imports and dependency direction;
- targeted searches;
- representative files;
- tests that express intended boundaries.

Do not recursively read the repository for completeness.

Before broadening inspection, identify the unanswered question and read only what can answer it.

Do not inspect Git history by default.

# Refactoring policy

Agentify may recommend refactoring when repository structure creates substantial agent cost, but never refactor application architecture solely to improve an audit score.

Require human approval before architectural changes.

# Human approval boundaries

Audit is read-only.

Before repository changes, present intended changes unless the user already approved a specific plan.

Never make these incidentally without explicit approval:

## Architecture and data
- architectural refactors;
- moving application code across boundaries;
- replacing persistence or current-work systems;
- database schema changes.

## Operations
- adding persistent dependencies or infrastructure;
- changing deployment behavior;
- changing auth, secrets, or privileged configuration.

## Context and history
- deleting historical material;
- introducing vendor-specific agent configuration.

# Validation after adaptation

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

A repository is sufficiently agent-ready when the human can see and control project progress, a new coding shift can recover the exact handoff, and an agent can start from a small canonical entry point, load only relevant context, respect critical invariants, and validate the result without reconstructing the whole repository.
