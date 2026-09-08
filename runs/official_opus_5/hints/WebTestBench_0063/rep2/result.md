# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mis-positioned and clipped off-screen, making it unusable (content and "View Details" action unreachable)
    - Actual: Clicking hotspot 苏绣 does render overlay data in the DOM (title 苏绣, category 传统工艺, region 江苏 · 江南水乡, 2006年入选, description, image, 查看详情). But .overlay-content computes to position:fixed; top:360px; left:640px; transform:none — the Tailwind -translate-x-1/2/-translate-y-1/2 centering is overridden by the .overlay-content rule, so the box's TOP-LEFT is anchored at the viewport centre instead of being centred. At 1280x720 the overlay spans y=360..936 and x=640..1312: only the upper image strip is visible; the title, region, description and both buttons (查看详情 at y=864..912, 继续探索) are below the fold. Because it is position:fixed, scrolling cannot reveal them (bodyOverflow visible, but fixed element ignores scroll). Reproduced at 1512x982: overlay spans y=491..1211, 查看详情 bottom=1187 > 982; detailInViewport=false, continueInViewport=false. Playwright refuses the real click with "element is outside of the viewport"; only a synthetic el.click() reaches it.

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
    - Issue: Category-browsing navigation from a detail page reaches the homepage but never brings the project list into view; the #categories anchor scroll is not honoured
    - Actual: From /heritage/spring-festival the breadcrumb 非遗项目 (detail.breadcrumb.categories) and from /heritage/paper-cutting the 浏览更多项目 button (detail.browse-more) both navigate to http://localhost:7063/#categories, but the viewport stays at scrollY=0. The target section id="categories" does exist (document offset 728px) and the listing grid renders 8 items, yet the grid's viewport top is 1068px — entirely below the 720px fold (gridInView=false). The user is dropped on the hero banner and must manually scroll ~1000px to reach the project list the link points at. Reproduced from two independent detail pages and both category entry points.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama does not wrap around 360°; rotating past the hotspot band leaves an empty scene permanently with no hotspots and no wraparound
    - Actual: Yaw is unbounded rather than modulo-360. Dragging continuously left (30 drags x 200px = 6000px) drove the layer transform translateX monotonically from 0px to 930px with NO wrap; hotspots disappeared after step 3 (translateX 120px) and the scene stayed completely empty (0 hotspots) for the remaining 27 drags. Hotspots exist only in a narrow band, translateX ≈ +120px..-90px; all 8 keys live there and the view never returns to them by continuing to rotate in the same direction — only by dragging back the opposite way (34 reverse drags restored translateX 0 and the original 4 hotspots). So a user cannot rotate a full 360 degrees and see hotspots wrap around. Zoom half of the item is fine: scale clamps 0.5..2.0, hotspot count varies with FOV (2 at scale 2.0, 4 at 1.0, 8 at 0.5) and all 8 return on zoom-out, so zoom does not permanently lose hotspots.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: A heritage card presents a broken image whose hardcoded fallback is another heritage item's photograph, so the image shown is neither displayable nor consistent with the item
    - Actual: Data check of all 8 items x 3 images (24 total): alt text always matches the item and no photo is shared between items in the source data. But 2 of 24 URLs are dead (404 on load): 苏绣 image #2 (photo-1513519245088-0e12902e35a6) and 春节 image #1 (photo-1548016655-f65f6a3d6e8c). HeritageCard.tsx line ~50 handles onError by hardcoding target.src = 'photo-1513519245088-0e12902e35a6' — i.e. it substitutes 苏绣's own second photograph for ANY item whose image fails, which by construction cannot be consistent with that item's name/description. In practice the 春节 homepage card ends up with src=photo-1513519245088 and naturalWidth=0, so it renders a broken image under the label 春节. Detail pages handle this correctly by contrast (/heritage/spring-festival shows an explicit "图片加载失败" state and its thumbnails 2 and 3 load fine). Note: subject-matter relevance of the 22 working stock photos could not be visually confirmed under the DOM-only constraint; this verdict rests on the broken image and the cross-item fallback.