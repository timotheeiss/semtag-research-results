# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete set of featured products
    - Actual: Marking/unmarking a product as featured works dynamically: turning off Sony WH-1000XM5's featured flag removed it from home.featured and QA Test Smart Speaker (also featured) surfaced in its place. However, home.featured is hard-capped at 3 items with no pagination, and the section's 'View all' link (home.featured.view-all) navigates to the generic /products page, which is unfiltered and has no 'featured' filter option (category/sort/min-rating/tag filters present, but no featured toggle). Therefore visitors cannot actually browse the complete set of featured products if more than 3 exist.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag not added to tag-filter choices
    - Actual: Added a brand-new tag "qa-newtag123" to existing product Sony WH-1000XM5 and saved. The tag correctly appeared on the product detail page and in keyword search, but it never appeared as a selectable chip in the Products page "Tags" advanced filter list (list remained the same fixed ~35 tags before and after the edit). Since the tag never becomes a filter choice, it cannot be selected to filter products.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range slider is broken / not usable as a dual-range control
    - Actual: Sort by Newest, Top Rated, Price Low-to-High, and Price High-to-Low all worked correctly, and the Minimum Rating dropdown (4.5+ stars) correctly filtered to 8/10 products. However the "Price Range" control only exposes ONE focusable/interactive slider thumb (role=slider, aria-label="Minimum") in the DOM; there is no visible/focusable maximum thumb even though state tracks both filters.price-min and filters.price-max. Keyboard interaction (ArrowRight/ArrowUp) on the thumb moved the value from $0 to $10 once, then became completely unresponsive to further key presses. A user cannot reliably set a price range through the exposed control.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin changes do not persist across page reload
    - Actual: After creating a new product (QA Test Smart Speaker) and unmarking Sony WH-1000XM5's featured flag (saved via admin form), a full page reload/re-navigation to http://localhost:6001/ reverted both changes: Sony WH-1000XM5 reappeared as featured, and the QA Test Smart Speaker product disappeared entirely from home.featured. State is held only in-memory client-side with no backend persistence.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Product can be published without at least one complete spec
    - Actual: The admin form correctly blocks submission when required text fields (name, description) are empty, and correctly blocks submission when price is negative (-50) or rating is out of the 0-5 range (9.9) — the form stays open with no new product created in these cases. However, when all required text fields plus a valid price/rating were provided but zero specification rows were added, the product ('CS20 Test Product') was created successfully and is fully published: it appears in the admin list, the public /products grid, and its own detail page at /product/cs20-test-product with an empty product.specs collection. This violates the 'at least one complete spec' publishing requirement.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category filter dropdown on category pages does not change selection
    - Actual: On /category/tech, the category filter dropdown is available and shows "Tech Gadgets". Selecting "Fitness" or "All Categories" from the dropdown does not change the dropdown's displayed value (remains "Tech Gadgets") and does not update the displayed products (still shows only the 3 tech products). The dropdown is effectively locked to the page's URL category. Note: switching category via the top navigation links (Tech/Home/Fitness) does work correctly and updates the products, but the category-filter control itself (the one offered directly on the category page, matching the checklist wording) is non-functional.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.