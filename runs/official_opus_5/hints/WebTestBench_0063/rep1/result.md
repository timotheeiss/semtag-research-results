# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is clipped off-screen at common desktop viewports; title, description and the "查看详情" action are unreachable
    - Actual: Overlay content is correct (传统工艺 / 苏绣 / Suzhou Embroidery / 江苏 · 江南水乡 / 2006年入选 / brief description / image / 查看详情 / 继续探索), but panorama.overlay is position:fixed with top:400px and fixed height 640px, overflow hidden. At 1280x800 its bottom 240px falls below the viewport: overlay.title y=802, panorama.overlay.detail (查看详情) y=1048 and 继续探索 y=1048 are outside the viewport and cannot be scrolled into view (Playwright click times out with "element is outside of the viewport"); same at 1440x1000. Only the image, category badge and close button are reachable. Content becomes fully visible only at viewport height >= ~1300px.

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
    - Issue: Panorama yaw does not wrap; all hotspots disappear permanently after rotating past ~300°
    - Actual: Dragging right continuously: at yaw≈150° hotspots dragon-boat/yixing/spring-festival shown, at 210° only spring-festival, and from yaw≈270° onward zero hotspot elements are rendered — they never reappear through 360°, 450°, ... up to 870° of continued rotation. Hotspots only came back after dragging backwards to yaw≈-30°. (Hotspot visibility uses relativeYaw = hotspotYaw - yaw with no 360° normalisation.) Zoom behaviour is fine: zoom-in 1.0→2.0 narrows the visible set and zoom-out 2.0→0.5 restores up to 7 hotspots, so no permanent loss from zooming.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Item images are generic/irrelevant stock photos, some permanently unavailable
    - Actual: Images are unlabeled Unsplash stock URLs with generic alt text ("苏绣 - 1"). 2 of 24 image URLs return HTTP 404 and render the "图片加载失败" placeholder (苏绣 image 2 = photo-1513519245088, 春节 image 1 = photo-1548016655), so they can carry no content relevant to the item. Another (端午龙舟 image 3 = photo-1494500764479) is EXIF-geotagged at 44.70°S 169.12°E (New Zealand), which cannot depict Guangdong dragon-boat racing described by the item.