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
    - Issue: Drag-to-rotate is non-functional
    - Actual: The panorama UI advertises "拖拽旋转" (drag to rotate), but performing real trusted mouse drags (Playwright dragTo) across large distances (viewer center to nav.logo, viewer center to zoom buttons) as well as synthetic mouse/pointer event sequences produced zero change in the scene's rotation transform (stayed exactly "rotateX(0deg)") and zero change in hotspot left/top percentages (identical to 4 decimal places before/after). Rotation cannot be performed at all, so a full 360° rotation and hotspot wrap-around cannot be achieved. Zooming in/out, by contrast, does work (scale changes 1→1.2→1.4) and hotspots that momentarily go out of the visible set at high zoom (4→2 hotspots) do reappear when zooming back out, so zoom itself does not permanently lose hotspots, but the core rotate-through-360 requirement fails.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [X] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.