# Targeted Job Search

A personal daily job-search automation pipeline: pulls open roles from a configurable list of target companies, scores them against a resume/niche, ingests networking leads from a human-supervised Claude in Chrome sourcing session, and emails a ranked daily digest.

See `/Users/aaryanpurohit/.claude/plans/pasted-content-id-1737-i-want-recursive-sonnet.md` for the full architecture, schema, and design rationale.

## Setup

1. `python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt && playwright install chromium`
2. `cp .env.example .env` and fill in `ANTHROPIC_API_KEY`, `GMAIL_APP_PASSWORD`.
3. Edit `config/companies.yaml` to add target companies.
4. `python scripts/run_daily.py` to run the pipeline once; schedule it with cron for daily runs.
5. `python scripts/mark_status.py <job_id> applied|dismissed` to update a job's status; `--list-pending` to see open ones.

## Project layout

- `config/` — companies and matching criteria (user-editable)
- `data/` — SQLite store, resume text, Chrome-agent lead drop zone
- `src/jobsearch/` — collectors, filters, agents, store, email, orchestrator
- `scripts/` — CLI entrypoints
- `tests/` — pytest suite (fixtures, no live network/LLM calls)
- `docs/` — Chrome-agent sourcing prompt template
