# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight slider is non-functional
    - Actual: Attempted to change competency weight (e.g. Talent Acquisition 90%, HR Compliance 80%) via click, mouse drag (multiple distances/targets), keyboard (ArrowLeft/Home) and synthetic pointer events on the Weight slider thumb. In every case aria-valuenow and the displayed 'Weight X%' label remained unchanged, and dependent metrics (Coverage, Critical Path) did not update from these attempts. Edge/relationship percentage badges in the graph (e.g. 'Edge from talent-acq to perf-mgmt 70%') also have no click-to-edit control - clicking only selects/highlights the edge with no editable weight surfaced anywhere. So users cannot change a competency relationship's weight through the UI.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disable/enable toggle: visual switch state does not reflect actual state, and repeated toggles fail to restore original state
    - Actual: Clicking the Talent Acquisition 'enabled' switch correctly excluded it functionally from calculations (coverage 71%→68%, avg level 3.5→3.4, critical path list changed from including Talent Acquisition to [Employee Relations, HR Compliance, Performance Management]). However the switch's DOM state (aria-checked='true', data-state='checked') and the node UI never changed to show a disabled state (no opacity/style change, no visual indicator) - violating the requirement that the node/relationships clearly show the disabled state. Worse, clicking the same switch two more times did NOT restore coverage/avg-level back to baseline (remained at 68%/3.4 after 2nd and 3rd clicks) - repeated toggles do not correctly act on the current underlying state, appearing to get stuck in the disabled state despite the switch always visually showing 'checked'. Required a full page Reset to restore the model.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates title/metrics/category-adjustment counts but does NOT update the competency graph nodes/edges
    - Actual: Clicked Apply on the 'Software Engineer' template while viewing the HR Manager model. Title header correctly changed to 'Software Engineer' / 'Engineering' / '6 competencies', Coverage/Avg Level metrics recalculated (71%→68%, 3.5→3.3), and Critical Path summary text changed to list 'Programming Languages, Problem Solving, System Design' (Software Engineer competencies). Category Adjustments panel counts also changed (technical 0→3, behavioral 1→2, leadership 2→0, domain 3→1), confirming the underlying data model did switch. HOWEVER the graph itself still rendered the OLD HR Manager nodes unchanged (Talent Acquisition, HR Compliance, Performance Management, Employee Relations, Leadership, Organizational Development with their original levels/weights/edges) - none of the new Software Engineer competencies (e.g. Programming Languages, Problem Solving, System Design) appeared as graph nodes at all. This is a severe inconsistency: the Critical Path metric names competencies that do not exist anywhere in the visible graph, and the graph/relationships were not replaced as required.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saving current model as a template shows a success toast but does not actually add it to the Template Library
    - Actual: On the HR Manager model page, clicked 'Save Current' in the Template Library panel. A toast notification appeared: 'Template Saved - Current model configuration has been saved as a template.' However, the Template Library list still shows exactly the same 5 original templates (Software Engineer, Product Manager, HR Manager [Current], Data Scientist, Sales Manager) both immediately after saving and after re-checking - no new custom template entry was added/retrievable. The save action is not actually persisted/reflected in 'My Templates'.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Save does not persist level/weight/enabled changes across navigation
    - Actual: Changed Talent Acquisition level to 2 (avg level 3.5→3.2, coverage 71%→63%, confirming the change was applied), then clicked 'Save'. Navigated to Models list and reopened the HR Manager model. Metrics reverted to original baseline (avg level 3.5, coverage 71%) - the saved level-2 change was NOT restored/persisted after leaving and reopening the model.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight bound (10%-100%) enforcement cannot be verified because the weight slider control is completely non-functional
    - Actual: Level bounds (1-5) are properly enforced: only discrete set-level-1..set-level-5 buttons exist (no way to set below 1 or above 5), and simulation level-delta sliders are correctly bounded (aria-valuemin=0, aria-valuemax=4). However, weight bounds (10%-100%) cannot be verified as enforced because the per-competency weight slider does not respond to any interaction (click, drag, or keyboard Home/End - already confirmed broken in FT-4): aria-valuenow stayed at 90 despite pressing Home which should clamp/move it toward the min=10 bound. Since the control that would need to enforce the 10-100% constraint is non-operational, the constraint's enforcement during direct edits cannot be confirmed. Additionally, running simulations with weight-delta sliders pushed to maximum did not change the displayed per-competency Weight text at all (stayed 90%/80%/80%), consistent with the known display-freeze bug, making it impossible to confirm the effective weight is clamped within bounds during simulations either.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node control does not display the current value/state after an edit; visual Level label desyncs from actual state
    - Actual: Clicked 'set-level-1' button for Talent Acquisition (button became [active], correctly indicating level 1 was selected). This correctly cascaded to summary metrics: Coverage 71%→59%, Avg Level 3.5→3.0, Risk Gaps 0→1 with a 'Competency Gap Alert' naming Talent Acquisition as below recommended level. However the graph node itself still displayed 'Level: Advanced' text (unchanged from before the edit) - the node's displayed Level label does NOT reflect the current/actual state (level 1), even though the set-level-1 button shows [active] and all dependent calculations correctly used the new value. This is a direct violation of the requirement that the graph control display the current value/state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical Path summary list disagrees with the graph's visual critical-path highlighting (invalid/inconsistent path)
    - Actual: Disabling Employee Relations recalculated the Critical Path summary immediately (count 3→4, list: Talent Acquisition, HR Compliance, Performance Management, +1). However, inspecting the graph's visual highlighting (React Flow edges with the 'animated' CSS class, the only distinguishing style found for any path-like emphasis) showed only edges rf__edge-e1 (talent-acq→perf-mgmt) and rf__edge-e5 (perf-mgmt→org-dev) were animated/highlighted, forming a graph path Talent Acquisition→Performance Management→Organizational Development. HR Compliance - explicitly named in the textual Critical Path summary - has no edge connecting it to this highlighted path (its only edge, hr-compliance→emp-relations, goes to the now-disabled Employee Relations node and is not animated). This means the textual critical-path list and the graph's visual highlighting name/imply different, disconnected sets of competencies - they do not agree with each other, and the highlighted graph 'path' is not a valid path containing all listed critical-path competencies.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.