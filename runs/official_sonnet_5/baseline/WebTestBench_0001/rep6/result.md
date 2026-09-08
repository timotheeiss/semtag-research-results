# Test Result

## Functionality
- [ ] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.
  - Bug Report:
    - Issue: Data persistence bug - Create Product
    - Actual: Creating a new product ("QA Test Gadget") in Admin panel shows "Product created successfully" toast and appears in the in-session admin list, but the new product is absent from /products and vanishes from the admin list itself after a fresh navigation to /admin. Changes are never persisted to any backing store.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Data persistence bug - Edit Product
    - Actual: Editing "Theragun Pro" (price $649, rating 4.6, added tag, changed description, toggled Featured) shows "Product updated successfully" toast, but /product/theragun-pro and /products still show old data ($599, rating 4.5), and a fresh /admin navigation reverts the admin panel's own display to original values.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Data persistence bug - Featured toggle
    - Actual: Toggling "Featured on homepage" switch during edit is accepted in the form and shows success toast, but is not reflected on the public homepage/product cards after navigation, and reverts on fresh admin reload - consistent with the general CRUD persistence failure.

- [ ] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.
  - Bug Report:
    - Issue: Data persistence bug - Admin edits don't survive navigation
    - Actual: Any field update (price, rating, description, tags) made via the Admin edit form is only reflected in transient client state; re-navigating to /admin from scratch reverts the panel's own list to pre-edit values, confirming no backend/store write occurs.

- [ ] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.
  - Bug Report:
    - Issue: Data persistence bug - Delete Product
    - Actual: Deleting "Philips Hue Starter Kit" (via delete icon + confirm dialog) shows "Product deleted successfully" toast and removes it from the in-session list, but the product reappears unchanged after a fresh navigation to /admin, proving the delete was never persisted.

- [ ] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.
  - Bug Report:
    - Issue: Data persistence bug - Category reassignment
    - Actual: Changed "Breville Barista Express" category from Home Goods to Fitness via Admin edit form and saved successfully, but the product does not appear on /category/fitness and a fresh /admin reload shows it still under Home Goods.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Critical architectural bug - Admin CRUD never persists
    - Actual: All Admin Panel CRUD operations (Create, Edit, Delete) only mutate ephemeral/local UI state. None are persisted to any backing store shared with public pages (/products, /product/[slug]) or even with the admin panel's own state on re-navigation. Success toasts are shown but are misleading since no data is actually saved.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [X] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [ ] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.
  - Bug Report:
    - Issue: Price range slider - keyboard inaccessible, mislabeled
    - Actual: The price-range filter slider (role="slider") is mislabeled with aria-label="Minimum" despite functioning as a maximum-price upper bound. Keyboard interaction (ArrowRight, End after clicking/focusing the thumb) had zero effect on aria-valuenow or displayed price/product count. Only a precise pixel-coordinate drag succeeded in moving it.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [X] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.