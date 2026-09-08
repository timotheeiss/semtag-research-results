# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mis-positioned: its lower part, including the "查看详情" (View Details) action, is rendered outside the viewport and cannot be clicked.
    - Actual: Clicking the 京剧 hotspot opens panorama.overlay with correct title/category/region/description/image, but the overlay is position:fixed at top:50%/left:50% without a centering translate. At 1280x800 the overlay box is x 639→1312 (overflows right edge) and y 399→1040 (bottom 240px below viewport); the 查看详情 button rect is y=1048–1096, fully off-screen, and page scroll is locked, so Playwright click fails with "element is outside of the viewport". Same at 1440x1000 (button y=1148).

- [X] FT-3: A heritage detail page presents the matching title, full introduction, category, region, inclusion year when available, and a gallery containing multiple images.

- [X] FT-4: Users can search the heritage list, filter it by any supported category or region, and sort it by name, year, or region; the displayed items and result count update accordingly.

- [X] FT-5: After visiting a heritage detail page and returning to the panorama, the previous camera orientation and zoom level are restored.


## Constraint
- [X] CS-7: Every displayed heritage item belongs to one supported category: Traditional Crafts, Performing Arts, Folk Activities, or Traditional Art.

- [X] CS-8: A valid project detail link opens the matching heritage item; an invalid project identifier shows a clear not-found state and a way back to the homepage without displaying unrelated project details.

- [X] CS-9: Every heritage card displays the item's title, brief description, region or province, and category.


## Interaction
- [X] IX-10: Category-browsing navigation from a heritage detail page returns the user to the project list on the homepage.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama rotation does not wrap around 360°: after rotating roughly past ±150° every hotspot disappears and never returns, no matter how far the user keeps dragging in the same direction.
    - Actual: Drag-rotating right from the start view, hotspots vanish once the scene offset passes about -150 (translateX) and stay absent through -360 (a full turn), -720 (two turns) and -960 — 0 hotspots at every step; they only return by dragging back toward offset 0. Hotspots are only rendered in a ~±140 window around the initial yaw. Zoom behaviour is fine: zoom in 1.0→2.0 reduces visible hotspots but zooming back out to 1.0/0.5 restores all 8 with recalculated positions.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: An image presented for 苏绣 (Suzhou embroidery) is actually the Spring Festival photo and additionally fails to load.
    - Actual: 苏绣 gallery image #2 uses photo-1513519245088-0e12902e35a6, the exact same Unsplash asset used as the primary image of the unrelated item 春节 (Spring Festival) on the homepage card; in the 苏绣 gallery/overlay it reports naturalWidth 0 and the detail page renders "图片加载失败". Other items' image sets are distinct and load (naturalWidth 800) with alt text matching the item name.