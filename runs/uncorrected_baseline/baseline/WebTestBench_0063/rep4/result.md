# Test Result

## Functionality
- [X] FT-1: Users can view a list of all intangible cultural heritage items on the website.

- [X] FT-2: Users can click on a heritage item from the list to navigate to its dedicated details page.

- [X] FT-3: Users can view complete details of heritage projects on their details page, including titles, descriptions, images, and regional information.

- [X] FT-4: Users can browse cultural heritage projects organized by type and category (traditional crafts, folk customs, performing arts).

- [ ] FT-5: The state is not lost after all operations on the page.
  - Bug Report:
    - Issue: Panorama drag-to-rotate interaction is non-functional, so its resulting view state can never be preserved.
    - Actual: While search text ("京剧") and the hotspot-overlay state (open overlay + panorama context for 苏绣) correctly persist when navigating to a detail page and back — confirming state preservation works for those actions — the panorama's core drag-rotate operation produces no state change at all. Simulated real mouse drags (via Playwright dragTo across 400+px, and via multi-waypoint hover simulating a continuous mousedown→mousemove→mouseup drag) on the drag layer (`.cursor-grab.active:cursor-grabbing.select-none`, hinted by on-screen text "拖拽旋转 • 滚轮缩放 • 点击热点探索") left the scene's transform unchanged (`scale(1.4) rotateX(0deg) translateX(0px)` before and after) and hotspot inline position (`left: 38.3333%; top: 46.1111%`) exactly unchanged. Since the drag operation itself never updates the rotation state, that operation's state cannot be said to be "preserved" — it simply never applies. (Zoom via the +/- buttons did work and did change which hotspots were visible, so zoom state changes correctly; only drag-rotate is broken.)

- [X] FT-6: The page should provide corresponding prompts for issues such as slow panorama loading and inability to open product detail page images.


## Constraint
- [X] CS-7: Each heritage item must belong to at least one valid category (traditional crafts, folk customs, or performing arts).

- [X] CS-8: You will be directed to the heritage project details page only when you click on a valid heritage project.

- [X] CS-9: Each heritage item on display must include all required fields (title, description, region).


## Interaction
- [X] IX-10: The navigation between the list view and the details view maintains a clear visual distinction between the two interface states.


## Content
- [X] CT-11: All exhibited artifacts must be genuine Chinese intangible cultural heritage items, and all are based on authentic and reliable historical materials and knowledge.