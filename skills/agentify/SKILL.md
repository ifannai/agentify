---
name: agentify
description: Audit, adapt, and maintain software repositories for safe, efficient coding-agent work. Use when asked to make a repository agent-friendly, improve coding-agent context, create or review AGENTS.md, STATE.md, ROADMAP.md, or CONTEXT.md files, audit repository structure for coding agents, reduce agent context cost, clarify architectural boundaries, or check whether agent-facing documentation has drifted from the codebase.
---

---

# Agentify

Adapt existing software repositories so coding agents can work safely with minimal relevant context and clear architectural boundaries.

The objective is not to generate documentation for its own sake.

The objective is to reduce the amount of unrelated repository context an agent must consume before making a safe, narrow change.

An agent entering the repository should be able to answer cheaply:

1. What am I doing?
2. Where does this change belong?
3. What must I not break?
4. How do I know I am finished?
5. What should I read, and what should I explicitly not read?

The preferred context path is:

```text
AGENTS.md
    ↓
STATE.md or existing current-task source
    ↓
relevant subsystem CONTEXT.md
    ↓
minimal relevant source files
    ↓
minimal relevant tests
```

Not every repository needs every layer.

---

# Core principle

Adapt agent context to the existing architecture first.

Do not redesign application architecture merely to produce cleaner agent documentation.

Agentification may reveal architectural problems, but architectural refactoring requires human approval.

---

# Modes

The skill has three modes:

- `audit`
- `adapt`
- `maintain`

## Mode selection

Interpret explicit mode names first.

Natural-language requests map as follows.

### Audit

Use `audit` for requests such as:

- audit this repo for coding agents;
- review agent readiness;
- is this repository agent-friendly?;
- assess the repo structure;
- identify context problems;
- review AGENTS.md or subsystem boundaries;
- tell me what should change before agents work here.

### Adapt

Use `adapt` for requests such as:

- agentify this repository;
- set this repo up for coding agents;
- apply the approved audit;
- create the agent-facing repository structure;
- create or update AGENTS.md, STATE.md, ROADMAP.md, or CONTEXT.md based on the audit.

If adaptation is requested but no previous audit or approved plan exists, perform `audit` first and stop for human review before modifying the repository.

### Maintain

Use `maintain` for requests such as:

- check whether the agent docs are stale;
- review agent-readiness after recent development;
- check routing or context drift;
- audit whether AGENTS.md or CONTEXT.md still match the code;
- perform an agent-readiness maintenance review.

Maintain mode is human-invoked.

Do not autonomously enter maintain mode during unrelated implementation work.

If the user's intent is ambiguous and modification may be involved, default to `audit`.

---

# Context model

Distinguish three forms of context.

## Durable context

Stable rules and architectural knowledge.

Examples:

- `AGENTS.md`
- subsystem `CONTEXT.md`
- architecture rules
- invariants
- dependency boundaries
- security constraints
- validation conventions

## Current context

Information about active work.

Examples:

- current task
- active scope
- expected files
- blockers
- acceptance criteria
- immediate backlog

This may live in:

- `STATE.md`
- an issue
- a PR
- a project-management file
- another clearly authoritative system

Do not create duplicate current-state systems.

## Historical context

Examples:

- Git history
- completed PRs
- old issues
- implementation diaries
- past architectural discussions

Agents should normally consume durable + current context first.

Historical context should be consulted only when necessary.

---

# Progressive inspection

Inspect progressively.

Once this skill has been loaded, do not read `SKILL.md` from disk again unless the user explicitly asks to inspect or debug the skill itself.

Prefer low-cost structural evidence before source contents:

- file and directory names;
- package/build configuration;
- imports and dependency relationships;
- targeted searches;
- representative files.

Do not dump broad directories or large groups of source files into context merely for completeness.

Do not recursively read the entire repository merely for completeness.

Start with:

1. repository tree;
2. existing agent/developer documentation;
3. package/build configuration;
4. test configuration;
5. CI configuration where relevant;
6. obvious application entry points;
7. obvious architectural directories.

Form an initial architecture hypothesis before opening broad source areas.

Before substantially expanding inspection, identify the specific unanswered question that requires more context.

