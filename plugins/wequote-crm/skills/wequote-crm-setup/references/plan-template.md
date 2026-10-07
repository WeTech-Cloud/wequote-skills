# Flow plan format

Show the plan in chat as markdown, using exactly the format below. Save the same content to `crm-setup-plan-<org>-<YYYY-MM-DD>.md` with a **Status** column added, which you update during execution.

Tags:
- **CREATE**: a new item.
- **UPDATE**: a change to an existing item, with its old → new value.
- **SKIP**: already exists, or the admin chose to leave it.
- **EMAIL**: sends an email, such as an invite.

````markdown
# CRM setup plan — <Org name> (<host>/<org>)
Prepared <date> for <admin name>. Nothing has been changed yet.

## Summary
- 1 company setting · 2 roles · 3 users · 2 labels · 1 source + 2 contacts · 1 pipeline · 3 stages · 4 automations
- ⚠️ Changes apply to everyone immediately.

## Steps (in this order)

| # | Area | Action | What | Where |
|---|------|--------|------|-------|
| 1 | Company | UPDATE | Use quote review: Off → **On**, Reviewer Email **reviews@acme.com** | Settings → Company |
| 2 | Roles | CREATE | **CRM Manager** — WeQuote, Manage Users ✗, Can See Costs ✓, Can See All Sales Data ✓, Access CRM ✓, Manage Configurations ✓ (+ same quote permissions as "Manager") | Settings → Users → User Roles |
| 3 | Users | UPDATE | Sam Lee: Manager → **CRM Manager** | Settings → Users |
| 4 | Users | EMAIL | Invite **new@acme.com** as **Sales** | Settings → Users |
| 5 | Labels | CREATE | CRM label **Repeat customer** (#1E8539) | Settings → Configure → Labels |
| 6 | Sources | CREATE | Source **Builders' merchant** | CRM → Leads → Lead sources |
| 7 | Sources | CREATE | Contact **Mark Bennett**, Bennett Ltd, mark@… → Builders' merchant | same |
| 8 | Pipelines | SKIP | **Sales Pipeline** (Quotes) — kept as is | – |
| 9 | Stages | CREATE | Sales Pipeline: **Site Visit** — after Qualified · 20% · Flag · #B97A00 | CRM → Deals → Manage pipeline stages |
| 10 | Automations | CREATE | See A1–A4 below | CRM → Automations |

## Resulting pipelines

**Sales Pipeline** (Quotes):
Qualified 🔒 → **Site Visit** (new) → In Progress 🔒 → ~~In Review 🔒 → Passed Review 🔒~~ (hidden, quote review off) → Sent 🔒 → Won 🔒 → Invoicing 🔒 / Lost 🔒

## Automations

### A1 · Sales Pipeline › Qualified — "Owner and site info" (template)
Runs again: company default · Scope: all companies · **Publish, leave it off**
```
Runs on Qualified
├─ Assign the deal to a person → Sam Lee
└─ Request a file → "Site photos and measurements" (required)
```

### A2 · Sales Pipeline › Site Visit — "Book the visit" (custom)
Runs again: Only when the deal moves forward · **Publish and turn on**
```
Runs on Site Visit
└─ Rule: Does this deal need a site visit?
   ├─ Yes → Schedule a meeting "Site visit" · On site · in 2 working days at 09:00 · wait until it has happened
   │        └─ Move the deal to a stage → (none valid — ✋ resolve before approval)
   └─ No  → Create a note "No visit needed — quote from the brief"
```

### A3 …

## Warnings
- Turning an automation on affects only deals that arrive after that point. The 12 deals already in Qualified are not touched.
- A2's move has no valid target. Choose a later custom stage in the Qualified segment, or drop the move.
- Automations run on a ~15-minute schedule.

## Not in this plan (do it yourself if wanted)
- Removing the default lead source "Social".
````

Rules for the plan:
- Number steps in dependency order. A step must never refer to something created by a later step.
- Resolve every ✋ before you ask for approval. You can't approve a plan that still has an unresolved ✋.
- Keep values exact (names, emails, colours, percentages). The executor copies them verbatim.
- The approval question comes straight after the plan: **Approve and run** / **Change something** / **Save the plan and stop**.
