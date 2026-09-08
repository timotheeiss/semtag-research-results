# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured collection is not fully browsable; homepage caps featured at 3 and the "View all" featured link ignores the featured filter
    - Actual: Toggling featured does update the homepage strip (un-featuring Dyson removed it and QA Aeropress Go appeared). However with 4 featured products the homepage showed only 3 (the newly featured QA product was hidden), and home.featured "View all" → /products?featured=true renders the unfiltered catalogue ("10 products found", all products incl. non-featured), so the complete featured set can never be browsed.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter vocabulary is static; newly assigned tags never become filter choices
    - Actual: After adding tags "deep-cleaning" and "qatag-robotic" to the existing Dyson V15 Detect (saved, visible on its detail page and via keyword search), the /products tag filter still lists the same 35 hard-coded seed tags — no "deep-cleaning"/"qatag-robotic" chip exists, so the new tag cannot be selected. It still offers removed tags "cleaning"/"cordless", which now return 0 products. Tags added when creating a product ("qatag-travel", "qatag-compact") are likewise absent.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter applies wrong/stale boundary and has no maximum handle
    - Actual: Sorting (Newest/Top Rated/Price asc/desc) and the min-rating filter work correctly. But with the price slider showing $600–$2495 and no other filter active, the grid still lists Theragun Pro $599 and Bowflex SelectTech 552 $549 (7 products found) while excluding $399/$199 — i.e. the effective cut-off is ~$500, one step behind the displayed value, and it never self-corrects. The range slider also renders only a "Minimum" thumb, so a maximum price cannot be set.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin additions and edits are lost on page reload
    - Actual: After reloading, /product/qa-aeropress-go-xl shows "Product Not Found" and the admin list is back to the 9 seeded products with Dyson's price reverted from the saved $729 to $749 (tags and featured flag also reverted). localStorage and sessionStorage are both empty, so state lives only in memory for the current page session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Results violate the active price-range criterion
    - Actual: Search, category and multi-tag criteria are respected (tags OR-combined: wireless+coffee → Sony + Breville; category tech → only Tech items; "cleaning" after tag removal → 0). But with the price filter showing $600–$2495 and no other filter active, the grid still returned Theragun Pro $599 and Bowflex SelectTech 552 $549 (7 products found); after resetting the handle to $0 the grid showed only 5 of 9 products, i.e. the applied price bound lags one interaction behind the displayed value.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only covers name/category/description; price, review, image, retailer link and specifications are unvalidated
    - Actual: A product ("QA Invalid Probe") was created successfully ("Product created successfully") with price = -50, empty Image URL, empty Full Review, affiliate link = "not-a-url" (plain text field, no URL validation) and zero specification rows. Only an empty name/description triggers "Please fill in all required fields"; rating is the sole numeric field bounded (HTML min=0/max=5), price has no min attribute.

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