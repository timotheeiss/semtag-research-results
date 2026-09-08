# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete featured set; homepage featured strip is capped at 3 and the "View all" featured link ignores the featured filter
    - Actual: Marking/unmarking does update the strip (after creating featured "QA Zen Desk Lamp" and unfeaturing Sony, the homepage strip changed from Sony/Peloton/Dyson to Peloton/Dyson/QA Zen Desk Lamp). However the strip only ever shows 3 items: while 4 products were featured, the newly featured QA Zen Desk Lamp was hidden from the homepage. The section's "View all" link points to /products?featured=true, but that page ignores the parameter and renders "11 products found" (all products, featured and not), so visitors have no view of the complete featured collection.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter facet list is static (built from seed tags), not derived from current product tags
    - Actual: After adding tag "qa-fresh-tag" (and "qa-lighting-renamed") to the existing Philips Hue Starter Kit — a change that is live, since keyword search "qa-fresh-tag" returns that product and its detail page lists the tag — the /products advanced tag filter still lists only the original 35 seed tags; "qa-fresh-tag" and "qa-lighting-renamed" are absent, so the new tag cannot be selected as a filter. Conversely removed tags remain selectable and now return nothing (selecting "ambiance" → "0 products found"). Same for tags on newly created products (qa-desk/qa-lighting were never offered).

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Admin changes are in-memory only and are lost on reload/revisit
    - Actual: Created "QA Zen Desk Lamp" (Home Goods, featured) and "QA Minimal Product", unfeatured Sony, and changed Philips Hue tags — all visible in the running session. After reloading (navigating to http://localhost:6001/product/philips-hue-starter-kit) the app reset to the 9 seeded products: /products shows "9 products found" with no QA products, Philips Hue tags reverted to lighting/smart-home/ambiance/automation, and Sony is featured again. localStorage is empty (no persistence layer).


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation only checks name + short description; incomplete/invalid products are accepted
    - Actual: Submitting the admin form with only Name="QA Minimal Product" and a short description created and published the product (empty full review, empty image URL, empty affiliate link, no specification rows, price $0). Re-editing it and saving price="-50" and affiliate link="not-a-valid-url" also saved successfully — the admin list now shows "-$50" for that product. Only a fully empty form is rejected ("Please fill in all required fields"); rating is bounded 0-5 solely by the native number input max attribute.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category selector on the category page is inert
    - Actual: On /category/fitness the filter bar offers a category select (options All/Tech Gadgets/Home Goods/Fitness). Selecting "Home Goods" and then "Tech Gadgets" left everything unchanged: URL stayed /category/fitness, heading/category.name stayed "Fitness", the select still read "Fitness", and the grid still showed only Bowflex, Theragun, Peloton. On /products the same control works, so the failure is specific to category pages.


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.