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
    - Issue: Category-browsing nav link on detail page does not return to homepage list
    - Actual: On /heritage/peking-opera, clicking the header "分类浏览" (category browsing) link only changes the URL to /heritage/peking-opera#categories; the page stays on the detail view (no #categories section, no heritage cards rendered). Footer category links (传统工艺/传统表演/...) use the same relative "#categories" href and behave identically. Only the breadcrumb "非遗项目" / "浏览更多项目" (/#categories) return to the list.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw does not wrap around 360°; hotspots are lost once rotated past a narrow band
    - Actual: All 8 hotspots live in a narrow yaw band (background translateX ≈ +120px…-120px). Dragging continuously in one direction (33 gestures ≈ 6600px, translateX grew linearly to 990px) never wraps and never brings any hotspot back — 0 hotspots rendered the whole time; they only reappear by dragging back the exact same distance to the original band. Dragging 6600px in the opposite direction behaves the same. Zoom itself is fine: scale range 0.5–2 keeps recomputing hotspot positions and zooming back out restores all 8 hotspots.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Image shown for a heritage item is another item's picture / broken, not consistent with the item
    - Actual: 春节's list card image (heritageData images[0] = unsplash photo-1548016655-f65f6a3d6e8c) fails to load, and HeritageCard's onError handler hard-codes a substitute image "photo-1513519245088-0e12902e35a6" — which is 苏绣 (Suzhou Embroidery)'s own gallery image #2, i.e. an unrelated heritage item's picture. That fallback also fails to load (complete=false, naturalWidth=0), so the 春节 card presents a broken image, while /heritage/spring-festival shows "图片加载失败" for the same photo. Galleries elsewhere also reuse generic non-heritage stock photos (e.g. 京剧 image 3 = photo-1506905925346-21bda4d32df4, a well-known landscape shot; 剪纸 image 3 = photo-1497366216548-37526070297c, a generic office/workspace shot).