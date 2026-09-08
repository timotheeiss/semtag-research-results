# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured products "View all" link does not filter to featured-only products
    - Actual: Unmarking Sony WH-1000XM5 as Featured correctly removed it from the homepage "Editor's Top Picks" section (leaving only Peloton Bike+ and Dyson V15 Detect). However, clicking "View all" (link to /products?featured=true) navigated to the All Products page showing all 9 products ("9 products found", heading "All Products") instead of only the 2 featured products. The featured=true query parameter is not applied as a filter.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tags added via admin edit do not appear in the /products Tags filter panel
    - Actual: Edited Bowflex SelectTech 552 tags via admin, adding new tags "home-fitness" and "qa-verified-2026" (replacing "home-gym" and "strength"). Product detail page correctly shows the updated tag chips (weights, dumbbells, home-fitness, qa-verified-2026). However, the /products page's Tags filter panel still lists the old tags "home-gym" and "strength" and does not include "home-fitness" or "qa-verified-2026" — the filter tag list is not derived from current product data, so the new tags cannot be selected as filter choices.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin panel changes (new products, edits) do not persist across a page reload/revisit
    - Actual: Created "QA Test Smart Speaker" via admin and confirmed it live in the app (10 products, searchable by tag, visible on detail page). After a hard reload of its detail page URL, the product returned "Product Not Found". Reloading /products showed only the original 9 products — the new product was gone, and previously edited products (Sony WH-1000XM5 Wireless→reverted to "Sony WH-1000XM5", Bowflex's category change back to Fitness) also reverted to their original seed values. Admin changes exist only in transient client-side state and are not persisted to any backend/storage that survives a reload.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [X] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category page's embedded category dropdown filter is non-functional/decoupled from navigation
    - Actual: On /category/fitness and /category/home pages, selecting a different category from the page's own category combobox does not update the displayed products or URL — the page remains on the original category. Additionally, after navigating via navbar link to a different category, the dropdown displayed a stale value inconsistent with the actual products shown.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.