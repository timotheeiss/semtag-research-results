# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: Edge weight is not editable
    - Actual: Clicking an edge (e.g. "Edge from business-acumen to storytelling", 70%) only toggles a "selected"/"animated" CSS class on the SVG path - no dialog, side panel, input field, or slider appears to change the weight value. Double-clicking and inspecting the DOM confirmed there are zero role=dialog elements, zero number inputs, and no modal/sheet/popover elements anywhere on the page after interacting with edges. The edge label is a static SVG <text> node, not an editable control. There is no discoverable mechanism to modify an edge's weight.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Enable/disable switch on competency nodes does not work
    - Actual: Clicking the enable/disable switch on a node (e.g. Data Visualization, Business Acumen) does not toggle it off: aria-checked stays "true", the node's CSS class/opacity remains unchanged (opacity:1, no disabled/greyed styling), and no visual graying occurs. Repeated clicks (via Playwright click and native DOM click) all left aria-checked="true". Coverage jumped once from 71% to 73% after the very first click but did not change again on subsequent identical toggle attempts and the node was never visually disabled, indicating the disable feature is non-functional/inconsistent rather than working as specified.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template does not correctly load its structure into the graph editor
    - Actual: Clicking "Apply" on the "Software Engineer" template updated the page header (title "Software Engineer", "Engineering", "6 competencies") and the Critical Path stat card (now listing "Programming Languages, Problem Solving, System Design"), but the actual graph nodes were NOT replaced: the React Flow canvas still contained the previous Data Scientist model's nodes (data-testid rf__node-ml, rf__node-statistics, rf__node-python, rf__node-data-viz, rf__node-business-acumen, rf__node-storytelling) with their old labels "Machine Learning", "Statistical Analysis", etc. This produces an inconsistent state where the header/stats describe one model while the visible graph shows a different, stale one.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Saving a custom template does not prompt for name/description and is not retrievable
    - Actual: Clicking "Save Current" immediately shows a toast "Template Saved - Current model configuration has been saved as a template" with no dialog to enter a template name or description. After saving, the Template Library list still shows only the original 5 pre-built templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) - no new custom template entry appears anywhere, and there is no "My Templates" section/tab in the UI at all. Checking localStorage shows it is empty, confirming the saved template is not persisted or retrievable.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [X] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: No state persistence and no explicit save/load mechanism
    - Actual: After making edits to the Data Scientist model (moved the "Machine Learning" node from its default position, changed its Level from Advanced back down triggering Coverage 73%->67% and a Risk Gap alert, then separately applied "Software Engineer" template and ran a Promotion simulation), reloading the page (full navigation to http://localhost:7097/) returned to the Models list, and re-opening the "Data Scientist" model showed it fully reset to its original state: Coverage back to 71% (not 67%), Critical Path back to "3 competencies" (Machine Learning, Python Programming, Statistical Analysis) instead of the simulated 4, the Scenario Simulation tab reset to "Recruitment" (not the "Promotion" tab last used), and the Machine Learning node's transform back to translate(0px, 0px) i.e. its original position (the drag was not preserved). The "Save" button in the header shows only a generic label with no confirmation of what it saves or where, and clicking through the flow earlier ("Save Current" in Template Library, tested under FT-8) does not persist to localStorage or any retrievable store. There is no explicit save/load/session-restore mechanism, and all in-progress edits are lost on navigation away and back.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Cannot verify prevention of invalid edges to disabled nodes - edge creation mechanism itself is non-functional
    - Actual: Attempted to create new dependency edges by dragging from a node's source connection handle to another node's target handle (React Flow connectable handles present in DOM, class react-flow__handle source/target). Tried business-acumen(source) -> data-viz(target), a pair with no existing edge, and data-viz(source) -> storytelling(target). In both cases, the edge list (queried via .react-flow__edge aria-label) remained unchanged (still exactly the original 5 pre-defined edges) - no new edge was created. No "Add Edge/Connection" button or context menu exists anywhere in the UI either. Combined with FT-5's finding that the node enable/disable switch does not functionally toggle a node's disabled state, there is no way to construct or observe the specific invalid scenario (adding an edge to a disabled node) because neither disabling a node nor creating a new edge works at all in this build. The constraint cannot be confirmed as implemented/enforced.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [X] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.