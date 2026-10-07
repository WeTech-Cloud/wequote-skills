# Interview question bank

Ask in this order. Each section starts with a one-line summary of what already exists, taken from the inventory, then asks the questions.

Use the structured ask-the-user tool:
- ≤4 questions per call;
- the recommended option first, with "(Recommended)";
- free text via "Other".

Always offer **Leave as is / skip this section**.

Start with a scoping question (multi-select):
> "Which parts should I set up today?"
> Company CRM settings · Roles & who has them · Labels · Lead sources & contacts · Pipelines & stages · Automations

Then ask one context question that shapes the defaults:
> "What kind of work does this pipeline track?"
> Quoted jobs (supply/install, projects) · Service/maintenance without quotes · Both · Other

---

## A. Company CRM settings

1. **Quote review.** "Should quotes go through internal review before they're sent?"
   - Options: Keep as is (current: On/Off) · Turn on · Turn off.
   - If turning on, ask for the **Reviewer Email**. Default: the admin's email, but ask; never assume.
   - If turning off while it's on, warn that quotes in review go back to In Progress and their deals move with them.
   - Explain the effect: the In Review and Passed Review stages, and any automations on them, are only used when review is on.
2. **Default re-run mode for automations.**
   - Options: Every time the deal arrives (Recommended for most) · Once only, ever · Only when the deal moves forward.
   - One-line explanations:
     - **Every time**: moving a deal back and forward runs it again, and may duplicate notes.
     - **Once**: never repeats for that deal, including its required work.
     - **Forward**: runs only when the deal moved to a later stage.

## B. Roles and users (needs Manage Users)

1. "Who should run the CRM setup (pipelines, stages, automations)?" Pick users. They need a role with **Access CRM + Manage Configurations**.
2. "Who should work deals but not change the setup?" Pick users. They need a role with **Access CRM** only.
3. For each group, ask:
   - "Should they see everyone's deals and leads?" This is **Can See All Sales Data**.
   - "Should they see values and margins on deals they don't own?" This is **Can See Costs**.
4. Recommended roles, offered only if no existing custom role already fits:
   - **CRM Manager**: WeQuote, Access CRM, Manage Configurations, Can See All Sales Data, Can See Costs, plus the users' other current permissions.
   - **Sales**: WeQuote, Access CRM, Can See All Sales Data off, plus the users' other current permissions.

   Important: a role replaces the user's **whole** permission set. Base each new role on the role the users have today, ticking the same boxes, and add only the CRM boxes. Show the full checkbox list in the plan.
5. Never change the admin's own role, or root/elevated users.
6. New people to invite: ask for each email and role. Warn that inviting sends them an email.

## C. Labels

1. "Your CRM labels are: <list>. Add any?" Free text: "name — colour".
   - Suggestions by business: *Hot / Warm / Cold* (already there), *Repeat customer*, *Trade*, *Large project*, *Price-sensitive*.
2. If planned automations add or remove a quote label or check a deal label, make sure those labels are in this section.

## D. Lead sources and contacts

1. "Your lead sources are: <list>. Add or rename any?"
   - Don't offer deletion. If asked, tell the admin where to do it.
2. "Do you have named referrers or partners to record as source contacts?" For each, ask: source, name, company, email, phone.

## E. Pipelines

1. "You have: <pipelines with kinds>. Keep them as they are?"
2. "Add a pipeline?" For each new one, ask:
   - **Name**.
   - **Kind**: **Quotes** (follows the quote lifecycle) or **Standalone** (work without quotes: only Won and Lost are fixed).
   - Or **copy** an existing pipeline (takes its kind and stages).
   - Remind the admin that the kind can't change later.
3. Rename? Ask for old → new name.

## F. Stages (per pipeline)

Show the current lanes in order, protected ones marked 🔒.

**Quote-connected pipelines:**
- "Add your own stages between the quote stages?" For each, ask:
  - name;
  - **placement**: After Qualified / After In Progress / After In Review / After Passed Review / After Sent / After Won / After Invoicing;
  - probability;
  - optional icon and colour.
- Typical additions: *Site Visit* (after Qualified), *Measuring* (after Qualified), *Follow-up call* (after Sent), *Installation* (after Won), *Handover* (after Invoicing).
- Warn when the placement is After In Review or After Passed Review and quote review is off. Deals never reach those stages.

**Standalone pipelines:**
- "Rename or add to New / In Progress / Complete?" Ask for the order and probabilities.

**Probability defaults:** rising through the board (10 → 90). Won is 100, Lost is 0, and both are fixed.

Never remove stages in this skill.

## G. Automations (per pipeline, per stage)

1. "Which stages should do something automatically when a deal arrives?" Multi-select from the lanes. Mark review stages as *inactive while quote review is off*.
2. For each chosen stage, ask: "Start from the guided template or build your own?" Show the template's title and what it does (see `automations.md`).
   - **Template:** ask only for its blanks (person, amount, label…), plus whether to change anything.
   - **Build your own:** walk the steps in plain language. "First…? Then…? Should anything depend on a condition?"
     - Map each answer onto an Action / Rule / Wait and its settings.
     - Ask for every required setting.
     - For users and labels, offer only names that exist or are planned.
     - For **Move the deal to a stage**, offer only valid targets: later custom stages, same segment.
3. Ask for each automation:
   - **Name** (≤100 chars) and the one-line **What it is for**.
   - **Runs again**: company default (Recommended), or a specific mode.
   - **Company scope**: only if there are several companies.
   - **Publish and turn on** · **Publish, leave it off** (Recommended for a first run, so the admin can review) · **Save as draft only**.
4. Read the whole flow back as a step tree before moving on:
   - a Wait isn't last;
   - each rule has a branch;
   - required settings are present.

## Defaults when the admin says "just set it up sensibly"

Propose a full plan built from these defaults, and still show it for approval:
- Keep Sales Pipeline. Add *Site Visit* after Qualified (20%).
- Labels: keep Hot/Warm/Cold.
- Sources: keep the defaults.
- Automations, published and left **Off**:
  - Qualified: the template, with the owner set to the admin.
  - Sent: the chase template.
  - Won: the hand-over template.
  - Lost: the record-reason template, with label **Cold**, or ask.
- Re-run: the company default, unchanged.
