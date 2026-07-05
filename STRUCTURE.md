# KSE-codebase — structure & conventions

How the shared code space is organized. Read this before creating a repo or adding a unit.

## The model

GitHub has **no nested folders of repositories**. A "unit" is therefore expressed with a **GitHub Team** — the single source of truth for which unit a repo belongs to and who can access it. Adding a person to a team grants them access to that unit's repositories; a repo belongs to a unit by being assigned to that unit's team.

Each **automation / project is its own repository** ("micro-repo"), owned by the relevant person, who has full rights to change it. Repos can be public within the org, private, or restricted per project.

```
KSE-codebase (organization)
├─ Team: president-office   → this unit's repos
├─ Team: business-school
├─ Team: university
├─ Team: proftech
├─ Team: kse-institute
└─ Team: charity-fund
```

Browse units and their repos at **[Teams](https://github.com/orgs/KSE-codebase/teams)**.

## Naming convention

`<what-it-does>`, lowercase, hyphenated, source→destination when it's a pipeline. **No unit prefix** — the team says which unit it belongs to.
Examples: `moodle-to-notion`, `comms-scrum-bot`, `crm-to-sheets`.

> If two units ever need a repo with the same generic name, disambiguate then (e.g. a short suffix) — don't pre-emptively prefix everything.

## What every repo should have

- A **README** that a newcomer can follow end-to-end: what it does, the use case, setup, secrets, how to test.
- **No secrets in code.** GitHub Secrets (Actions) or Apps Script Script Properties. Ship an `.env.example` / a Script-Properties table instead.
- A `workflow_dispatch` trigger on scheduled Actions so anyone can run it manually to test.
- Clear **ownership** — the person who maintains it is the repo admin.

## Access & the "our domain only" requirement

The goal "only people with our domain can access" is an **org-level, paid-plan** capability, not something set per repo:

- **GitHub Team or Enterprise plan** is required for SAML SSO / verified-domain enforcement and to make private repos + fine-grained team permissions practical at scale.
- Configure it once at **Org → Settings → Authentication security** (SSO) and **Member privileges**.
- Day-to-day access is then granted by adding people to the right **Team**; the team's repos inherit that access.
- Individual repos can still be made **private** or given custom collaborator lists for anything sensitive.

> On the free plan you can have private repos and invite people individually, but you cannot enforce "must belong to our domain." Flag this to whoever owns the KSE GitHub billing.

## Adding a new unit
1. Create a **Team** in the org.
2. Add an index page (like `units/president-office.md`) listing the unit's repos.

## Adding a new automation
1. Create a repo `<name>` in `KSE-codebase`.
2. Assign it to the unit's Team; set the maintainer as admin.
3. Add README + `.env.example`, put secrets in GitHub Secrets, enable the workflow.
4. Add a row to the unit's index page.
