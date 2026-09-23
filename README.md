# agentify

A reusable agent skill for adapting existing software repositories so coding agents can work with less irrelevant context, clearer boundaries, and safer implementation workflows.

The goal is not to generate documentation for its own sake.

The goal is to make it cheap for an agent to answer:

1. What am I doing?
2. Where does this change belong?
3. What must I not break?
4. How do I know I am finished?
5. What should I read — and what should I explicitly not read?

## Skill

This repository contains:

```text
skills/
└── agentify/
    └── SKILL.md
```

`agentify` has three modes:

- `audit` — inspect repository agent-readiness without changing files
- `adapt` — create or update the smallest useful agent-oriented repository structure
- `maintain` — detect drift after the repository evolves

## Install

Check that the skill is discoverable:

```bash
npx skills add ifannai/agentify --list
```

Install the skill:

```bash
npx skills add ifannai/agentify --skill agentify
```

The `skills` CLI supports multiple coding agents and installs or links the skill into the appropriate location for the selected agent.

For a global installation:

```bash
npx skills add ifannai/agentify --skill agentify --global
```

You can also explicitly target one or more agents using the CLI's `--agent` option.

## Usage

Invoke the installed `agentify` skill using the skill mechanism provided by your coding agent.

Run it in one of three modes:

```text
agentify audit
agentify adapt
agentify maintain
```

Exact invocation syntax depends on the agent environment.

### `audit`

Use first on an existing repository.

It inspects architecture, documentation, validation, boundaries, context cost, and agent-readiness without modifying files.

Review the audit before approving changes.

### `adapt`

Run after approving the audit.

It may create or update:

```text
AGENTS.md
STATE.md
ROADMAP.md
subsystem/CONTEXT.md
architecture checks
```

Only the smallest useful structure should be added.

### `maintain`

Run after substantial repository evolution to detect:

- stale routing
- outdated context
- boundary drift
- new context hotspots
- duplicated responsibilities
- obsolete agent documentation

## Core model

Preferred context flow:

```text
AGENTS.md
    ↓
current operational state
    ↓
relevant subsystem CONTEXT.md
    ↓
minimal relevant source files
    ↓
minimal relevant tests
```

Not every repository needs every layer.

## Principles

- Adapt agent context to the existing architecture first.
- Do not redesign application architecture merely for cleaner agent documentation.
- Keep implementation tasks narrow.
- Minimize unrelated context consumption.
- Keep durable architectural context separate from current operational state.
- Do not duplicate an existing task or issue-tracking system unnecessarily.
- Treat file size as a signal, not an automatic refactoring trigger.
- Prefer executable architecture checks over prose when practical.
- Parallelize by architectural boundary, not simply by available agents.
- Require human review before meaningful architectural refactoring.

## Human workflow

```text
1. Run agentify in audit mode.
2. Review GOOD / WATCH / PROBLEM findings.
3. Approve or reject recommendations.
4. Run adapt.
5. Review the resulting diff and validation.
6. Commit the agent-facing structure.
7. Develop normally.
8. Run maintain after coherent development phases.
```

The skill identifies architectural friction; the human decides whether that friction justifies architectural change.
