# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured collection view-all does not filter to only featured products
    - Actual: Unfeaturing Peloton Bike+ and featuring Apple MacBook Pro 14 in admin correctly updated the homepage 'Editor's Top Picks' section (now shows Sony, Dyson, MacBook Pro instead of Sony, Peloton, Dyson) - so the toggle itself works. However, clicking 'View all' (linking to /products?featured=true) does not filter the list to only featured products: it shows 'All Products - 9 products found' with all 9 products (including non-featured ones like Samsung Galaxy S24 Ultra, Bowflex, Theragun, Peloton, Breville, Philips Hue), rather than restricting to the 3 featured items. Visitors cannot browse the complete set of featured products via this link.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter panel choice list is static and does not update with newly assigned tags
    - Actual: Edited Bowflex SelectTech 552 to add a new tag 'outdoor-gear' (tags now: weights, dumbbells, resistance, outdoor-gear) and saved. The detail page and keyword search correctly reflect the new tag (searching 'outdoor-gear' returns exactly Bowflex). However, the Filters > Tags checkbox/chip list on /products still shows the full static/hardcoded alphabetical list (ambiance, android, ... weights, wireless) and does NOT include 'outdoor-gear' or 'resistance' as selectable filter chips, so the new tag cannot be selected via the tag-filter UI.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range minimum filter does not exclude products below the threshold
    - Actual: Sort by Newest, Top Rated, Price: Low to High, and Price: High to Low all worked correctly (verified correct ordering by rating/price for each). Minimum Rating filter (4.5+ stars) correctly reduced 9→8 products. However, setting Price Range Minimum slider to $600 (with 4.5+ rating filter active) still showed 7 products including Bowflex SelectTech 552 ($549) and Theragun Pro ($599), both priced below the $600 minimum threshold shown in the UI.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Edits not persisted across page reload/session
    - Actual: After editing Sony WH-1000XM5 (name changed to 'Sony WH-1000XM5 (Updated)', slug became sony-wh-1000xm5-updated) and reloading the page (revisiting /product/sony-wh-1000xm5-updated), the app shows 'Product Not Found'. Revisiting /admin also shows the original unedited product data. All edits exist only in in-memory client state and are lost on reload, not persisted server-side.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results violate active price range criterion
    - Actual: With Minimum Rating=4.5+ stars and Price Range Minimum=$600 both active, the results included Bowflex SelectTech 552 ($549) and Theragun Pro ($599), which do not satisfy the $600 minimum price criterion, while all products did correctly satisfy the rating criterion.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Add Product form allows publishing with missing required fields (image, retailer link, specs)
    - Actual: Submitting the Create Product form completely empty correctly showed a 'Please fill in all required fields' error and blocked creation. However, after filling in only Product Name and Short Description (leaving Image URL, Affiliate Link, and Specifications all empty, Price at default $0, Rating at default 4.5), clicking 'Create Product' succeeded with a 'Product created successfully' toast and the product ('Test Validation Product') was added to the admin list with no image, no retailer link, and no specifications — violating the requirement that a product cannot be published without a complete image, valid retailer link, and ≥1 complete spec.

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