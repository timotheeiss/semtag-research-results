# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mis-positioned; its lower half (including the "查看详情" action) is off-screen and not clickable
    - Actual: Clicking hotspot 苏绣 opens panorama.overlay with correct content (传统工艺 | 苏绣 | Suzhou Embroidery | 江苏 · 江南水乡 | 2006年入选 | description | 查看详情 | 继续探索, image photo-1578662996442 loaded). However the overlay is position:fixed with its TOP at exactly 50% of the viewport height (top=359.5 at 720px viewport, top=499 at 1000px viewport) and is 577-721px tall, so its bottom (1220 vs innerHeight 1000) always lies below the viewport. Page scroll is locked while it is open, so 查看详情/继续探索 can never be reached; Playwright click aborted with "element is outside of the viewport".

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
    - Issue: Header category-browsing link does not navigate back to the homepage list from a detail page
    - Actual: From /heritage/suzhou-embroidery, clicking nav.categories (分类浏览) only changed the URL to /heritage/suzhou-embroidery#categories and kept the user on the detail page (project list never shown). Breadcrumb 非遗项目 and 浏览更多项目 do reach http://localhost:7063/#categories, but the page stays at scrollY=0 while the list section starts at 728px and the grid at 1068px in a 720px viewport, so the project list is still off-screen.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw is not wrapped at 360°; hotspots disappear permanently after ~270° of rotation
    - Actual: Starting from camera yaw 0 (4 hotspots: 苏绣/京剧/宜兴紫砂/昆曲) and dragging left in 30° steps, the hotspot set thins out and becomes empty at yaw -270°, -300°, -330° and remains empty at exactly -360° (which should be identical to the 0° view). Continuing to ~-2760° never brings any hotspot back; only dragging back toward 0° restores them. Zooming is fine: wheel/zoom-in to scale 1.5 reduces visible hotspots to 2, and zooming back out to 1.1 restores 苏绣/京剧/昆曲.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Item image mismatched with the item / primary image never renders
    - Actual: Homepage card for 春节 renders img alt="春节" with src photo-1513519245088-0e12902e35a6, which is 苏绣's gallery image #2 in heritageData (that URL is not in spring-festival's image list) and it fails to load (naturalWidth 0). On /heritage/spring-festival the first gallery image (photo-1548016655-f65f6a3d6e8c) also fails, so the main area shows "图片加载失败" instead of any 春节 image; 苏绣's image #2 is likewise dead. So 2 items present images that are neither loadable nor tied to the item.