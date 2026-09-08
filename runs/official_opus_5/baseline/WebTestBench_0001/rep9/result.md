# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: The "View all" featured link does not filter — visitors cannot browse the complete set of featured products (homepage section is capped at 3)
    - Actual: Marking/unmarking does update the homepage section: after creating a featured product and unfeaturing Sony, "Editor's Top Picks" changed from [Sony, Peloton, Dyson] to [Peloton, Dyson, QA Zen Air Purifier]. However the section only ever renders 3 cards, and its "View all" link to /products?featured=true ignores the featured parameter: the page renders h1 "All Products" with "10 products found", listing all 10 products including the 6 non-featured ones (Samsung, MacBook, Bowflex, Theragun, Breville, Philips Hue, Sony). With 4 featured products earlier, the 4th was reachable through no featured view at all.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is hard-coded to the seeded tags — newly assigned tags never appear as filter options, and removed tags remain listed
    - Actual: Added tags "qa-added-tag" and "qa-premium-renamed" to the existing product Sony WH-1000XM5 (saved; both appear on its detail page and are matched by keyword search). The Filters panel on /products still lists exactly the same 35 seeded tags — neither new tag is offered, so it cannot be selected as a filter. Tags created with a new product (qazen, air-quality, purifier) are likewise absent. Conversely the removed tags "wireless" and "premium" are still offered, and selecting "wireless" yields "0 products found".

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product additions and edits are not persisted — all admin changes are lost on page reload
    - Actual: Created "QA Minimal Product" (admin list showed 10 products) and edited its price to -500 ("Product updated successfully"). After reloading, /products reported "9 products found" with no QA Minimal Product, /product/qa-minimal-product returned "Product Not Found", and /admin listed only the original 9 seeded products. State is held in memory only (localStorage is empty), so every addition/edit is discarded on reload or in a later session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name/category/short description; a product can be created with no full review, image, retailer link, or specifications, and can be saved with a negative price
    - Actual: Empty form correctly showed "Please fill in all required fields". However, filling ONLY Product Name ("QA Minimal Product") + default category + Short Description created the product: toast "Product created successfully", product appeared in admin list (10 items) with Full Review empty, Image URL empty, Affiliate Link empty, and zero specification rows. Additionally, editing it to Price = -500 saved successfully ("Product updated successfully") and the admin card rendered "-$500". Only the rating bound is enforced (native min=0/max=5 blocked 9.9); the price input has no min attribute.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category dropdown on category pages is inert — selecting a different category does not change the active category or the displayed products
    - Actual: On /category/home the toolbar offers a category select showing "Home Goods". Choosing the "Fitness" option (verified via explicit listbox ref) produced no change: URL stayed /category/home, H1 stayed "Home Goods", the select reverted to "Home Goods", and the same 3 Home products (Breville, Philips Hue, Dyson) remained. Repeated twice with the same result. The equivalent select on /products does work.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.