# Claude in Chrome — Social Sourcing Prompt Template

Run this as an interactive, human-supervised session in Claude in Chrome whenever you want to source LinkedIn/X leads for a target company. Not automated, not scheduled — you trigger it yourself.

Substitute `{COMPANY}` with the company's name exactly as it appears in `config/companies.yaml`.

---

```
You are helping me do manual, human-supervised social sourcing research for a
job search. I am looking for LinkedIn and X posts related to hiring, open
roles, or "looking for a job at {COMPANY}" style posts connected to {COMPANY}.

For each relevant post you find, extract:
- platform (linkedin or x)
- author_name
- author_profile_url
- post_url
- post_text (a representative excerpt, not necessarily the full post)
- related_company (always "{COMPANY}" for this session)
- contact_info (anything indicating how to reach the poster — open DMs, email
  mentioned, "comment below", etc. — or null if nothing)
- application_link (if the post links directly to a job posting, else null)

Only include posts that are clearly about {COMPANY} hiring, someone at
{COMPANY} discussing open roles, or someone explicitly job-seeking in a way
relevant to AI/ML/LLM engineering roles at {COMPANY}. Skip irrelevant posts.

When you're done browsing, assemble everything into a single JSON file matching
this exact schema and save it to:
  data/leads_inbox/{COMPANY}_<timestamp>.json

{
  "session_meta": {
    "captured_at": "<ISO 8601 timestamp of now>",
    "related_company": "{COMPANY}",
    "source_platform_hint": "linkedin"
  },
  "leads": [ { ...one object per lead, fields as above... } ]
}

Do not fabricate posts or details. If you find nothing relevant, still write
the file with an empty "leads" array so the pipeline has a record this session
ran. Do not attempt to log in, scrape automatically, or use any API — only
use what I can see and manually confirm as you browse with me.
```

---

The daily pipeline (`scripts/run_daily.py`) globs `data/leads_inbox/*.json` on its next run, upserts into the `leads` table (idempotent on `platform + post_url`), and moves the file to `data/leads_inbox/processed/`. It's safe to run this session on any cadence — more often than daily, or not for a week — the pipeline just picks up whatever's new.
