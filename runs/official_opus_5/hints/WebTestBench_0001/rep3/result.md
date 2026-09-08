# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Homepage featured collection is hard-capped at 3 and the complete featured set cannot be browsed — the ?featured=true listing ignores the filter
    - Actual: Toggling featured does update the homepage: after marking "QA Grip Trainer X1" featured it was NOT shown (section still listed Sony, Peloton, Dyson), but after unfeaturing Sony it took the free slot, proving the section renders only the first 3 featured products. With 4 products featured (Sony, Peloton, Dyson, QA Grip Trainer X1 per the admin list), the homepage shows just 3. The featured "View all" link navigates to /products?featured=true, which ignores the parameter and renders all 11 products ("11 products found"), and the products page offers no featured filter — so the complete featured set is never browsable.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is a static seeded set — newly assigned tags never become filter options, and removed tags remain as dead options
    - Actual: After adding tag "qatag-existing" to the existing product Theragun Pro (and renaming "massage"→"massage-gun"), the tag filter list still contained exactly the original 35 seeded tags. "qatag-existing" and "massage-gun" were absent, so the new tag cannot be selected as a filter at all. Tags from the newly created product ("qatag-alpha", "qatag-beta") were likewise absent. Conversely the now-unused tags "massage", "muscle" and "therapy" are still offered; selecting "muscle" yields "0 products found". Keyword search does pick up the new tags, but the tag filter does not.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter is broken: it has no maximum handle and filters using a stale (one-step-behind) minimum value
    - Actual: Sorting works correctly (newest, rating 4.9→4.4, price asc $199→$2,495, price desc $2,495→$199) and the min-rating filter is accurate (4.5+ → 8 products, all ≥4.5). But the price slider renders only ONE thumb (aria-label "Minimum"); no maximum thumb exists, so an upper bound cannot be set even though the UI displays a "$0 – $2495" range. The minimum also filters on the previous value: with min displayed as $700 the $699 Breville Barista Express was still listed (5 results); with min displayed as $800 the $749 Dyson V15 Detect was still listed (4 results). State persisted unchanged after a 2.5s wait, so it is not debounce.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Created products are not persisted — all admin changes are lost on page reload
    - Actual: "QA Minimal Product" was created and appeared as a 10th row in the admin list. After reloading /admin the list was back to the original 9 seeded products and the new product was gone. localStorage, sessionStorage and document.cookie are all empty, and there is no backend API (/api/products returns the SPA HTML shell), so product data exists only in memory for the lifetime of the page.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results violate the active price criterion
    - Actual: With the price minimum showing $800, the result grid still included Dyson V15 Detect at $749; with the minimum at $700 it still included Breville Barista Express at $699. Each product shown is below the active minimum price, so results do not satisfy all active criteria. Category, search and rating criteria were individually respected.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name, description, price and rating bounds — image, full review, valid retailer URL and specifications are not required
    - Actual: Only Name and Short Description are marked required. "QA Minimal Product" was published successfully with an empty image URL, empty full review, empty affiliate link and zero specifications (listed as Tech | $0 | 4.5). A second product "QA Bounds Probe" was published with affiliate link literally "not-a-valid-url", again with no image, review or specs. Price/rating bounds are enforced (price -50 with rating 9 was rejected), but rejection shows no error message at all — the form silently does nothing.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category select on /category/* pages is inert — choosing a different category changes nothing
    - Actual: On /category/tech the category dropdown opens and lists All Categories/Tech Gadgets/Home Goods/Fitness (Tech marked aria-selected=true). Selecting "Fitness" and then "Home Goods" left everything unchanged: heading stayed "Tech Gadgets", trigger still displayed "Tech Gadgets", URL stayed /category/tech, and the grid still showed the same 3 tech products (Samsung, MacBook, Sony). Verified twice with a 600ms settle wait. The same control works correctly on /products.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.