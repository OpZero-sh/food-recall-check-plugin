---
name: recent-recalls
description: "List and summarize the newest FDA food recall reports for a time window, optionally only Class I, and open any of them for lot codes and distribution. Use when someone asks \"what food was recalled this week\", \"any recent food recalls\", \"show me the latest Class I recalls\", \"recalls in the last 90 days\", or wants a regular roundup of new recalls."
---

# Recent FDA food recalls

Show what FDA has newly reported, grouped so a reader can scan it, with the record number and link for each entry.

## When to use this skill

Use it for browsing by time: the latest reports, a window of days, the most serious class only.

Use `check-a-product` when the user wants to know about a package they have, and `recall-history` to search by company, allergen or hazard.

## Tools

| Tool | Use | Arguments that matter |
| --- | --- | --- |
| `recent_food_recalls` | Newest reports in a window, newest first | `days` (7 to 365, default 30), `class_one_only` (default false), `limit` (1 to 25, default 10) |
| `get_food_recall` | Full record for one entry | `recall_number` exactly as returned |
| `search_food_recalls` | Narrow a roundup to a food, allergen or company | `query`, `class_one_only`, `limit` |

## Workflow

1. **Set the window.** Convert the request to `days`. Use the default of 30 when the user says "recent" or "lately". For "this week" use 14, because reports are published weekly and a window of 7 days is often empty. State the window you used.
2. **Set the class filter.** Use `class_one_only: true` only when the user asks for the most serious recalls, Class I, or "dangerous" ones. Otherwise leave it false and show the class on each line.
3. **Call `recent_food_recalls`.** Ask for up to 25 when the user wants a full roundup. The header gives the total number of reports in the window and how many are shown.
4. **Group the entries.** One recall event often appears as several records, one for each product or size, from the same firm on the same report date. Group those under the firm. Then order groups by class, Class I first, or by reason: undeclared allergens, contamination with a named organism, foreign material, other.
5. **Open records on request, or when detail is the point.** Call `get_food_recall` for entries the user picks, or for the top few when they asked for lot codes and distribution. Do not open all 25 unprompted.
6. **Answer** in the format below, then offer to open any entry or to check a product the user has.

## Decision points

- **The user names a food or a company with the time window**, such as "recent cheese recalls". `recent_food_recalls` has no topic filter. Call `search_food_recalls` with one plain food word or the brand. The query is matched as an exact phrase, so keep it short. It returns newest first with the report date on each line, so keep the entries whose `Reported` date falls in the window.
- **The total in the header is larger than 25.** Say "showing the newest 25 of N". Offer to narrow by class or by a food word.
- **The user asks whether recalls are getting worse or more common.** Report the counts returned and their windows. Do not draw a trend from them. Counts are records, not events, and reporting volume varies week to week.

## Empty results

"0 FDA food recall reports in the past N days" with a short window usually means no weekly report has landed inside it yet. Widen to 14 or 30 days and say that you did. An empty result is not a statement that no food was recalled. Company announcements and FDA's public notices run ahead of this data. Point to https://www.fda.gov/safety/recalls-market-withdrawals-safety-alerts for the newest notices.

## Errors

If a call fails, try once more. If it fails again, say the lookup is unavailable and give the FDA page above.

## What the answer looks like

```text
FDA food recall reports, past [N] days: [total] reports[, Class I only]. Showing [n].

Class I
- [Firm, state]: [product, shortened to the identifying words]
  Reason: [as recorded] · Reported [date] · [recall number or numbers]
  [link]

Class II
- ...

Not in this list: USDA regulated meat, poultry and certain egg products, and recalls FDA has not yet classified.
```

Rules for the summary:

- Quote the reason as recorded. Do not explain health effects beyond the class.
- Give the class in FDA's terms. Class I means a reasonable probability of serious adverse health consequences or death. Class II means temporary or medically reversible consequences, or a remote chance of serious ones. Class III means adverse consequences are not likely.
- `Reported` is the date of the FDA report. The recall itself usually started weeks earlier. The start date is in the full record.
- `Status at time of FDA report` is about the firm's recall process, not about what is on shelves.
- Do not tell readers a product not on the list is safe.

More on what makes a useful roundup, and what the numbers do and do not mean, is in [references/roundup-notes.md](references/roundup-notes.md).

## Limits to state honestly

- The data is FDA food enforcement reports, updated weekly. It trails public announcements.
- It covers foods FDA regulates, including supplements and pet food. It does not cover USDA regulated products. See https://www.fsis.usda.gov/recalls for those.
- At most 25 entries come back per call, and the window cannot exceed 365 days.
- This is information from public records, not medical advice.

## Examples

**"Any food recalls this week?"**
Call `recent_food_recalls` with `days: 14`. If the newest report date is more than seven days old, say so: "The latest FDA enforcement report in this data is dated [date]." List the entries grouped by firm and class.

**"Show recent Class I recalls and open one for its lot codes."**
Call `recent_food_recalls` with `class_one_only: true`. List them, then call `get_food_recall` for the one the user picks, or the first if they said "one", and show its lot codes, distribution, quantity and start date.

**"Recent recalls for undeclared peanuts?"**
Call `search_food_recalls` with `query: "undeclared peanut"`, and again with `undeclared peanuts`, because the query is matched as an exact phrase. Keep entries reported in the last 30 days unless the user gave another window, and state the window.
