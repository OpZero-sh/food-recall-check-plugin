# Notes for recall roundups

Background for summarizing lists of FDA food recall reports without overstating what they show.

## What a count means

- The header count is the number of enforcement records in the window. Each product, size or variety in a recall event has its own record, so one event with twelve products is twelve records.
- Counts say nothing about how many people were affected. Records carry no illness or injury numbers.
- Reports are published weekly. A recall is reported after FDA classifies it, usually weeks after the firm began it. A window measures when FDA reported, not when the product was sold or recalled.
- For those reasons, do not present week to week or month to month counts as a trend in food safety.

## Grouping that helps a reader

1. Merge records from the same firm with the same reason and report date into one entry, and list the recall numbers together.
2. Lead with Class I.
3. Within a class, group by the kind of reason. The reason text is free text, so group by its plain meaning:
   - undeclared allergen, for example milk, peanuts, tree nuts, egg, soy, wheat, fish, shellfish, sesame
   - contamination with a named organism, for example Listeria monocytogenes, Salmonella, E. coli
   - foreign material, for example metal, plastic, glass
   - other, for example labeling errors, temperature abuse, unapproved ingredients
4. Keep the firm's state on the line. Distribution is only in the full record, so do not imply a recall is local or national from the firm's address.

## What to carry on each line

- firm and state
- the identifying words of the product, including size when the list shows it
- reason as recorded
- class
- report date
- recall number or numbers
- record link

Product descriptions in lists are cut short with an ellipsis. Open the record with `get_food_recall` before quoting sizes, UPCs or lot codes.

## Wording

- "FDA reported" for the report date. "The firm began the recall on" for the start date from the full record.
- "Class I", "Class II", "Class III" as recorded, with FDA's meaning the first time each appears.
- "Not in this data" for USDA regulated products and for recalls announced but not yet classified.
- Never "safe", "cleared" or "no risk" for products that are absent from the list.

## Regular roundups

When the user wants the same roundup on a schedule, keep the window slightly longer than the interval, for example 14 days for a weekly check, and note entries already seen by recall number so repeats can be dropped. This plugin does not schedule anything or store past results. The user's client decides whether that is possible.
