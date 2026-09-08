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
- [X] IX-10: Category-browsing navigation from a heritage detail page returns the user to the project list on the homepage.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Drag-to-rotate does not work; panorama never rotates from mouse drag input.
    - Actual: Dispatched real trusted mousedown→mousemove→mouseup gestures on the drag container (confirmed via captured event log, e.g. mousedown at 640,432 then mousemove to 941,433 then mouseup) in both horizontal (~300px) and vertical (~200-150px) directions, twice each. In every case the scene transform (rotateX/translateX) and all hotspot left/top positions remained completely unchanged (e.g. "scale(1) rotateX(0deg) translateX(0px)" before and after). Only the zoom +/- buttons visibly changed the scene (scale value), confirming the app is responsive but the drag-rotate gesture itself is non-functional, so users cannot rotate the panorama 360° or see hotspots wrap around via dragging as instructed by the on-screen hint "拖拽旋转 • 滚轮缩放 • 点击热点探索".

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [X] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.