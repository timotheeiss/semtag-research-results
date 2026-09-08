# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: Edge weight controls are not clickable/interactable
    - Actual: Each edge (e.g. "Edge from python to ml" 90%) is rendered as a clickable button with a weight badge, but clicking it always fails because the react-flow__nodes container div (class "space-y-3") overlays the entire graph canvas and intercepts all pointer events, even at different zoom levels (tested at default, fit-view, and 2x zoomed-out). Playwright's click action timed out after retries with "div.space-y-3 intercepts pointer events". Even dispatching a raw click event directly on the edge DOM element produced no visible change (no dialog, no selection state change), so there is no accessible way for a user to modify edge weight.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Enable/disable switch on competency nodes does not work
    - Actual: Clicking the enable/disable switch on a competency node (e.g. "Statistical Analysis", "Business Acumen") via precise Playwright locator click and via direct native DOM click() does not change the switch's aria-checked/data-state (remains "true"/"checked"), and the node's visual style is unaffected (opacity stays 1, no grayscale filter, no "disabled" class applied). The node and its edges remain fully visible and active; there is no way to actually disable a competency node in the UI.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template does not correctly load the new model into the graph editor
    - Actual: Clicking "Apply" on the "Software Engineer" template updated the page header (title, category), metrics panel (coverage 68%, critical path listing "Programming Languages, Problem Solving, System Design"), and marked the template as "Current" in the library — but the actual graph canvas still displays the old Data Scientist competency nodes (Machine Learning, Statistical Analysis, Python Programming, Data Visualization, Business Acumen, Data Storytelling) and their edges, unchanged. The editing interface (graph) was not actually loaded with the new template's competencies/edges, creating an inconsistent state between metrics and the visual model.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Saving a custom template does not actually persist or expose the new template anywhere
    - Actual: Clicking \"Save Current\" in the Template Library immediately shows a toast \"Template Saved - Current model configuration has been saved as a template\" with no dialog to name/describe the template. However, the Template Library list still only shows the original 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with no new entry added; there is no \"My Templates\" section anywhere on the page (confirmed via full-text search). Checking localStorage shows it is completely empty, confirming no template data was persisted anywhere. The save action is a no-op that only shows a false-positive success toast.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [ ] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.
  - Bug Report:
    - Issue: Running a simulation produces no visible impact on any key indicator
    - Actual: On the Software Engineer model's Training tab, Category Adjustments were pre-set to non-zero values (technical Level Change +1, domain Level Change +1). Clicking \"Run Simulation\" did not change the Scenario Simulation panel (no results/projected-impact section appeared, panel still just shows the same adjustment sliders and Run Simulation/Reset buttons), and the top metrics bar (Coverage 81%, Avg Level 4.0, Critical Path 3, Risk Gaps 0) and \"Model Healthy\" alert remained completely unchanged before and after running the simulation. There is no way to see any quantitative or qualitative impact of the simulated adjustment.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: Edits to the model are not persisted when navigating away and back
    - Actual: On the Data Scientist model, dragged the \"Machine Learning\" node from its default position (translate(0px,0px)) to a new position (translate(770.8px,-306.7px)) — the move succeeded and was visually reflected immediately. Then navigated away via the in-app \"Models\" nav link (client-side SPA navigation, no full page reload) back to the home page, and re-opened the Data Scientist model. The \"Machine Learning\" node was back at its original default position (translate(0px,0px)), meaning the layout edit was lost as soon as the user left the model page — there is no session/state persistence of user edits at all, not even within the same SPA session (let alone across a page reload). Additionally, localStorage was confirmed empty (checked earlier), and the \"Save\" button does not appear to write any state that survives re-entering the model.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Invalid operations on disabled competencies are not prevented
    - Actual: Toggling the enable/disable switch on \"Business Acumen\" does have a real backend effect (Coverage rose 68%→73% and Avg Level rose 3.3→3.6 immediately after toggling, consistent with the competency being excluded from calculations), even though the switch's own visual state never flips to \"unchecked\" (a separate display-desync bug). However, after disabling it, all of its editing controls (5 level-selector buttons and the weight slider) remained fully enabled with disabled=false/no aria-disabled attributes, and clicking a level-selector button on this disabled competency was accepted and immediately changed the global metrics again (Coverage 73%→66%, Avg Level 3.6→3.2, Risk Gaps 0→1). This shows the system does not block edits to a disabled competency's level, weight, or other properties — an invalid operation that should be prevented is allowed.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [ ] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.
  - Bug Report:
    - Issue: Node's own displayed level does not update after a level-editing interaction, despite backend recalculation happening
    - Actual: Repeatedly clicking different level-selector buttons on the \"Python Programming\" node (Beginner, then Intermediate) correctly triggered real-time recalculation of global metrics each time (Coverage 71%→61%→68%, Avg Level 3.5→3.0→3.3, Risk Gaps 0→1→0), confirming the backend state changes. However, the node's own \"Level\" text badge on the card being edited never updated and remained frozen at \"Advanced\" throughout both changes, even though the level-selector button correctly received the pressed/active state. This means the very control the user just interacted with gives no visual confirmation of the change on itself, only on decoupled downstream metrics elsewhere on the page — this is not a smooth or coherent editing experience, since the user cannot tell from the node itself what level is actually set.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [X] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.