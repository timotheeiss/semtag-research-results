# Test Result

## Functionality
- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter applies the wrong threshold (off by one 100-unit step) and offers no maximum bound
    - Actual: Sorting (Newest/Top Rated/Price asc/Price desc) and Min-rating 4.5+ (8 results, excludes Philips Hue 4.4) work correctly. But the price slider has a single 'Minimum' thumb only (no max thumb, so no true range). With minimum displayed as $700 the grid still shows Breville Barista Express $699 (5 results); with minimum $600 it shows Bowflex $549 and Theragun $599. Effective threshold = displayed minimum minus 100, and it persists after re-render (changing sort).


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results violate the active price criterion
    - Actual: With price minimum $700 active, the result set (5 products found) includes Breville Barista Express at $699; with minimum $600 it included $549 and $599 products. Search/category/rating criteria were respected, but the price criterion is not.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: No validation of required recommendation content
    - Actual: Admin form accepted a product with only a name and short description: price left at 0, empty image URL, empty affiliate/retailer link, empty full review and zero specifications. Toast said 'Product created successfully' and 'Incomplete Test Product' was added to the admin list and published.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on category pages is inert
    - Actual: On /category/home the category dropdown shows 'Home Goods'; selecting 'Fitness' and then 'Tech Gadgets' left the URL at /category/home, the heading/active value at 'Home Goods' and the same 3 Home products displayed. (The same control does work on /products.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.
