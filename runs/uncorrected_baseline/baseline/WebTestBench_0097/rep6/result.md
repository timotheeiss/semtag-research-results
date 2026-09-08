# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: No UI control to edit an edge's weight value
    - Actual: Clicking, double-clicking, right-clicking, and hovering an edge (e.g. "Edge from talent-acq to perf-mgmt 70%") only selects/highlights it; no input, slider, or dialog appears to change its weight. The only editable "Weight" sliders belong to nodes (competencies), which control a separate node-level attribute (e.g. Talent Acquisition node Weight=90% differs from its outgoing edge's 70%), not the edge itself. No mechanism exists in the DOM to modify edge weight.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Disabling a competency node does not visually gray out/hide it, and toggle state is unreliable
    - Actual: Clicking the enable/disable switch on nodes (e.g. Talent Acquisition, Employee Relations) via mouse click, keyboard Space, and repeated toggles never changes the switch's rendered state (aria-checked/data-state remain "true"/"checked", node opacity stays 1, no gray styling applied) even though Coverage/Avg Level/Critical Path values did shift somewhat after clicks (e.g. Coverage 71%→68%→69%, critical path membership changed inconsistently, at one point re-including a node that should have been toggled off). This indicates the disable toggle provides no visual feedback and its effect on calculations is inconsistent/unreliable, rather than cleanly removing the node from calculations with a grayed-out/hidden appearance.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template only partially updates the model; the graph itself does not load the new template's competencies
    - Actual: Clicking "Apply" on the Data Scientist template while viewing HR Manager showed a "Template Applied" toast and updated the page title to "Data Scientist" and the Critical Path panel to show Machine Learning/Python Programming/Statistical Analysis, but the graph canvas still rendered the old HR Manager nodes (data-id: talent-acq, hr-compliance, perf-mgmt, emp-relations, leadership, org-dev) with their original edges/weights instead of the Data Scientist competencies. The model was not actually loaded into the editing interface.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Save-as-template flow missing naming/description input and does not persist retrievable template
    - Actual: Clicking "Save Current" in the Template Library immediately shows a "Template Saved" toast with no dialog/prompt to name the template or add a description. After the toast, a fresh snapshot of the Template Library panel still shows only the original 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist [Current], Sales Manager) — no new custom template entry appears, and there is no "My Templates" section or list anywhere in the DOM. The saved template is therefore not retrievable, contrary to the requirement that users can name it, add a description, and retrieve it later from "My Templates".

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [ ] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.
  - Bug Report:
    - Issue: Simulation impact not reflected anywhere in UI
    - Actual: Selected Promotion mode (presets: technical Weight Change -5%, behavioral Weight Change +10%) and clicked "Run Simulation". A toast appeared: "Simulation Applied — Competency levels and weights have been adjusted based on the scenario." However, comparing full-page state before and after: all four top KPIs (Coverage 71%, Avg Level 3.5, Critical Path 3 competencies, Risk Gaps 0) are byte-identical, and every node's Level/Weight values in the graph (Talent Acquisition 90%, HR Compliance 80%, Performance Management 80%, Employee Relations 85%, Leadership 75%, Organizational Development 70%) are unchanged from the pre-simulation snapshot. No impact/diff/comparison panel appears anywhere on the page. The simulation therefore shows no quantitative or qualitative impact on any key indicator despite claiming to have applied changes.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: Explicit Save does not persist edits across navigation
    - Actual: On the HR Manager model, toggled a node's switch causing Coverage to change from 71% to 72%, then clicked the explicit "Save" button in the page header (a toast briefly appeared). Navigated away via the "Models" nav link (not a reload) and back into the HR Manager model. Coverage reverted to 71%, the original unedited value — the saved edit was not retained. Neither the default leave/return flow nor the explicit Save button persists model edits, and there is no other save/load mechanism (e.g., localStorage-backed autosave) that retains changes.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: No functional edge-creation mechanism exists to test/enforce the constraint
    - Actual: Attempted to create a new dependency edge by dragging from a node's source handle (bottom) to another node's target handle (top), both between two enabled nodes and between a disabled node and an enabled node. In all cases no new edge was created (edge count/list remained unchanged at 5 original edges). Since the graph provides no working way to add new dependency edges at all, there is no observable mechanism that specifically prevents adding edges to/from disabled competencies - the constraint cannot be demonstrated as implemented.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [X] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.