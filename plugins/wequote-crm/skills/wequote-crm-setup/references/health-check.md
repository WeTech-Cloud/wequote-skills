# Health check and report mode (read-only)

Use this mode when the user asks things like "is our CRM set up right?", "what needs attention?", "how is the pipeline doing?" or "give me a CRM report".

This mode **never changes anything**. It doesn't click Save, Publish, On/Off, Mark as done, Archive or any other control that writes. If the user wants something fixed, offer to switch to setup mode, or to leads and deals mode for record work, and plan it there.

Run Phase 1 (Connect) from `SKILL.md` first. Then work through the two parts below. Skip either part if the user only asked for the other.

## Part 1 — Setup health

Run the preflight checks and inventory from `preflight.md`. Then flag each item below.

| Check | Where | Flag when |
|---|---|---|
| Automations not ready | Automations page filters **Needs setup** and **Not published** | Any automation needs setup, or has **Unpublished changes** |
| Automations that can't run | Pipeline Map | An automation is On for In Review or Passed Review while **Use quote review** is off |
| Automations left Off | Pipeline Map | Published but Off. List them; they may be off on purpose |
| Skipped checks | **Skipped checks** on the automations page | The count is above zero |
| Unused stages | Deals board, plus the Pipeline Map | A custom stage with no deals and no automations, or one placed After In Review / After Passed Review while quote review is off |
| Roles | **User Roles** tab | Users whose role lacks **Access CRM**, or nobody but the admin has **Manage Configurations** |
| Quote review | Company settings | Quote review is on but **Reviewer Email** is empty |
| Notifications | Notification triggers, **CRM** tab | A trigger is enabled with no channel (the row shows a warning icon) |
| Labels | Labels page vs automations | An automation adds or checks a label that no longer exists, or that shows as **Needs setup** |

## Part 2 — Work and pipeline report

### Needs attention
- On `<base>/crm/deals`, use **Filter deals** → **Attention**: count **Overdue**, **Due today**, **Due this week** and **No next activity**. Then **Clear all**.
- **Close date** filter: count **Close date passed** and **No close date**.
- Cards marked "Payment overdue" or "Action required · Create invoice".
- The Leads **Inbox** count, and how many leads are **Unassigned**.

### Dashboard summary — `<base>/crm/dashboard`
- Use the period and pipeline the user asks for. Otherwise use this financial year, all months, and each pipeline in turn.
- Read the period tiles: **New leads**, **New Deals** (converted vs created directly), **Won** (value, count, margin), **Lost**, **Win rate**.
- **Open pipeline:** open value and margin, expected to close, **Close date passed**, **No close date**, and value by stage.
- **By owner** and **By source:** won, lost, win rate and open value.
- If the user's role lacks **Can See All Sales Data**, the dashboard shows a banner and only their own figures. Say so at the top of the report.
- If the user's role lacks **Can See Costs**, margins show as "—". Leave them out rather than guessing.

## Output

```
CRM health — <Org name> (<host>/<org>) · <date>

Setup
| Area | Result | Detail | Fix |
|------|--------|--------|-----|
| Automations | ⚠️ | "Chase it…" has unpublished changes | Setup mode: publish it |
| Quote review | ✅ | Off; no review-stage automations | – |

Work
| Measure | Value |
|---------|-------|
| Overdue follow-ups | 3 |
| Deals with no next activity | 7 |
| Leads in Inbox (unassigned) | 12 (5) |

Pipeline — <pipeline>, <period>
| New leads | New deals | Won | Lost | Win rate | Open value |
| … |
```

End with at most five suggested next steps, each naming the mode that would do it. Don't start any of them without the user asking.
