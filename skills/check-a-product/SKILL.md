---
name: check-a-product
description: "Check whether a specific food label or lot matches an FDA recall, or browse recent recalls for a product, company, or allergen."
---

# Food Recall Check

Ask for the product or brand. If the person has a package, the useful label fields are product name, size, UPC, lot or batch code, and best-by date. Preserve leading zeroes and punctuation. Start searching with what the person has already provided.

1. Use `search_food_recalls` with distinctive product or company terms; try an alternate printed name if needed. For dated browsing use `recent_food_recalls`. Apply the Class I filter only when asked.
2. For every plausible match, call `get_food_recall` with the FDA recall number returned by the search. Compare exact codes, size, date, and description. Keep distinct recall numbers separate even if one company issued them all.
3. Lead with match, mismatch on an explicit field, or more information needed. Quote the shortest identifying portion of the record, state which label field settles the match, and link the FDA record. If code details end with an ellipsis or provide only a partial list, use the linked source and leave the match unresolved until the full list can be compared.
4. Include initiation date and report date under their own labels when relevant. Keep the report's recorded status distinct from current shelf availability. An empty FDA search does not establish a product's safety. For USDA-regulated products, point to the FSIS recalls page. For a breaking recall, the FDA public notices may precede an enforcement report.
