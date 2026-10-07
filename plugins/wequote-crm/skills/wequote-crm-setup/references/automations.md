# Automations

## The model in one paragraph

An automation belongs to **one stage of one pipeline** and runs when a deal **arrives** in that stage. There is no trigger to pick. It is a list of **steps**. A step is one of:
- an **Action**, which does something to the deal;
- a **Rule**, which checks something and branches into **Yes** and **No**;
- a **Wait**, which pauses for N hours or days;
- an **End**, which stops.

Its lifecycle is: **Save draft**, then **Publish changes**, then **turn it on**.
- Publishing does **not** turn it on. A first publish leaves it **Off**.
- Turning it on only affects deals that arrive afterwards.

Automations are processed on a schedule, roughly every 15 minutes.

## Rules (conditions)

| Rule (rail label) | Settings | Notes |
|---|---|---|
| **The deal has a label** | Label (one or more CRM labels) | Yes if any of them is on the deal |
| **The deal is worth** | Compares as (**is exactly / is at least / is no more than / is between**), Amount (+ **and** for between) | |
| **The deal's margin is** | same as above | |
| **Does this deal need a site visit?** | none | Asks on the deal and waits for someone to answer, then goes down Yes or No |
| **The deal's invoices are paid** | none | |

If a rule can't be answered, it goes down **No**. A rule needs at least one step on Yes or No.

## Actions

