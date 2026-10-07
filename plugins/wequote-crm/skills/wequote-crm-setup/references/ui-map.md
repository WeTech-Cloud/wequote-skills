# UI map and recipes

`<base>` = `https://<host>/<org>`. Labels in **bold** are the exact on-screen text. Custom dropdowns, icon grids and colour swatches have to be clicked; plain `<select>` and text inputs can be set with `form_input`. After every save, wait for the modal to close or the button to re-enable, then verify the result.

Run the sections in this order: company settings → roles → users → labels → sources → pipelines → stages → automations (see `automations.md`).

---

## 1. Company CRM settings — `<base>/settings/company`

Fields:
- **CRM automations** — a select with three options: **Every time the deal arrives** / **Once only, ever** / **Only when the deal moves forward**. This is the default that every automation starts with.
- **Quote review**:
  - **Use quote review** checkbox.
  - When it's ticked, a **Reviewer Email** field appears. Review submissions are sent to that address.
- Save with **Update**, at the top of the page. The button only appears once something has changed.

Turning quote review **off** pops a dialog titled "Switching off quote review":
- It says how many quotes in In Review or Passed Review will be set back to In Progress. Their deals move with them.
- Only press **Yes, switch off quote review** if the plan says to and the admin accepted that consequence in the plan. Otherwise press **No, keep using quote review**, then stop and ask.

Verify: reload the page and confirm the values.

## 2. Roles — `<base>/settings/user`, **User Roles** tab

- **Create:** click **Add User Role**, which opens the modal **Add User Role**.
  - **Name**: text input.
  - **Access Permissions**: tick **WeQuote**. The CRM section only appears for WeQuote-access roles.
  - **Admin Permissions → Manage Users**: tick only if the plan says so.
  - **Quote Permissions → Can See Costs**: tick if planned. This shows values and margins on deals the user doesn't own.
  - **Sales Permissions → Can See All Sales Data**: decides whose deals and leads the role can see. Without it, users see only deals and leads they own, watch, or that are marked visible to all.
  - **CRM → Access CRM**. Ticking it reveals **Manage Configurations**, which is pipelines, stages and automations.
  - Leave every other permission as the plan says. Unless told otherwise, mirror the role the users have today.
  - Click **Save**.
- **Edit:** the pencil on a role row. Built-in roles and the admin's own role ("My Role") have no pencil and cannot be edited.
- **Never** use the delete icon.
- Verify: the role row's CRM badges show green for the granted permissions.

## 3. Users' roles — `<base>/settings/user`, **Users** tab

- Each user row has a role `<select>`. Changing it saves immediately.
- You cannot change your own role.
- Root and Elevated users ignore their role. Note this, and skip them.
- **Inviting a new user** sends an email, so only do it when the plan lists it explicitly:
  1. Click **Invite user to organisation**.
  2. Fill **Email Address** and choose **User Role**.
  3. Click **Send Invite**.

## 4. CRM labels — `<base>/settings/configure/labels`

- Click **New Label** to open **New Label**:
  - **Text**.
  - **Label Color**: a hex value in the text input next to the picker, e.g. `#1E8539`.
  - **Type**: choose **CRM** for deal labels, or **Quote** for quote labels used by automation actions.
  - Click **Save**.
- Verify: the label appears in the **CRM Labels** card, or the quote labels card.
- The organisation starts with the CRM labels Hot, Warm and Cold.
- Labels can also be created from inside the automation builder with **New CRM label** / **New quote label**. Prefer creating them here, in the plan's Labels section.

## 5. Lead sources and source contacts — `<base>/crm/leads`, **Lead sources** tab

The same panel also appears on Deals → Deal sources.

- The left rail lists the sources, starting with **All**. Defaults: Word of mouth, Contractor, Designer, Website, Social, Referral, Exhibition, Phone-in, Other.
- **Add a source:** click **Create source** at the foot of the rail. If the rail search box has text, the button reads **Create "<text>"**. This opens **Add lead source**: fill **Source name**, then click **Add source**.
- **Rename a source:** select it in the rail, then click its pencil ("Rename or delete"). This opens **Edit lead source**: change the name and click **Save**. Records under the old name move with it. Never click Delete.
- **Add a contact:** click **Add source contact**. This opens **Add source contact**:
  - **Lead source**: a select.
  - **Contact name**: required.
  - **Company**, **Email**, **Phone**: optional.
  - Click **Add contact**.
