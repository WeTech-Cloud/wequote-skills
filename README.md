# WeQuote skills for Claude

Claude skills that help WeQuote customers set up and run WeQuote.

| Plugin | What it does |
|---|---|
| `wequote-crm` | Sets up the WeQuote CRM for your organisation through your browser. It covers company CRM settings, roles, labels, lead sources, pipelines, stages and automations. Claude checks your access, asks you every decision, and shows you a plan to approve **before** it changes anything. |

## Before you start

- A Claude plan that supports plugins.
- **Claude in Chrome**, installed and connected. Claude uses it to work in your browser.
- A WeQuote login whose role has **Access CRM** and **Manage Configurations**.
  - To set up roles too, the role also needs **Manage Users**.
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
2. Ask Claude: **"Set up our WeQuote CRM"**.

Claude will then:
1. Confirm which WeQuote organisation you're in.
2. Check your access, and list what's already set up.
3. Ask you how you want things configured. Every question has a recommended default.
4. Show you the full plan. **Nothing changes until you approve it.**
5. Make the changes, checking each one, then give you a summary.

Claude never deletes your existing setup or touches your leads, deals or quotes. It also never types your password.

## Updates

**Claude Code:**
```bash
claude plugin marketplace update wequote
claude plugin update wequote-crm@wequote
```

**Claude web or desktop:** updates show up in **Customize → Plugins**.
