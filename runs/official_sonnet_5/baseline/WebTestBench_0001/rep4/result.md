# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured set not fully browsable after marking a new product featured
    - Actual: Toggled "Featured on homepage" ON for Apple MacBook Pro 14" via Admin edit; save succeeded and a "Featured" badge appeared on the product in category/listing views (4 featured products now exist: Sony, Peloton, Dyson, MacBook). However, the homepage "Editor's Top Picks" section still showed only the original 3 products (Sony, Peloton, Dyson) — MacBook did not appear. Clicking "View all" (→ /products?featured=true) did not filter to featured items at all; it showed all 9 products (including non-featured ones like Samsung Galaxy S24 Ultra, Bowflex, Theragun), so visitors cannot browse the complete featured set.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag not offered as a filter choice
    - Actual: Added new tag "audio-gear" to Sony WH-1000XM5 via Admin edit (saved successfully; tag appeared on detail page and was matched by keyword search). However, opening /products Filters panel's Tags chip list did not include "audio-gear" among the ~34 selectable tag chips (list appears to be a static/precomputed set, not derived from current product data), so the new tag cannot be selected as a filter choice.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product edits are not persisted after page reload
    - Actual: Edited Sony WH-1000XM5 (price 399→379, added tag "audio-gear", appended text to description), save succeeded with toast "Product updated successfully" and changes propagated correctly to /products and /product/sony-wh-1000xm5 via in-app client-side navigation. However, after a full page reload/fresh navigation to /product/sony-wh-1000xm5 and /admin, all three changes were reverted to original values ($399, original description, no audio-gear tag) — edits do not survive reload or a later session revisit.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Product can be published while missing many required fields
    - Actual: Submitting the "Add Product" form completely empty was blocked with toast "Please fill in all required fields" (name/category/description enforced). However, filling in only Product Name and Short Description (leaving Price at $0, Image URL empty, Affiliate Link empty, Full Review empty, and zero specifications) succeeded — toast "Product created successfully" and the product ("QA Test Product") was published and appeared live on /products with $0 price, no image, empty Buy Now link (href=""), no full review, and no specifications. Required fields (non-negative price beyond default 0, image, valid retailer link, ≥1 complete spec, full review) are not enforced.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on category page does not switch category
    - Actual: On /category/tech, opening the category dropdown and selecting "Home Goods" left the page unchanged: URL stayed /category/tech, heading remained "Tech Gadgets", combobox still displayed "Tech Gadgets", and the same 3 tech products remained listed. Reproduced twice.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.