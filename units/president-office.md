# President Office — automations index

This is the index of automations owned by the **President Office** unit of KSE, published as separate repositories under the [`KSE-codebase`](https://github.com/KSE-codebase) organization. Each automation is its own repo so it can be owned, forked, and run independently.

> This file is a human-readable map of the unit. On GitHub the unit itself is a **Team** ([`president-office`](https://github.com/orgs/KSE-codebase/teams/president-office)) — membership grants access to these repos.

## Repositories

| Repo | What it does | Stack |
|---|---|---|
| [`comms-scrum-bot`](https://github.com/KSE-codebase/comms-scrum-bot) | Telegram reminders for the Comms team (approval queue, publish times, daily roles, days-off) from Notion Schedule + Content | Google Apps Script |
| [`comms-performance-stats`](https://github.com/KSE-codebase/comms-performance-stats) | Daily per-person post counts → Notion Comms Performance Log | Google Apps Script |
| [`comms-analytics`](https://github.com/KSE-codebase/comms-analytics) | Comms efficiency: match published X/FB posts to Notion authors + engagement | Python (pandas/sklearn) |
| [`notion-digest-agents`](https://github.com/KSE-codebase/notion-digest-agents) | Prompt set for the Notion AI agents that build the daily news digest | Notion AI (prompts) |
| [`moodle-to-notion`](https://github.com/KSE-codebase/moodle-to-notion) | Moodle LMS teaching evidence → Notion | Python + GitHub Actions |
| [`rada-to-notion`](https://github.com/KSE-codebase/rada-to-notion) | Rada TSK stenographic records → Notion | Python + GitHub Actions |
| [`sheets-to-notion`](https://github.com/KSE-codebase/sheets-to-notion) | Schema-driven Google Sheets → Notion | Python + GitHub Actions |
| [`drive-inquiries-to-notion`](https://github.com/KSE-codebase/drive-inquiries-to-notion) | Drive inquiry files → Notion Requests dashboard | Python + GitHub Actions |
| [`gmail-inquiries-to-notion`](https://github.com/KSE-codebase/gmail-inquiries-to-notion) | Gmail inquiries → Notion Requests dashboard | Python + GitHub Actions |
| [`sebastian-transcriber`](https://github.com/KSE-codebase/sebastian-transcriber) | Telegram bot: audio/video transcription, YouTube subtitles, file metadata/OSINT | Python (Telegram bot) |

The `*-to-notion` scrapers are the split of the original monorepo `asobkiv/kse-notion-sync` — one repo per scraper, each with its own README, workflow, and `.env.example`.

## Common threads
- **Everything flows into Notion.** These automations feed the President Office's Notion workspace (Comms content, teaching evidence, crisis-topic documents, inquiry intake).
- **No servers.** Apps Script triggers or GitHub Actions cron.
- **No secrets in code.** Script Properties (Apps Script) or GitHub Secrets (Actions). Every repo ships an `.env.example` or a Script-Properties table.

See the org-level [`STRUCTURE.md`](../STRUCTURE.md) for naming, teams, access, and how to add a new automation.
