# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Competency relationship/weight slider inside graph nodes is non-functional
    - Actual: Dragging or keyboard-adjusting a node's Weight slider (e.g. Agile Methodology, weight 50%) via real mouse drag, click, and native element.click()/keyboard ArrowRight/End all leave aria-valuenow and the displayed weight-value unchanged (stayed at 50%). Equivalent slider controls elsewhere in the app (Scenario Simulation category Level Change slider) do respond correctly to the same interaction technique, confirming the graph node's weight control itself is broken rather than a testing artifact.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Competency enable/disable toggle inside graph nodes is non-functional
    - Actual: Clicking the enabled-switch on multiple competency nodes (Agile Methodology, Collaboration) via Playwright click and native element.click() does not change aria-checked (stays "true"); competency-count, coverage, and other metrics remain unchanged, confirming the disable/re-enable action never takes effect.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates title/metrics/scenario categories but not the competency graph
    - Actual: After clicking Apply on the Product Manager template, the title ("Product Manager"), department ("Product"), coverage (78%), avg level (3.8), critical-path names (Product Strategy, User Research, Communication, +1) and scenario category counts (technical 1, behavioral 1, leadership 1, domain 3) all updated correctly, but the competency graph still displays the previous Software Engineer nodes/edges (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) instead of the Product Manager competencies referenced by the critical path summary — the graph and relationships are inconsistent with the rest of the applied template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save-as-template does not collect name/description and does not persist a retrievable template
    - Actual: Clicking "Save Current" immediately shows a "Template Saved" toast with no dialog/form to enter a template name or description. No new entry appears in the Template Library (still exactly 5 built-in templates, "5 Templates" stat unchanged), no localStorage/sessionStorage data was written, and there is no "My Templates" collection anywhere in the app to retrieve a saved custom template from.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Save does not persist model state across navigation or reload
    - Actual: Ran Training scenario simulation (changing Coverage 68%→81%, Avg Level 3.3→4.0), then clicked Save — a \"Model Saved\" toast appeared (\"Your changes have been saved successfully.\"). However, localStorage and sessionStorage remained empty (no persistence mechanism). After navigating back to the Models list and reopening the Software Engineer model (no page reload even required), Coverage reverted to 68% and Avg Level to 3.3, i.e. the saved simulation-adjusted state was completely lost. Save has no real effect beyond showing a success toast.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: No functional UI path exists to test level/weight range enforcement on individual competencies
    - Actual: Direct per-node level buttons, weight slider, and enable switch are non-functional (confirmed in FT-4/FT-5 via multiple independent techniques including native DOM clicks). As an alternate path, pushed a scenario simulation's Technical category adjustments to their maximum allowed deltas (level +2, weight +30%) and ran the simulation: aggregate metrics changed (Coverage 68%→87%, Avg Level 3.3→4.2), but the individual competency nodes in the graph (Programming Languages, System Design, Code Review) still displayed their original unchanged level/weight values (Advanced/90%, Intermediate/80%, Intermediate/60% respectively) — i.e. the simulation never actually writes adjusted values onto the competencies, so there is no way, through any working control, to drive an individual competency's level above 5 or below 1, or weight above 100% or below 10%, to verify clamping/validation. The constraint is therefore untestable and the underlying editing mechanism is broken.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage/progress indicator cannot be verified to update from direct competency edits because those edit controls are non-functional
    - Actual: Attempted to change an enabled competency's level (via level-set buttons and slider) and its enabled state (via the toggle switch) directly on graph nodes; as established in FT-4/FT-5, none of these controls register any change (values and switch state remain frozen regardless of click, native click, or keyboard interaction). Consequently, Coverage % and the progress bar never update in response to a direct level/weight/enabled-state edit, since no such edit can actually be made through the UI. The only mechanism that does move Coverage is running a scenario simulation (aggregate category deltas), which is a different feature (tested separately in FT-10/IX-19), not a direct per-competency edit as this item specifies.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Running the initially-selected scenario without switching tabs applies zero-value adjustments and produces no visible model output change
    - Actual: On a fresh reload, Recruitment is the default active scenario tab with all Category Adjustments shown at 0 level / 0% weight for every category (technical, behavioral, leadership, domain). Clicking Run Simulation without switching tabs shows a \"Simulation Applied\" toast, but Coverage (68%), Avg Level (3.3), Critical Path (3: Programming Languages, Problem Solving, System Design), and Risk Gaps (0) are all identical to the pre-run baseline — i.e. no model outputs actually changed, because the default scenario's adjustments are all zero. This fails the requirement that running the initial scenario \"changes the corresponding model outputs.\"

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Cannot verify graph controls/indicators stay consistent after edits because per-node edit controls are non-functional
    - Actual: In the untouched baseline state, graph node \"Critical\" badges (Programming Languages, System Design, Problem Solving) do agree with the Critical Path summary list, and node Level/Weight labels match displayed values. However, since the node level buttons, weight slider, and enable switch do not respond to any interaction (per FT-4/FT-5), it is impossible to make an edit and then verify (a) the control reflects the new current value, (b) Gap/Critical indicators recalculate to match, or (c) a subsequent edit starts from the just-edited value rather than the stale original. The scenario-simulation path also does not update per-node values (per CS-12 finding), so there is no working mechanism anywhere in the app to test this requirement.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical Path summary recalculates after a scenario simulation but graph node \"Critical\" badge highlighting does not, producing inconsistent results
    - Actual: After running a simulation that lowered Technical category Level (-2) and Weight (-30%), the Critical Path summary changed from \"Programming Languages, Problem Solving, System Design\" (3) to \"Problem Solving, Collaboration, Agile Methodology, +1\" (4) — now including Collaboration and Agile Methodology. However, the graph nodes' \"Critical\" badges remained exactly as before: only Programming Languages, System Design, and Problem Solving are marked Critical in the graph; Collaboration and Agile Methodology (now listed in the Critical Path summary) still show no Critical badge at all. The graph highlighting and the critical-path summary are therefore inconsistent with each other after the recalculation (consistent with the CS-12 finding that per-node graph data never actually updates from simulations).

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.