# Search tips for `search_food_recalls`

How the search behaves and how to get useful results from it. Based on the tool schema and on live results.

## Behavior

- `query` is 1 to 60 characters. It is matched against the product description, the reason for recall and the company name.
- A query of several words is matched as an exact phrase. The words must appear next to each other, in the same order. Examples from live searches: `undeclared milk` matches about 1,700 records and `milk undeclared` matches 18. `Family Farmstead` finds a record whose label reads "FAMILY FARMSTEAD MILD RAW CHEDDAR", and `Farmstead cheddar` does not. `listeria` matches thousands of records and `listeria cheese` matches none.
- Letter case does not matter.
- Results are newest first by report date. There is no relevance order and no paging.
- `limit` is 1 to 25, default 10.
- `class_one_only: true` keeps Class I records only. There is no filter for Class II or III alone, for dates, or for states.
- The header reads "N FDA food recall reports mention [words], newest n shown". N counts records across the whole data set.
- Product descriptions in results are cut at about 160 characters with an ellipsis.
- A search with no matches answers "No FDA food recall reports mention [words]" with a reminder that a missing record cannot establish whether a product is safe.

## Choosing words

| Looking for | Try | Why |
| --- | --- | --- |
| A brand | The brand name alone | Brand names appear in the product description |
| A company | The distinctive part of the name, without "Inc", "LLC" or "Company" | The firm field holds the legal name, which may differ in punctuation |
| A store brand product | The store's brand name, then the manufacturer named on the package | The recalling firm is often the manufacturer |
| An allergen | "undeclared" plus the allergen | Reasons are usually worded "Undeclared milk", "Undeclared peanuts" |
| An organism | The genus or common report wording: "Listeria", "Salmonella", "E. coli" | Reasons name the organism |
| A food type | One plain word: "cheese", "granola", "salmon" | A single word matches wherever it appears |
| A specific product | The brand alone, then read the results for the product | A brand plus a product word matches only when the record prints them side by side |
| An organism in a food | The organism alone, or the food alone, then filter the results by reading them | Two ideas in one query are treated as one phrase and usually match nothing |
| A UPC | The digits alone | Works when the report printed the UPC in the product description. Many reports do not |

## When a search returns too much

- Use a longer exact phrase taken from the label or from a result you already have.
- Set `class_one_only` when the user cares about the most serious class.
- Remember that the tool shows only the newest 25. Older records are reachable only with narrower words.

## When a search returns nothing

1. Remove words until one distinctive word is left. Most empty searches come from a phrase that no record prints in that exact order.
2. Try the singular and plural forms and common spellings.
3. Try the other name for the same thing: brand in place of company, ingredient in place of product.
4. If it is still empty, report the queries tried and say the data does not contain a matching record. Do not say there was no recall.

## Reading matches with care

- A word can match in a part of the record you did not intend. A company search can hit a record where the name appears in a "distributed by" line inside another firm's product description. Read the `Firm` line.
- An allergen word can match a product description, not the reason. "Milk" matches milk chocolate products recalled for other reasons. Read the `Reason` line.
- The same allergen or organism appears under different classes in different records. Report each record's own class.
- Several results from one firm on one report date are usually one event split by product.
