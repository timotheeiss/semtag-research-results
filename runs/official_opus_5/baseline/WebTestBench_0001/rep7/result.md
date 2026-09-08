# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: No way to browse the complete set of featured products; homepage caps the featured collection at 3 and the "View all" link ignores its featured filter
    - Actual: Marking/unmarking does update the homepage: featuring "QA Trail Recovery Press" and unfeaturing Sony changed "Editor's Top Picks" from Sony/Peloton/Dyson to Peloton/Dyson/QA Trail Recovery Press. However with 4 featured products the homepage still rendered only 3 (the 4th was hidden), and the section's "View all" link → /products?featured=true renders h1 "All Products" with "10 products found" listing every product (featured and non-featured). The featured=true query param is ignored and no featured filter control exists, so the complete featured set cannot be browsed.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Tag filter list is static and not derived from current product tags
    - Actual: After adding the new tag "qafreshtag" to the existing product Sony WH-1000XM5 (saved successfully; the tag shows on the detail page and matches keyword search), the Filters panel tag list on /products still contains only the original 35 seed tags. "qafreshtag" (and "luxury", plus "qatravel"/"portablebrew" from a newly created product) are absent, so the tag cannot be selected as a filter. Conversely, the removed tags "wireless" and "premium" are still offered — selecting "wireless" yields "0 products found".

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: No persistence — product data is held only in memory and resets to the seed data on every page reload
    - Actual: After creating "QA Aeropress Go"/"QA Trail Recovery Press" and editing Sony WH-1000XM5's tags and featured flag, reloading http://localhost:6001/category/fitness returned only the 3 seeded Fitness products — the created product was gone. Reloading /product/qa-aeropress-go earlier showed "Product Not Found". Sony's detail page after reload again shows the original tags headphones/wireless/noise-cancelling/premium (edits to "luxury"/"qafreshtag" and the featured=off change lost). localStorage and sessionStorage are both empty, confirming nothing is persisted.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publishing validation only covers name, category, short description and rating range; price, image, review, retailer link and specifications are unvalidated
    - Actual: "QA Minimal Product" was created successfully with only a name + short description — no image URL, no full review, no affiliate/retailer link and zero specifications (toast "Product created successfully"). A second product "QA Invalid Values Product" was created with price = -250 (admin list renders "-$250"), image URL "not-a-url" and retailer link "javascript:alert(1)". Only rating 9.9 was rejected (silent rejection, form stayed open); once rating was set to 4.5 the record saved despite the negative price and malformed URLs.

- [X] CS-22: An invalid product or category identifier shows a clear not-found state and does not display unrelated product data.


## Interaction
- [X] IX-13: Changing a search, filter, or sort criterion updates the product set, order, and result count without requiring a manual page reload.

- [X] IX-14: Selecting a product's image or name opens the detail page for that same product.

- [X] IX-15: Each product's purchase action opens the corresponding product page on an external retailer website.

- [ ] IX-21: Whenever category filtering is offered, choosing a different category updates both the active category and the displayed products.
  - Bug Report:
    - Issue: Category dropdown is inert on category pages
    - Actual: On /category/home the toolbar offers a category combobox pre-set to "Home Goods". Selecting "Fitness" (and separately "Tech Gadgets") from its listbox left the combobox still displaying "Home Goods", the H1 still "Home Goods", the URL still /category/home, and the same 3 Home products displayed. Nothing updated. (The same control does work on /products.)


## Content
- [X] CT-16: For every complete product record, the detail page displays the matching name, category, rating, price, brief description, full review, tags, key specifications, image, and retailer action.

- [X] CT-17: Every seeded product uses relevant category and tag values, and those values remain consistent between browsing and detail views.

- [X] CT-18: A product's name, category, price, rating, and brief description remain consistent across homepage, category, list, and detail views, while the detail view presents its complete review and specifications without truncation.