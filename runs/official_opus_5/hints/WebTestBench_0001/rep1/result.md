# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Homepage featured strip is hard-capped at 3 items and the "View all" featured link does not filter to featured products
    - Actual: With 4 featured products (Sony, Peloton, Dyson, QA Aeropress Go) the homepage Featured section showed only Sony, Peloton, Dyson — the newly featured QA Aeropress Go was invisible. Un-marking Dyson made QA Aeropress Go appear (so the toggle does write), and re-marking Dyson pushed it out again. The section's "View all" link goes to /products?featured=true, but that page ignores the parameter and renders "10 products found" — all products, featured and not — so there is no way for a visitor to browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is static and does not reflect tags added to products
    - Actual: Edited existing product "Theragun Pro" tags from "recovery, massage, muscle, therapy" to "recovery, massage-gun, muscle, qatag-gamma" (saved: "Product updated successfully"). The tag filter panel on /products still lists exactly the same 35 seeded tags: "qatag-gamma" and "massage-gun" are absent, so the new tag cannot be selected as a filter. The list also still offers the now-unused "massage" and "therapy" — selecting "therapy" yields "0 products found". New product tags qatag-alpha/qatag-beta are likewise missing from the list.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — created/edited products are lost on page reload
    - Actual: Created "QA Minimal Product" then edited it into "QA Aeropress Go" (admin list and /products both showed 10 products). After reloading the app (navigating to http://localhost:7001/products), the count dropped back to "9 products found", the new product was gone, and /product/qa-minimal-product showed "Product Not Found". localStorage is empty and there is no persistence API (fetch /api/products returns the SPA index.html), so all admin data lives only in memory.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name + short description; image, retailer link, review and specifications are not required
    - Actual: Submitting the admin form with only Product Name ("QA Minimal Product") and Short Description filled produced "Product created successfully". The product is live at /product/qa-minimal-product with price $0, empty image src (""), empty "Buy Now" href (""), an empty "My Review" section and an empty "Key Specifications" section (0 spec rows). Only a fully empty form is rejected ("Please fill in all required fields").

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category filter control on category pages is inert
    - Actual: On /category/fitness the filter bar offers a Category select (All Categories / Tech Gadgets / Home Goods / Fitness). Selecting "Home Goods" and then "Tech Gadgets" changed nothing: URL stayed /category/fitness, the page heading stayed "Fitness", the select still displayed "Fitness", and the same 3 Fitness products (Bowflex, Theragun, Peloton) remained. The same control works correctly on /products.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.