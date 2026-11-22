# FDA enforcement record fields

What each line of a result means. The data is FDA's food enforcement reports as published through openFDA. Each record link points at that data.

## Lines in every result

| Line | Meaning | Notes |
| --- | --- | --- |
| Product description | The recalled product as described in the report: label wording, sizes, packaging, sometimes UPC and the firm's address | Cut short with an ellipsis in search and recent lists. Complete in `get_food_recall` |
| `Reported` | The date of the FDA enforcement report that lists the recall | Not the day the recall began |
| `Recall number` | FDA's number for this product's recall, a letter, a sequence number and a year, such as `H-0197-2027` | The letter varies. Records from 2026 carry H and older ones carry F. The year follows the federal fiscal year, which starts in October, so reports from late in a calendar year carry the next year. Always pass the number exactly as returned |
| `Firm` | The recalling firm and its state | May be a manufacturer, distributor or importer and may differ from the brand on the label |
| `Reason` | Why the product was recalled, in the report's words | Typical reasons are an undeclared allergen, contamination with a named organism, or foreign material |
| `Class` | FDA's hazard classification | See below |
| `Status at time of FDA report` | Where the recall process stood when the record was last updated | See below |
| Link | The openFDA query for this record | Raw data in JSON, not a consumer notice |

## Extra lines in `get_food_recall`

| Line | Meaning | Notes |
| --- | --- | --- |
| `Distribution` | Where the product was shipped, as free text | Often ends with a note that product may be further distributed |
| `Lot codes` | The code information that identifies affected production: lots, dates, sometimes UPCs | Free text. Can say "No lot code" or cover all lots |
| `Quantity` | How much product the recall covers, in the firm's units | Units vary: cases, pounds, consumer units |
| `Recall started` | The date the firm began the recall | Usually weeks before the `Reported` date |

A record has no line for consumer instructions, refunds, illnesses reported, or retailers unless the firm wrote them into another field.

## Recall classes

FDA assigns one of three classes to each recall based on the health hazard.

- **Class I.** There is a reasonable probability that using or being exposed to the product will cause serious adverse health consequences or death.
- **Class II.** The product may cause temporary or medically reversible adverse health consequences, or the probability of serious consequences is remote.
- **Class III.** Using or being exposed to the product is not likely to cause adverse health consequences.

How to use the class:

- Report it as recorded. Do not upgrade or downgrade it.
- The class describes the hazard in general. It does not say how any one person will be affected. An undeclared allergen is serious for a person with that allergy whatever the class, and the same allergen appears under different classes in different records.
- A Class II or III recall is still a recall. A matching package should not be eaten.

## Status

- **Ongoing.** The recall is in progress.
- **Completed.** The firm has finished its recall actions.
- **Terminated.** FDA has reviewed the recall and closed it.

A completed or terminated recall can still describe packages sitting in a pantry or freezer, especially products with long dates. Status never turns a match into a non match.

## Timing

- Enforcement reports are published weekly. A window of only seven days can come back empty between reports.
- A recall appears in an enforcement report after FDA classifies it. In records checked while writing this file, the report date was four to eight weeks after the recall started.
- A company's own announcement and FDA's public notice page can carry a recall well before the enforcement report does. Not every recall has a public announcement.

## Several records for one event

Each product, size or variety in a recall event gets its own recall number. A list can show several entries from the same firm on the same report date. They are separate records with separate code lists.
