# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured "View all" link does not filter to featured items
    - Actual: Homepage Featured section correctly shows/updates featured products (marking/unmarking a product as Featured via admin toggle immediately adds/removes it from the homepage Featured section). However, clicking "View all" navigates to /products?featured=true, which displays all 9 products (unfiltered) instead of only the featured ones - the featured=true query parameter is not applied as a filter on the All Products page.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list not updated with new tags
    - Actual: After adding new tags ("adjustable-weights", "qa-edit-tag") to a product via admin, the tags correctly appear on the product's detail page and are matched by keyword search, but the Filters panel's tag chip list (34 alphabetically sorted tags) was not updated to include the new tags, making them unavailable for tag-based filtering.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range minimum filter does not correctly filter results
    - Actual: Sort by Newest and Top Rated work correctly (Top Rated correctly ordered products descending by rating: 4.9,4.8,4.7,4.7,4.6,4.6,4.5,4.5,4.4). Minimum Rating filter (e.g. "4.5+ stars") correctly excludes lower-rated products. However, the Price Range minimum slider is broken: after raising the minimum to $610 via keyboard (slider showed "$610" to "$2495"), the result set still included Bowflex SelectTech 552 ($549) and Theragun Pro ($599), both priced below the $610 minimum threshold - only one product (Sony WH-1000XM5, $399) was actually excluded when count dropped from 8 to 7, indicating the price filter does not properly apply the minimum threshold to all products.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence across page reload
    - Actual: Created/edited products exist only in in-memory React state. After a full page reload/revisit (browser_navigate causing full reload) of admin panel or public pages, newly created/edited products disappeared entirely - changes are not persisted to any backend/storage.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [X] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.

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