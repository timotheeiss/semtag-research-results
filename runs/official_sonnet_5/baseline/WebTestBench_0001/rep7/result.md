# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured product browsing does not filter to featured-only set
    - Actual: Unmarking Sony WH-1000XM5 as featured correctly removed it from the homepage 'Editor's Top Picks' section (now shows only Peloton Bike+ and Dyson V15 Detect). However, clicking 'View all' (linking to /products?featured=true) to browse the complete featured set shows all 9 products including non-featured ones (e.g. Samsung Galaxy S24 Ultra, Sony WH-1000XM5) with header '9 products found' instead of filtering to only the 2 featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter choice list does not update when a product's tags change
    - Actual: After adding a new tag 'studio' to Sony WH-1000XM5 and renaming 'premium' to 'premium-audio', the Filters panel's Tags list on /products still shows the old tags 'premium' and 'headphones' (which no longer belong to any product) and does not include the newly assigned 'studio' or 'premium-audio' tags at all. Selecting the stale 'premium' tag correctly returns 0 products (since no product has it), confirming the filter list is not derived live from current product tags.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Edits not persisted across page reload
    - Actual: After editing Sony WH-1000XM5 (price 399->379, rating 4.8->4.9, plus tag/spec/review changes) and confirming the changes appeared live in Admin, Home, and Detail views, reloading the app (navigating to http://localhost:6001/admin) reverted the product back to its original seed values (price $399, rating 4.8, original tags/specs). This was reproduced twice.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Product publish validation not enforced for content-completeness fields
    - Actual: Created product via Admin 'Create New Product' with only Product Name and Short Description filled; Price left at 0, no Image URL, no Affiliate Link, no Tags, no Specifications, no Full Review, Category left at default 'Tech Gadgets'. Clicking 'Create Product' showed toast 'Product created successfully' and the product was created and immediately published to /products listing and /product/test-incomplete-product. On the public product list, it displays $0 price, a broken image, and a 'Buy Now' link pointing to empty URL (/url: ""). This violates CS-20 which requires non-negative price, image, valid external retailer link, and at least one complete specification before a product can be published.

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