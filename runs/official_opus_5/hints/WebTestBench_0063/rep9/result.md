# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mis-positioned; its "查看详情" action renders off-screen and cannot be clicked
    - Actual: Clicking hotspot 苏绣 opens panorama.overlay containing correct data (title 苏绣, category 传统工艺, region 江苏 · 江南水乡, brief description, image). However the overlay element has classes "top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2" but computed transform is "none", so at viewport 1440x1000 it renders at rect x=720,y=500,w=672,h=720 (inset bottom -220px), i.e. offset into the bottom-right and overflowing the viewport. panorama.overlay.detail (查看详情) and panorama.overlay.continue sit at y=1148, below the 1000px viewport; the overlay is position:fixed so page scrolling cannot reveal them. Playwright browser_click failed with "element is outside of the viewport" for both buttons; they could only be activated by a scripted DOM click.

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
    - Issue: Panorama yaw is never wrapped at 360°; all hotspots disappear permanently once the view is rotated past the initial window
    - Actual: Starting view showed 4 hotspots (苏绣, 京剧, 宜兴紫砂, 昆曲). Dragging horizontally rotates the scene (background transform translateX tracks the yaw), and one short drag correctly swapped in new hotspots (京剧, 端午龙舟, 宜兴紫砂, 春节). Continuing to drag in the same direction, every hotspot element was removed from the DOM and never returned: 42 successive drag samples from translateX -135px through -2025px (~5 full turns' worth of yaw, numerically 2025°) all reported zero rendered [data-semtag-id^='panorama.hotspots.item.'] elements. There is no wrap-around, so a user who rotates a full 360° can no longer see or interact with any hotspot.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [ ] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.
  - Bug Report:
    - Issue: Some heritage images are unrelated stock photos or broken links, not consistent with the item's name/description
    - Actual: 京剧 (Peking Opera, described as 中国传统戏曲) uses https://images.unsplash.com/photo-1518611012118-696072aa579a as its primary card/overlay/detail image; the file's embedded IPTC keywords are ["woman","girl","young","carpet","gym","aerobic","beautyrobic","caucasian","blond","fashion","person","people","asian","group","fit","exercise","health","teen"] — a fitness/aerobics stock photo with no relation to Peking Opera. In addition two configured images do not exist at all (HTTP 404): photo-1513519245088-0e12902e35a6 (苏绣 image 2, which renders as "图片加载失败") and photo-1548016655-f65f6a3d6e8c (春节 image 1), so those items present no relevant image for those slots.