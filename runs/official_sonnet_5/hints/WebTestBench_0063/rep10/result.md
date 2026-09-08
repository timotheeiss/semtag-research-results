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
    - Issue: Panorama rotation does not wrap around; continuing to drag in one direction permanently moves hotspots out of view
    - Actual: Camera yaw is an unbounded value (translateX = -yaw*0.5, no modulo). Hotspot visibility uses |hotspotYaw - yaw| < 90/zoom with hotspot yaws fixed in range [-160,160]. Dragging continuously in one direction (tested ~750° via 2500px drag) results in ALL 8 hotspots becoming invisible with no reappearance as rotation continues past their yaw range - they only return if the user reverses drag direction back toward the original yaw. This means the view does not 'wrap around' a full 360°; it is an unbounded pan, not a cyclic panorama, so a user rotating continuously in one direction (as a real 360° panorama would allow) permanently loses access to hotspots until manually reversing. Zooming itself, however, correctly does not permanently hide hotspots (verified: zoom in hid 2 of 4 loaded hotspots, zoom out restored all 4).

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Irrelevant/mismatched image displayed for a heritage item when its own image fails to load
    - Actual: On the homepage listing card for '春节' (Spring Festival), the item's own first image (photo-1548016655) fails to load, and HeritageCard.tsx's onError handler swaps it to a hardcoded fallback URL (photo-1513519245088) which is actually 苏绣 (Suzhou Embroidery)'s second image — content unrelated to Spring Festival. The rendered card thus shows/attempts an unrelated (and here still-broken, naturalWidth 0) image under the 春节 title instead of a relevant image or an explicit failure indicator. (By contrast, the detail page correctly shows an explicit 'image failed to load' placeholder rather than an unrelated photo, e.g., for spring-festival's own thumbnail 1.)