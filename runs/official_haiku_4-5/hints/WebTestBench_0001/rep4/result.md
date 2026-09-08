# Test Result

## Functionality
- [ ] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.
  - Bug Report:
    - Issue: Created product did not persist after page reload
    - Actual: Test products "Test Wireless Headphones" and "Featured Test Product" were created and visible in admin list and products page, but disappeared after navigation and page reload. They no longer appear in admin list or products page, suggesting they were deleted or not properly saved. Possibly due to missing required fields (CS-20), but this should be handled by form validation, not silent deletion.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured status changes do not persist
    - Actual: Marked Theragun Pro as featured (toggle ON), submitted form, and verified featured toggle was ON after submission. Navigated to home page and featured product did not appear in featured section. Returned to admin and checked Theragun Pro - featured toggle is now OFF (false). Featured status change did not persist to database/render on homepage.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tags don't appear in tag filter options
    - Actual: Added tag "new-test-tag-xyz" to Philips Hue product and saved it. Tag was persisted on the product. However, when checking the tag filter list on products page, the new tag does not appear in the available tag filter options. Tag filter still shows only the original 34 tags, not the newly added tag.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [X] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Incomplete products are published without validation
    - Actual: Created product "CS-20 Test Product" with only name and description (missing: full review, image URL, affiliate link, specifications, and potentially other fields). Product was created and appears in public product list showing "10 products found". Form lacks validation to prevent saving incomplete products. Incomplete product should not be published per CS-20 requirements.

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