# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [X] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.

- [X] FT-3: A heritage detail page presents the matching title, full introduction, category, region, inclusion year when available, and a gallery containing multiple images.

- [X] FT-4: Users can search the heritage list, filter it by any supported category or region, and sort it by name, year, or region; the displayed items and result count update accordingly.

- [X] FT-5: After visiting a heritage detail page and returning to the panorama, the previous camera orientation and zoom level are restored.


## Constraint
- [X] CS-7: Every displayed heritage item belongs to one supported category: Traditional Crafts, Performing Arts, Folk Activities, or Traditional Art.

- [X] CS-8: A valid project detail link opens the matching heritage item; an invalid project identifier shows a clear not-found state and a way back to the homepage without displaying unrelated project details.

- [X] CS-9: Every heritage card displays the item's title, brief description, region or province, and category.


## Interaction
- [ ] IX-10: Category-browsing navigation from a heritage detail page returns the user to the project list on the homepage.
  - Bug Report:
    - Issue: Category-browsing navigation from detail page does not leave the detail page
    - Actual: On /heritage/suzhou-embroidery, clicking header nav "分类浏览" (nav.categories) and footer category link "传统工艺" (footer.categories.item.craft) only changes the URL to /heritage/suzhou-embroidery#categories; the detail page remains rendered (detail.title=苏绣, no listing.grid present). Only the breadcrumb "非遗项目" and "浏览更多项目" correctly navigate to /#categories.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Yaw is not wrapped; hotspots disappear permanently past ~250° of rotation, so a full 360° turn shows no hotspots
    - Actual: Dragging right increments panoramaStore.yaw without normalization (0→30→90→…→690). Visible hotspots: yaw 30 → 3 hotspots, 90 → 4, 150 → 3, 210 → 1, and from yaw 270 through 690 (well past a full 360° turn) ZERO hotspots are rendered. At yaw=360 (one full revolution) the view should again show 苏绣/京剧/昆曲/宜兴紫砂 but shows none; hotspots only return when dragging back below ~250°. Cause: visibility uses raw |hotspotYaw - yaw| < 90/zoom with no modulo. Zooming itself is recoverable (zoom 1.2 hid 京剧, zoom back to 1.0 restored it).

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Gallery images are generic/broken stock photos not consistent with the heritage items
    - Actual: All 24 gallery URLs are generic Unsplash stock photos with only auto-generated alt text ("苏绣 - 1"). 2 of 24 are invalid/hallucinated and 404 (photo-1513519245088… used as 苏绣 image 2, photo-1548016655… used as 春节 image 1), so those items present an error placeholder instead of a relevant image. Others are unrelated western stock: 书法 (Chinese calligraphy) gallery uses photo-1481627834876-b7833e8f5570 (Janko Ferlic library-bookshelf photo, per embedded IPTC Byline/Copyright) and photo-1455390582262 (Olympus 60mm macro pen/paper shot), neither depicting Chinese brush calligraphy; 京剧 gallery includes photo-1506905925346-21bda4d32df4, a well-known landscape stock image.