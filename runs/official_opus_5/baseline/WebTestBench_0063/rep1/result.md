# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot info overlay is mispositioned and overflows the viewport, making the "View Details" action unreachable
    - Actual: Clicking the 苏绣 hotspot does open an overlay containing the correct title (苏绣 / Suzhou Embroidery), image, category 传统工艺, region 江苏 · 江南水乡, 2006年入选 and description. However `.overlay-content` uses Tailwind `top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2` while its CSS class applies `transform: matrix(1.00127,0,0,1.00127,0,0)` (a scale), which overrides the centering translate. The overlay's top-left is therefore anchored at the viewport centre: at 1400x1100 the overlay box is top=549.5 bottom=1270.5 (viewport 1100). The 查看详情 link and 继续探索 button sit at y≈1198-1246, entirely below the fold. Because the overlay is position:fixed, scrolling does not help - after window.scrollBy(0,500) (scrollY=304) the link rect was unchanged at y=1198 and elementFromPoint at the viewport bottom returned the overlay body div, not the link. Playwright click on the button failed repeatedly with "element is outside of the viewport". The overlay only becomes marginally clickable at an unrealistic ~1400px viewport height.

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
    - Issue: Detail-page category navigation returns to the homepage but ignores the #categories anchor, leaving the user at the top of the hero instead of at the project list
    - Actual: Both category-browsing controls on the detail page (the "非遗项目" breadcrumb and the "浏览更多项目" button) point at /#categories and do navigate back to the homepage with the list rendered and filters intact. However the #categories anchor is not honoured on this cross-page navigation: at 1280x800 the page lands at scrollY=0, the #categories section starts at viewport y=800 (exactly the fold) and the first heritage card is at y=1466.5 — completely off-screen. The user is dropped on the hero banner and must scroll manually to reach the project list. This is not a scroll-restoration artefact: the same anchor works for same-page navigation (clicking the footer "传统工艺" #categories link scrolled the window from scrollY=0 to scrollY=800). Re-confirmed at 1400x1400 (scrollY stayed 0, catTop 1340).

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama rotation does not wrap around 360°; all hotspots disappear permanently once the view is rotated past a limited angular range
    - Actual: Drag-rotation itself works (hotspot left/top % recompute live and the background transform updates), and zoom is fine (wheel zoom 0.5x–1.72x; hotspot count went 4→3 while zoomed in and returned to 4/5 when zoomed back out, so zoom never loses hotspots permanently). However the rotation angle is accumulated without any modulo-360 wrap. Sweeping left by ~5520px of drag (rotation state translateX went 0 → 828, i.e. >2 full turns) left button.hotspot count at 0 for 16 consecutive steps — the panorama showed no hotspots at all. Rotating back, hotspots only reappeared once the rotation value fell back inside roughly [-90, +130]: count was 0 at tx=153, 1 at tx=108, 3 at tx=63, 4 at tx=18/-27. So the user cannot rotate a full 360° and keep seeing hotspots as the view wraps around; instead the scene becomes a permanently empty view until they drag all the way back.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: One item's card presents another heritage item's photo as its own, and two configured images are dead URLs that cannot be displayed
    - Actual: Programmatically loading all 24 configured image URLs showed 2 failures: 春节 image[0] (photo-1548016655-f65f6a3d6e8c) and 苏绣 image[1] (photo-1513519245088-0e12902e35a6) both fire onerror. On the homepage the 春节 card's React src is the correct 春节 image, but its onError handler hard-codes target.src = 'https://images.unsplash.com/photo-1513519245088-0e12902e35a6?w=800' — i.e. it substitutes 苏绣's (Suzhou Embroidery) gallery image and renders it under alt="春节" with no failure indication. The rendered DOM src for the 春节 card is therefore photo-1513519245088-0e12902e35a6, which does not belong to Spring Festival and is inconsistent with its name/description; that fallback URL is itself dead, so the card ends up showing a broken image. (The detail page handles the same broken image better, showing an explicit 图片加载失败 state.)