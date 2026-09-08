# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete featured set; homepage shows only 3 featured items and the "View all" featured link is ignored
    - Actual: Marking works in isolation: new product "QA Aeropress Brewer" (featured) did not appear on the homepage until Dyson V15 Detect was un-featured, showing the homepage section is hard-capped at 3 of the 4 featured products. The section's "View all" link points to /products?featured=true, but that page ignores the parameter and lists all 11 products (count "11 products found", only 3 carrying a Featured badge), and /products offers no featured filter — so the complete featured set cannot be browsed.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is static and ignores tags added to products
    - Actual: After adding tag "qatag-anc" (and renaming wireless→wireless-audio) on the existing product Sony WH-1000XM5, the /products advanced filter tag list still contained the same 35 original tags — no "qatag-anc", "wireless-audio", "qatag-brew" or "qatag-travel" entries — while the removed tag "noise-cancelling" and the old "wireless" are still offered, so the new tag cannot be selected as a filter.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter has no upper-bound control (only one slider thumb rendered)
    - Actual: All four sorts work (Top Rated: 4.9→4.4; Price asc $199→$2,495; Price desc $2,495→$199; Newest default) and min-rating "4.5+ stars" correctly returns 8 of 9 products. However the "Price Range" slider (data-semtag-id=filters.price) renders only a single thumb with aria-label="Minimum"; the max label stays fixed at $2495 and no handle exists to lower it, so a visitor can only set a lower bound (clicking track center set $1250 → 3 products ≥ $1,299) and can never filter to an upper price limit.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin changes are not persisted — state resets to seed data on reload
    - Actual: After creating "QA Aeropress Brewer" and "QA Minimal Product" and editing Sony (price $399→$379, new tags) and Theragun (renamed "Theragun Pro QA", rating 4.8), reloading /products showed "9 products found" with only the original seeded products; the created products are gone and Sony is back to $399 / Theragun back to "Theragun Pro". localStorage is empty and there is no persistence API, so all data lives in memory only.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name/description/category; incomplete and invalid products can be saved
    - Actual: Submitting with only Name="QA Minimal Product" and Short Description saved successfully ("Product created successfully") with no image URL, no affiliate/retailer link, no full review and zero specifications. Editing it to Price=-50 with Affiliate Link="not-a-url" also saved: the admin list now shows "-$50" for that product. Only rating is constrained (native max=5); price has no min attribute and the retailer link is a plain text field with no URL validation.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category filter on the category page is inert
    - Actual: On /category/home the offered category select (filters.category, value "Home Goods") was set to "Fitness" twice (via semtag option and via the listbox option ref). After each selection the trigger reverted to "Home Goods", the heading stayed "Home Goods", the URL stayed /category/home and the same 3 Home products (Breville, Philips Hue, Dyson) remained. The same control works correctly on /products.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.