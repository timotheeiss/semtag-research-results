# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured collection cannot be browsed in full; "View all" featured link ignores the featured filter
    - Actual: Toggling featured does update the homepage strip (unfeaturing Dyson removed it and newly featured "QA Aeris Air Purifier Pro" took its slot), but the strip is capped at 3 items, so with 4 featured products one is hidden. The section's "View all" link goes to /products?featured=true, which renders "10 products found" — the entire catalogue including non-featured items — and the products page offers no featured-only filter, so the complete featured set is never browsable.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choices are static and ignore newly assigned tags
    - Actual: After editing Theragun Pro's tags to "recovery, massagegun, qanewtag" (and creating a product tagged "qatestair, purifier"), the /products Filters tag list still shows only the original seed tags — qanewtag, massagegun, qatestair and purifier are absent, so the new tag cannot be selected as a filter. The list also still offers removed tags (muscle, therapy, massage); selecting "muscle" yields "0 products found".

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin changes are lost on page reload
    - Actual: After reloading, /product/qa-aeris-air-purifier-pro shows "Product Not Found" and /products is back to "9 products found" (the created product is gone). The Theragun Pro tag edit reverted to the seeded "recovery, massage, muscle, therapy", and the unfeatured Dyson is featured again. localStorage and sessionStorage are both empty — state is held only in memory.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Missing publish validation: incomplete products and negative prices can be saved
    - Actual: Only Name, Category and Short Description are validated ("Please fill in all required fields" on empty submit). Created "QA Incomplete Widget" with empty Full Review, empty Image URL, empty Affiliate Link and zero specifications — it was accepted ("Product created successfully") and now appears in admin/browse. Editing it to Price = -50 also saved and the card renders "-$50". Only the rating field is constrained (native min=0/max=5 blocks 9).

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [X] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.