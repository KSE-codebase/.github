# KSE-codebase — structure & conventions

How the shared code space is organized. Read this before creating a repo or adding a unit.

## The model

GitHub has **no nested folders of repositories**. A "unit" is therefore expressed with two mechanisms that together reproduce the "org → unit-folder → project" mental model:

1. **A GitHub Team per unit** — controls who can see and push (access & ownership).
2. **A repo-name prefix per unit** — groups repos visually in the org's repo list and makes ownership obvious at a glance.

Each **automation / project is its own repository** ("micro-repo"), owned by the relevant person, who has full rights to change it. Repos can be public within the org, private, or restricted per project.

```
KSE-codebase (organization)
├─ Team: president-office   → repos prefixed  po-
├─ Team: business-school    → repos prefixed  bs-
├─ Team: university         → repos prefixed  uni-
├─ Team: proftech           → repos prefixed  pt-
├─ Team: kse-institute      → repos prefixed  kic-
└─ Team: charity-fund       → repos prefixed  cf-
```

(Prefixes are a proposal — adjust to taste, but keep them short and stable.)

## Naming convention

`<unit-prefix>-<what-it-does>`, lowercase, hyphenated, source→destination when it's a pipeline.
Examples: `po-moodle-to-notion`, `po-comms-scrum-bot`, `bs-crm-to-sheets`.

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
2. Agree a short **prefix**.
3. Add an index README (like `president-office/README.md`) listing the unit's repos.

## Adding a new automation
1. Create a repo `<prefix>-<name>` in `KSE-codebase`.
2. Assign it to the unit's Team; set the maintainer as admin.
3. Add README + `.env.example`, put secrets in GitHub Secrets, enable the workflow.
4. Add a row to the unit's index README.