Do not broaden repository reading without a concrete hypothesis to confirm or reject.

Then inspect only the files necessary to confirm or reject that hypothesis.

Expand into additional source areas only when needed.

Prefer representative files over exhaustive reading.

Do not inspect Git history by default.

Use commits, file-touch frequency, PR history, or blame only when the current working tree cannot answer a relevant audit question, or when testing a specific hypothesis such as repeated churn, ownership ambiguity, or concentrated merge conflicts.

Historical evidence should answer a specific question, not serve as general repository context.

Examples:

- inspect one or two typical components before reading every component;
- inspect the canonical repository/data layer before every data-access file;
- inspect architectural imports before reading implementation detail;
- inspect tests that reveal intended boundaries before broad source exploration.

The goal is to understand the repository sufficiently to route future work, not to memorize the repository.

#### Audit context budget

Treat context consumption as part of the audit's own quality.

A repository audit intended to reduce agent context cost should itself avoid unnecessary context consumption.

Prefer:

structure → hypothesis → targeted evidence → conclusion

over:

broad reading → accumulated context → conclusion

# Audit mode

Audit mode is read-only.

Do not modify files.

## Inspect

Examine enough of the repository to understand:

- repository structure;
- existing `AGENTS.md`, `STATE.md`, `ROADMAP.md`, `CONTEXT.md`, or equivalents;
- README and developer documentation where relevant;
- package/build configuration;
- test structure;
- lint/typecheck/build commands;
- CI configuration where relevant;
- architectural/module boundaries;
- generated files;
- configuration boundaries;
- security-sensitive boundaries;
- major dependency directions;
- current-task tracking if present.

During audit, identify available validation commands but do not automatically run the complete validation suite.

Run validation during audit only when:

- the user asks for it;
- current repository health is relevant to an audit finding;
- documentation claims cannot otherwise be verified;
- a lightweight check is needed to establish the baseline.

Full validation is normally required after `adapt` or approved maintenance changes.

## Assess

### Navigation

Can an agent locate the correct subsystem without reading most of the repository?

Look for:

- meaningful directory structure;
- clear entry points;
- naming consistency;
- routing documentation;
- obvious subsystem ownership.

### Boundaries

Can the agent understand what different areas own?

Examples:

- UI;
- domain;
- data;
- persistence;
- API;
- backend;
- infrastructure;
- styles;
- configuration.

Look for unclear or accidental cross-boundary imports.

### Current state

Can the next agent determine where active work stopped and what comes next?

Check whether the repository already uses:

- issues;
- PR descriptions;
- project boards;
- task files;
- planning documents;
- `STATE.md`;
- another clear authoritative mechanism.

Do not recommend `STATE.md` merely because this skill knows about `STATE.md`.

### Validation

Can an agent determine exactly how correctness is checked?

Identify applicable commands for:

- tests;
- lint;
- type checking;
- architecture checks;
- build;
- formatting;
- migrations where relevant.

Prefer directly executable commands.

### Invariants

Identify dangerous or non-obvious rules whose violation could cause subtle bugs.

Examples:

- domain code must remain framework-independent;
- UI must not access persistence directly;
- server-only configuration must never reach the client;
- generated files must not be edited manually;
- a particular date/serialization pattern must not be used;
- only approved modules may import a specific dependency.

Determine whether these rules are:

- mechanically enforced;
- documented;
- implicit only.

### Context cost

Ask:

> How much unrelated context must an agent consume to make one safe narrow change?

Do not use line count alone as the decision rule.

A file becomes a context hotspot when one or more are true:

- unrelated features repeatedly modify it;
- narrow changes require reading most of it;
- merge conflicts concentrate there;
- it contains multiple independently describable responsibilities;
- its tests require unrelated fixtures or setup;
- its ownership is difficult to explain simply;
- changes routinely require understanding distant code.

A large file may be acceptable.

A much smaller file may still be agent-hostile.

### Coupling

Look for:

- unrelated changes repeatedly touching the same module;
- feature logic concentrated in application entry points;
- UI/data/domain responsibilities mixed together;
- generic modules accumulating unrelated behavior;
- central files acting
