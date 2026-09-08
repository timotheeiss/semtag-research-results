# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete set of featured products — the homepage strip is capped at 3 and the "View all" featured link (/products?featured=true) ignores the featured filter
    - Actual: Marking/unmarking works: un-featuring Dyson V15 Detect removed it from the homepage strip and revealed the newly featured "QA Aurora Desk Lamp"; re-featuring Dyson restored it. However with 4 featured products the homepage shows only 3 (Sony, Peloton, Dyson) and QA Aurora Desk Lamp is hidden. Clicking "View all" navigates to /products?featured=true, which renders "10 products found" and lists all 10 products (including non-featured ones such as Samsung, MacBook, Theragun) — the featured=true parameter has no effect, so the full featured set is not browsable anywhere.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is static (built from seed data only) — newly assigned tags never become selectable, and removed tags remain listed
    - Actual: Edited existing product Dyson V15 Detect, replacing tags with "vacuum, smart-home, deepclean, qafresh". The detail page and keyword search picked up the new tags, but the /products tag filter panel still lists exactly the same 35 seeded tags: "qafresh" and "deepclean" are absent, so they cannot be selected as filter choices. Conversely the now-unused tags "cleaning" and "cordless" are still offered, and selecting "cleaning" yields "0 products found". Same result for a newly created product ("qanewtag"/"desklamp" were also missing from the filter list).

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin additions and edits are lost on page reload (in-memory state only)
    - Actual: Created product "QA Aurora Desk Lamp" and edited Dyson V15 Detect (tags changed to vacuum, smart-home, deepclean, qafresh; featured toggled). After reloading the app (navigating to /product/dyson-v15-detect and /admin), the admin list shows only the 9 original seeded products — "QA Aurora Desk Lamp" is gone — and Dyson's tags reverted to the seeded vacuum/cordless/smart-home/cleaning. localStorage is empty (Object.keys(localStorage) = []), confirming nothing is persisted.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation is limited to name/description (and rating max); products can be saved with a negative price, an invalid retailer link, and no image, review or specifications
    - Actual: Created "QA Minimal Probe" supplying only Name + Short Description. It saved successfully (form closed, product added to the admin list and to the public catalog) with Image URL empty, Full Review empty, Affiliate Link empty and zero specification rows. Editing it then accepted Price = -50 and Affiliate Link = "not a url"; after saving, the admin list shows the price as "-$50". Only Rating > 5 was blocked (native max=5 message "Value must be less than or equal to 5.").

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category dropdown on the category page is inert — selecting a different category does not change the active category or the products shown
    - Actual: On /category/tech the "filters.category" select was set to Fitness, then to Home Goods. After each selection the URL stayed /category/tech, the category heading (category.name) stayed "Tech Gadgets", the select trigger reverted to/stayed "Tech Gadgets", and the grid still listed only Samsung Galaxy S24 Ultra, Apple MacBook Pro 14", Sony WH-1000XM5. (The same control works correctly on /products.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.