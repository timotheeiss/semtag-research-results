# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured "View all" is broken; visitors cannot browse the complete set of featured products
    - Actual: Homepage featured section shows only a 3-item preview. Marking/unmarking a product as featured correctly updates that preview (e.g., unmarking Sony removed it, leaving 2 items). However clicking "View all" on the Featured section navigates to /products?featured=true, which does NOT filter by featured - it shows all 10-11 products instead of only featured ones. No advanced filter option for "featured" exists either, and no dedicated /featured route exists (404-style error). So there is no way for a visitor to browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter choice list is static and does not update when a product's tags change
    - Actual: Created a new product with tags "newtagone, newtagtwo" and edited an existing product's tags (removed ambiance/automation/smart-home, renamed smart-home to smart-home-renamed, added colorful/newaddedtag). The tags appeared correctly on the product detail pages and were searchable, but the Tags filter chip list on /products still shows only the original 35 seeded tag values (still includes now-unused "ambiance", "automation", "smart-home") and never gained "newtagone", "newtagtwo", "smart-home-renamed", "colorful", or "newaddedtag" as selectable filter chips.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product additions/edits are not persisted; lost on page reload
    - Actual: After creating "Test Gadget XYZ" via the admin panel (confirmed present in admin list and /products, 10 total), performing a hard page reload/navigation (browser back to /products via URL) reverted the product count to the original 9 and the admin product list also reverted to only the 9 seeded products - the new product and all edits were lost. State is kept only in-memory client-side with no backend/localStorage persistence.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Admin can publish incomplete products lacking required content
    - Actual: Created a product with only Name and Short Description filled in (price left at $0, no image URL, no affiliate/retailer link, no specifications). The form submitted successfully with no validation error, and the product was immediately published and visible to visitors on /products (count increased to 11), despite missing image, retailer link, and specifications.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on the dedicated category page does not change category
    - Actual: On /category/home, the category filter select (which is offered on that page) shows "Home Goods". Selecting "Fitness" from its dropdown does not navigate or change the displayed products/active category - the combobox snaps back to showing "Home Goods" and the grid still shows the 3 Home Goods products. (Note: the same select works correctly to filter category on the /products page.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.