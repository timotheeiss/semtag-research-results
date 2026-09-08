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
    - Issue: Category-browsing links are hash-only and do not leave the detail page
    - Actual: On /heritage/kunqu-opera, the header nav "分类浏览" (href="#categories") only changed the URL to /heritage/kunqu-opera#categories; the page still showed the 昆曲 detail (main h1 "昆曲", no search box / project list). The footer category links (传统工艺, 传统表演, 民俗活动, 传统美术, all href="#categories") behave the same. Only the breadcrumb link "非遗项目" (href="/#categories") actually returns to the homepage project list.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw does not wrap: hotspots permanently disappear after rotating past ~180°
    - Actual: Dragging continuously to the right from yaw 0: at +90° 4 hotspots visible, at +180° only 2, at +270° zero hotspots, and at +360° (a full turn, i.e. the original view) still zero hotspots — no hotspot ever wraps around. They only return when dragging back the same amount (−360° restored the original 4: 苏绣, 京剧, 宜兴紫砂, 昆曲). Source confirms visibility uses unwrapped relativeYaw: isVisible = Math.abs(hotspotYaw - yaw) < 90/zoom. Zoom behaviour is OK (zoom-in reduces visible hotspots, zoom-out restores them, e.g. back to 6 hotspots).

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Item images are unrelated generic stock photos, not the heritage subject
    - Actual: Images are random Unsplash stock. 京剧 (Peking Opera) cover image photo-1518611012118-696072aa579a carries IPTC keywords ["woman","girl","young","carpet","gym","aerobic","caucasian","blond","fashion","fitness","exercise"] — a gym/aerobics photo, not opera. 剪纸 (paper cutting) 3rd image photo-1497366216548-37526070297c is a modern office interior (EXIF copyright "©2016 nastuh.com", a known office-interiors photographer). 昆曲 gallery uses generic Western music photos (piano keys photo-1493225457124, concert photo-1470225620780). 春节 first image photo-1548016655-f65f6a3d6e8c returns HTTP 404 and does not load at all.