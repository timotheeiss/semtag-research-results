# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete set of featured products; homepage featured section is capped at 3 and the "View all" featured link ignores its filter
    - Actual: Marking/unmarking does update the homepage collection: creating a featured product and then unfeaturing Sony WH-1000XM5 changed the featured row from [Sony, Peloton, Dyson] to [Peloton, Dyson, Nordic Ceramic Kettle]. However the homepage shows at most 3 featured items — when a 4th product was featured it was not displayed at all until a slot freed up. The featured "View all" link points to /products?featured=true, but that page ignores the featured query parameter and renders "10 products found" — the entire catalogue including non-featured items (Samsung, MacBook, Bowflex, Theragun, Breville, Philips Hue, Sony). There is no featured filter control on the products page either, so the complete set of featured products cannot be browsed.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is static/hardcoded and does not derive from current product tags
    - Actual: After editing Theragun Pro to add the new tags "qaedittag" and "deep-tissue" (saved and visible on its detail page, and matchable via keyword search), the tag filter list on /products still contained exactly the same 35 seed tags. Neither "qaedittag" nor "deep-tissue" appeared as a filter choice, so the new tag cannot be selected to return the matching product. The same applied to tags of the newly created product (kettle, ceramic, qanewtag). Conversely, tags removed from products ("massage", "therapy") are still offered as filter choices, and selecting "massage" yields "0 products found".

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — created/edited products are lost on page reload
    - Actual: After creating "QA Minimal Product" (admin list showed 10 products), reloading the app returned the admin list to the original 9 seeded products; the new product was gone and /product/qa-minimal-product rendered the not-found state. localStorage and sessionStorage are both empty and there is no backend API (/api/products returns the Vite index.html), so all product state is in-memory only and resets on every reload or new session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name and short description; incomplete products can be published
    - Actual: Submitting the Add Product form with ONLY "QA Minimal Product" (name) and a short description succeeded — the product was created and appeared in the admin list (count 9→10). Full review, image URL, affiliate/retailer link and specifications were all left empty and no specification row was added, yet no validation error was shown and publishing was allowed. Price stayed at default 0 and rating 4.5. Only the fully-empty form was rejected, and even then no validation message was displayed.

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