# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight slider control non-functional
    - Actual: On the "Programming Languages" competency node, the weight slider (Radix Slider, aria-valuenow=90) did not change value after: (1) browser_click on thumb, (2) native element.click(), (3) keyboard ArrowLeft/ArrowRight/Home while focused, (4) synthetic PointerEvent dispatch, and (5) a genuine successful Playwright mouse drag (verified via elementFromPoint that the correct thumb element was grabbed, and the drag completed without timeout/error) to a marker positioned at 20% of the track. aria-valuenow remained "90" and the displayed "Weight 90%" text never changed in all cases. A structurally similar Radix slider elsewhere on the page (Scenario Simulation Category Adjustment) DID respond correctly to the identical drag technique, confirming this is not an automation limitation but a genuine defect specific to the competency node's weight control.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable switch non-functional
    - Actual: Clicking the enable/disable switch on a competency node (role="switch", aria-checked="true") did not toggle its state; aria-checked and data-state remained "true"/"checked" after the click. The click did register on the node (parent node gained a "selected" CSS class), but the switch's own toggle handler did not fire despite elementFromPoint confirming the switch element was correctly under the pointer and not disabled/covered.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not consistently replace the model
    - Actual: Clicking "Apply" on the Product Manager template while viewing Software Engineer updated the page title to "Product Manager", summary metrics (Coverage 78%, Avg Level 3.8), Critical Path list (Product Strategy, User Research, Communication, +1), and the Template Library card's "Current"/"Applied" badges — but the competency graph itself did NOT update: it continued showing the previous Software Engineer nodes/edges (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology with unchanged weights), which are inconsistent with the new Critical Path competency names shown in the summary.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not persisted/retrievable
    - Actual: Clicking "Save Current" displayed a success toast ("Template Saved - Current model configuration has been saved as a template."), but the Template Library list still showed only the original 5 built-in templates afterward, and the homepage "Templates" stat remained "5" (unchanged) both immediately and after navigating away to Models and back — no new template entry was added or retrievable anywhere in the UI.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Run Simulation produces NaN outputs for category weight/level-boundary adjustments
    - Actual: Dragging the Technical category "Weight Change" slider to +15% and clicking "Run Simulation" produced Coverage="NaN%" and Avg Level="NaN"/"N/A" in the summary panel (Critical Path did update to a plausible value). Separately, setting Technical "Level Change" to its minimum (-2) and running also produced Coverage="NaN%" (Avg Level computed correctly as 2.3/Beginner in that case). A moderate Level Change (+1, Training tab default) computed correctly (Coverage 81%, Avg Level 4.0). Reset correctly restored all sliders and summary metrics to baseline (68%/3.3/3). This shows the simulation calculation is unreliable and produces invalid (NaN) results under several legitimate adjustment scenarios.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Save does not persist model changes across navigation
    - Actual: After applying the Product Manager template to the Software Engineer model and clicking the "Save" button (header toolbar, which showed a success/"Saved" confirmation), navigating away via the "Models" nav link and back into "Software Engineer" showed the original, unmodified Software Engineer data (title "Software Engineer", Coverage 68%) instead of the saved Product Manager changes — the save did not persist.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Cannot verify range enforcement; edit controls non-functional
    - Actual: The node weight slider has correct structural bounds (aria-valuemin="10", aria-valuemax="100") and the level control renders exactly 5 segments (levels 1-5), but since the slider, level-rating buttons, and switch on competency nodes do not respond to any interaction (see FT-4/FT-5), it is impossible to verify that edited values are actually clamped/constrained to 10%-100% weight and level 1-5 during real edits or simulations.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage does not reliably update immediately/correctly on changes
    - Actual: Direct node-level weight/level/enabled edits produce no state change at all (controls non-functional, see FT-4/FT-5), so Coverage cannot update from them. Via the working category-adjustment simulation path, running a simulation frequently causes Coverage to display "NaN%" instead of a correctly recalculated percentage (see FT-10), so even where inputs are changeable, the Coverage/progress bar update is unreliable and often invalid.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls do not reflect interaction/current state
    - Actual: Because the node's weight slider, level buttons, and enable switch do not respond to any tested interaction (see FT-4/FT-5), their displayed positions/states never change from the initial values, so it cannot be confirmed that these controls accurately reflect a "current" edited value, nor that subsequent edits would start from an updated state — no edit ever takes effect.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path recalculation is inconsistent/unreliable
    - Actual: Applying a different template (Product Manager) updated the Critical Path list to name competencies (Product Strategy, User Research, Communication) that don't exist in the still-displayed graph (see FT-7), showing the critical path calculation is out of sync with the actual model. Additionally, Critical Path numbers changed after Run Simulation calls that concurrently produced NaN Coverage/Avg Level values (see FT-10), making the recalculation's correctness unverifiable/untrustworthy.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Risk-gap warning does not trigger on lowered level
    - Actual: Setting the Technical category "Level Change" to its minimum (-2) and running the simulation dropped Avg Level to 2.3 ("Beginner") but the Risk Gaps counter remained "0" and no gap warning appeared anywhere in the UI, indicating the risk-gap warning does not reliably appear when weight/level is substantially lowered via the one working adjustment path available.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: Risk-gap alert not reliably produced via legitimate interactions
    - Actual: In the controlled test that legitimately lowered a category's level via simulation (Technical Level Change to -2, then Run Simulation), Avg Level dropped to "Beginner" tier but no prominent Competency Gap Alert appeared near the summary metrics, and Risk Gaps stayed at 0 (see CT-17), so the alert display could not be reliably triggered through working interactions.