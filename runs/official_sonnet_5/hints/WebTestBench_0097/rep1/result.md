# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship weight is not editable via the graph UI
    - Actual: Clicking (single or double) an edge/relationship label (e.g. "Edge from agile to collab", 50%) only toggles a visual "active"/selected state on the edge (confirmed via aria-current/active attribute and re-snapshot). No dialog, popover, panel, or inline input appears anywhere in the DOM (checked via querySelectorAll('[role=\"dialog\"],[role=\"alertdialog\"],.modal,[data-state=\"open\"]') = empty array both before and after single/double click) that would allow changing the edge weight percentage. No other UI element on the page exposes per-edge weight editing.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable toggle inside competency node is non-functional
    - Actual: Clicking the "enabled" switch inside a competency node (e.g. Agile Methodology, role=switch aria-checked=true) does not change aria-checked/data-state. Root cause identified via DOM inspection: every interactive control inside .react-flow__node (switch, level buttons, weight slider) lacks the required React Flow "nodrag" class (checked buttonSample/sliderInfo/switchInfo via browser_evaluate - hasNodrag:false for all 36 inner buttons, both sliders, both switches). Because the whole node div is a React Flow draggable node, React Flow's pointer-drag handling intercepts/swallows the pointerdown before it reaches the nested switch's click handler, so the click has no effect on the underlying state despite Playwright reporting a successful click.

- [ ] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.
  - Bug Report:
    - Issue: Competency level selector inside graph node is non-functional
    - Actual: Clicked "set-level-4" control on Code Review competency (data-semtag-id model.graph.item.code-review.set-level-4) intending to raise its level from Intermediate. Playwright reported a successful click with no error, but a re-check via semantic_observe(model.graph.item.code-review.level-value) immediately after showed the level unchanged at "Intermediate". Same root cause as FT-5: the level buttons live inside a React Flow draggable node and lack the "nodrag" class, so pointerdown is captured by the node's drag/click-suppression logic instead of reaching the button's onClick handler.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Weight slider inside graph node does not update value
    - Actual: Focused the Code Review weight slider thumb directly via JS focus() (bypassing any pointer/drag interception - confirmed document.activeElement === thumb, aria-valuenow="60"), then dispatched a genuine ArrowRight keypress via Playwright's real keyboard (page.keyboard.press). A working Radix slider should increment aria-valuenow by its step on ArrowRight even without pointer interaction. After the keypress, aria-valuenow remained "60" and the displayed Weight text remained "60%" - the slider does not respond to keyboard interaction either, indicating the value-change handler itself is not wired up (not merely a pointer/nodrag issue).

- [X] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.

- [ ] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.
  - Bug Report:
    - Issue: Applying a template desyncs model header/metrics from the competency graph
    - Actual: Clicked "Apply" on the Product Manager template card while viewing the Software Engineer model. Result: model.title changed to "Product Manager", model.department changed to "Product", metrics updated to Product Manager values (Coverage 78%, Avg Level 3.8, Critical Path 4), and the Template Library correctly re-marked Product Manager as "Current" (Apply now disabled) while Software Engineer lost its "Current" badge. However, the competency graph itself was NOT updated: it still rendered the original Software Engineer competencies verbatim (Programming Languages/System Design/Code Review/Problem Solving/Collaboration/Agile Methodology with unchanged descriptions, levels, and weights, plus the same 5 edges), instead of Product Manager's competencies. The page title, metrics, and graph are now inconsistent with each other.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: "Save Current" as template produces no effect
    - Actual: Clicked "Save Current" button in the Template Library panel intending to save the active model as a new reusable template. No naming dialog/prompt appeared (checked DOM for [role=dialog]/[role=alertdialog] = empty), no confirmation toast appeared (the Sonner notifications region <ol> remained empty immediately after click), and the Template Library list was unchanged (still exactly 5 templates: Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager - no 6th entry for the newly "saved" template).

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Export and Share actions produce no observable effect
    - Actual: Clicked "Export" button: no dialog/menu opened, no toast/notification fired, no new network request issued (checked browser_network_requests, no non-static request triggered), and no console log/error indicating a download was initiated. Clicked "Share" button: likewise no dialog, menu, toast, or any DOM change occurred. Neither control produces any user-visible feedback or accomplishes an export/share action.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [X] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [X] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.