- Verify: the source shows in the rail, and the contact shows in the source's list.

## 6. Pipelines

Pipelines can be created in two places, and both open the same modal:
- `<base>/crm/automations` → **New pipeline**, shown on the hub when the organisation has more than one pipeline.
- `<base>/crm/deals` → the pipeline name dropdown → **Create pipeline**.

**Create pipeline** modal:
- **Pipeline name**.
- **What this pipeline follows**: choose one of two cards:
  - **Quotes**: the protected quote stages. Custom stages can be added between them.
  - **Standalone**: starts with New / In Progress / Complete, plus Won and Lost.
- **Or copy an existing pipeline**: a select. Copying takes the source pipeline's kind and its stages. Deals are not copied.
- The preview badges show the stages it will start with.
- Click **Create pipeline**.

The kind is fixed for good once the pipeline exists.

**Rename:** on the Deals board, open the pipeline dropdown and click the pencil next to the pipeline ("Rename or delete"). This opens **Pipeline settings**: change **Pipeline name** and click **Save**. Never click Delete.

The organisation starts with **Sales Pipeline**, which is quote-connected. Its stages are:
- Qualified 10%
- In Progress 30%
- In Review 45%
- Passed Review 60%
- Sent 75%
- Won 100%
- Invoicing 100%
- Lost 0%

All of them are protected.

Verify: the pipeline appears in the dropdown with the right tag: "Connected to quotes" or "Standalone".

## 7. Stages — `<base>/crm/deals?pipeline=<pipelineId>`, Pipeline view

**Recommended path: edit all stages at once.**
1. Click the pencil with the tooltip **"Manage pipeline stages"**, at the top right of the board, or use a column's menu → **Edit all stages**. The board switches to manage mode, and a floating bar appears with **Cancel** / **Save changes**.
2. Click **Add another stage**, after the last lane. This opens the **Add pipeline stage** modal:
   - **Stage name** \*.
   - **Place this stage**: quote-connected pipelines only. Options read "After <segment>, before <next>":
     - After Qualified, before In Progress
     - After In Progress, before In Review
     - After In Review, before Passed Review
     - After Passed Review, before Sent
     - After Sent, before Won / Lost
     - After Won, before Invoicing
     - After Invoicing
   - **Icon**: a grid of icon buttons. Their tooltips are No icon, Checked, Quote, Email, Edit, Handshake, Target, Flag, Check, Close, Clock, Archive.
   - **Icon colour**: swatches #576A92, #7C3AED, #B97A00, #1E8539, #F12B53, or a custom colour.
   - **Probability**: select 0–100% in steps of 10. It's locked on Won and Lost.
   - Click **Add stage**. The stage joins the draft in manage mode and isn't saved yet.
3. To reorder custom stages, drag the lane's drag handle ("Drag to reorder"). Protected stages can't move. On quote-connected pipelines a custom stage stays inside its segment.
4. To edit an existing custom stage's name, placement, icon, colour or probability, use the inline fields in its lane. Protected stages only allow icon, colour and probability.
5. Click **Save changes** in the floating bar.

If the bar says "N deals move to Archive on save", a stage was removed. Click **Cancel** and stop, because this skill never removes stages.

**Single stage edit (outside manage mode):** a column's menu → **Edit stage** opens **Edit stage**; finish with **Save stage**.

New deals can only enter **Qualified**, or a custom stage placed in the Qualified segment, so tell the admin when an early stage sits elsewhere.

Verify: the board shows the lanes in the planned order, with the planned probabilities. In Review and Passed Review stay hidden while quote review is off.

## 8. Automations

See `automations.md`.
- Hub: `<base>/crm/automations`.
- Pipeline: `<base>/crm/automations/<pipelineId>`.
- Map: add `?view=map`.

## Useful read-only URLs

| Purpose | URL |
|---|---|
| Deals board for one pipeline | `<base>/crm/deals?pipeline=<id>` |
| Automations list | `<base>/crm/automations/<pipelineId>` |
| Pipeline Map | `<base>/crm/automations/<pipelineId>?view=map` |
| Help guide | `<base>/crm/guide` |
| Notification triggers | `<base>/settings/notifications/triggers` |
| Companies | `<base>/settings/secondary` |
