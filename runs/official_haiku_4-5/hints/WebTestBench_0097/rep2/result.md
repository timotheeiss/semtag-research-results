# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight slider interaction not responding to any input method (mouse, pointer events, keyboard, drag operations). Weight values display correctly (90%, 80%, 60%, etc.) but cannot be changed via UI.
    - Actual: Weight slider element found but does not respond to interaction attempts. However, level changes DO update metrics immediately (e.g., changing Programming Languages from Advanced to Level 1 changed coverage from 67% to 56%, avg level from 3.3 to 2.8, and created gap alert).

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Competency toggle switch (enabled/disabled) does not respond to any interaction method: Playwright browser_click, DOM element.click(), synthetic mouse events, or change events. All competencies remain in enabled state (aria-checked="true", data-state="checked") after multiple toggle attempts.
    - Actual: Toggle elements exist (6 found, one per competency) with proper ARIA attributes (role="switch", aria-checked="true"), but clicking or dispatching events does not change the aria-checked or data-state attributes. State remains frozen at checked=true for all competencies.

- [ ] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.
  - Bug Report:
    - Issue: Template library display does not show comparison metadata. Only job titles and Apply/Current buttons are visible; department, competency count, relationship count, and keywords are not displayed for comparison.
    - Actual: Template list shows 5 templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with only titles and action buttons. To see template details like department and competency count, must apply the template which replaces the current model. No side-by-side or browse-only comparison view exists.

- [X] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save Current template button does not respond. Clicking "Save Current" action button does not open a dialog, input field, or any UI for entering template name and description.
    - Actual: Button exists with action="save-as-template" but clicking it produces no visible response. No dialog, modal, or input fields appear. Template list remains unchanged with only the 5 predefined templates visible.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Model edits are not persisted after leaving and reopening. Changed Programming Languages level to Level 2 (coverage dropped from 68% to 60%), clicked Save, left model, returned to home, and reopened Software Engineer. Programming Languages level reverted to Advanced and coverage returned to 68%, indicating the edit was not saved.
    - Actual: Edit was temporarily reflected in metrics (coverage 60%) but was not persisted. After leaving detail view and reopening Software Engineer, Programming Languages level was back to "Advanced" and coverage was back to 68%. The save action did not persist the changes.


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