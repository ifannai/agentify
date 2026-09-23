# Repomix audit procedure

Load this reference when running `audit` or `maintain` and Repomix is available.

## Preferred inspection path

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

## Procedure

1. Read root `AGENTS.md` first if present.
2. Generate a fresh temporary Repomix snapshot of the current working tree.
3. Inspect summary, directory structure, metrics, and compressed representations.
4. Search the snapshot to test specific hypotheses.
5. Read full source directly only for files needed to resolve an unanswered question.

Never dump the entire Repomix output into model context merely because it exists.

Never treat repository-tracked `repomix-output.*` as authoritative unless freshness against current HEAD and working tree is verified. If the user supplies a snapshot, use it as evidence but verify freshness before relying on current paths or behavior.

Prefer temporary or ignored output. Example:

```bash
repomix . --compress --no-git-sort-by-changes \
  --output "${TMPDIR:-/tmp}/agentify-repomix.xml"
```

Do not include Git logs or diffs by default. Add them only to test a specific hypothesis about churn, recent changes, ownership, or drift.

If Repomix is unavailable, fall back to a repository tree, targeted search, and representative reads. Do not add a persistent project dependency merely to run it.

## Expansion rule

Before broadening inspection, identify the unanswered question and read only what can answer it.

Prefer:

```text
structure → hypothesis → targeted evidence → conclusion
```

over:

```text
broad reading → accumulated context → conclusion
```
