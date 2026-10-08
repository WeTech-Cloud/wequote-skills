# Entering a batch of leads

WeQuote has no lead import. To add many leads, read the user's list and create each lead through **Create Lead**, one at a time, after one approval for the whole batch.

## 1. Read the list

- Accept a CSV or `.xlsx` path the user gave, or rows pasted into chat.
- Never upload the file or send its contents anywhere else.
- Show the column names and the first 5 rows, and ask the user to confirm the mapping onto the lead fields:

| Lead field | Required | Notes |
|---|---|---|
| Customer | – | Existing customer name, or a new one |
| Lead title | – | Defaults to the customer or contact name |
| Contact first name / surname | – | Becomes the primary contact |
| Contact email / mobile | – | Only if present in the file |
| Estimated value | – | Number only; strip `£`, `$`, commas |
| Currency | – | Defaults to the organisation's default |
| Lead source | – | Must match an existing source |
| Source contact | – | Must exist under that source |
| Labels | – | Must exist (CRM labels) |
| Interests | – | Must exist (the organisation's Systems) |
| Owner | – | Must be an existing user |
| Address lines, town, county, postcode, country | – | Copied to the customer on convert |
| Description | – | Project / Enquiry description |

A row needs at least a customer, a lead title or a contact name.

## 2. Check every row (read-only)

For each row, work out:
- **Customer:** an existing customer (search **Link customer**), or NEW.
- **Duplicates:** a lead with the same title or customer already in **Inbox**, or a duplicate row in the file. Mark as ⏭ SKIP unless the user says otherwise.
- **Values that don't exist:** unknown sources, labels, interests or owners. Either drop that field for the row or ask the user. Never create them in this mode.
- **Bad values:** non-numeric values, malformed emails.

## 3. Preview and approve

Show a preview table, numbered, with a result column:

```
| # | Customer | Title | Contact | Value | Source | Owner | Plan |
|---|----------|-------|---------|-------|--------|-------|------|
| 1 | Acme Ltd (existing) | John Smith | John Smith | £5,000 | Website | Sam Lee | CREATE |
| 2 | Brightside Homes (NEW) | Kitchen refit | Jane Doe | £12,000 | Referral | Unassigned | CREATE |
| 3 | Acme Ltd (existing) | John Smith | – | – | – | – | SKIP: already in Inbox |
```

Then ask: **Approve and run** / **Change something** / **Cancel**. Also ask whether to convert them to deals afterwards, and into which pipeline and stage; the bulk **Convert to Deals** handles that in one step.

For more than 50 rows, run in batches of 25 and report after each batch.

## 4. Run and log

- Save a progress file next to the user's list, or in the scratchpad: `crm-leads-<org>-<date>.md`, with a Status column (✅ / ⏭ / ❌ and the reason).
- Create each lead with the recipe in `records-ui-map.md`. Wait for the modal to close before the next one.
- On resume, re-check the Inbox and mark rows that already exist as ⏭ SKIP.
- If one row fails, stop and ask: *Retry* / *Skip this row and continue* / *Stop here*.

## 5. Report

A count of created / skipped / failed, the failures with reasons, and the path of the progress file.
