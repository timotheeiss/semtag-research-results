# Test Result

## Functionality
- [ ] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot perform test: Playwright browser automation tools (browser_click, browser_type, etc.) are not available in this environment. Unable to interact with the search input, department filter, or verify search results.

- [ ] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot perform test: Unable to click on job models in the grid (ids like jobs.grid.item.software-engineer) to navigate to detail editor. Browser click functionality is not available.

- [ ] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test drag-and-drop functionality for graph nodes. No drag/drop or pointer event APIs available in browser automation.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test weight change interactions. No ability to interact with weight controls or sliders.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test toggle competency enable/disable functionality. No ability to interact with toggle controls.

- [ ] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot navigate to template library. No ability to click navigation links or access secondary pages.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test template application. No ability to interact with template selection and apply controls.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test custom template saving and retrieval. No ability to interact with save dialog or form inputs.

- [ ] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test scenario switching (Recruitment, Training, Promotion). No ability to click scenario tabs or buttons.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test scenario simulation with adjustments. No ability to interact with adjustment controls or run simulation button.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test model state persistence. No ability to save model, navigate away, and return to verify restoration of state.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test constraint enforcement (competency levels 1-5, weights 10%-100%). No ability to interact with level and weight controls to verify constraints.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test immediate UI updates after model changes. No ability to make changes and observe coverage percentage updates without page reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test scenario initialization. No ability to navigate to detail view and verify that initially selected scenario has default adjustments applied.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test graph control value updates. No ability to make edits and verify graph controls display current values and states.


## Content
- [ ] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test interactive graph representation. No ability to navigate to detail view and verify graph structure, nodes, edges, and pan/zoom functionality.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test critical path recalculation. No ability to make weight or enabled-state changes and verify critical path updates and highlighting.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test risk-gap warning creation and clearing. No ability to lower high-weight competencies below recommended levels and verify warning appearance and disappearance.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: Browser automation tools unavailable
    - Actual: Cannot test risk competency gap alert display. No ability to interact with the app to trigger risk gap conditions and verify prominent alert display with affected competency names.