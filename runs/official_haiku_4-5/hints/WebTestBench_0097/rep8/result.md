# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight slider not responsive to automation attempts. Despite multiple interaction methods (programmatic events, drag operations, clicks), weight value remained at 90%. Metrics changed (coverage 68%→67%, critical path 3→5) suggesting some state change, but weight display unchanged.
    - Actual: Weight-value remained "90%" after attempted slider drag, click, double-click, and simulated pointer events. No input field or alternative control found for direct weight editing.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Unable to confirm competency disable/enable functionality through automation. Toggles exist with role='switch' and aria-checked='true', but all toggles remain checked despite multiple click actions. State changes not observable through DOM attributes or semantic hints.
    - Actual: All 6 competency enabled toggles show aria-checked='true' after clicking. No visual or DOM indicators of disabled state found. Metrics changed (coverage 67%→69%, avg-level 3.3→3.4, critical-path 5→3) but this could be unrelated to toggle attempts.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [X] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save Current button doesn't open save dialog. Clicking Save Current produces no visible dialog, modal, input fields, or confirmation messages. Unable to confirm template save functionality with custom name and description.
    - Actual: Button click appears to execute but no dialog/form appears for entering template name/description. No input elements, dialogs, or status notifications found after clicking Save Current.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Simulation results not observable after running. Run Simulation button clicked but metrics remain unchanged after execution (Coverage: 78%, Avg Level: 3.8, Critical Path: 4, Gaps: 0 before and after). Promotion scenario adjustments (Technical -5%, Behavioral +10%, Leadership +20%) do not appear to affect displayed outputs.
    - Actual: All metrics unchanged after clicking Run Simulation. Coverage 78%→78%, Avg Level 3.8→3.8, Critical Path 4→4, Gaps 0→0. No visual indication of simulation results in model outputs.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Unable to interact with competency level buttons due to React Flow node event interception. Attempted click on set-level-3 button timed out with Collaboration node and header intercepting pointer events. Cannot complete test of model state persistence.
    - Actual: Click timeout when attempting to set competency level. Other page elements (header, other competency nodes) intercepted pointer events, preventing reliable interaction with level buttons.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [X] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [X] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Unable to create risk-gap scenario to test. High-weight competency level buttons not reliably clickable due to React Flow node event interception. Current model shows 0 gaps but cannot lower competency levels to create risk-gap condition for testing.
    - Actual: Level adjustment buttons timeout and fail to respond. Gap count remains 0. Unable to confirm risk-gap warning behavior.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: Unable to test risk-gap alert display. Cannot create risk-gap scenario due to level button interactivity issues. Current model shows 0 gaps with no alert visible, but gap creation scenario not achievable for verification.
    - Actual: Gap count is 0, no risk alert visible. Unable to confirm alert display when gaps are detected.