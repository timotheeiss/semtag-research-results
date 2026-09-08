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
    - Issue: Panorama yaw is clamped instead of wrapping 360°; continued rotation permanently loses all hotspots
    - Actual: Dragging horizontally moves the scene (translateX 0→37.5→75), but the view never wraps. In one continuous gesture sweeping 40,000px in one direction, translateX saturated at 973.95px and the rendered hotspot count fell to 0 of 8 and stayed 0 at every sample (dx -4000 … -40000); no hotspot ever reappeared, i.e. the view never wrapped around. Dragging back within the same gesture (dx +1000…+8000) also stayed pinned at translateX 973.95 with 0 hotspots; a subsequent real drag only crept back ~38px (973.95→935.55) still showing 0 hotspots, so the user is left in an empty view with no hotspots to see or interact with. Repeated same-direction drag gestures are also ineffective: 20 consecutive identical drags left translateX frozen at 75 with an unchanged hotspot set. Zoom alone is fine (scale clamps 0.5–2; hotspot count 8 at 0.5 and returns when zooming back), so the defect is the non-wrapping, clamped rotation.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [X] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.