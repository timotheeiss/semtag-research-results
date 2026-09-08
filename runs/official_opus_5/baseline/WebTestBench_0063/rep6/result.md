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
- [ ] IX-10: Category-browsing navigation from a heritage detail page returns the user to the project list on the homepage.
  - Bug Report:
    - Issue: Category-browsing nav link on detail page does not navigate to homepage list
    - Actual: On /heritage/suzhou-embroidery the header "分类浏览" link has href="#categories"; clicking it only changed the URL to http://localhost:6063/heritage/suzhou-embroidery#categories, page still showed the 苏绣 detail (h1=苏绣, no project list). Footer category links (传统工艺/传统表演/民俗活动/传统美术) use the same href="#categories" and share the defect. Only the breadcrumb "非遗项目" and "浏览更多项目" links (href="/#categories") return to the homepage list.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw does not wrap around 360°; hotspots disappear permanently when rotating past the hotspot band
    - Actual: Dragging accumulates an unbounded yaw (inner layer style went 0 → translateX(660px) after 22 left-drags and −240px in the other direction, never resetting/wrapping). Hotspots only exist in the band translateX −120…+120 (e.g. 0: 苏京宜昆; +120: 书 only; +150 and beyond: zero .hotspot elements). Continuing to rotate far past a full turn (translateX 150→660) shows an empty scene with no hotspots and no wrap-around; hotspots only return by dragging back. Zooming itself is fine: zoom-in 1→2 narrows to 苏京 and zoom-out back to 1/0.5 restores all hotspots (all 8 visible at scale 0.5), so no permanent loss from zoom.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [X] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.