# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured "View all" ignores the featured filter, so the complete featured set cannot be browsed
    - Actual: Marking/unmarking does update the homepage: after unmarking Sony WH-1000XM5, the homepage featured strip changed from [sony, peloton, dyson] to [peloton, dyson, qa-aeropress-go]. However the strip is capped at 3 items, and the "View all" link (/products?featured=true) does not apply the featured filter - it renders "10 products found" listing every product including non-featured ones (samsung, macbook, bowflex, theragun, breville, philips). With 4 products featured, there is no view that shows the complete featured set.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag-filter choice list is static and ignores tags added to products
    - Actual: After saving tag "qa-newtag" (and renamed "mood-lighting") on the existing Philips Hue Starter Kit, the advanced tag-filter list still contained exactly the original 35 seeded tags. "qa-newtag" and "mood-lighting" are absent, so the new tag cannot be selected as a filter and cannot return the matching product. The list also still offers now-unused tags "ambiance" and "automation" (ambiance matches 0 products). Tags on a newly created product (qatag-travel, qatag-brewing) were likewise absent.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price range filter applies a stale value and offers no maximum bound
    - Actual: Sorting works correctly (newest, rating 4.9->4.4, price asc 199->2495, price desc 2495->199) and min-rating 4.5+ correctly returns 8 products excluding Philips Hue (4.4). However the price slider is broken: it renders only ONE thumb (aria-valuemin=0, aria-valuemax=2495) so no upper bound can be set, and the applied minimum lags one step behind the displayed value. With min label "$710" the grid showed 5 products including Breville Barista Express at $699; with min label "$810" it still showed Dyson V15 at $749. State persisted after a 2s wait, so it is not a debounce.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin changes are in-memory only and are lost on reload
    - Actual: Created "QA Aeropress Go" via the admin form; it appeared in the admin list (id mtcy3kea3jaw8i7zmq9, Home/Featured/$39.99/4.3). After navigating to its detail URL /product/qa-aeropress-go (a page load), the page rendered "Product Not Found - The product you're looking for doesn't exist or has been removed." localStorage is empty and no backend API exists (/api/products returns the SPA HTML), so all additions and edits are discarded on reload or in a later session.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results violate the active minimum-price criterion
    - Actual: With price minimum displayed as $810 (and min-rating 4.5+), the result set included Dyson V15 Detect at $749, which is below the active minimum. At $710 minimum, Breville Barista Express ($699) was shown. The price filter evaluates the previous slider value rather than the current one, so results do not satisfy all active criteria.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only enforces name and description; incomplete products can be published
    - Actual: Submitting with only Name and Short Description created "QA Incomplete Product" (id mtcy1lm1fgr1ylfta74) and closed the form. The product has no full review, no image, no retailer link and zero specifications, yet it is published and appears in browsing as the 10th product. Its card renders an img with empty src and a "Buy Now" anchor with href="" (dead link). Only Name and Short Description are marked required; there is no validation of review, image, affiliate URL format, or a minimum of one complete specification.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [ ] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.
  - Bug Report:
    - Issue: Changing the price criterion does not update the product set; applied filter diverges from displayed control state
    - Actual: Search, category and sort changes update set/order/count reactively without reload. But after resetting the price slider to $0 (Home key), the grid stayed at 3 products (only items >=$810, the previously applied value). With all controls reading neutral (All Categories, Newest, Any rating, $0-$2495, thumb aria-valuenow=0) the count remained "3 products found" instead of 9, and changing sort did not force a recompute. Only a full page reload restored the correct 9-product set.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on the category page is inert
    - Actual: On /products the category select works correctly. But on /category/tech the same category select does nothing: choosing "Fitness" and then "Home Goods" both left the trigger label reverted to "Tech Gadgets", the heading at "Tech Gadgets", the URL at /category/tech, and the grid unchanged (samsung, macbook, sony). Neither the active category nor the displayed products update.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.