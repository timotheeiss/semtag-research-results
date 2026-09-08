# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Information overlay is mispositioned; its "查看详情" (view full details) action is permanently off-screen and unclickable
    - Actual: Clicking the 苏绣 hotspot does open panorama.overlay with the correct title (苏绣), image, category (传统工艺), region (江苏 · 江南水乡), year and brief description. However the overlay panel is position:fixed with computed transform matrix(1.00084,0,0,1.00084,0,0) — the inline scale animation overrides the intended -translate-x-1/2/-translate-y-1/2 centering, so the panel is laid out from the viewport centre outward: rect x 640→1312 and y 360→936 in a 1280x720 viewport. The action row sits at y≈864–912, entirely below the viewport, and because the element is fixed, page scrolling does not bring it into view. Playwright refused the click on panorama.overlay.detail and panorama.overlay.continue with "element is outside of the viewport" after scrolling. Geometry makes this permanent: overlay top = 50vh and height = 80vh, so 30% of the panel is always clipped at any viewport height.

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
    - Issue: Panorama yaw is not wrapped modulo 360, so hotspots vanish after rotating past ~±180° and a full 360° turn returns to an empty scene
    - Actual: Dragging right in 90° increments from yaw 0: yaw 90 → 4 hotspots (peking-opera, dragon-boat, yixing-teapot, spring-festival); yaw 180 → 2 hotspots; yaw 270 → 0 hotspots; yaw 360 (a complete revolution, geometrically identical to the start) → 0 hotspots instead of the original 4 (suzhou-embroidery, peking-opera, yixing-teapot, kunqu-opera). The background layer kept translating (translateX -180px) so the view itself rotates, but hotspot visibility uses the unwrapped test |hotspotYaw - yaw| < 90/zoom, which can never be satisfied once |yaw| exceeds ~270. Dragging back to yaw 0 restored all 4 hotspots, confirming the loss is caused by the missing angular wrap rather than by unloaded data. Zoom behaviour is correct: zoom 2 → 1 hotspot, zoom 0.5 → all 8, and returning toward zoom 1 restored them, so zoom alone does not permanently lose hotspots.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Item images are generic/unrelated stock photos, plus some broken URLs
    - Actual: Images come from generic Unsplash stock IDs with no relation to the heritage subject. Canvas colour analysis contradicts the subjects: 剪纸 (paper-cutting, characteristically bright red) images have red-dominant pixel shares of 1%/0%/0% and one is a blue-grey landscape-like image (avg RGB 150,167,187); 春节 (Spring Festival, characteristically red/lantern imagery) shows 0%/6% red with neutral grey averages; 昆曲's third image is a dark purple night-lighting image (avg RGB 46,13,76) typical of a stage/DJ stock photo rather than Kunqu opera; 京剧's third image (photo-1506905925346-21bda4d32df4) is a well-known blue-toned outdoor landscape photo (avg RGB 100,101,120, 0% red). Additionally two configured images never load at all: photo-1513519245088-0e12902e35a6 (苏绣 image 2) and photo-1548016655-f65f6a3d6e8c (春节 image 1), rendering only the "图片加载失败" placeholder.