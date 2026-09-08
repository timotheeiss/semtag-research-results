# Test Result

## Functionality
- [ ] FT-1: Supports the creation/editing of product information for technology, home furnishings, and fitness equipment.
  - Bug Report:
    - Issue: Persistence bug: admin product creation does not persist
    - Actual: Created 'QA Test Blender 3000' (Home category, $129) via Admin Panel's Create Product form. Success toast shown and product appeared instantly in admin grid (count went 9->10). However, it never appeared on /products (still 9 products), /category/home (still 3 products), or homepage. Navigating directly to /product/qa-test-blender-3000 returned 'Product Not Found'. After navigating away and back to /admin (full reload), the product had vanished even from the admin's own list (reverted to 9). Testing across all 3 categories is moot since the base create operation itself does not persist.

- [ ] FT-2: Supports marking specific products as featured on the homepage and displaying them.
  - Bug Report:
    - Issue: Persistence bug: admin product edits do not persist
    - Actual: Edited Sony WH-1000XM5 via Admin Edit Product form: changed price 399->379 and added tag 'qa-tested'. Success toast shown and admin grid immediately reflected $379. Navigating to the live product detail page /product/sony-wh-1000xm5 (fresh load) showed price reverted to $399 and tags reverted to the original 4 (no 'qa-tested'). Edits only affect in-memory admin state, never reach the actual data store read by public pages.

- [X] FT-3: It provides a product keyword search function, covering multi-dimensional information.

- [ ] FT-4: Users can add product tags to any existing product.
  - Bug Report:
    - Issue: Persistence bug: tag additions via admin edit are not saved
    - Actual: Added tag 'qa-tested' to Sony WH-1000XM5 through the Admin Edit Product form. Toast confirmed success and tag appeared in admin grid immediately, but the live product detail page (/product/sony-wh-1000xm5) after a fresh navigation still showed only the original 4 tags with no 'qa-tested' tag added or persisted anywhere (search/tag filters on /products also do not surface it).

- [ ] FT-5: Users can create product labels.
  - Bug Report:
    - Issue: Persistence bug: featured flag on newly created product does not persist
    - Actual: Created 'QA Test Blender 3000' with Featured=true via the admin form. Because the create operation itself does not persist (see FT-1), the featured flag never reached the homepage Featured section, /products, or /category/home — the product disappeared entirely on reload, so its featured status could never be verified as displayed.

- [ ] FT-6: Users can edit any existing product label.
  - Bug Report:
    - Issue: Persistence bug: edits to product spec/price/rating fields do not persist to storefront
    - Actual: Editing Sony WH-1000XM5's price via the admin Edit form updated the admin grid instantly (399->379) but the change never reached the live product detail page, which continued to show the original $399 after a fresh page load, confirming edits are not written to the data store consumed by the storefront.

- [ ] FT-7: Users can categorize any existing product labels.
  - Bug Report:
    - Issue: No dedicated tag categorization mechanism; also blocked by persistence bug
    - Actual: The admin Create/Edit Product forms only expose a free-text tags field (comma-separated) with no mechanism to browse existing tags, categorize/group tags, or manage a tag taxonomy separately from a product. Additionally, any tag changes made do not persist past a page reload (same root cause as FT-4), so tag management is non-functional end-to-end.

- [X] FT-8: Users can filter products by category (such as technology products, home furnishings, fitness equipment, etc.).

- [X] FT-9: Users can browse the full product list, which displays key information, brief reviews, and purchase links for each product.

- [X] FT-10: Users can sort and filter products by price range or rating attributes.


## Constraint
- [X] CS-11: Product categories are limited to three vertical sectors: technology, home furnishings, and fitness equipment.

- [X] CS-12: After a user applies filtering or sorting criteria, the product results displayed by the system must strictly match the selected criteria.


## Interaction
- [X] IX-13: After switching sorting/filtering criteria, the product list updates the matching results in real time.

- [X] IX-14: Clicking on a product will redirect you to its own details page without any lag.

- [X] IX-15: The external retailer link button on the product details page redirects to the target page normally.


## Content
- [X] CT-16: The product details page fully displays reviews, specifications, and retailer links.

- [X] CT-17: Product categorization and label matching are accurate, with no mislabeling or omissions.

- [ ] CT-18: The core content of the same product details remains consistent across different views. The detailed view displays complete and accurate conditions and final results without any truncation or data loss.
  - Bug Report:
    - Issue: Data inconsistency between admin view and public views due to persistence bug
    - Actual: After editing Sony WH-1000XM5 price and tags in admin, the admin grid showed the new values ($379, +qa-tested tag) while the public product detail page simultaneously showed the old, unmodified values ($399, original tags) — core content is inconsistent across views because admin changes never propagate to the underlying data store.