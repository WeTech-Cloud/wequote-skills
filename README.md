# WeQuote skills for Claude

Claude skills that help WeQuote customers set up and run WeQuote.

| Plugin | What it does |
|---|---|
| `wequote-crm` | Sets up and works the WeQuote CRM for your organisation through your browser. **Setup:** company CRM settings, roles, labels, lead sources, pipelines, stages, automations and CRM notifications, with an optional test run. **Leads and deals:** create leads, convert them to deals, enter leads from a spreadsheet, create and move deals, mark them Won or Lost, and add notes, follow-ups, meetings and files. **Health check:** a read-only check of your setup and a pipeline report. **Lead capture:** a guide to getting website or Zapier leads into the CRM. Claude checks your access, asks you every decision, and shows you a plan to approve **before** it changes anything. |

## Before you start

- A Claude plan that supports plugins.
- **Claude in Chrome**, installed and connected. Claude uses it to work in your browser.
- A WeQuote login whose role has **Access CRM** and **Manage Configurations**.
  - To set up roles too, the role also needs **Manage Users**. For CRM notifications, it needs **Manage Notifications**.
  - To work leads and deals only, **Access CRM** is enough. To see everyone's records, the role needs **Can See All Sales Data**.
  - If the CRM isn't switched on for your organisation, contact WeQuote support.

## Install

### Claude (web or desktop app)
1. Go to **Customize → Plugins → Add → Add marketplace**.
2. Enter `WeTech-Cloud/wequote-skills`.
3. Install **wequote-crm**.

### Claude Desktop — Code tab
Click **+** → **Plugins** → **Add plugin**, and add the marketplace `WeTech-Cloud/wequote-skills`. Then install **wequote-crm**.

### Claude Code (terminal)
```bash
claude plugin marketplace add WeTech-Cloud/wequote-skills
claude plugin install wequote-crm@wequote
```

### For a whole company (Team / Enterprise)
Your Claude admin can add the marketplace once for everyone, under **Organization settings → Plugins & skills**.

## Use it

1. Log in to WeQuote in Chrome.
2. Ask Claude, for example:
   - **"Set up our WeQuote CRM"**
   - **"Add a lead for John Smith at Smith Ltd, £5,000, from our website, and convert it to a deal"**
   - **"Add the leads in this spreadsheet to WeQuote"**
   - **"Mark the Acme deal as lost: they went with a cheaper quote"**
   - **"Is our CRM set up right? What needs attention?"**
   - **"Turn on the Sent automation and test it"**

Claude will then:
1. Confirm which WeQuote organisation you're in.
2. Check your access, and list what's already set up.
3. Ask you how you want things configured. Every question has a recommended default.
4. Show you the full plan. **Nothing changes until you approve it.**
5. Make the changes, checking each one, then give you a summary.

Claude never deletes anything. During setup it doesn't touch your leads, deals or quotes. When it works leads and deals, it lists every change and saves only after you approve it, asks separately before marking a deal Won or Lost, and never edits, sends or cancels a quote directly. It also never types your password.

## Updates

**Claude Code:**
```bash
claude plugin marketplace update wequote
claude plugin update wequote-crm@wequote
```

**Claude web or desktop:** updates show up in **Customize → Plugins**.
