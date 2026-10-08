# UI map and recipes — leads and deals

`<base>` = `https://<host>/<org>`. Labels in **bold** are the exact on-screen text. Custom dropdowns (labels, interests, lead source, customer search) have to be clicked; plain `<select>` and text inputs can be set with `form_input`. After every save, wait for the modal to close, then verify.

Modals in this app shift their layout as fields appear (for example, **Source contact** appears once a source is picked). Take a fresh screenshot before clicking by coordinate after any field changes.

---

## 1. Leads — `<base>/crm/leads`

Tabs: **Inbox (n)** · **Archive (n)** · **Lead sources**. Search: "Search lead by title / owner". With secondary companies there's also an **All companies** picker.

Columns: Lead title, Product Value, Next Activity, Label, Lead source, Lead created date, Owner (plus Owning Company with secondary companies).

### Create a lead
1. Click **Create Lead**. The **Create Lead** modal opens.
2. **Link or add customer**:
   - Existing customer: click **Link customer**. The **Select Customer** modal lists customers (Customer Name, Account No., Phone Number, Email Address, Primary Contact, Assigned to) with a search box. Clear the search to see all of them. Click the customer's name. The modal shows "Existing customer · <name> is already on the account." and copies the customer's address into the form.
   - New customer: type the name. A **NEW** badge appears. The customer record is only created when the lead converts to a deal.
   - If **Possible duplicates** appears, stop and show it to the user.
3. **Lead title**: optional.
4. **Currency** (select) and **Estimated Value** (number, no symbol, e.g. `5000`).
5. **Labels**: the **Add label** dropdown. Tick existing labels. Don't use the **+** (new label) without a separate yes.
6. **Interests**: the organisation's Systems. Pick from the dropdown; they can't be created here.
7. **Owner**: select. Defaults to **Unassigned**.
8. **Lead source**: a searchable dropdown of the sources. Don't use **Add source** without a separate yes. Once set, **Source contact** ("Choose who sent this") appears.
9. **Visible to all users**: toggle, if shown.
10. **Project / Enquiry description**: free text.
11. **Address** block: three lines, **Town / City**, **County / State**, **Postcode / ZIP**, **Country**. "Copied to the customer record when this Lead is converted to a Deal."
12. **Contacts**: tabs **PRIMARY** / **SECONDARY**. Pick the tab, then click the **+** next to "Contacts". The **New Contact** modal has **First Name**, **Surname**, **Position**, **Email Address**, **Mobile Number**, **Primary** ("Set contact as primary"), and an address. Click **Save**.
13. Click **Save**.

Verify: the lead appears in **Inbox** with the right title, value, source and owner.

### Open a lead
Click the row, or go to `<base>/crm/leads?lead=<id>`. The **Lead** drawer has inline-edit pencils for each field, the activity panel (section 4), an Archive/Restore icon and **Convert to Deal**.

### Convert leads to deals
- One lead: row menu (⋮) → **Convert to Deal**, or the drawer's **Convert to Deal**.
- Several: tick the rows, then **Convert to Deals** in the floating bar.
- The modal **Convert selected Leads to Deals** asks for **Pipeline** and **Pipeline stage**. On a quote-connected pipeline the stage is fixed to the entry stage ("This pipeline's stages follow the quotes, so the deal enters here and moves when a quote does."). It says how many leads become deals and that "A customer record is created for each lead that has not been linked to one."
- Click **Convert to Deals**.

Verify: the leads leave the Inbox, and the deals appear in the chosen lane on `<base>/crm/deals?pipeline=<id>`.

### Archive or restore a lead
Row menu → **Archive** (Inbox) or **Restore** (Archive tab). Bulk versions sit in the floating bar. Only when the user asked for that lead.

---

## 2. Deals board — `<base>/crm/deals`

Views: **Pipeline** · **Table** · **Archive** · **Deal sources**. The toolbar has "Search deals", Total value / Total margin, the pipeline dropdown, **Filter deals** and sort.

- **Filter deals**: **My work**, **Deal owner** (Anyone / Me / a user), **Attention** (Overdue, Due today, Due this week, No next activity), **Activity type**, **Close date**, **Clear all**.
- **Table** view: "All stages" filter; columns #, Title, Deal value, Margin, Stage, Owner, Customer, Last activity.
- **Archive** view: "Archived deals appear here and can be restored." Columns: Archived, Deal, Outcome, Previous stage, Value, Margin, Owner, Archived by.

