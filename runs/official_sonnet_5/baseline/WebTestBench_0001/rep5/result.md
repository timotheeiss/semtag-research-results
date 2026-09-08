# Test Result

## Functionality
- [ ] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.
  - Bug Report:
    - Issue: Admin edits do not persist / are not reflected in browsing or detail views
    - Actual: Edited Samsung Galaxy S24 Ultra in /admin (price 1299->1349, rating 4.7->4.8, description prefixed 'QA-EDITED', tags += qatagxyz, featured toggled on). Toast said 'Product updated successfully' and the admin list briefly showed the new values. However /product/samsung-galaxy-s24-ultra, /products, and the homepage all still showed the original unedited values (price $1,299, rating 4.7, original description, original tags, not featured). Re-navigating back to /admin also reverted to the original values, confirming the edit was never actually persisted server-side.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured toggle change not persisted / not reflected on homepage
    - Actual: Toggling 'Featured on homepage' switch on for Samsung Galaxy S24 Ultra and saving showed a success toast and an immediate Featured badge in the admin list, but the homepage 'Editor's Top Picks' featured collection still only showed the original 3 featured products (Sony, Peloton, Dyson) - Samsung was absent. Re-visiting /admin also showed the product no longer marked Featured, confirming the change was not actually saved.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Newly added tag on existing product never becomes available as a filter choice
    - Actual: Added a new tag 'qatestunique' to Samsung Galaxy S24 Ultra via admin edit and saved (success toast shown). Even navigating via in-app client-side link (no hard reload) to /products and opening Filters, 'qatestunique' does not appear anywhere in the Tags filter list or elsewhere on the page - confirmed via DOM text search returning false.

- [ ] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.
  - Bug Report:
    - Issue: New product tags do not persist to backend/public catalog
    - Actual: Created a new product "QA Zenith Rowmaster 3000" via /admin Create Product form with tags "qazenithtag, rowing, cardio". Toast confirmed "Product created successfully" and it briefly appeared in the admin list. However, after a full page navigation to /products?search=qazenithtag, result was "0 products found" - the product and its tags never persisted to the public catalog/search index. A plain reload of /products also showed only the original 9 products (no Rowmaster entry), confirming the same non-persistence bug seen with edits also affects creation.

- [ ] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.
  - Bug Report:
    - Issue: Tag edits on existing product not persisted/reflected
    - Actual: Editing the tag list of Samsung Galaxy S24 Ultra (adding 'qatestunique') via the admin Edit Product form and clicking Save Changes produced a success toast, but the product detail page (/product/samsung-galaxy-s24-ultra) and /products keyword search continued to show only the original 4 tags with no trace of the new tag.

- [ ] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.
  - Bug Report:
    - Issue: New product category assignment does not persist to public catalog
    - Actual: Created "QA Zenith Rowmaster 3000" and assigned Category = Fitness via the admin Create Product form; it briefly appeared in the admin list tagged "Fitness". After a full page reload of /products, the product count remained 9 (unchanged) and the new product was completely absent - not just miscategorized but never persisted at all, so the category assignment could not be verified as saved to the backend.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Edits are lost on revisit/reload, not persisted
    - Actual: After editing and saving Samsung Galaxy S24 Ultra in admin (price/rating/description/tags/featured), navigating away to homepage/products and back to /admin showed the product reverted to its original pre-edit values, i.e. the edit did not remain available after revisiting.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [X] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.

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