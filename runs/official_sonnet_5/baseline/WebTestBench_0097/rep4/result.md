# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight editing control is non-functional
    - Actual: Per-node "Weight" slider (role="slider", aria-valuemin=10, aria-valuemax=100, e.g. Code Review at aria-valuenow=60) did not respond to any interaction method: a real trusted click on the thumb, a real trusted mouse drag from the thumb to a point ~55px further along the track, and repeated ArrowRight keyboard presses while focused all left aria-valuenow unchanged at 60. Since aria-valuenow is maintained internally by the slider component itself (independent of the app's outer stale-render issue), this indicates the control itself does not process input, not merely a display-sync bug. Additionally, clicking an edge (the actual "competency relationship" between two nodes) only toggles a "selected" CSS class on the edge and surfaces no weight-value editing UI — no mechanism was found to change a relationship's weight number.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable switch does not visually reflect its own new state after toggling
    - Actual: Clicking the System Design competency's enable/disable switch (role="switch") correctly altered underlying analysis state (Coverage 68%→70%, Avg Level 3.3→3.4, Critical Path list changed from [Programming Languages, Problem Solving, System Design] to [Programming Languages, Problem Solving, Collaboration, +1] i.e. System Design was excluded as disabled), confirming the toggle IS registered by the engine. However the switch's own aria-checked attribute stayed "true" after the click (never flips to "false"), and the node card gives no visual indication of being disabled — so users cannot see/confirm the disable action took effect on the node itself.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the graph/relationships, only title/metrics/category counts
    - Actual: Clicked Apply on the "Product Manager" template while viewing Software Engineer. Title changed to "Product Manager", department to "Product", Coverage to 78%, Avg Level 3.8, Critical Path list changed to [Product Strategy, User Research, Communication, +1], and Scenario Simulation Category Adjustment competency counts changed (Technical 1/Behavioral 1/Leadership 1/Domain 3, vs Software Engineer's 3/2/0/1) — indicating the underlying analysis data DID switch to the Product Manager model. However, the graph itself did NOT update: node data-testids remained rf__node-prog-lang/sys-design/code-review/problem-solve/collab/agile (Software Engineer's competency IDs), and inspecting the React fiber props for the prog-lang node confirmed it still holds Software Engineer's competency data ("Programming Languages", level 4, weight 0.9) rather than any Product Manager competency (e.g. Product Strategy). The graph/nodes/relationships are not consistently replaced by the applied template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saving current model as a custom template does not actually create a retrievable template
    - Actual: Modified Software Engineer (lowered Agile Methodology to level 1, distinct from any existing template) and clicked "Save Current" in the Template Library panel. A toast appeared: "Template Saved - Current model configuration has been saved as a template." However, the Template Library list still showed exactly the same 5 pre-built templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) before and after the save — no new custom template entry was added, and there is no "My Templates" section anywhere in the UI to retrieve a saved custom template from. The save action is not functionally implemented despite the success toast.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Edits are not persisted after leaving and reopening a model
    - Actual: Opened Software Engineer (baseline Coverage 68%), lowered Agile Methodology's level to 1 via level-dot click (Coverage correctly dropped to 64%), then clicked the "Save" button in the toolbar. Navigated back to the Models list (in-app, no reload) and reopened Software Engineer: Coverage showed 68% (original baseline), Avg Level 3.3, Critical Path unchanged — the level-1 edit to Agile Methodology was lost. Edits do not persist across leaving/reopening the model.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls (enable switch, level, weight) show stale state, disagreeing with actual/summary state
    - Actual: After clicking System Design's enable/disable switch (real trusted click, confirmed to change underlying state since Coverage/Avg Level/Critical Path summary all updated), the switch's own aria-checked attribute remained "true" (should be "false"). Similarly the node's Level/Weight text and border-node-critical class remained at pre-edit values across nodes. Graph control state is inconsistent with the app's actual/summary state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path node highlighting does not re-render after data changes; graph shows stale highlighting
    - Actual: Toggled System Design's enable switch off. Critical Path summary correctly updated to 4 competencies (Programming Languages, Problem Solving, Collaboration, +1), removing System Design and adding Collaboration. However the graph's border-node-critical CSS class did NOT update: System Design node kept the critical-highlight class (stale, should be removed) and Collaboration node did NOT gain the critical-highlight class (stale, should be added) — critical path highlighting on the graph disagrees with the summary panel.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.