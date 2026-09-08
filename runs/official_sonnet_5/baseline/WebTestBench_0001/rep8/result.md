# Test Result

## Functionality
- [ ] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.
  - Bug Report:
    - Issue: Admin product edits do not persist to backend
    - Actual: Edited Bowflex SelectTech 552 (price 549→579, rating 4.6→4.8, tags, description, Featured toggle) via admin form; got "Product updated successfully" toast and admin list showed new values immediately. However navigating to /product/bowflex-selecttech-552 and /products via full navigation showed original unmodified values. Re-navigating fresh to /admin also reverted to original values, proving no persistence.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured toggle change not persisted / not reflected on homepage
    - Actual: Toggled Bowflex SelectTech 552 "Featured on homepage" switch ON and saved; homepage Editor's Top Picks section did not include Bowflex, and re-opening admin showed switch reverted to off, confirming the change was never persisted.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [X] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.

- [ ] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.
  - Bug Report:
    - Issue: Newly created admin product does not persist / never appears publicly
    - Actual: Created "Test Smart Scale Pro" (Home Goods, $129, 4.2 rating, tags smart-home/health/newtagxyz) via admin form; got "Product created successfully" toast and it appeared in admin list (10 products). However /products still showed only the original 9 products, /product/test-smart-scale-pro returned "Product Not Found", and re-navigating to /admin showed the product had vanished entirely (back to 9 products).

- [ ] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.
  - Bug Report:
    - Issue: Tag changes made via admin do not persist or propagate to public site
    - Actual: Tags edited on Bowflex SelectTech 552 in admin form were not reflected on the public product page or /products tag filter list after navigation/reload, consistent with the general admin persistence failure (see FT-1).

- [ ] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.
  - Bug Report:
    - Issue: Category reassignment via admin does not persist
    - Actual: Changed Breville Barista Express's category from Home Goods to Tech Gadgets in the admin Edit form and clicked Save Changes. After navigating (full page load) to /category/tech, Breville Barista Express was absent (only Samsung Galaxy S24 Ultra, MacBook Pro 14, Sony WH-1000XM5 shown); the reassignment was not persisted, consistent with the general admin persistence failure.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range filter does not enforce minimum price threshold
    - Actual: All 4 sort options (Newest/Top Rated/Price Low-High/Price High-Low) and the Minimum Rating filter worked correctly, but the Price Range slider failed: setting minimum to $710 (confirmed via aria-valuenow="710" and displayed "$710" label) still returned Breville Barista Express ($699) in the filtered results, violating the stated minimum.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin panel changes (create/edit) are not persisted across navigation/reload
    - Actual: Both edits to existing products and newly created products vanish/revert after leaving and returning to the admin panel or navigating elsewhere, confirming no durable persistence layer backs the admin panel.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Price range filter constraint violated
    - Actual: Same as FT-10: product priced below the selected minimum ($699 Breville shown when min set to $710) is incorrectly included in filtered results.

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