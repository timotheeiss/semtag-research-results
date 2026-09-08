# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured "view all" does not filter to featured products; homepage featured preview is capped and doesn't surface all featured items
    - Actual: With 4 products marked featured (sony, peloton, dyson, bowflex), homepage preview still only showed 3 (sony/peloton/dyson), and clicking "View all" navigated to /products?featured=true which displayed all 9 products (unfiltered), not just the 4 featured ones. Toggling featured off correctly removed a product from the homepage preview, and toggling on correctly added it back when there was room in the 3-slot cap, but there is no way to browse the complete set of featured products when it exceeds the homepage preview size.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter checkbox list does not include newly assigned tags on existing products
    - Actual: After adding tags "bestseller" and "luxury" to Sony WH-1000XM5 via admin edit (verified reflected on its detail page and matched by keyword search), the Filters > Tags checkbox list on /products still only shows the original static seed tag set and never includes "bestseller" or "luxury", so they cannot be selected as a tag-filter choice.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range minimum filter does not correctly exclude products below the threshold
    - Actual: Sort by Newest/Top Rated/Price Low-to-High all worked correctly (verified full ascending price order and descending rating order). Minimum-rating filter (4.5+) correctly returned only products with rating >= 4.5. However, setting the price-range minimum slider to $610 still included Bowflex SelectTech 552 ($549) and Theragun Pro ($599) in the 7 results, even though both are below the $610 minimum - the price filter is not reliably applied.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin changes are not persisted to any backend/storage; they exist only in-memory for the current SPA session
    - Actual: After creating "Test Gadget Pro" and editing Sony WH-1000XM5's tags/category changes earlier (all correctly reflected while navigating client-side within the SPA), a full page reload of /admin shows exactly the original 9 seeded products with no "Test Gadget Pro" entry — the creation and edits were completely lost.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Price-range minimum filter does not enforce its criterion on all displayed results
    - Actual: With price-range minimum set to $610, Bowflex SelectTech 552 ($549) and Theragun Pro ($599) — both below the threshold — remained in the displayed results, violating the "every displayed result satisfies all active... price... criteria" requirement. Category, rating, tag, and search criteria were each individually verified to correctly constrain results.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Products can be created/published without required content for a usable recommendation
    - Actual: Submitting the admin create form with only Name and Short Description filled (leaving Image URL, Affiliate Link, Full Review, and Specifications all blank) succeeded — "Incomplete Product" was created ($0 price, 4.5 default rating, no image/link/specs) and immediately appeared in the public /products listing (count went 9→10). Only Name and Description are enforced as required; image, retailer link, review, and at least one specification are not enforced before publishing.

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