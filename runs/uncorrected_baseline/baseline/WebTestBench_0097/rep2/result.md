# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: No UI control to edit edge weight
    - Actual: Clicking an edge (e.g. "Edge from talent-acq to perf-mgmt") only selects/highlights it (adds "active" state); single click, double-click, right-click, Enter, and ArrowUp keys produce no dialog, input, or slider for editing the weight. No dialog/number/range input elements appear in the DOM after interaction. Only node-level "Weight" sliders exist; edges show a static percentage label with no editable control.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Disabling a node does not visually gray out/hide it or its edges
    - Actual: Clicking the enable/disable switch on the "Leadership" node (via real Playwright click and via synthetic click) never changes its aria-checked/data-state from "true"/"checked", the node's computed opacity stays 1 with no filter, and all 5 connected edges keep opacity 1 with unchanged classNames. The only observable effect was Coverage moving from 71% to 73%, suggesting some backend recalculation occurred, but there is no visible gray-out/hide indication on the node or edges as required.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template does not actually load its graph into the editor
    - Actual: Clicking "Apply" on the "Software Engineer" template changed the page header title to "Software Engineer" and updated the Critical Path stat text to reference Software Engineer competency names ("Programming Languages", "Problem Solving", "System Design"), but the actual graph nodes remained the old HR Manager nodes (data-id: talent-acq, hr-compliance, perf-mgmt, emp-relations, leadership, org-dev) with their original names/edges unchanged. The model data is inconsistent/partially applied.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: No naming/description dialog when saving custom template; no "My Templates" retrieval area
    - Actual: Clicking "Save Current" immediately shows a "Template Saved" toast with no dialog prompting for a template name or description (0 dialog elements in DOM). The Template Library list still shows only the same 5 original templates, with the current job's template card marked "Current" — no new distinct custom template entry was created, and there is no "My Templates" section to retrieve saved custom templates from.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [X] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: Neither implicit persistence nor the explicit Save button actually persists edited state
    - Actual: Reduced Programming Languages' level to minimum on Software Engineer model (Coverage dropped 68%→56%, Avg Level 3.3→2.8), then clicked the explicit "Save" button which showed a "Model Saved" toast ("Your changes have been saved successfully."). Navigated back to Home and re-opened Software Engineer: Coverage and Avg Level reverted to original values (68%, 3.3), and the level edit was lost. The Save mechanism gives a false-positive confirmation without actually persisting changes.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Cannot verify constraint validation because manual edge creation (drag-to-connect) is non-functional entirely
    - Actual: Attempted to drag a new connection from the Collaboration node's source handle to the (internally toggled-off, per the FT-5 switch bug) Agile Methodology node's target handle — no new edge appeared in the DOM (edge list unchanged, still only the original 5 edges). As a control, the same drag technique was tried between two normal, fully-enabled, unconnected nodes (Programming Languages → Problem Solving) and also produced no new edge. This shows edge creation via the UI is broken/unsupported in general, not that the system specifically validates and blocks invalid connections to disabled competencies — so the required constraint-prevention behavior (e.g., a blocked-connection cursor, error toast, or explicit rejection message) could not be demonstrated.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [X] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.