# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mis-positioned; its action row (查看详情 / 继续探索) renders off-screen and is unclickable
    - Actual: Clicking the 苏绣 hotspot opens an overlay whose content (title 苏绣, 传统工艺, 江苏 · 江南水乡, 2006年入选, description, 3 images) is correct, but .overlay-content is position:fixed at left:50%/top:50% with its centering transform overridden by the animation transform (computed transform = matrix(1.0014,0,0,1.0014,0,0)). At 1280x800 the panel rect is left 640, top 400, right 1312, bottom 1040 — it starts at the viewport center and overflows the viewport. The 查看详情 link and 继续探索 button sit at top 1048–1097, i.e. entirely below the 800px viewport and clipped by the panel's overflow:hidden, so Playwright click fails with "element is outside of the viewport" (also reproduced at 1440x1100).

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
    - Issue: Category-browsing links from the detail page do not bring the project list into view (hash anchor never scrolled)
    - Actual: From /heritage/kunqu-opera, both the breadcrumb 非遗项目 link and the 浏览更多项目 button navigate to http://localhost:6063/#categories, but the page stays at scrollY=0 with the hero section filling the screen. The #categories project-list section's bounding top is 800px in an 800px-tall viewport (document height 1914px), i.e. entirely below the fold, and it is still not scrolled into view after waiting 2s. The user is returned to the homepage but not to the project list.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw does not wrap around 360°; it clamps at ±360 and hotspots disappear over most of the rotation range
    - Actual: Dragging horizontally changes the scene layer transform translateX from 0 up to +360px and down to -360px, then stops responding to further drags in that direction (no wrap-around). Hotspots are only rendered while translateX is within roughly ±120px: at 150–360px and -150 to -360px, document.querySelectorAll('.hotspot').length === 0, so the user who keeps dragging one way ends up in a dead zone with no hotspots and cannot continue rotating to bring them back — only reversing the drag direction recovers them. Zooming itself behaves correctly (scale 0.5–1.8, hotspot count grows from 4 to 8 when zoomed out and returns when zoomed back in), so the failure is the missing 360° wrap.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Item images are broken / borrowed from another item, so the imagery shown does not match the heritage item
    - Actual: 春节 (Spring Festival): its data image photo-1548016655-f65f6a3d6e8c returns 404, and the homepage card instead renders <img alt="春节" src="…photo-1513519245088-0e12902e35a6?w=800"> — the exact URL used as 苏绣's gallery image #2 — which also fails to load (naturalWidth 0) and shows no failure prompt on the card. 苏绣's gallery image #2 (same photo-1513519245088) likewise fails to load on its detail page. So for these items the displayed image is either a different item's photo or nothing at all, i.e. not consistent with the item's name/description. (Remaining 22 of 24 image URLs load, and alt text matches item titles.)