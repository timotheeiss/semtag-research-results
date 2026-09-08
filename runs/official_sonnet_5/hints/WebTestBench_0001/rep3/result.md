# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured "view all" does not filter to featured products; homepage featured section is capped and browsing complete featured set is broken
    - Actual: Marking Test Widget Pro as featured did update it into the (3-slot) homepage featured carousel once another item was unfeatured, but clicking "View all" on the Featured section navigates to /products?featured=true which shows all 10 products (unfiltered) instead of only the featured ones, so visitors cannot browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag assigned to existing product does not become available in the tag-filter list
    - Actual: Edited Philips Hue Starter Kit to add tag "voicecontrol", saved successfully (visible on detail page), but opening Filters on /products the tag-filter checklist (35 items) does not include "voicecontrol" or any newly added tag, so it cannot be selected as a filter.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Sort works correctly but price-range minimum filter does not exclude products below the selected minimum
    - Actual: Sorting by Price: Low to High and Top Rated both produced correctly ordered results. However, setting the price-range minimum slider to $430 still displayed Sony WH-1000XM5 priced at $399 in the results (8 products found), violating the active minimum-price criterion.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product additions/edits are not persisted; a full page reload/revisit reverts all admin data to the original seed state
    - Actual: After creating "Test Widget Pro" and editing several products (Philips Hue tags, Sony featured flag, Bowflex price), reloading/revisiting the app (browser_navigate to a fresh URL) reverted the admin product list to the original 9 seeded products with all original values; none of the created/edited data survived.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results can violate the active price-range criterion
    - Actual: With price-range minimum set to $430 (and no other filters), Sony WH-1000XM5 ($399) remained in the 8 displayed results despite being below the active minimum threshold.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Products can be created/published without required content
    - Actual: Created a product "Incomplete Product" supplying only Name and Short Description (no image, no affiliate/retailer link, no full review, no specifications, price left at default 0). The admin form accepted and saved it as a full product entry rather than rejecting the submission or preventing publication.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category filter dropdown offered on the category-specific page does not change the active category or displayed products
    - Actual: On /category/tech, the category filter select shows "Tech Gadgets"; selecting "Home Goods" or "Fitness" from that same dropdown leaves the select displaying "Tech Gadgets" and the grid still shows the 3 Tech products unchanged. (The category filter on /products works correctly; only the category-page instance is broken.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.