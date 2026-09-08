# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured browse page ignores the featured filter, so the complete featured set cannot be viewed
    - Actual: Marking/unmarking does update the homepage strip (new featured "QA Zen Desk Lamp" only appeared after Sony WH-1000XM5 was unfeatured, since "Editor's Top Picks" is capped at 3). But the "View all" link (/products?featured=true) renders "All Products" with "10 products found" listing every product including non-featured ones (Samsung, MacBook, Bowflex, Theragun, Breville, Philips Hue), so with 4 featured products there is no way for a visitor to browse the complete featured set.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choices are a hard-coded static list; newly assigned tags never appear
    - Actual: Added tag "qafilter-tag" to the existing product Theragun Pro (saved and visible on its detail page and via keyword search). The Filters panel on /products still lists exactly the same 35 seeded tags: "qafilter-tag" (and "deep-massage", plus the new product's "qatag-desk"/"qatag-lighting") are absent, so the tag cannot be selected as a filter. Conversely the removed/renamed tags "massage", "therapy" and "recovery" are still offered; selecting "massage" yields "0 products found".

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter applies a stale value (one change behind), returning products cheaper than the selected minimum
    - Actual: Sorting works: Newest, Top Rated (4.9→4.4), Price Low→High ($199→$2,495), Price High→Low ($2,495→$199) all order correctly, and "Minimum Rating: 4.5+ stars" correctly returns 8 products (excludes Philips Hue 4.4). But the Price Range slider is off by one step: with the panel showing minimum $600, results (7) still included Theragun Pro $599 and Bowflex $549; at minimum $700, results (5) still included Breville Barista Express $699. Resetting the slider to $0 still showed only 4 products (still filtering at $700) until another change event occurred.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin changes are lost on page reload (in-memory state only)
    - Actual: After reloading http://localhost:6001/products, the list is back to "9 products found" and the created product is gone (/product/qa-zen-trainer-mat shows "Product Not Found"). Edits also reverted: Theragun Pro's tags are back to recovery/massage/muscle/therapy instead of the saved deep-massage/muscle/qafilter-tag, and the unfeatured Sony WH-1000XM5 is featured on the homepage again. localStorage is empty, confirming nothing is persisted. (Earlier, two other created products also vanished after a reload.)


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results violate the active price-range criterion
    - Actual: Category+tag+search combinations are correct (tags laptop+coffee -> MacBook & Breville, OR semantics; adding category Home Goods -> only Breville, "1 product found"). However with the Price Range panel showing minimum $700, the result set still included Breville Barista Express at $699; with minimum $600 it included Theragun Pro $599 and Bowflex $549. Results therefore do not satisfy the active price criterion.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Product can be published without image, retailer link, full review or specifications; validation failures give no feedback
    - Actual: Created "QA Minimal Product" with only a name, category and short description (price left at 0, rating 4.5, Image URL, Affiliate Link, Full Review, Tags and Specifications all empty) — toast "Product created successfully". Its detail page /product/qa-minimal-product renders an empty image (src resolves to the page URL), an empty "Key Specifications" section, an empty "My Review" section, and a "Buy Now" button whose href is the page itself instead of an external retailer. Price/rating validation does exist (price -50 / rating 9 blocked submission) but blocked submits show no error message at all — the Create button silently does nothing.

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