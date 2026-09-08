# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Non-functional control: edge/node weight cannot be changed
    - Actual: Attempted to change a competency's relationship weight via (1) clicking and double-clicking the edge label (e.g., "Edge from prog-lang to code-review", 80%) — only toggled a 'selected' CSS class, no editable input appeared; (2) the node-level Weight slider (role=slider, aria-valuenow) inside the Code Review competency node — tried click, keyboard ArrowLeft/ArrowRight, synthetic PointerEvent/MouseEvent dispatch, and a verified-working marker-based real Playwright drag targeting ~95% of the track width. In all cases aria-valuenow remained unchanged at 60 (confirmed via DOM read after each attempt). For comparison, the identical marker-drag technique successfully changed an equivalent Radix slider in the Scenario Simulation panel (value 2→0), proving the technique is valid and isolating the failure to controls nested inside .react-flow__node elements (likely missing 'nodrag' class causing React Flow's node drag handler to intercept/block pointer events before they reach the slider).

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Non-functional control: enable/disable switch does not toggle
    - Actual: Clicked the Enable/Disable switch (role="switch", aria-checked="true") on multiple competency nodes (System Design, Code Review, Agile Methodology). aria-checked and data-state remained "true"/"checked" after each click; no visual change to node styling, and metric cards (Coverage %, Risk Gaps) did not recalculate. Consistent with the same nodrag/pointer-interception issue affecting other node-nested controls (see FT-4).

- [ ] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.
  - Bug Report:
    - Issue: Template apply desyncs graph from header/metrics
    - Actual: Clicked "Apply" on the Product Manager template card in the Template Library. Expected the entire model (header, metrics, AND the competency graph) to switch to the Product Manager template. Observed: the page header title/department updated to "Product Manager"/"Product", and metric cards updated (Coverage 78%, Avg Level 3.8, Critical Path listing Product Strategy/User Research/Communication/+1), BUT the React Flow graph itself still rendered the previous Software Engineer model's 6 nodes unchanged (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) instead of Product Manager competencies. The graph and the surrounding metrics/header are now inconsistent with each other.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Save Current template action reports success but does not persist a new template
    - Actual: Clicked "Save Current" in the Template Library panel while viewing the unmodified Software Engineer model. Expected a new template entry to appear in the Template Library list reflecting the saved current configuration. A toast appeared: "Template Saved — Current model configuration has been saved as a template," implying success, but the Template Library list still shows exactly the same 5 original template cards (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) — no new template was added.

- [X] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.

- [ ] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.
  - Bug Report:
    - Issue: Simulation produces corrupted/invalid (NaN) results
    - Actual: Set the Behavioral category's Level Change to +2 (max) in the Recruitment simulation tab and clicked "Run Simulation". Expected the metric cards to recalculate with valid updated values reflecting the scenario. A toast "Simulation Applied" appeared and Avg Level updated to 3.8 (from 3.3), showing the simulation engine does apply adjustments; however the Coverage metric card displayed "NaN%" instead of a valid percentage — because the sibling Weight Change slider for the same category had independently corrupted to a "NaN%" value as a side-effect of dragging the Level Change slider (a reproducible bug also observed under CS-12), and that NaN propagated into the Coverage calculation shown to the user.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [X] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Value clamping constraint violated: slider produces NaN / blank display when dragged toward its bound
    - Actual: Expected the Category Adjustment sliders (Level Change: range 0-4 internal / -2..+2 displayed; Weight Change: range 0-60 internal / -30%..+30% displayed) to clamp to their min/max bounds when dragged past the track edges, always displaying a valid numeric value. Instead, after dragging the technical category's Weight Change slider toward/past its upper track boundary, the technical category's Level Change slider ended up with aria-valuenow="NaN" and its displayed numeric value disappeared entirely (label shows "Level Change" with no number), while Weight Change correctly clamped to its max (aria-valuenow="60", displayed "+30%"). This demonstrates the app does not reliably constrain slider values within their valid range — it can enter an invalid NaN state instead of clamping, both in a prior isolated test (Weight Change showed literal "NaN%") and in this cross-slider reproduction.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Non-functional control: no real-time visual feedback when dragging node-level weight slider
    - Actual: Performed a genuine Playwright pointer drag on the Code Review node's Weight slider thumb from its current position (55.5% left / 60% value) to a marker positioned at ~95% of the track width, expecting the thumb to visually slide and the percentage/aria-valuenow to update in real time. After the drag, the thumb's wrapper style remained at 'calc(55.5556% - 1.11111px)' and aria-valuenow remained '60' — no visual movement or value feedback occurred at all. The identical drag technique worked correctly on an equivalent slider in the Scenario Simulation panel (value changed 2→0 with visible thumb movement), confirming the technique is valid and the failure is specific to sliders nested inside React Flow competency nodes.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Non-functional control: enable/disable switch shows no interaction feedback
    - Actual: Clicked the enable/disable switch control on the Code Review, System Design, and Agile Methodology competency nodes. Expected immediate visual feedback (thumb slides, data-state/aria-checked flips, node dims or restyles when disabled). Observed: data-state stayed "checked" and aria-checked stayed "true" on every node after every click attempt; no visual or DOM state change occurred at all.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Metric cards do not recalculate because underlying node controls are non-functional
    - Actual: Expected the Coverage % / Avg Level metric cards to recalculate immediately when a competency's level, weight, or enabled state is changed on the graph. Since the node-level Weight slider, Level bar buttons, and Enable/Disable switch do not register any state change on click, drag, or keyboard interaction (see FT-4, FT-5, IX-13, IX-20), the displayed Coverage % and Avg Level values never changed across all attempted edits, so real-time recalculation could not be observed or confirmed.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Critical Path list does not update because underlying node controls are non-functional
    - Actual: Expected the Critical Path competency name list in the metrics panel to update when a competency's critical/enabled status or level is changed. Since clicking the Level bar buttons and the Enable/Disable switch on graph nodes produced no DOM state change whatsoever (verified via aria-checked and button class inspection before/after), the Critical Path list content never changed and could not be confirmed to react to edits.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.