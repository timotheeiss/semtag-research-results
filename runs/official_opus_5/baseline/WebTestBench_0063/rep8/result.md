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
    - Issue: Panorama yaw does not wrap at 360°; hotspots are permanently lost when rotating a full turn
    - Actual: Dragging right continuously increases yaw without modulo (measured via panorama layer gradient angle: 0° → 467°). Hotspot count is 4 at yaw ≈ 0/-0.6°, drops to 1 at 23°, and stays 0 for every step from 35° through 467° — including yaw 359.4° and 371.4°, where the view should have wrapped back to the starting scene and shown the same 4 hotspots. Hotspots only reappear by dragging back the opposite way toward yaw 0. (Zoom itself is fine: zoom in to scale 2 hides hotspots but zooming back out to 1.0/0.5 restores 4 and all 8 hotspots respectively.)

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Item images are generic/unavailable stock photos that do not match the heritage item's name and description
    - Actual: 春节 (Spring Festival): its primary image URL https://images.unsplash.com/photo-1513519245088-0e12902e35a6 returns HTTP 404, so the card image renders broken (naturalWidth 0) and the detail page shows '图片加载失败' instead of any Spring Festival imagery. 苏绣 (Suzhou Embroidery, described as 精细雅洁、图案秀丽、色彩清雅): its image photo-1578662996442-48f60103fc96 is effectively an all-black photo (avg luminance 17/255, max luminance 42/255, 0% of pixels above luminance 60) and cannot depict the described delicate colourful embroidery. 昆曲's third gallery image (photo-1470225620780-dba8ba36b745) is a dark magenta/purple concert-stage-lighting photo (avg RGB 48,15,77), unrelated to Kunqu opera.