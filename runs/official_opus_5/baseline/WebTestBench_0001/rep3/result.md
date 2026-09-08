# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Complete featured set is not browsable; homepage featured collection hard-capped at 3
    - Actual: Marking Philips Hue as featured gave 4 featured products, but "Editor's Top Picks" still rendered only 3 (Sony, Peloton, Dyson) — the newly featured product did not appear. Its 'View all' link points to /products?featured=true, but that page ignores the featured query param: it showed 'All Products' / '9 products found' including non-featured items (Samsung, MacBook, Bowflex, Theragun, Breville), and /products offers no featured filter control. So visitors can never browse the complete featured set. Toggling does affect the homepage within the 3-slot cap: after unmarking Sony, Top Picks became Peloton, Dyson, Philips Hue Starter Kit QA.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is static and ignores tags on newly added products
    - Actual: With 'QA Smart Desk Lamp' present in the catalog ('10 products found') and its tags 'qazephyr'/'adjustable' shown on its detail page and matchable by keyword search, the /products Filters > Tags list still offers only the 35 seed-derived tags (ambiance, android, apple, ... wireless). 'qazephyr' and 'adjustable' are absent, so the newly assigned tag can never be selected as a tag-filter choice.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin additions and edits are lost on reload
    - Actual: Product data is held only in in-memory React state; localStorage, sessionStorage and cookies are all empty (0 keys). After reloading, the created product 'QA Smart Desk Lamp' was gone from /admin and /products, and /product/philips-hue-starter-kit-qa returned 'Product Not Found'. All edits reverted to seed values: the admin list is back to the original 9 products with 'Philips Hue Starter Kit' at $199/4.4 and Sony re-marked as Featured (its featured flag had been turned off before the reload). This also silently destroyed work mid-session when the header search form did a full navigation.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Incomplete/invalid product can be published; missing required-content validation
    - Actual: Created 'QA Invalid Product' with price -50 (rendered as '-$50'), no image URL, no affiliate/retailer link, no full review, and zero specifications. Only Name, Category and Short Description are enforced. The price input has no min attribute so negative values are accepted. (Rating is the only correctly bounded field: min=0/max=5 blocked a rating of 9.)

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