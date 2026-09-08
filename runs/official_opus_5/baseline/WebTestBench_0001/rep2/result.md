# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: "View all" featured page ignores the featured filter — complete featured set cannot be browsed
    - Actual: Marking/unmarking does update the homepage "Editor's Top Picks" strip (unfeaturing Sony removed it; featuring Philips Hue added it). But the section's "View all" link points to /products?featured=true, and that page ignores the featured param: it renders H1 "All Products" with "9 products found" listing every product (including 6 non-featured ones). There is no view that shows only the featured collection, and the homepage strip caps at 3 items.

- [ ] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.
  - Bug Report:
    - Issue: Keyword search ignores admin-created products (only seeded data is searchable)
    - Actual: Case-insensitive matching works for seeded products (SONY→Sony WH-1000XM5 "1 product found"; "LASER dust"→Dyson; tag "smart-home"→2 products). But the admin-created product "Zephyr Air Purifier Z9" is never returned: searching "Zephyr", "zephyr", "quietflow" or "QUIETflow" all yield "0 products found", even though the product is listed on /products and /category/home. So search omits matching product names/descriptions/tags and reports an inaccurate count of 0.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter option list is static and never includes newly assigned tags
    - Actual: Edited Sony WH-1000XM5 tags to "headphones, wireless-pro, premium, ldac-audio" (saved, confirmed on its detail page). The Filters panel tag list on /products still shows the original 35 seed tags — "wireless-pro" and "ldac-audio" are absent, so the new tag cannot be selected as a filter choice. (The list still offers the now-removed "wireless", and selecting it returns "0 products found".) The same happened for tags "quietflow"/"hepa-elite" on a newly created product.

- [ ] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.
  - Bug Report:
    - Issue: New tags saved on the product do not participate in keyword search
    - Actual: Created "Zephyr Air Purifier Z9" with tags "quietflow, hepa-elite". Both tags are saved and displayed on /product/zephyr-air-purifier-z9. However keyword search for "quietflow" or "hepa-elite" returns "0 products found" (as does searching the product name), so the new tags do not participate in keyword search.

- [ ] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.
  - Bug Report:
    - Issue: Edited tag set is not reflected in keyword search (search uses stale seed tags)
    - Actual: Sony WH-1000XM5 tags were edited from "headphones, wireless, noise-cancelling, premium" to "headphones, wireless-pro, premium, ldac-audio" (add + rename + remove). The detail page correctly shows the new set. However keyword search for the added tag "ldac-audio" returns "0 products found", while searching the removed tag "noise-cancelling" still returns Sony WH-1000XM5 — search matches the old tags, not the saved set.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product additions are not persisted across page reload
    - Actual: Created "Zephyr Air Purifier Z9" via /admin (confirmed visible in admin list, /products, /category/home and its detail page). After reloading /admin the admin list contains only the 9 seeded products and Zephyr is gone; /product/zephyr-air-purifier-z9 now renders "Product Not Found". All admin changes are in-memory only.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Insufficient publish validation — product created without required content
    - Actual: In /admin > Create New Product, entering only Name ("Test Partial Product") and Short Description, leaving Full Review, Image URL, Affiliate Link and Specifications completely empty (price 0), the form submitted successfully with toast "Product created successfully" and the product now appears in the admin list and product browsing. Only name + short description are enforced.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [ ] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.
  - Bug Report:
    - Issue: Purchase links point to placeholder example.com URLs, not real retailer product pages
    - Actual: "Buy Now" correctly opens an external site in a new tab (target=_blank, rel=noopener noreferrer) — clicking it opened tab 1 at https://example.com/theragun-gen5 titled "Example Domain". However every seeded product's affiliate link is an example.com placeholder (https://example.com/sony-xm5, /peloton, /dyson-v15, /macbook-pro, /s24-ultra, /philips-hue, /breville, /theragun, /bowflex), so no purchase action reaches an actual retailer's product page.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category dropdown on category pages is inert
    - Actual: On /category/home the toolbar offers a category select pre-set to "Home Goods". Selecting "Fitness" (and separately "Tech Gadgets") from its listbox has no effect: URL stays /category/home, H1 stays "Home Goods", the select trigger still reads "Home Goods", and the same 4 Home products remain displayed. (The equivalent select on /products does work.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.