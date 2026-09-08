# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured collection is capped at 3 and the "browse all featured" view ignores the featured filter
    - Actual: Marking the new QA Zen Yoga Mat as featured did not add it to the homepage "Editor's Top Picks" (still Sony, Peloton, Dyson); it only appeared after Dyson was unmarked, i.e. the section shows at most 3 of the featured set. The featured "View all" link navigates to /products?featured=true, but that page ignores the featured parameter and lists all 10 products ("10 products found"), and no featured filter exists elsewhere, so the complete featured set cannot be browsed. (Unmarking Dyson did correctly remove it from the homepage.)

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list is static; newly assigned tags never become filter choices
    - Actual: After editing Theragun Pro's tags to "recovery, deep-tissue, qanewtag" (and after creating a product with tags qatesttag/yoga), the /products tag filter still lists exactly the same 35 original tags: qanewtag, deep-tissue, qatesttag and yoga are absent, while removed tags (therapy, muscle, massage) remain. The new tag therefore cannot be selected as a filter at all.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin changes lost on reload
    - Actual: After creating "QA Zen Yoga Mat (Pro)" and editing Theragun Pro's tags, reloading the app resets everything to the seed data: /product/qa-zen-yoga-mat-pro returns "Product Not Found", admin list is back to the original 9 products, and Theragun Pro's tags revert to recovery/massage/muscle/therapy. localStorage is empty (no keys), so edits exist only in memory for the session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publishing allowed with incomplete/invalid content
    - Actual: Created "QA Incomplete Product" with price -50 and empty image URL, empty affiliate/retailer link, empty full review and zero specifications; the admin form accepted it and listed it as "-$50". Only the rating field enforces 0–5 (HTML min/max); price has no min attribute and image/link/review/specs are not required.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on category page is inert
    - Actual: On /category/home the offered category filter (filters.category, showing "Home Goods") does not respond: choosing "Fitness" and then "Tech Gadgets" from the dropdown left the trigger label "Home Goods", the page heading "Home Goods", the URL /category/home and the same 3 Home Goods products (Breville, Philips Hue, Dyson). Only the /products page category filter and the top-nav category links work.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.