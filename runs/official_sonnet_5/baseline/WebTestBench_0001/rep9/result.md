# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: "View all" featured link does not filter to only featured products
    - Actual: Marking Apple MacBook Pro 14" as Featured correctly added the "Featured" badge to it in category/product listings, and homepage "Editor's Top Picks" section shows a capped preview of featured items. However, clicking "View all" (which links to /products?featured=true) does not filter the list at all — it shows "9 products found" including non-featured items (Samsung Galaxy S24 Ultra, Bowflex SelectTech 552, Theragun Pro, Breville Barista Express, Philips Hue Starter Kit) instead of only the 4 featured products. There is no way for visitors to browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag added to an existing product does not appear as a selectable tag-filter chip on /products
    - Actual: Edited existing product "Breville Barista Express" via Admin Panel, adding a brand-new tag "espresso-machine-test" to its tag list and saving (toast "Product updated successfully" shown). Navigated client-side to /products and opened the Filters panel: the Tags chip list (35 alphabetically sorted chips: ambiance...wireless) does NOT include "espresso-machine-test" anywhere in the DOM. The tag filter option list appears to be static/precomputed and does not update when new tag values are added to products. As a secondary check, the same new tag "kitchen-gadget" assigned during product creation (FT-5) was also absent from this same filter chip list, confirming the tag-filter population is broken for any new tag, whether added at creation or via edit.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product edits are not persisted; they are lost on page reload
    - Actual: Edited Bowflex SelectTech 552 price from $549 to $599 and saved (toast confirmed "Product updated successfully"). After reloading /admin (page refresh), the price reverted back to the original $549, showing the edit was not persisted to durable storage/backend — only held in transient in-memory state that resets on reload.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Products can be published/created without required content
    - Actual: Created "Test Incomplete Product" via Admin Panel supplying only a name and short description, leaving price at default $0, no image URL, no affiliate/retailer link, no full review, and no specifications. The form only validated for empty Name/Short Description ("Please fill in all required fields") but allowed submission and immediately published the product — it appeared publicly on /products (10 products found) with no image, no Buy Now-capable retailer link, no review, and no specs, which does not meet the "usable recommendation" content requirements.

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