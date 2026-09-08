# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete featured set; homepage featured strip is capped at 3 so a newly featured product can be omitted
    - Actual: Unmarking Sony WH-1000XM5 correctly removed it from the homepage "Editor's Top Picks" and the newly featured "QA Trail Runner Vest" then appeared. However, while 4 products were featured the homepage showed only the original 3 and omitted the newly featured vest. The section's "View all" link goes to /products?featured=true, but that page ignores the featured parameter and renders "10 products found" (every product, featured or not); the Filters panel offers no featured filter, so the complete featured set cannot be browsed.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter choices are hardcoded to the seeded tag set and do not reflect tag changes
    - Actual: Added tag "qa-percussion" to existing product Theragun Pro (saved successfully; it shows on the product detail page and is matched by keyword search). The /products Filters panel still lists exactly the original 35 seeded tags — "qa-percussion" and "deep-tissue" are absent, so the new tag cannot be selected as a filter. Conversely, removed tags "massage" and "therapy" remain offered as filter choices. Tags on the newly created product (qa-hydration, qa-trail) are likewise missing from the list.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product data is not persisted; all admin changes are lost on reload
    - Actual: Created "QA Minimal Product" via /admin (toast "Product created successfully", 10 products listed). After navigating to its detail URL the app reloaded and showed "Product Not Found"; returning to /admin listed only the original 9 seeded products. localStorage and sessionStorage are both empty (no keys) and no backend API exists (/api/products returns the SPA HTML), so state is in-memory only.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation is incomplete: only name/category/short-description are required, and negative prices are accepted
    - Actual: (1) Submitting with only Product Name + Short Description filled (no Full Review, no Image URL, no Affiliate Link, no specifications) created the product ("Product created successfully"). (2) A product was created with Price = -50, rendering as "-$50" on the admin card; the price input has no min attribute. Only the rating input is guarded (min=0/max=5, blocked rating 9 via native validation). Fully empty submits show "Please fill in all required fields".

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