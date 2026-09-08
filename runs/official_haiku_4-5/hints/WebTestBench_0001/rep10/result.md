# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [X] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Newly assigned tags do not appear as filter options. Assigned new tag 'newtag123testfilter' to existing product (Minimal Test Product), saved changes, but tag does not appear in tag filter dropdown.
    - Actual: Tag filter options remain unchanged after adding new tag to product. Tag list still contains only original tags (ambiance through wireless alphabetically). New tag 'newtag123testfilter' is not in the filter options.

- [ ] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.
  - Bug Report:
    - Issue: New tags assigned when creating products don't participate in tag-based filtering. Tag filter only includes pre-existing/seeded tags, not user-created tags.
    - Actual: New tags assigned during product creation are not indexed/available for filtering. Tag filter options remain limited to original seeded tags only.

- [ ] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.
  - Bug Report:
    - Issue: Modified tags on existing products don't appear in tag filter options or participate in filtering. Tag system is static to seeded tags only.
    - Actual: Assigned new tag 'newtag123testfilter' to existing product and saved changes. New tag does not appear in tag filter dropdown on products page, confirming tag modifications don't update filter availability.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Newly created products do not persist after page reload. Test Wireless Earbuds and Tag Test Product disappeared after navigation/reload. Only edits to existing products persist. Minimal Test Product created but not verified after reload due to persistence issue pattern.
    - Actual: Created multiple products (Test Wireless Earbuds, Tag Test Product) that appeared immediately in admin list and products page, but disappeared after page reload or navigation to different views. Products vanished from both admin panel product list and browsing views. Changes to existing products (Sony category change) did persist correctly.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Product created without validation of required fields. Submitted form with only name and description filled, missing image, affiliate link, full review, and specifications. Product was created and appears in browsing view with $0 price.
    - Actual: Minimal Test Product created with: name='Minimal Test Product', price=$0, description='A minimal product', category='Tech Gadgets', rating=4.5 (default), no image, no affiliate link, no review, no specifications. Product successfully saved and displays in products list.

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