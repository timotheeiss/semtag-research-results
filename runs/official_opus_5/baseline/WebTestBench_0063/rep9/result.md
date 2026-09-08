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
    - Issue: Panorama rotation does not wrap at 360°; hotspots are permanently lost once the view is rotated beyond a narrow band
    - Actual: Rotation is tracked as an unbounded value (layer transform translateX = -5 x rotationDeg). At rotation 0° four hotspots render (苏,京,宜,昆); by ~36° zero hotspots render, and dragging further they never reappear. Sampled every 3° from 360° to 432° and every 18° from 0° to 360°: hotspot count stayed 0 for the entire remainder of the turn. At exactly 360° — visually the same view as 0° — 0 hotspots render instead of the original 4. Dragging back to rotation 0° restored the 4 hotspots, confirming hotspot azimuth is not computed modulo 360. Zoom alone is fine: zoom in to 1.8 hid some hotspots but zooming back out to 1.0/0.5 restored them (all 8 at scale 0.5).

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: The same photo is assigned to two unrelated heritage items, so it cannot be consistent with both; galleries rely on generic stock photos rather than imagery of the named heritage item
    - Actual: Unsplash photo-1513519245088-0e12902e35a6 is used both as 春节 (Spring Festival)'s primary/first image (its homepage card image and detail thumbnail 1) and as 苏绣 (Suzhou Embroidery)'s gallery image 2. A single photograph cannot depict both a folk New Year festival and Suzhou silk embroidery, so at least one item shows an image inconsistent with its name and description. That same shared image also fails to load (naturalWidth 0) in both places, so 春节's lead image never renders. Additionally several galleries use generic non-Chinese stock photography unrelated to the craft — e.g. 书法 (Chinese Calligraphy) uses photo-1455390582262-044cdead277a and photo-1481627834876-b7833e8f5570 (Latin-script notebook handwriting and a bookshelf), and 剪纸 (Paper Cutting) uses photo-1497366216548-37526070297c (an office interior).