# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Competency Weight slider cannot be changed via drag, click-on-track, or keyboard, blocking relationship/weight editing
    - Actual: On the "Code Review" node's Weight slider (role=slider, aria-valuenow=60, min=10, max=100): (1) dragging the thumb only moved the whole graph node by a few px instead of the slider value; (2) dispatching pointerdown/mouseup at 85% of the track position left aria-valuenow unchanged at "60"; (3) focusing the slider and pressing ArrowLeft/ArrowRight moved the selected node on the canvas instead of the slider (confirmed via react-flow's own a11y live-region announcement "Moved selected node..."). A browser console TypeError was also captured during the pointerdown: "Cannot read properties of null (reading 'document') at nodrag_default (reactflow.js)" - indicating the node's interactive controls are missing react-flow's required "nodrag" class, causing react-flow's own pan/drag handler to intercept and crash on pointer events meant for the slider. As a result there is no working way to change a competency's Weight (and no edge-level UI exists either), so relationship/weight cannot be edited at all.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable toggle switch does not visually update and cannot be re-enabled
    - Actual: Clicking a competency's enable switch (e.g. Collaboration) does change the coverage/avg-level calculation once (68%→70%, avg 3.3→3.4, consistent with the competency being excluded), but the switch's aria-checked attribute stays "true" and the node card's border/opacity never change to indicate a disabled state (border-color and opacity remain unchanged, no dimming). Clicking the same switch a 2nd and 3rd time has no further effect - metrics stay stuck at 70%/3.4, so the competency cannot be toggled back to enabled. This violates both the "clearly show disabled state" and "repeated toggles act on current state" requirements.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template only partially replaces the model - summary/critical-path text updates but the competency graph does not
    - Actual: Clicking "Apply" on the Product Manager template updated the page heading to "Product Manager" and the summary metrics (Coverage 78%, Avg Level 3.8, Critical Path 4: "Product Strategy, User Research, Communication, +1"), but the React Flow graph still rendered the OLD Software Engineer competency nodes unchanged: "Programming Languages", "System Design", "Code Review", "Problem Solving", "Collaboration", "Agile Methodology" with their original edge weights (80%, 70%, 90%, 60%, 50%) - none of which are Product Manager competencies. The model is left in an inconsistent state where the summary panel and the graph disagree.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not persisted or retrievable
    - Actual: Clicking "Save Current" in the Template Library shows a "Template Saved" toast, but the new template never appears in the Template Library list (still shows only the original 5: Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager). After reloading the app, the homepage "Templates" stat still reads "5" (unchanged), and there is no "My Templates" section anywhere to retrieve the saved template. The save action gives positive feedback but has no observable persisted effect.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Save does not persist competency state changes across reload
    - Actual: After disabling the "Collaboration" competency (Coverage changed 68%→70%, Avg Level 3.3→3.4, confirming the change registered), clicking the "Save" button showed a "Model Saved - Your changes have been saved successfully" toast. However, after reloading the page and reopening the "Software Engineer" model, Coverage/Avg Level reverted to the original values (68%/3.3), showing the disabled state was not actually persisted despite the success confirmation.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight bound enforcement (10-100%) cannot be verified because the Weight slider is completely non-interactive
    - Actual: Level bounds (1-5) are structurally enforced by design - each node exposes exactly 5 discrete Level bar buttons (verified: clicking them changes level within 1-5, e.g. Code Review 3→5, Programming Languages 3→2→5, with corresponding Coverage/Avg Level changes each time), so a level outside 1-5 can never be selected. However, Weight bound enforcement (declared as aria-valuemin="10"/aria-valuemax="100" on the slider) could not be tested at all: real DOM focus was confirmed on the slider (document.activeElement === slider) and pressing "End" (which should jump a Radix slider to its max, 100) left aria-valuenow unchanged at "60"; drag and click-on-track attempts (per FT-4) also had zero effect. Since the Weight control cannot be changed by any interaction, it is impossible to confirm the app actually clamps Weight to the 10-100% range - the constraint is unverifiable due to the control being non-functional.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage does not reliably update immediately for all three trigger types (weight changes are blocked entirely)
    - Actual: Coverage % does update immediately (same render, no reload/save needed) when a competency is enabled/disabled (68%→70%) or when its Level buttons are clicked (68%→74% after raising Code Review's level). However, Weight changes cannot be applied at all - the Weight slider is non-interactive (confirmed separately in FT-4: drag, click-on-track, and keyboard all fail to change aria-valuenow), so Coverage's response to a weight change can never be observed/verified. Additionally, the related Risk Gaps metric was observed to lag one interaction behind an actual level change (see CT-17), showing the "immediate update" guarantee is not consistently honored across all summary metrics.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls do not reflect current value/state after edits, and Critical/Gap indicators disagree with the summary panel
    - Actual: Multiple graph-vs-summary inconsistencies were found: (1) After toggling a competency's enable switch, the switch's own aria-checked/data-state and the node card border/opacity never change, even though the summary Coverage/Avg Level metrics confirm the toggle took effect (see FT-5). (2) After clicking a node's Level bars to change level (e.g. Code Review 3→5), the node's own Level bar fill and "Level: Intermediate" text label stay unchanged while Coverage/Avg Level in the summary change accordingly. (3) The node's Weight slider/label is frozen at its original value and cannot be changed at all. (4) The graph's "critical" highlight (border-node-critical CSS class) is static and tied to each competency's fixed Critical flag; when the computed Critical Path list in the summary changed (e.g., to include "Collaboration" after disabling "System Design"), the graph highlighting was NOT updated to match - System Design kept its highlight after leaving the list, and Collaboration never gained it. (5) When the summary showed a "Competency Gap Alert" for Programming Languages (Risk Gaps=1), no corresponding gap indicator/badge appeared on that competency's node card in the graph. In all cases the graph controls/indicators are stale relative to the authoritative summary panel state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path list only partially recalculates and its graph highlighting never updates
    - Actual: Two tests were run. (1) Raising Code Review's Level from Intermediate(3) to Expert(5) changed Coverage (68%→74%) and Avg Level (3.3→3.7) but the Critical Path list stayed exactly "Programming Languages, Problem Solving, System Design" - unchanged despite the underlying score change, so level edits do not recalculate the critical path. (2) Disabling the "System Design" competency (which IS in the critical path) DID change the critical path list (to "Programming Languages, Problem Solving, Collaboration, +1"), but the graph's visual highlight for critical nodes (CSS class "border-node-critical" on the node card) was NOT updated to match: "System Design" (removed from the critical path list) still shows the "border-node-critical" highlight, while "Collaboration" (now newly included in the critical path list) does NOT get the highlight. The graph highlighting is static (tied to each competency's fixed "Critical" badge flag) and never reflects the actual, currently-computed critical path.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Risk-gap warning appears correctly but does not clear immediately when the underlying issue is resolved
    - Actual: Lowering "Programming Languages" (a Critical competency) to a low level correctly triggered Risk Gaps to go from 0→1 and displayed a "Competency Gap Alert" naming it. Raising the level back up to the maximum (5/Expert) via the level buttons immediately updated Coverage (60%→72%) and Avg Level (3.0→3.5), proving the underlying level DID change - but Risk Gaps stayed at "1" and the "Competency Gap Alert" (still naming Programming Languages as "below recommended level") remained visible and did not clear. Only after one further, unrelated click on the same (already-at-max) level button did Risk Gaps drop to 0 and the alert disappear. This shows the risk-gap detection lags one interaction behind the actual competency state instead of clearing immediately when the gap is resolved.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.