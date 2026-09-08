# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete set of featured products
    - Actual: Homepage "Featured" section always shows only 3 items even when more products are marked featured (verified 4 featured items existed: Sony, Peloton, Dyson, plus a newly featured product). The "View all" link under Featured navigates to /products?featured=true, but that query parameter is ignored by the products page — it shows all 10 products (unfiltered) instead of just the featured ones, so visitors have no way to browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag not exposed as a filter option
    - Actual: Added tag "qatag1" to existing product "Philips Hue Starter Kit v2" via admin edit; it saved correctly and shows on the product detail page, but the products page's tag filter list (35 tag options) does not include "qatag1" even after toggling the filter panel closed/open again.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product data is not persisted server-side; lost on full page reload
    - Actual: Created a new product via the admin form ("QA Test Gadget"); it appeared correctly in the admin list and public product pages while navigating via in-app links. After performing a full page reload/navigation (page.goto) to /admin, the newly created product had completely disappeared from both the admin panel and the public product list, reverting to only the original 9 seeded products. Edits/additions do not survive a page reload or later visit.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: No publish-readiness validation on product creation
    - Actual: Created a product with only Name and Short Description filled (no image URL, no affiliate/retailer link, no full review, no specifications, price left at default $0). The admin form accepted and saved it without any validation error, and the product immediately became publicly visible on /products ("Incomplete Test Product", $0), violating the requirement that a product cannot be published until it has all required content.

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