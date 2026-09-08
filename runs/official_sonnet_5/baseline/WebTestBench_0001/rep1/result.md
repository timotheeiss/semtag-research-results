# Test Result

## Functionality
- [X] FT-1: An administrator can create and edit products in any supported category, including their name, price, rating, brief description, full review, image, retailer link, tags, specifications, and featured status; saved changes are immediately reflected in product browsing and detail views.

- [ ] FT-2: Marking or unmarking a product as featured updates the homepage featured collection, and visitors can browse the complete set of featured products.
  - Bug Report:
    - Issue: Featured filter link does not filter by featured status
    - Actual: Marked Apple MacBook Pro 14" as Featured via admin edit; it correctly shows a "Featured" badge on /products and category views. However homepage "Editor's Top Picks" section still only showed original 3 featured items (Sony, Peloton, Dyson) and did not include the newly-featured MacBook Pro. More critically, clicking "View all" (linking to /products?featured=true) does not filter at all - it displays all 9 products ("9 products found") instead of only the 4 actually-featured products.

- [X] FT-3: Keyword search matches product names, brief descriptions, and tags case-insensitively, and displays only matching products with an accurate result count.

- [ ] FT-4: A tag newly assigned to an existing product becomes available as a tag-filter choice and selecting it returns the matching product.
  - Bug Report:
    - Issue: Newly assigned tag not available as filter choice
    - Actual: After adding tag "qa-newtag" to Apple MacBook Pro 14" and saving, opening Filters on /products shows a fixed alphabetical list of tag chips (ambiance, android, apple, appliance, automation, camera, cardio, cleaning, coffee, cordless, cycling, dumbbells, espresso, flagship, headphones, home-gym, kitchen, laptop, lighting, massage, muscle, noise-cancelling, portable, premium, professional, recovery, smart-equipment, smart-home, smartphone, strength, subscription, therapy, vacuum, weights, wireless) that does NOT include "qa-newtag", even though the product carrying that tag exists and is searchable by keyword. The tag-filter chip list appears to be a static/precomputed set rather than derived from current product tags.

- [X] FT-5: When creating a product, an administrator can assign one or more new tag values, and the saved tags appear on the product and participate in keyword search.

- [X] FT-6: An administrator can add, rename, or remove tags on an existing product, and the saved tag set is reflected on the product detail page and in keyword search.

- [X] FT-7: An administrator can assign a product to exactly one supported category, and the product appears under the selected category after saving.

- [X] FT-8: Visitors can filter the complete product list by Tech Gadgets, Home Goods, or Fitness, and the visible products and result count update to match the selected category.

- [X] FT-9: Visitors can browse the complete product list, where every product exposes its name, category, price, rating, brief review, image, and a purchase action.

- [X] FT-10: Visitors can sort products by newest, top rating, ascending price, or descending price, and can filter them by a price range or minimum rating.

- [ ] FT-19: Product additions and edits remain available after the page is reloaded or revisited in a later session.
  - Bug Report:
    - Issue: Edits not persisted server-side; lost on reload
    - Actual: After editing Sony WH-1000XM5 (price 399->375, rating 4.8->4.9, description, tags, full review, added a spec) and saving successfully, a full page navigation/reload to /admin, /products, and /product/sony-wh-1000xm5 reverted all fields back to the original seeded values ($399, 4.8, original description/tags/review, no new spec). Changes only survive within the current in-memory SPA session and are lost on reload/revisit.


## Constraint
- [X] CS-11: Every product belongs to exactly one of the three supported categories: Tech Gadgets, Home Goods, or Fitness.

- [X] CS-12: Every displayed result satisfies all active search, category, price, and rating criteria; when multiple tags are selected, each result has at least one selected tag.

- [ ] CS-20: A product cannot be published until it has the content needed for a usable recommendation, including a name, supported category, non-negative price, rating from 0 to 5, brief description, full review, image, valid external retailer link, and at least one complete specification.
  - Bug Report:
    - Issue: Publish validation incomplete: allows saving products missing required fields and lacks explicit invalid-value feedback
    - Actual: Only Product Name, Category, and Short Description are enforced as required (submitting with all three blank shows toast "Please fill in all required fields" and blocks creation). However, submitting with Name/Category/Description filled but Image URL, Affiliate Link, Full Review, and Specifications left completely empty succeeded ("Product created successfully"), producing a live product ("QA Test Product", later "QA Fitness Gadget") with no image, an empty Buy Now link (href=""), no full review, and no specs - violating the requirement that image, a valid retailer link, and at least one complete spec be required to publish. Additionally, entering an out-of-range price (-50) and rating (7, outside 0-5) silently prevented submission with no error toast or inline message explaining why, giving the user no feedback on what was invalid.

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