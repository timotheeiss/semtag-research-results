# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured collection is capped at 3 and the "View all" featured page is not filtered
    - Actual: Toggling featured does update the homepage strip (unfeaturing Dyson removed it and QA Kitchen Marvel took its place; refeaturing works). However with 4 featured products the homepage "Editor's Top Picks" showed only 3 (the newly featured QA Kitchen Marvel was omitted until a slot freed up), and its "View all" link to /products?featured=true ignores the featured parameter — it renders "10 products found" with all products, featured and non-featured. There is no view listing the complete featured set.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list is static and ignores newly assigned tags
    - Actual: After adding tag "qanewtag" (and "massagegun") to Theragun Pro and creating a product with tags "qatag, zephyr", the /products Filters panel still lists the same 35 original seed tags — "qanewtag", "massagegun", "qatag" and "zephyr" are absent, so the new tag cannot be selected as a filter. The panel also still offers removed tags ("massage", "therapy") that no longer belong to any product.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin changes lost on reload
    - Actual: After creating "QA Test Gadget"/"QA Kitchen Marvel", editing Theragun Pro's tags and un-featuring Dyson, reloading http://localhost:6001/ reset everything to the 9 seeded products (category counts back to Tech 3 / Home 3 / Fitness 3, Dyson featured again). /product/qa-kitchen-marvel returns "Product Not Found". localStorage is empty and there is no backend API, so state exists only in memory for the session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Incomplete products can be published; no validation feedback
    - Actual: Created "QA Incomplete Product" with only name, category, price and short description — empty Image URL, empty Affiliate Link, empty Full Review and no specification. The form saved it and it now appears publicly at /products (11 products found) with an empty img src and a "Buy Now" link with no external href. Separately, submissions with price=-50 / rating=9 were rejected but with no error message, toast or field highlight at all.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on category pages is inert
    - Actual: On /category/home the filter bar offers a category dropdown showing "Home Goods". Selecting "Fitness" and then "Tech Gadgets" left the dropdown reading "Home Goods", heading "Home Goods", URL /category/home and the same 3 Home products — neither the active category nor the displayed products changed. (The same control works correctly on /products.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.