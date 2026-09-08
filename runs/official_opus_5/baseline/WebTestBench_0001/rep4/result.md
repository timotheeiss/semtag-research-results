# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: 'View all' featured link does not filter to featured products
    - Actual: Toggling featured works on the homepage collection (un-featuring Dyson V15 Detect and featuring Philips Hue Starter Kit QA changed Editor's Top Picks from Sony/Peloton/Dyson to Sony/Peloton/Philips QA). But the 'View all' link from that section goes to /products?featured=true, which ignores the featured flag: it renders the heading 'All Products' with '9 products found', listing all products including the 6 non-featured ones. There is no way to browse only the featured set, and the homepage section itself is capped at 3 items.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list does not include newly assigned tags
    - Actual: After creating 'QA Aeropress Go' with tags 'qatagalpha, portable' (tags visible on its detail page), the /products Filters panel still lists only the 35 seeded tags (ambiance…wireless). 'qatagalpha' is absent, so it cannot be selected as a tag filter, even though the product itself is in the 10-product list.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [ ] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.
  - Bug Report:
    - Issue: Price-range filter applies the wrong threshold (off by one step)
    - Actual: Sorting works: Newest, Top Rated (4.9→4.2), Price Low→High ($39.99→$2,495) and High→Low all ordered correctly. Minimum Rating '4.5+ stars' correctly returned 8 products all rated >=4.5. However the Price Range filter is wrong: with the minimum shown as $200 the $199 Philips Hue Starter Kit is still listed (9 products found); with the minimum shown as $100 the $39.99 product is still listed (10 products found).

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — all admin additions and edits are lost on reload
    - Actual: Created 'QA Aeropress Go' and edited 'Philips Hue Starter Kit' -> 'Philips Hue Starter Kit QA' ($219, 4.1, new tags/specs/featured). After reloading /products the list is back to the 9 seeded products: the created product is gone and the edited one reverts to 'Philips Hue Starter Kit', $199, rating 4.4 with the original description. localStorage is empty and no data is persisted server-side; state also resets on any full page load (e.g. submitting the header search form).


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [ ] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.
  - Bug Report:
    - Issue: Displayed results violate the active minimum-price criterion
    - Actual: On /products with Price Range minimum = $200 (label shows '$200'), the list still shows 'Philips Hue Starter Kit' at $199 and reports '9 products found'. With minimum = $100 all 10 products are shown, including 'QA Aeropress Go' at $39.99. The price filter behaves as if it lags one 100-step behind the displayed minimum. Search/category/rating criteria were satisfied correctly in other checks.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation does not enforce required recommendation content
    - Actual: In /admin > Add Product, submitting with only Name + Short Description filled (no full review, no image URL, no affiliate/retailer link, no specification, price left at 0) succeeded: toast 'Product created successfully' and 'QA Minimal Product' was added to the product list. Only Name, Category and Short Description are marked/enforced as required.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category dropdown on category pages is inert
    - Actual: On /category/tech the category dropdown offers All Categories / Tech Gadgets / Home Goods / Fitness, but selecting 'Fitness' and then 'Home Goods' left the trigger showing 'Tech Gadgets', the heading 'Tech Gadgets', the URL /category/tech and the same 3 Tech products. Neither the active category nor the displayed products changed. (The equivalent dropdown on /products works correctly.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.