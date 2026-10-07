# Preflight checks (read-only)

Run every check before the interview. Nothing in this file clicks a save, create or toggle control. Opening a modal just to read it is fine, but close it again with **Cancel**.

`<base>` means `https://<host>/<org>` as confirmed in Phase 1.

## 1. Logged in
- **How:** the current URL is not under `/auth/`, and the page shows the app sidebar.
- **⛔ if not:** ask the admin to log in themselves, then re-check. Never type credentials.

## 2. CRM is on for this organisation and this user
- **How:**
  - The sidebar has a **CRM** section with Dashboard, Leads and Deals.
  - Navigating to `<base>/crm/dashboard` stays there. It does not bounce back to `<base>`.
- **⛔ if not:** either the organisation doesn't have the CRM feature, or the admin's role lacks **Access CRM**.
  - In Settings → Users → **User Roles** tab, look at the admin's role. If its CRM row shows **Access CRM** as a red badge, the role is the problem: green badges are granted, red ones are not. If the CRM row is missing entirely, the organisation doesn't have the CRM. Someone with Manage Users can fix the role.
  - Otherwise the CRM isn't enabled for the organisation. That is done by **WeQuote support**, not in the app.
  - Stop the run either way.

## 3. Manage CRM Configurations
- **How:**
  - The sidebar's CRM section shows **Automations**.
  - On `<base>/crm/deals` (Pipeline view) there's a pencil button with the tooltip **"Manage pipeline stages"**.
  - Root users see both regardless of role.
- **⛔ if not:** the admin's role needs **Manage Configurations**, which sits under CRM in the role. Stop the run, because pipelines, stages and automations all need it.

## 4. Manage Users (only if roles/users are in scope)
- **How:** go to `<base>/settings/user` and open the **User Roles** tab. The **Add User Role** button must be visible.
- **⚠️ if not:** remove "Roles & users" from scope, tell the admin, and carry on.
- **Also:** the admin's own role shows a **My Role** badge and can't be edited from their own account. Built-in roles show no edit pencil. Note both.

## 5. Company CRM settings
- **How:** open `<base>/settings/company` and read:
  - **CRM automations**: the select showing the current default. Values: Every time the deal arrives / Once only, ever / Only when the deal moves forward.
  - **Quote review**: is **Use quote review** ticked? If it is, read **Reviewer Email**.
- **⚠️ if the CRM automations block is missing:** this is the same root cause as check 2.

## 6. Companies (for automation company scope)
- **How:** open `<base>/settings/secondary`.
- If there is only one company, the automation **Company scope** card doesn't appear. Skip scope questions.

## 7. Inventory snapshot

Record each of these in a compact list. You'll need it for the interview, the plan's SKIP detection, and the final diff.

| What | Where | Read |
|---|---|---|
| Pipelines | `<base>/crm/automations`, the hub table. It lists every pipeline with its lifecycle and automation count. If there is only one pipeline, it jumps straight into it. In that case read the pipeline dropdown on `<base>/crm/deals`. | Name, "Connected to quotes" or "Standalone" |
| Stages | `<base>/crm/deals?pipeline=<id>`, the board columns. On quote-connected pipelines, In Review and Passed Review are hidden when quote review is off. | Order, name, 🔒 protected or not, probability. For custom stages, their segment is shown under the lane. |
| Lead sources and contacts | `<base>/crm/leads`, **Lead sources** tab. The left rail lists the sources, and the right panel lists contacts. | Source names, contact name/company/email |
| CRM labels | `<base>/settings/configure/labels`, the **CRM Labels** card | Text, colour |
| Quote labels | same page, the quote labels card | Text |
| Interests | the Interests picker in **Create Lead** (open it, read it, then Cancel). Interests are the organisation's Systems and are not created here. | Names |
| Users | `<base>/settings/user`, **Users** tab | Name, role, root/elevated badges |
| Roles | the **User Roles** tab | Name, built-in or not (built-in roles have no edit pencil), CRM row badges (Access CRM / Manage Configurations / Can See All Sales Data). Green means granted, red means not. |
| Automations | `<base>/crm/automations/<pipelineId>`, the list view grouped by stage. Optionally **Open Pipeline Map** for On/Off. | Name, stage, readiness (Needs setup / Unpublished changes / Published), On/Off |

## 8. Clean starting state
- **How:** look for open modals and editors showing "Unsaved changes". Also look for a Deals board in stage-manage mode, which has a floating bar with **Save changes**.
- **⚠️ if found:** the admin may have unsaved work. Ask before cancelling anything.

## Output

```
Preflight — <org name> (<host>)
| Check                         | Result | Notes |
|-------------------------------|--------|-------|
| Logged in                     | ✅ | Jane Smith |
| CRM enabled (org + role)      | ✅ | |
| Manage CRM Configurations     | ✅ | |
| Manage Users                  | ⚠️ | Not on this role — roles & users left out |
| Company settings              | ✅ | Quote review OFF · re-run: Every time the deal arrives |
| Companies                     | ✅ | 1 company — no scope questions |
| Clean state                   | ✅ | |
```
Follow the table with the inventory summary.
