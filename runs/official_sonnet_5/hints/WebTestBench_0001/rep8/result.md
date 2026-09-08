# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Homepage featured collection does not update when a product is newly marked as featured
    - Actual: Created/edited a product with Featured toggle ON (visible as "Featured" badge in admin list and on its detail page related items), but the homepage Featured section (home.featured) kept showing only the original 3 seeded featured items (Sony WH-1000XM5, Peloton Bike+, Dyson V15 Detect) even after re-navigating to Home from other pages. The new featured product never appeared there.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Newly assigned tag on existing product does not become available as tag-filter choice
    - Actual: Edited a product's tags to include "renamed-tag" and "edited-tag" (confirmed saved on detail page and matched by keyword search). Opened the advanced Filters panel on /products: the tag-filter list only shows the 35 original seeded tags (ambiance, android, apple, ... wireless) and never includes "renamed-tag" or "edited-tag", even after navigating away and back to the products page.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Newly created/edited products are not persisted; a page reload reverts the catalog to the original seed data
    - Actual: After creating "Test Gadget Pro Updated" and "FT5 Fitness Widget" (verified present, 11 products total on /products), a full browser navigation/reload to another URL caused /products to revert to "9 products found" — the original seeded set only. Both admin-created/edited products were lost.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Products can be published/created without required content (image, retailer link, specification)
    - Actual: Created a product with only Name and Short Description filled (left Image URL, Affiliate Link, and Specifications entirely empty, price defaulted to $0). The admin form accepted it with no validation error, and the product ("Incomplete Product Test") immediately appeared live on the public /products browsing page with no image, no working retailer link, and no specs — violating the requirement that a product needs a valid image, valid retailer link, and at least one complete spec before being publishable.

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