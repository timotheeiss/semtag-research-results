# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Homepage featured section does not show all featured products; no way to browse complete featured set
    - Actual: With 4 products marked featured (Sony, Peloton, Dyson, Test Smart Speaker), the homepage "Featured" section only ever displayed 3 of them, hard-capped. The "View all" link navigates to /products?featured=true, but the featured=true query param is not applied - it shows all 10 products unfiltered (products.count stayed "10 products found"), so visitors have no way to browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: New tag added to an existing product does not appear as a tag-filter choice
    - Actual: Edited Sony WH-1000XM5 to add tag "audiophile-test" (saved successfully, confirmed via keyword search returning 1 product). Opened Filters panel on /products; the Tags list (35 tag chips) did not include "audiophile-test", so it could never be selected as a filter choice.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range filter has no maximum bound control; only a minimum threshold can be set
    - Actual: The "Price Range" slider is a Radix slider containing exactly one thumb labeled "Minimum" (aria-valuemin=0, aria-valuemax=2495); DOM inspection confirmed no second/"Maximum" thumb exists despite data-semtag-state listing both filters.price-min and filters.price-max. Moving the single thumb only filtered products with price >= threshold (e.g. moving to $1260 correctly reduced results to 3 items priced $1299+), so users cannot cap the upper price bound. Sort by "Newest", "Top Rated" (verified descending 4.9→4.3), "Price: Low to High" (verified ascending), and minimum-rating filter (4.5+ correctly returned 8/10 products) all worked correctly.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product additions and edits are not persisted; a full page reload reverts all data to the original seed state
    - Actual: After creating "Test Smart Speaker" and editing tags on Sony WH-1000XM5, a full navigation/reload to /admin showed only the original 9 seeded products with original tags - all created/edited data was lost.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Admin form allows publishing a product missing required content (image, retailer link, review, specs)
    - Actual: Submitting the Create Product form with only Name ("Incomplete Product Test") and Short Description filled (price left at default $0, no image URL, no affiliate link, no full review, zero specifications) succeeded: the product was created, appeared in the admin list, was publicly listed on /products (count 9→10), and even appeared as a "related product" on another product's detail page - despite having no image, no retailer link, no review, and no specs.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category filter dropdown on the /category/:slug page does not change the active category or displayed products
    - Actual: On /category/home, opening the category filter dropdown and selecting "Fitness" left the dropdown showing "Home Goods", category.name still "Home Goods", and the grid still showing the same 4 Home Goods products - no change occurred. (By contrast, the same dropdown on /products correctly switches category and product list; and the top-nav category links correctly navigate between category pages.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.