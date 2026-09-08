# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mis-positioned: its action buttons (查看详情 / 继续探索) fall outside the viewport and cannot be reached, so the overlay is not usable at normal window sizes
    - Actual: Clicking hotspot suzhou-embroidery opens panorama.overlay containing correct title 苏绣, category 传统工艺, region 江苏 · 江南水乡, brief description and image. However the overlay is position:fixed with top = 50% of viewport height and no vertical centering offset (top=399.6px at vh=800, 599.6px at vh=1200, 999.6px at vh=2000) while being ~720px tall. At 1280x800 the 查看详情 button sits at y=1048 (below the 800px viewport) and page scrolling does not move it (fixed). Playwright click on panorama.overlay.continue / .detail fails with "element is outside of the viewport". Buttons only become reachable at an unrealistic viewport height of ~2000px.

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
    - Issue: Header "分类浏览" (category browsing) navigation does not leave the heritage detail page
    - Actual: From /heritage/suzhou-embroidery, clicking nav.categories (分类浏览, data-semtag-target=home.categories) only changed the URL to /heritage/suzhou-embroidery#categories; the page stayed on the detail view (detail.title still "苏绣") and the homepage project list was never shown. The breadcrumb 非遗项目 and the 浏览更多项目 button do navigate correctly to /#categories, so the header link is inconsistent/broken.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw does not wrap at 360°; after one full revolution every hotspot disappears permanently
    - Actual: Drag rotation works (0.3°/px). From yaw≈60° showing peking-opera|dragon-boat|yixing-teapot, dragging +1200px (exactly +360°) drove translateX to -210px (yaw 420°, no modulo wrap) and rendered ZERO hotspots; hotspot count was already 0 from yaw≈240° and stayed 0 for the rest of the turn. Only dragging -1200px back restored the same hotspots. Zoom itself is fine: zoom.in 1.0→1.8 and zoom.out back to 0.5 temporarily hides some hotspots at high zoom but all six reappear when zoomed out, so the failure is specifically the missing 360° wrap-around.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Gallery images are generic/unrelated stock photos (and some are dead links), not content matching the heritage item
    - Actual: Images come from Unsplash and their embedded metadata contradicts the items. 京剧 (Peking Opera, traditional Chinese theatre) main image = photo-1518611012118-696072aa579a, whose embedded IPTC keywords are "woman,girl,young,carpet,gym,aerobic,beautyrobic,caucasian,blond,fashion,...,fit,exercise,health,teen" and raw filename "beautyrobic871.tif" — an aerobics/gym photo. 端午龙舟 (Dragon Boat, 广东) image 3 = photo-1494500764479-0c8f2919a3d8, IPTC City = "Wanaka" (New Zealand landscape). In addition 2 of the 24 configured image URLs return HTTP 404 (photo-1513519245088-0e12902e35a6 in 苏绣's gallery and photo-1548016655-f65f6a3d6e8c), so those slots render "图片加载失败" instead of any item image.