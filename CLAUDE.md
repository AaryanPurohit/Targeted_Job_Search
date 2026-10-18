# Targeted Job Search — Project Instructions

## Working style

- **Launch subagents for substantial or parallelizable work.** When a task spans multiple independent modules (e.g. building several collectors, or a module plus its tests), use the Agent tool to parallelize rather than doing it all serially in the main context. Keep the main context focused on integration, review, and decisions — not raw implementation output from every file.
- Keep modules lean: a component is a Claude Agent SDK agent only when it does genuine unstructured-text reasoning (see the Scoring Agent, Career-Page Extraction Agent, and Orchestrator Agent). Everything else — collectors, filters, SQLite I/O, email rendering/sending — is plain, deterministic, unit-tested Python.

## Git workflow

- Day-to-day work happens on `dev`. Commit as you go.
- When a feature or milestone is verified working, open a pull request `dev → main`. `main` is the known-good branch — never commit directly to it.
- Claude Code GitHub Actions runs an automated code review on every PR into `main` (`.github/workflows/claude-code-review.yml`). Findings post as inline PR comments; they don't block the merge — the decision stays with the repo owner.

## Review criteria

- Correctness bugs matter most: logic errors, broken edge cases (missing salary, empty leads, malformed Chrome-agent JSON, YOE parsing ambiguity), and anything that could cause `finish_run()` to silently skip sending the digest.
- No speculative abstractions: 4 collector types are 4 files and a dict, not a plugin-discovery framework. Don't flag this as a missing abstraction.
- Flag any place `status` suppression logic sneaks in — jobs must keep reappearing in the digest until explicitly marked `applied`/`dismissed` (see the plan's "no dedup/suppression" requirement). This is a frequent and easy regression to introduce by accident.

## Reference

The full architecture/design rationale lives in `/Users/aaryanpurohit/.claude/plans/pasted-content-id-1737-i-want-recursive-sonnet.md` (schema, config formats, agent prompts, diagrams, and the reasoning behind each infra choice).
