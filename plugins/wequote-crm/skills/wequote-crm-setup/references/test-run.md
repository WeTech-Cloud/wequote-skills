# Test run

A test run checks that stage automations really fire. It uses **one** clearly named test deal, then archives it.

A test deal is working data. So a test run:
- happens only when the user chose it, as a step in an approved setup plan or on its own request;
- follows the guardrails in `records.md`.

## Before you start: tell the user

- The test deal is visible to anyone who can see all deals, until it's archived.
- **An automation only runs while it's On.** If the automations under test are Off, offer two choices:
  - turn them on for the test only, then off again afterwards;
  - test only the ones that are already On.

  While an automation is On, real deals that arrive in that stage trigger it too. On a busy live organisation, recommend testing on a test organisation, or at a quiet time.
- The automation runner works on a schedule, so each check can take up to about 15 minutes.
- Steps that assign the deal, add watchers, or create notes and meetings will notify the people they name.

## Plan

Put the test into the plan or change list as numbered steps, for example:

| # | Change | Details |
|---|--------|---------|
| T1 | CREATE deal | **TEST – CRM check <date>** · customer: one the user picks, or a new one called **TEST customer** · Sales Pipeline › Qualified · no value |
| T2 | Turn on | "Give it an owner…" (Qualified), only for the test |
| T3 | Check | Qualified automation ran |
| T4 | MOVE | → Site Visit (custom stage) |
| T5 | Mark as Lost | reason "Test run — not a real deal" (there are no quotes to cancel) |
| T6 | Check | Lost automation ran |
| T7 | Turn off | the automations turned on in T2 |
| T8 | ARCHIVE | the test deal |

Notes on which stages a test can reach:
- **Qualified, and custom stages in the Qualified segment:** a new deal can start there.
- **Other custom stages:** reach them with **Move deal**.
- **Lost:** reach it with **Mark as Lost**.
- **Quote stages (In Progress, In Review, Passed Review, Sent, Won, Invoicing):** these move only with real quotes. Don't create, send or accept quotes for a test. Report these stages as "not testable without a quote".

## Checking that an automation ran

1. Open the test deal: `<base>/crm/deals/<id>`.
2. In the activity panel's **History**, set the type filter to **Automation**.
3. Look for an entry naming the automation, plus its effects: the owner set, a note or file request under **Needs your attention**, labels added, watchers added.
4. If nothing shows yet, wait and re-check every few minutes, up to about 15 minutes. If it still hasn't run, report ❌ with what you saw. Don't retry by moving the deal back and forth; that can create duplicates.

A **Wait** step inside an automation delays its later steps. Report those steps as "waiting until <time>" rather than as failed.

## Cleaning up

- Turn off again anything you turned on for the test. Confirm each one on the Pipeline Map.
- Don't **Mark as done** any required work the test created. Archiving the deal is enough.
- Archive the test deal (card menu → **Archive deal**). If you created **TEST customer**, tell the user, since customers aren't archived from the CRM.

## Report

| Stage | Automation | Expected | Seen | Result |
|---|---|---|---|---|
| Qualified | Give it an owner… | Owner → Sam Lee; file request | Both, 6 min after arrival | ✅ |
| Sent | Chase it… | – | not testable without a quote | ⏭ |