| Action (rail label) | Required | Optional |
|---|---|---|
| **Assign the deal to a person** | Person (one user) | – |
| **Create a note** | What the note says | Follow-up (**No follow-up / Follow up today / Follow up next working day / Follow up in 3 working days / Follow up in 7 working days**), Assign to (default **The deal's owner**), *Has to be finished before the deal moves on* |
| **Schedule a meeting** | Title | Assign to, Also invite (users), Book it in (days), Counted in (**working days / calendar days**), At (time, default 09:00), Kind of meeting (**On site / Video call / Phone**), Minutes, Where, *Wait until the meeting has happened*, *Has to be finished…* |
| **Request a file** | Title | What you need, in detail; Follow-up; Assign to. Always holds the deal until someone reviews the file. |
| **Add someone to watch this deal** / **Remove someone from watching this deal** | People (one or more users) | – |
| **Add a deal label** / **Remove a deal label** | Label (one or more CRM labels) | – |
| **Add a quote label** / **Remove a quote label** | Quote label (one or more) | Applies to every live quote on the deal |
| **Add an interest** | Interest (one or more) | – |
| **Move the deal to a stage** | Move the deal to (stage) | See the move rules below |
| **Don't notify about this deal** | none | Notifications to silence, Channels to silence (email / push / alert), Silence it for (owner / watchers). If all are left empty, it silences everything. |

**"Has to be finished before the deal moves on"** applies to Create a note, Schedule a meeting and Request a file. Open required work stops manual stage moves.

**Move the deal to a stage** targets must be:
- one of the organisation's **own** (unprotected) stages,
- in the **same pipeline**,
- **later** on the board than the automation's stage,
- and, on a quote-connected pipeline, in the **same lifecycle segment**.

If the stage dropdown is empty, the plan is wrong. Re-plan instead of forcing it. When a deal moves, any open work from this stage is marked as skipped.

## Wait

- Fields: an amount (≥ 1) and a unit (**hours / days**).
- A Wait **cannot be the last step** in a branch, so it must be followed by something.

## Guided templates

One template is offered per lifecycle stage. Custom stages get the template of the segment they sit in. A template arrives **Off**. Blanks marked "N to fill in" open the editor so you can fill them.

| Stage | Template | Blanks to fill |
|---|---|---|
| Qualified | Give it an owner and ask for what you need to quote: assign owner, then request "Site photos and measurements" (required) | Person |
| In Progress | Put a second pair of eyes on the big ones: if worth at least X, add a watcher and a note | Amount, People |
| In Review | Do not let a thin margin pass review quietly: if margin is under target, a required note explaining why | Amount |
| Passed Review | Talk the customer through it before it lands: a required meeting, "Walk the customer through the quote" | – |
| Sent | Chase it if the customer goes quiet: wait 3 days, then a required note with a next-working-day follow-up | – |
| Won | Hand the win over with what was promised: assign owner, then a required note about what was agreed | Person |
| Invoicing | Chase the payment, then hand the job on: wait 7 days; if invoices are paid, a hand-on note, otherwise a chase note due today | – |
| Lost | Record why it was lost: a required note, then add a label | Label |

The list above is a guide. Always read what the **template picker actually shows**, because templates can change.

## Recipe — create one automation

1. Go to `<base>/crm/automations/<pipelineId>`.
2. Start it one of two ways:
   - Click **Create automation**, or **Create your first automation** when the pipeline has none.
   - Or open **Open Pipeline Map** and click **Add automation** on the stage's lane. This skips the stage picker.
3. Dialog "How would you like to start?":
   - **Use the guided template** opens the template list for the stage. Click **Use this** on the planned template. If it has blanks, the editor opens; otherwise it's created as a draft and stays Off.
     - If you click **Build it myself instead**, that is the same as starting from scratch.
   - **Start from scratch** goes straight to the editor.
4. If asked "Choose a stage", click the planned stage. Stages captioned **Quote review is off** cannot be picked.
5. **Editor**, URL `.../automations/<pipelineId>/new?stage=<id>`:
   1. **Name**: the top input "Name this automation", max 100 characters. Required before saving.
   2. **Add steps**: click the **+** below "1 · Runs on <stage>", or the dashed "What should happen next?" box. The **Add next step** menu has **Action / Rule / Wait / End**.
      - *Wait* and *End* are inserted immediately. A Wait defaults to 3 days, so set the amount and unit.
      - *Action* / *Rule* switch the left panel to that tab. Then click the block by its rail label; use **Search workflow blocks** if needed. The block lands at the + you clicked and is selected.
   3. **Settings**: with a node selected, the left panel shows its settings. Fill the required ones first; nodes missing them show a **Needs setup** badge. Multi-pick fields are checkbox dropdowns, so click to open them, tick the items, and click outside to close.
   4. **Rule branches**: each rule node has **Yes** and **No** columns, each with its own **+**. Add steps under the right one.
   5. Click **← Workflow blocks** to return to the block list. **Remove step** deletes the selected node; use it only to fix your own mistakes in this run.
   6. **Right panel**:
      - **Runs again → Configure**: **Use the company default**, or a specific mode.
      - **Company scope → Configure**: shown only if the organisation has several companies. Options are **All companies / Only the companies I choose / Every company except those I choose**, then tick the companies.
      - **What it is for**: one line.
   7. Check the right panel:
      - **Before this can be published** must be absent. It lists blocking errors; fix each one.
      - **Worth knowing** contains warnings. Report them to the admin, but they don't block.
      - The green "Nothing is stopping this from being published." means it's ready.
6. Click **Save draft**. The status changes to "Draft saved".
7. If the plan says publish, click **Publish changes**. A dialog asks "… turn it on?":
   - The plan says *publish and turn on*: click **Turn it on**.
   - The plan says *publish only*: click **Leave it off**.
8. Leave the editor with the back chevron ("Exit editor"). If you get an unsaved-changes prompt, something wasn't saved, so go back and save.
9. Verify on the Pipeline Map:
   - The automation is on the right lane.
   - It shows **On** or **Off** as planned.
   - **View** shows the planned flow.

## Recipe — switch an existing automation on or off

On the **Pipeline Map**, each automation card has an **On**/**Off** button.
- Clicking it opens a confirmation showing how many deals are affected. Click **Turn it on** or **Turn it off**.
- An automation that has never been published can't be turned on.

## What blocks publishing (check during the interview)

- No name, or a name over 100 characters.
- No steps.
- A required setting is missing. An empty multi-pick counts as missing.
- A rule with nothing on either branch.
- A wait that is the last step, or has an amount under 1.
- Nesting deeper than 6 levels.
- A move target that breaks the move rules above.
- Company scope set to selected or except, with no company ticked.
- "There is nothing to publish": the draft is already the running version. That is fine; skip it.

## Quirks

- **End** is still offered in the Add next step menu. Only use it when the plan explicitly ends a branch early.
- Renaming, or changing scope, re-run or purpose, marks the automation unsaved but doesn't re-check errors. Make a step edit, or save, to refresh the panel.
- Dragging blocks from the left panel onto the canvas doesn't work. Use **+** and then click the block. Drag by a node's grip only to reorder.
- Review-stage automations (In Review / Passed Review) never run while quote review is off. The builder shows this as a warning.
