# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured browse view ignores featured filter — cannot browse the featured set
    - Actual: Toggling works: unfeaturing Sony WH-1000XM5 and featuring Theragun Pro in /admin correctly changed the homepage "Editor's Top Picks" collection from [Sony, Peloton, Dyson] to [Peloton, Dyson, Theragun]. However the "View all" link beside that section points to /products?featured=true, and that page ignores the featured=true query parameter: it renders heading "All Products" with "10 products found" listing every product (including unfeatured ones such as Sony, Samsung, MacBook, Philips, Breville, Bowflex, Aurora). The Filters panel offers only Price Range, Minimum Rating and Tags — there is no featured filter — so a visitor has no way to browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list is static — newly assigned tags never become filter choices
    - Actual: Edited existing product Sony WH-1000XM5, changing tags from "headphones, wireless, noise-cancelling, premium" to "headphones, wireless-audio, premium, zetaqafresh" (save succeeded; keyword search for "zetaqafresh" correctly returns Sony). But the /products Filters panel tag list is unchanged from the original seeded set of 35 tags: "zetaqafresh" is absent, so it cannot be selected as a tag filter. The list is equally stale in the other direction — the renamed-away "wireless" and the removed "noise-cancelling" are still offered as choices, and selecting "noise-cancelling" yields "0 products found", a dead filter option. Tags from the newly created product ("qatestzeta", "smart-kettle") are also missing from the list.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — admin changes lost on reload
    - Actual: Created "Aurora Smart Kettle QA" (Home Goods, $129.99, 4.3) and deleted "QA Validation Probe" in /admin. After navigating to /products (page reload), the products list showed "9 products found" with only the original seeded products; Aurora was absent. Returning to /admin also showed exactly the 9 seeded products. localStorage is completely empty (Object.keys(localStorage) === []), so state is in-memory only and resets to seed data on every reload.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Missing validation on required publishable content
    - Actual: Created "QA Validation Probe" with only Name + Short Description filled. Image URL, Affiliate Link, Full Review and Specifications were all left empty and the product was published anyway — no error shown, form closed, product appears in the admin list. Only Name/Category/Short Description are enforced.

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