### Create a deal directly
1. Click **Create Deal** at the foot of the lane, or the lane menu → **Create deal here**. Lanes that say "Deals arrive here from their quotes" can't take new deals.
2. The **Create Deal** modal:
   - **Customer** \*: type a name (**NEW**, "will be created with this deal") or click **Link customer** ("Linking to the existing customer"). Search first (records.md, guardrail 7).
   - **Deal title**: "Defaults to the customer's name."
   - **Pipeline** and **Starting stage**.
   - **Currency**, **Estimated Value**, **Expected close date**.
   - **Labels**, **Interests**, **Owner** (defaults to Me), **Deal source**, **Source contact**, **Visible to all users**.
   - **Project / Enquiry description**, address and contacts as on a lead.
3. Click **Create Deal**.

Verify: the card appears in the lane with the right customer and value.

### Move a deal
- Card menu (⋮) → **Move deal**. The "Move …" modal has **Pipeline stage**. Click **Confirm**.
- In Table view, the Stage column has an inline picker.
- Don't drag cards. Use the menu, so each move is deliberate.
- Quote-driven lanes only receive deals from their quotes. Won and Lost go through the deal actions (section 3).

### Archive a deal
Card menu → **Archive deal**, or in Table view tick it and use **Archive**. Restore from the Archive view (**Restore**). Only when the user asked.

---

## 3. Deal page — `<base>/crm/deals/<id>`

### Header
- **Watching this deal** (the eye): **Add watchers**, or remove a watcher. "No watchers yet." / "Nobody left to add."
- **Mark as Won**.
- The actions menu: **Mark as Won**, **Mark as Lost**, **Reopen deal**, **Archive deal** / **Restore deal**.

### Mark as Won
On a quote-connected pipeline a modal asks which quote won it (shown as "Winner" / "Alternative"). Pick the one the user named, then **Continue**. If no quote is "Not yet signed", stop and ask; don't sign quotes.

### Mark as Lost
The modal "Why was this deal lost?" needs a reason (free text, up to 255 characters). It lists "These quotes will be cancelled:" and warns when "There are invoices against this work". Read both out in the change list. Click **Mark as Lost**.

The reason can be changed later on the deal page with **Save reason**.

### Reopen deal
Actions menu → **Reopen deal**. A Won deal reopened reverses the acceptance. A Lost deal comes back when a quote is raised or revived.

### Sidebar fields
Labels, **Expected close date**, Interests, Value, Owner, **Visible to all users**, Source, Project (**Link project**), Customer and contacts. Each has its own edit control; click it, change the value, then **Save** / **Done**.

- **Value** stops being editable once the quotes set it ("The value itself comes from this deal's quotes.").
- **Owner** may not be editable here. If there's no control, say so and stop.

### Quotes
The **Quote** card: **Create**, **Link quote**, or **View N Quotes**. "Link a customer to this deal before creating or linking a quote." Revisions and change orders are raised on the quote itself, so don't edit quotes from here.

---

## 4. Activity panel (deal page and lead drawer)

Tabs: **Notes** · **Meetings** · **Files**.

### Add a note
1. **Notes** → "Add a note". Type the note; `@` mentions a user (they get notified).
2. Optional **Follow-up**: **Set follow-up date**, and **Assigned to:** a user ("for Anyone" by default).
3. Click **Save note**.

### Schedule a meeting
1. **Meetings** → **Schedule a meeting**. It books into the Workhub calendar.
2. Fields: title, date and time, duration (**30 min**, **45 min**, **1 hour**, **1.5 hours**, **2 hours**), type (**On site** / **Call** / **Video**), location, **Meeting members:** and agenda.
3. Click **Schedule meeting**.

Meeting members are notified, so confirm the invite list in the change list.

### Attach a file
**Files** → **Attach a file** → **Choose a file**. "PDFs, documents, spreadsheets and images up to 20 MB." Use only a path the user gave you.

### Needs your attention
Items "Holding this deal" (required work from automations) show **Mark as done**. Only mark something done when the user says the work really was done.

### History
Filters: source (**Any source**, **Manual**, **Quote-driven**, **Automation-driven**, **Portal-driven**, **Background task**), person (**Everyone**), type (**All activity**, **Notes**, **Meetings**, **Files**, **Changes**, **Automation**). Use **Automation** to check whether an automation ran on this record.
