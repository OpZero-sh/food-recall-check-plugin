---
name: check-a-product
description: "Check whether a specific food package matches an FDA recall by comparing its product name, size, UPC, lot code and best by date with the FDA enforcement record. Use when someone has a product in hand or in mind and asks \"is this recalled\", \"was my peanut butter recalled\", \"check this lot code\", \"is this UPC part of the recall\", \"I heard about a recall, is mine affected\", or reads you details from a label."
---

# Check a product against FDA recalls

Decide, from the record and the label, whether a package is covered by an FDA food recall. The answer is one of three: it matches, it does not match on a named field, or it cannot be settled yet.

## When to use this skill

Use it when the user wants to know about one product or one package.

Use `recent-recalls` to browse what was recalled lately, and `recall-history` for the recall record of a company, an allergen or a hazard. For a list of many products, hand the list to the `pantry-sweep` agent where agents are available.

## Safety first

- If the user says someone ate the product and is ill or worried, say first that they should contact a health care provider, and that trouble breathing, swelling of the face or throat, or other severe symptoms call for emergency services (911 in the US). Then continue with the check. Do not assess symptoms or suggest treatment.
- Never tell the user a product is safe to eat. The most this skill can say is that no matching FDA enforcement report was found.

## Tools

| Tool | Use | Arguments that matter |
| --- | --- | --- |
| `search_food_recalls` | Find candidate recalls by a word or exact phrase in the product, the reason or the company. Newest first | `query` (1 to 60 characters), `class_one_only` (default false), `limit` (1 to 25, default 10) |
| `get_food_recall` | The full record: complete product description, lot codes, distribution, quantity, dates | `recall_number` exactly as the search returned it, for example `H-0011-2027` |
| `recent_food_recalls` | Newest reports in a window | See the `recent-recalls` skill |

## Workflow

1. **Start with what the user gave you.** A brand or product name is enough for a first search. Do not hold the search until you have every label detail.
2. **Search.** Call `search_food_recalls` with the single most distinctive word or exact phrase, usually the brand name. The query is matched as a phrase: the words must appear together and in that order in the record. `Family Farmstead` finds a record that `Farmstead cheddar` misses, so do not join a brand and a product word unless the label prints them side by side. If the user has a UPC, search the digits alone as a second query. Records often include the UPC in the product description. Leave `class_one_only` false. The filter hides recalls the user needs to see.
3. **Try again before giving up.** If nothing fits, search the manufacturer or distributor named in small print on the package, then one plain product word such as the main ingredient. The recalling firm on a record is often not the brand on the front of the package.
4. **Pick candidates.** A candidate is a result whose product description could be the user's product. Results cut the description short with an ellipsis, so include anything plausible.
5. **Open every candidate** with `get_food_recall`. Each product size or variety has its own recall number, and one company can have several on the same day. Keep them apart.
6. **Ask for the label details the record turns on.** Now that you can see what the record lists, ask only for those: size, UPC, lot code, best by date. Tell the user where these are printed. See [references/reading-a-label.md](references/reading-a-label.md).
7. **Compare field by field** using [references/matching-rules.md](references/matching-rules.md). In short:
   - Product name and size must fit the record's description.
   - The lot code or date must be one the record lists, compared character by character. Keep leading zeros and punctuation.
   - A UPC identifies the product line, not the lot. A UPC match without a lot or date match is not yet a match unless the record covers all lots.
   - Distribution supports a match and never rules one out. Records often say product "may be further distributed".
8. **State the result** in the format below.

## The three results

- **Match.** The product fits the description and the lot code or date is listed, or the record covers all lots or codes. Say which fields matched.
- **Not a match on [field].** The product fits but the user's lot code, date or size is not in the record. Name the field and show both values. Add that this covers this recall number only.
- **Cannot tell yet.** A needed detail is missing or unreadable, the record's code list is cut short or unclear, or the record uses a range you cannot apply with confidence. Say exactly what would settle it.

If any line of the record ends in an ellipsis, treat that line as incomplete and do not use it to rule a product out.

## When nothing is found

Say "No matching FDA enforcement report was found for [the terms searched]." Then say why that is not an all clear:

- Enforcement reports are posted weekly and can appear weeks after a company announces a recall.
- Meat, poultry and certain egg products are regulated by USDA and are not in this data. A USDA mark of inspection on the package means the product belongs to that system. Point to https://www.fsis.usda.gov/recalls.
- The search matches an exact phrase in the record. A different spelling, a different word order or a private label name can miss.

For a recall in the news that is not here yet, point to FDA's public notices at https://www.fda.gov/safety/recalls-market-withdrawals-safety-alerts and to the store or the manufacturer.

## Errors

- **"No FDA food recall with number ..."** The number is wrong or was retyped. Take it again from a search result.
- **A tool error or timeout.** Try once more. If it fails again, say the lookup is unavailable and give the two official pages above.

## What the answer looks like

```text
Result: [Match / Not a match on <field> / Cannot tell yet]

Your package: [name, size, UPC, lot code, best by date as the user gave them]
FDA record [recall number], [firm]
- Product: [the identifying part of the description]
- Lot codes: [as recorded]
- Reason: [as recorded]
- Class: [as recorded]
- Distribution: [as recorded]
- Recall started: [date] · Reported by FDA: [date] · Status at time of report: [status]
- Record: [link]

What settled it: [the field and the two values]
Next step: [see below]
```

Next step wording:

- For a match: do not eat it. Keep it away from others and follow the recalling company's instructions for disposal or refund. Those instructions are in the company's notice or at the store. They are not in the enforcement record.
- For no match or cannot tell: what to check, and the official pages for anything newer than this data.

Keep "Recall started" and "Reported by FDA" under separate labels. "Status at time of report" describes the company's recall process. It does not say whether packages are still in stores or in kitchens.

The record link opens the raw government data for that recall. It is not a consumer notice.

## Limits to state honestly

- The data is FDA food enforcement reports, updated weekly. Field meanings are in [references/record-fields.md](references/record-fields.md).
- It covers foods FDA regulates, including supplements and pet food. It does not cover USDA regulated products, recalls not yet classified by FDA, or state and local actions.
- Lot codes and distribution are free text written for each recall. They are not standardized.
- This is information from public records, not medical advice and not an official determination. The recalling company and FDA are the authorities on what is covered.

## Examples

**"Is my Donutful chocolate donut box recalled? UPC 850041392103."**
Search `Donutful`, then `850041392103`. Open the match with `get_food_recall`. The record lists one lot and one best by date, so ask the user to read the lot number and best by date from the carton. If both agree, the result is Match. If the carton shows a different lot, the result is Not a match on lot code for that recall number.

**"I heard there was a cheddar recall. I have raw milk cheddar from a farm in New York."**
Search the farm or brand name first, then `cheddar`. Open each candidate. If a record says "No lot code" and gives a best by date, ask for the best by date and package size, and compare those.

**"Was Brand X chicken salad recalled?"**
Search the brand. If nothing fits, explain that products with meat or poultry usually fall under USDA, ask whether the package carries a USDA mark of inspection, and point to the FSIS recalls page. Do not say the product is fine.
