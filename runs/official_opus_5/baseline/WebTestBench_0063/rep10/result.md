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
    - Issue: Panorama yaw does not wrap; hotspots are permanently lost after rotating a full 360°
    - Actual: Dragging continuously reduces yaw without modulo (state yaw reached -720, clamped). Hotspot count by yaw: 36°→4, -72°→4, -180°→2, -288°→0, -396°→0, -504/-612/-720°→0. At yaw -360/-720 (equivalent to 0° where 4 hotspots show) zero hotspots are rendered, and further rotation in either direction never brings them back until yaw returns numerically near 0.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: List image for 春节 is not its own image: a hard-coded fallback belonging to another item (苏绣) is shown, and it also fails to load
    - Actual: 春节's data image[0] (unsplash photo-1548016655) fails to load; HeritageCard's onError swaps src to photo-1513519245088, which is 苏绣's 2nd gallery image, so the 春节 card presents an unrelated item's picture. That fallback URL is itself broken (naturalWidth 0, ok:false), so the 春节 card shows no valid image and no failure message. The other 7 cards show their own matching, loading images.