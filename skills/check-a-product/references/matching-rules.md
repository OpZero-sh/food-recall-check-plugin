# Matching rules

How to compare a package with an FDA enforcement record. The record's wording decides. When the wording is unclear, the result is "cannot tell yet", never a guess.

## Order of comparison

1. **Product identity.** Brand, product name, variety or flavor, and form (frozen, canned, dried) should fit the record's product description. Records describe the label in the company's words, often in capitals and with packaging details.
2. **Package size.** Records usually name the sizes covered, for example "8oz. (226g)" or "7.05oz/10ct". A different size of the same product is a different item and often has its own recall number or is not recalled.
3. **Code information.** The `Lot codes` line lists what identifies the affected production: lot or batch numbers, best by or use by dates, sometimes both, sometimes UPCs.
4. **UPC.** When the record gives a UPC, usually inside the product description, it confirms the product line and size.
5. **Distribution.** Use it as supporting context only.

A match needs steps 1 and 2 to fit and step 3 to be satisfied.

## Reading the `Lot codes` line

The line is free text. Forms seen in records:

| Record says | How to apply it |
| --- | --- |
| One lot and one date, such as "Lot # 26078 BEST BY: 09/15/2026" | The package must show that lot. If the package shows the lot, the date should agree. If the user can read only one of the two, say which one you matched |
| "No lot code" with a best by date, such as "No lot code, Best By JUL 22 27" | The product carries no lot number. Compare the best by date and size |
| A list of lots or dates | The package's code must be one of the listed values |
| A range, such as two lot numbers or two dates joined by "through" or "to" | Apply the range only when the format is plainly a number or a date sequence and the user's value has the same format. Otherwise the result is "cannot tell yet" |
| "All lots", "all codes", or all product within a date window | The product identity and size decide. Any lot is covered |
| A line that ends in an ellipsis or says "see attached" or similar | The list is incomplete in this data. Do not rule anything out. Send the user to the company's notice or FDA's public notice |

## Comparing codes

- Compare character by character. Keep leading zeros, letters, spaces and punctuation as printed.
- Ask the user to read the code twice when it contains characters that are easy to confuse: 0 and O, 1 and I and L, 5 and S, 8 and B.
- Dates appear in many styles. "09/15/2026", "SEP 15 2026" and "15SEP26" can be the same date. "JUL 22 27" reads as July 22, 2027. When day and month order is ambiguous, say so and ask the user to read the label exactly.
- Some lot codes are production dates in a compact form. Do not decode a lot code into a date unless the record explains the format.
- A code on the user's package that is close to a listed code is not a match.

## UPC

- A UPC is the 12 digit number under the barcode. It identifies the product and size, not the production run.
- A UPC that matches the record, with no lot or date to compare, gives "cannot tell yet" unless the record covers all lots.
- A UPC that differs from the record's UPC usually means a different size or variety. Say "not a match on UPC for this recall number" and check whether another recall number covers the user's size.
- Searching the UPC digits with `search_food_recalls` finds records that print the UPC in the product description. A miss on a UPC search proves nothing, because many records leave the UPC out.

## Distribution

- The `Distribution` line is free text: "nationwide", a list of states, named retailers or distributors, or countries.
- Many records add that product "may be further distributed". Online sales and resale move product beyond the listed states.
- A package bought in a listed state is a little more likely to be affected. A package bought elsewhere is not excluded.
- Never give "not a match" on distribution alone.

## One company, several records

A single recall event often produces several recall numbers, one for each product or size. Open each one. Report results per recall number, and do not merge code lists across records.

## What to say for each result

- **Match:** "The product, the size and the [lot code / best by date] on your package are the ones in FDA record [number]."
- **Not a match on a field:** "Your package shows [value]. FDA record [number] lists [value]. On that basis your package is not in this recall number." Add what else you checked.
- **Cannot tell yet:** "To settle this I need [field], printed [where]." or "The record's code list is incomplete here, so check the company's notice."

In every case, name the record and give its link, and remind the user that the recalling company and FDA are the authorities on what is covered.
