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
    - Issue: Panorama yaw is unbounded and never wraps at 360°, so rotating a full turn lands in an empty void with no hotspots.
    - Actual: Camera state yaw is not normalized modulo 360 (observed values up to yaw=3300 and yaw=-1650 with no wrap). Direct comparison at identical zoom=1.3: at yaw=0 hotspots rendered = [peking-opera, suzhou-embroidery]; after rotating exactly one full turn to yaw=360 hotspots rendered = NONE (empty); dragging back to yaw=0 restored [peking-opera, suzhou-embroidery]. Hotspots only ever render in a single non-repeating band (~yaw -240..+240); sweeping ~18000px of drag beyond it (yaw up to 3300) never made any hotspot reappear, so the view never wraps around. Zooming itself is fine: zoom clamps to 0.5–2.0 and zooming back out restored up to 7 hotspots, so hotspots are not permanently lost by zoom.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: The 春节 card presents an image belonging to a different heritage item (苏绣) instead of its own, via a hardcoded fallback that is itself broken.
    - Actual: HeritageCard renders item.images[0] with onError that overwrites src with the hardcoded URL photo-1513519245088-0e12902e35a6?w=800. 春节's own images[0] (photo-1548016655-f65f6a3d6e8c) fails to load, so the 春节 card ends up displaying photo-1513519245088-0e12902e35a6 — which the dataset assigns to 苏绣 as its images[1], i.e. an unrelated item's image. That fallback URL is also broken (verified naturalWidth=0 and an explicit load FAIL), so the 春节 card shows a broken image with no placeholder or failure prompt, unlike the detail page which does render a proper failure placeholder. The other 7 cards correctly show their own images[0] and load fine, and each item otherwise has a distinct image set with alt text matching its title.