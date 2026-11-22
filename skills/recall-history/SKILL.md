---
name: recall-history
description: "Search FDA food recall reports by company, brand, food type, allergen or contaminant, and explain what a recall record says. Use when someone asks \"has this company had recalls\", \"recalls for undeclared milk\", \"listeria recalls in cheese\", \"what was the reason for this recall\", \"explain recall number H-0011-2027\", or wants the recall record behind a news story."
---

# Recall history

Search FDA enforcement reports by words, then report the matching records accurately: who recalled what, why, how FDA classified it and when.

## When to use this skill

Use it for questions about a company, a brand, a kind of food, an allergen or a hazard across records, and to explain one record by its recall number.

Use `check-a-product` to decide whether a package the user has is covered, and `recent-recalls` to browse the latest reports by date.

## Tools

| Tool | Use | Arguments that matter |
| --- | --- | --- |
| `search_food_recalls` | Records whose product, reason or company text contains the query as a phrase. Newest first | `query` (1 to 60 characters), `class_one_only` (default false), `limit` (1 to 25, default 10) |
| `get_food_recall` | Full record | `recall_number` exactly as returned, for example `H-0011-2027` |
| `recent_food_recalls` | Newest reports by window | See the `recent-recalls` skill |

The search has no date, state or class II and III filters. Results come newest first, and the header gives the total number of matching records.

## Workflow

1. **Choose the search words.** Keep them short and literal. The query is matched as an exact phrase: the words must appear together and in that order in the record's text.
   - Company: the distinctive part of the name as it would appear on a report, without "Inc" or "LLC". Try the brand and the corporate name as separate searches.
   - Allergen: "undeclared" plus the allergen, for example `undeclared milk`. Word order matters. `milk undeclared` finds far fewer records. Try singular and plural, such as `undeclared peanut` and `undeclared peanuts`.
   - Organism: the name used in reports, for example `Listeria`, `Salmonella`, `E. coli`.
   - Food type: one plain word, for example `cheese`, `spinach`, `salmon`.
   - Do not combine two ideas in one query. `listeria cheese` finds nothing because no record prints those two words side by side. Search one idea and read the results for the other.
2. **Call `search_food_recalls`.** Use `limit: 25` for a history question. Set `class_one_only` only when asked.
3. **Check each result against the question.** A company search can match a product description that mentions the name in an address. An allergen search can match records where the word appears for another reason. Keep what answers the question.
4. **Group records into events.** Records from one firm with one reason and one report date belong together. Count events and records separately when the user asks "how many".
5. **Open records for detail.** Call `get_food_recall` for the records the user asks about, and for the most recent event in a company history. The full record adds lot codes, distribution, quantity and the date the recall started.
6. **Answer** in the format below.

More search advice is in [references/search-tips.md](references/search-tips.md).

## Explaining one record

When the user gives a recall number, call `get_food_recall` and walk through it:

- what product, in the identifying words of the description
- who recalled it
- why, in the report's words
- the class and what that class means
- which lots or dates, as recorded
- where it was distributed, as recorded
- how much product
- when the firm began the recall and when FDA reported it
- the status at the time of the report
- the record link

Do not add consumer instructions the record does not contain. Say that disposal and refund instructions come from the recalling company's notice.

## Decision points

- **The header count is far larger than what is shown.** Say "the newest 25 of N records". The tool returns at most 25, newest first, and cannot page further back. For older history, narrow the words.
- **The user asks whether a company is safe or trustworthy.** Report the records and stop there. A recall record shows that a firm removed a product. It does not rate the firm. Many recalls are started by the firm itself.
- **The user asks which allergen or hazard is most common.** The tool cannot rank. Offer counts for specific searches, labeled as record counts.
- **Nothing is found for a company.** Try the brand, the parent company and the plain product word. Then say no FDA enforcement reports matched those words, and that this does not show the company never had a recall. USDA regulated products and older or differently worded records would not appear.
- **The user's real question is about a package they own.** Switch to `check-a-product`.

## Errors

- **"No FDA food recall with number ..."** The number is wrong or incomplete. Search by the firm or product to recover it. Recall numbers are a letter, four digits and a year, such as `H-0011-2027` or `F-0423-2024`.
- **A tool error.** Try once more, then say the lookup is unavailable and point to https://www.fda.gov/safety/recalls-market-withdrawals-safety-alerts.

## What the answer looks like

```text
[N] FDA enforcement records match "[words]". Showing the newest [n], in [e] recall events.

[Report date] · [Firm, state] · Class [I/II/III]
  Products: [identifying words for each record]
  Reason: [as recorded]
  Recall numbers: [list]
  [link to the first record]

Searched: [each query]. Not covered: USDA regulated products, recalls not yet classified by FDA.
```

FDA's classes, to state when a class first appears: Class I is a reasonable probability of serious adverse health consequences or death. Class II is temporary or medically reversible consequences, or a remote chance of serious ones. Class III is not likely to cause adverse health consequences.

## Limits to state honestly

- The data is FDA food enforcement reports, updated weekly, for foods FDA regulates.
- Counts are records. One event can be many records.
- Search results cut product descriptions short. Open a record before quoting details.
- This is information from public records. It is not medical advice and not a judgment about any company.

## Examples

**"Has Family Farmstead Dairy had any recalls?"**
Call `search_food_recalls` with `query: "Family Farmstead"` and `limit: 25`. Group the results into events, open the newest with `get_food_recall`, and report the product, reason, class, dates and status for each event.

**"How often is undeclared milk the reason for a recall?"**
Call `search_food_recalls` with `query: "undeclared milk"`. Report the header count as the number of records that mention those words, show the newest few with their classes, and say that this is a record count over the whole data set, not a rate.

**"Explain recall H-0011-2027."**
Call `get_food_recall` with that number and walk through the record line by line as described above.
