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
    - Issue: Hotspots permanently disappear after rotating and do not reappear on wrap-around
    - Actual: Dragging to rotate the panorama works initially (translateX/rotateX update and different hotspots become visible as you pan), but after continued rotation in one direction hotspots vanish entirely (0 hotspots rendered) and never return even after dragging far enough that the internal yaw state (read from React fiber state) returned to an angle numerically equivalent mod 360 to the original heading (yaw=-1436.1° ≈ 3.9° effective, nearly identical to the starting yaw=0 which showed 4 hotspots) — yet 0 hotspots were rendered at that point. This indicates the hotspot visibility calculation uses the raw unbounded accumulated yaw instead of a properly wrapped/normalized angle, so hotspots are effectively lost permanently once the view has been rotated substantially, rather than reappearing as the view wraps around a full 360°.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Duplicate/inconsistent imagery across unrelated heritage items
    - Actual: The same underlying stock photo (unsplash photo id 1513519245088-0e12902e35a6) is used both as the primary card/hero image for "春节" (Spring Festival, a folk activity) and as gallery thumbnail 2 for "苏绣" (Suzhou Embroidery, a traditional craft) — two unrelated items in different categories. This shows images are generic placeholders not curated to each item's actual name/description, so image content cannot be relied on as relevant/consistent per item.