# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured collection capped, no complete browsable set
    - Actual: Marked "QA Full Product" featured (4th featured item total); homepage "Featured" widget still showed only the original 3 items. "View all" from Featured links to /products (unfiltered, no featured filter available), and /featured route does not exist (shows not-found). Visitors cannot browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag on existing product not added to filter list
    - Actual: Added tag "uniquetagxyz" to Sony WH-1000XM5 via admin edit; tag correctly appeared on the product detail page, but the products page's tag-filter list (Filters panel) did not include "uniquetagxyz" as a selectable option, so it cannot be chosen as a filter.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin changes not persisted across reload
    - Actual: After creating "Minimal Test Product" and editing Sony's price to $349 (both visible immediately in-session), reloading /products via full navigation reverted the product list to the original 9 seeded products and Sony's price back to $399. All admin creations/edits are lost on page reload, indicating changes are only held in client-side memory, not persisted to a backend/storage.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Products can be published without required content
    - Actual: Created "Minimal Test Product" with only Name and Short Description filled in (price left at $0, no rating change, no image URL, no affiliate/retailer link, no specifications). The product was successfully created and immediately published to the live product catalog (appeared on /products, detail page) despite missing image, retailer link, and specifications, and having a $0 price.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category dropdown non-functional on category pages
    - Actual: On /category/tech, the page offers its own category filter dropdown. Selecting "Home Goods" from this dropdown left the active category displayed as "Tech Gadgets" and the product grid unchanged (still showing the 3 Tech products), i.e. choosing a different category on this control does not update the active category or displayed products. (The equivalent dropdown on /products works correctly.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.