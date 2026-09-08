# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured 'view all' does not filter to only featured products
    - Actual: Marking/unmarking featured correctly updates the homepage featured preview (toggling Sony's featured off removed it from home.featured section, leaving Peloton and Dyson). However, clicking 'View all' on the featured section navigates to /products?featured=true which shows all 10 products (unfiltered 'products.count: 10 products found'), not just the featured subset, so visitors cannot browse the complete set of featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list does not update with newly added tags
    - Actual: Edited Philips Hue Starter Kit tags to include new tag 'qanewtag' (also renamed 'ambiance'->'mood-lighting', removed 'automation'). Detail page and keyword search correctly reflect the new tag set (search 'qanewtag' returns 1 product). However, the Filters > tag-filter list (filters.tags) still shows only the original static seed tag set (35 tags including 'ambiance' and 'automation' which were removed) and does not include 'qanewtag' or 'mood-lighting', so the new tag cannot be selected as a filter choice.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Product additions/edits are not persisted; lost on page reload
    - Actual: After creating 'QA Test Gadget Pro' and 'Incomplete QA Product' (both visible pre-reload, product count 11), and editing Philips Hue Starter Kit's category/tags, a full page reload of /products shows only the original 9 seeded products ('9 products found') with Philips Hue Starter Kit reset to Home Goods category. All admin additions and edits were lost, indicating state is held only in-memory client-side rather than persisted server-side.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: No publish gating - incomplete products are published and visible to visitors
    - Actual: Created a product with only Name and Short Description filled (no image, no affiliate link, no full review, no specifications, price left at default $0). The admin form accepted and saved it without validation error, and it immediately appeared publicly on /products ('Incomplete QA Product', 11 products found, listed with $0 price, presumably missing image/retailer link). There is no distinct publish/draft state or validation preventing an incomplete record from being shown to visitors.

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