# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Homepage featured strip is hard-capped at 3 and the "View all" featured link ignores the featured filter, so the complete set of featured products cannot be browsed
    - Actual: Marking works: after creating "Hydrow Wave Rower" with featured=on it got a "Featured" badge, and after unmarking Dyson in admin the homepage strip changed from [sony, peloton, dyson] to [sony, peloton, hydrow] — so marking/unmarking does update the collection. BUT with 4 products featured (sony, peloton, dyson, hydrow) the homepage strip rendered only 3 and hydrow was omitted with no way to reach it. The strip's "View all" link points to /products?featured=true, and that page ignores the featured query parameter entirely: it rendered "10 products found" listing every product including non-featured ones (macbook, theragun, philips-hue, breville, bowflex, samsung), and the heading stayed "All Products". There is no working view of the complete featured set.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is a static hard-coded set that never reflects tags assigned to products
    - Actual: Assigned the new tag "qafresh" (plus renamed "cordless"→"cordless-stick") to the existing product Dyson V15 Detect, and created a product with new tags "rowing, qamarker, low-impact". The saved tags are on the products (Dyson detail page lists qafresh; keyword search "qafresh" finds it). But the /products advanced tag filter still lists exactly the same 35 original tags: none of cordless-stick, qafresh, rowing, qamarker or low-impact is offered as a filter choice, so the new tag cannot be selected at all. Conversely the removed tags "cleaning" and "cordless" are still offered, and selecting "cleaning" yields "0 products found" — a dead filter option.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter is not a working range control: only one slider thumb is rendered, minimum price can never be set
    - Actual: All four sorts work correctly (newest; rating 4.9→4.4; price-asc $199→$2,495; price-desc $2,495→$199) and the minimum-rating filter works (4.5+ → 8 products, all ≥4.5). However the price slider [data-semtag-id='filters.price'] contains exactly ONE [role=slider] thumb (aria-label="Minimum", aria-valuenow permanently 0) although it is bound to two states (filters.price-min, filters.price-max). Dragging that single "Minimum" thumb changes the MAXIMUM instead (max label went $2495→$2410→$1250, count 9→8→6, correctly filtering ≤ max). The minimum price stays $0 and there is no second thumb to grab, and keyboard (ArrowRight/End) does not move the thumb at all. So visitors cannot filter by a price range, only by a maximum price.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin product additions/edits are in-memory only and are lost on any page reload or revisit
    - Actual: Created "Hydrow Wave Rower" via the admin panel; it appeared immediately in the admin list (id mtc7u55jbft177545c) and the product count rose to 10. After reloading (navigating to http://localhost:7001/product/hydrow-wave-rower) the page rendered "Product Not Found", and /products reverted to "9 products found" with no Hydrow entry. localStorage and sessionStorage are both completely empty (0 keys), confirming nothing is persisted — every create/edit/delete reverts to the seeded 9 products on reload.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name, description and rating range; review, image, retailer link and specs are all optional, and negative prices are accepted
    - Actual: Created "QA Minimal Product" supplying ONLY name + short description. It saved with no error, closed the form, and went live: /products now shows "10 products found" with a card whose <img src=""> is empty and whose "Buy Now" href is empty (dead link). No full review and zero specifications were required. Separately, editing it to price=-50 saved successfully and the admin list renders "-$50"; the price input has no min attribute. The affiliate link field is type=text with no URL validation and accepted the value "not-a-valid-url". Only the rating field is properly constrained (min=0/max=5 blocked the value 9 with "Value must be less than or equal to 5.").

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category select on the category page is inert — choosing a different category changes neither the active category nor the displayed products
    - Actual: On /category/fitness a category filter (filters.category) is offered with all four options (All Categories, Tech Gadgets, Home Goods, Fitness) and shows "Fitness". Selecting "Home Goods" produced no change: URL stayed /category/fitness, h1 and category.name stayed "Fitness", the select trigger still displayed "Fitness", and the grid still showed the same 3 fitness products (bowflex-selecttech-552, theragun-pro, peloton-bike-plus). Repeating with "Tech Gadgets" gave the identical no-op result. (The same control works correctly on /products, and the header nav category links do navigate, so the defect is specific to the category-page select.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.