# Leads and deals mode

Use this mode when the user wants to work records rather than configure the CRM, for example:
- add a lead;
- create a deal;
- convert leads;
- enter leads from a spreadsheet;
- move a deal;
- mark a deal Won or Lost;
- add a note, follow-up, meeting, file or watcher;
- link a quote or project.

Each change is listed in a short **change list** and saved only after the user approves it.

The mode runs in five phases:

1. **Connect** — the same as Phase 1 in `SKILL.md`.
2. **Check** — read-only checks: access, the records as they are today, and duplicates.
3. **Gather** — collect every field the change needs, with sensible defaults.
4. **Approve** — show the change list and get an explicit yes.
5. **Do and verify** — make the approved changes one by one, check each, and report.

Recipes are in `records-ui-map.md`. Batches from a spreadsheet are in `bulk-leads.md`.

## Guardrails for this mode

These replace setup guardrail 4 ("do not touch working data") for this mode only. Every other guardrail in `SKILL.md` still applies.

1. **Nothing is saved before the change list is approved.** Phases 2–3 are read-only. That means not clicking any of these:
   - Save, Create Deal, Convert to Deals, Confirm
   - Mark as Won, Mark as Lost
   - Archive, Restore
   - Save note, Schedule meeting
2. **Only the approved changes are made.** If something new is needed mid-run, such as a customer that doesn't exist or a missing label, stop and ask. Each extra change needs its own yes.
3. **No deleting.** Archive and Restore only the records the user asked for. Never use any delete control.
4. **Outcomes need their own yes, with the consequences stated:**
   - **Mark as Lost** cancels the deal's quotes. Existing invoices stay as they are. A reason is required.
   - **Mark as Won** on a quote-connected pipeline asks which signed quote won it.
   - **Reopen deal** and **Restore** move a record back into the active lists.
5. **The CRM configuration is off limits in this mode.** That covers pipelines, stages, automations, labels, sources, roles and settings. If something is needed, such as a new label, either switch to setup mode or create it only after a separate yes, using the setup recipe.
6. **Quotes:** only create, link or view quotes from the deal page, and only when asked. Never edit, send, accept or cancel a quote directly.
7. **Customers:**
   - Always search for an existing customer first, and show the user any matches.
   - Create a new customer (the **NEW** badge) only when the user says so.
   - Show any "Possible duplicates" warning to the user.
8. **Personal data:** enter only what the user gave in chat or in the file they pointed to. Never invent emails, phone numbers or addresses.
9. **Automations may react.** Creating or moving a deal can start that stage's automations if they are On. Say so in the change list (see Phase 2).

## Phase 2 — Check (read-only)

- **Access.** The sidebar shows **CRM** with Dashboard, Deals and Leads, and `<base>/crm/deals` doesn't send you back to `<base>`. If either fails, stop and explain, as in preflight check 2.
- **What the user can see.**
  - Without **Can See All Sales Data**, the user only sees records they own, watch, or that are visible to all.
  - Without **Can See Costs**, values and margins on other people's records show as "—".
  - Mention either one if it explains a "missing" record.
- **Pipelines and stages.**
  - Read the pipeline dropdown on `<base>/crm/deals` and the lanes of the target pipeline.
  - On a quote-connected pipeline, new deals can only start in Qualified or in a custom stage in the Qualified segment.
- **Automations that would react.** Open `<base>/crm/automations/<pipelineId>?view=map`. Note which stages touched by the change have automations **On**, and list them in the change list.
- **Labels, sources, interests and users.** Open the Create Lead modal, read the options, then click **Cancel**. Offer only these values.
- **The records themselves.** Find the lead or deal by search. If there are several matches, show them and ask which one.

## Phase 3 — Gather

Ask only for what's missing. Use these defaults:
- **Owner:** the user. Create Deal defaults to "Me". Create Lead defaults to Unassigned, so set the owner unless told otherwise.
- **Currency:** the organisation's default.
- **Lead title:** the customer or contact name when none is given.
- **Pipeline and stage on convert:** the only pipeline, if there's just one, and its entry stage (Qualified).
- **Visible to all users:** off.

Check each value as you collect it:
- **Customer:** search first (guardrail 7). A deal needs a customer; a lead doesn't.
- **Labels, sources, interests and owners:** they must already exist (see Phase 2). If one doesn't, stop and ask (guardrail 5).
- **Stage moves:**
  - On a quote-connected pipeline, the protected stages follow the quotes.
  - A lane that says "Deals arrive here from their quotes" can't take a deal moved by hand.
  - For Won or Lost, use the deal actions instead.
- **Lost reason:** required, up to 255 characters.
- **Files:** PDFs, documents, spreadsheets and images up to 20 MB, from a path the user gave.

## Phase 4 — Approve

Show the change list:

```
Changes — <Org name> (<host>/<org>)
| # | Record | Change | Details |
|---|--------|--------|---------|
| 1 | Lead "John Smith" | CREATE | Customer: Acme Ltd (existing) · Contact: John Smith (primary) · £5,000 GBP · Source: Website · Owner: Sam Lee |
| 2 | Lead "John Smith" | CONVERT | → Deal in Sales Pipeline › Qualified |
⚠️ Automations On at Qualified: "Give it an owner…" will run for this deal.
```

Ask: **Approve and run** / **Change something** / **Cancel**. For more than 10 records, follow `bulk-leads.md`.

## Phase 5 — Do and verify

Work through the list **in order**. For each item:
1. **Re-check** that it isn't already done. The lead may already exist, or the deal may already be in that stage. If so, mark it ⏭ SKIP.
2. **Do it** using the recipe in `records-ui-map.md`, with the exact labels.
3. **Verify** that the record shows with the right values and that no error toast appeared.
4. **Report progress** in chat: ✅ done / ⏭ skipped / ❌ failed.

If a step fails, stop and ask: *Retry* / *Skip this item and continue* / *Stop here*. Never leave a modal open.

End with a final report:
- a table of each item and its result;
- a link to each record: `<base>/crm/deals/<id>` for a deal, `<base>/crm/leads?lead=<id>` for a lead;
- any automation that will now run.
