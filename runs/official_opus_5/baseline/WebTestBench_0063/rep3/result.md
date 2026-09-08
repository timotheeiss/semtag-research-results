# Test Result

## Functionality
- [X] FT-1: Users can view the complete list of heritage items, together with an accurate visible result count.

- [ ] FT-2: Selecting a loaded hotspot in the panorama opens a usable information overlay for the matching heritage item, including its title, image, category, region, brief description, and an action to view full details.
  - Bug Report:
    - Issue: Hotspot overlay is mispositioned and its "View Details" / dismiss actions are rendered outside the viewport, so the overlay is not usable
    - Actual: Clicking the 京剧 hotspot renders overlay content with the correct data (category 传统表演, title 京剧/Peking Opera, region 北京 · 京城古韵, 2010年入选, description, 3 loaded images, 查看详情 link to /heritage/peking-opera). However .overlay-content carries the Tailwind centering classes "top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2" while an inline style transform:scale(1.00158) overrides them, so the panel is anchored at the viewport centre instead of centred on it: its rect is x=639,y=360,w=673,h=577 in a 1280x720 viewport. Consequently the 查看详情 link sits at y=864-912 and the 继续探索 button at y=864, both fully below the 720px viewport. The panel is overflow:hidden with scrollHeight 720 > clientHeight 576, so the actions cannot be scrolled into view. Playwright click on 继续探索 timed out with "element is outside of the viewport".

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
    - Issue: Category-browsing navigation link is a bare hash anchor and does not leave the detail page
    - Actual: On /heritage/suzhou-embroidery the header nav item "分类浏览" (Category Browsing) has href="#categories" instead of "/#categories". Clicking it changed the URL to http://localhost:6063/heritage/suzhou-embroidery#categories while the page remained the 苏绣 detail page (main h1 = "苏绣", no #categories section present, 0 heritage cards rendered). The four footer category links (传统工艺/传统表演/民俗活动/传统美术) share the same href="#categories" defect. Only the breadcrumb "非遗项目" and the "浏览更多项目" button (href="/#categories") correctly return to the homepage list.

- [ ] IX-12: Users can rotate the panorama through a full 360 degrees and continue to see and interact with hotspots as the view wraps around; zooming does not cause hotspots to be permanently lost.
  - Bug Report:
    - Issue: Panorama yaw is unbounded and never wraps at 360 degrees; hotspots exist only in a narrow band and are permanently lost once the user rotates past it
    - Actual: The panorama layer transform exposes yaw as translateX. Dragging horizontally moves it linearly and without limit: 24 successive left drags took it from 105px to 1185px, and 60 successive right drags took it from 1185px down to -1515px, with no modulo/wrap back to the start. Hotspots render only while translateX is roughly between +105px and -120px: across the 60-step sweep only 6 steps showed any hotspot (tx=105 →书法; 60 →剪纸,昆曲,书法; 15 →苏绣,京剧,剪纸,昆曲; -30 →苏绣,京剧,端午,宜兴; -75 →端午,宜兴,春节; -120 →春节). At every other position 0 hotspots were in the DOM. Rotating "a full 360 degrees" therefore scrolls into permanently empty space instead of wrapping around to the hotspots again.

- [X] IX-13: The detail-page image gallery lets users select each available image and updates the main image area or its explicit failure state to match the selected image.

- [X] IX-14: Search, filter, and sort selections remain applied after a user opens a heritage detail page and returns to the project list.


## Content
- [X] CT-11: Images presented for a heritage item are relevant to and consistent with the item's name and description.