# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship/competency weight control non-functional
    - Actual: No UI exists to edit an edge's own weight (clicking/double-clicking/right-clicking an edge only selects it, no editor appears). The node's "Weight" slider (which drives edges from that competency) does not respond to real click on the track, real pointer drag (dragTo), or keyboard ArrowLeft/ArrowRight after genuine focus - aria-valuenow stayed at 80/90 in every attempt. No dependent value or analysis updates because the control cannot be changed at all.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable switch non-functional
    - Actual: Clicking the enable/disable switch on competency nodes (tested on "System Design" and "Agile Methodology", with and without first selecting the node) never changes aria-checked/data-state from "true"/"checked". The competency cannot be disabled via direct interaction, so its exclusion from coverage/gap/critical-path calculations cannot be verified.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template produces an inconsistent model - graph not synced with new template data
    - Actual: Clicking \"Apply\" on the Product Manager template correctly updated the page heading to \"Product Manager\", department to \"Product\", Coverage to 78%, Avg Level 3.8, Critical Path list to Product Strategy/User Research/Communication/+1, and the Category Adjustments competency counts (technical 1, behavioral 1, leadership 1, domain 3) - all consistent with a Product Manager model. However the competency graph itself (React Flow nodes/edges) was NOT updated and still rendered the old Software Engineer competencies verbatim (\"Programming Languages\", \"System Design\", \"Code Review\", \"Problem Solving\", \"Collaboration\", \"Agile Methodology\" with the same edges/weights), confirmed via DOM query of .react-flow__node h4 elements. The visual model is inconsistent with the applied template's metadata/metrics.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved template not added/retrievable in Template Library
    - Actual: Clicking \"Save Current\" on the Software Engineer model produced a success toast (\"Template Saved - Current model configuration has been saved as a template\"), but the Template Library list was not updated with any new entry - it still shows exactly the same 5 pre-built templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) as before saving. There is no \"My Templates\" section or any other UI location where the newly saved template can be found/retrieved/applied.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Simulation produces corrupted (NaN) aggregate metric
    - Actual: On the Software Engineer model (Recruitment tab), setting the \"technical\" category Level Change to +2 via keyboard (ArrowRight on the slider) caused the sibling \"Weight Change\" slider/value for the same category to display \"NaN%\" instead of a numeric value. Clicking \"Run Simulation\" applied the adjustment (Avg Level correctly rose 3.3→4.2 and Critical Path list reordered), but the top-level Coverage metric became \"NaN%\" (was 68%) - a broken aggregate calculation caused by the NaN weight-change value propagating through. \"Reset\" did correctly restore Coverage to 68% and Avg Level to 3.3, and cleared Level/Weight Change back to 0/0%, so Reset itself works, but Run Simulation is not reliably functional.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Save button does not persist edited model state across reload
    - Actual: On the Software Engineer model, the Recruitment tab's \"technical\" category Level Change was set to \"+1\" via keyboard interaction. Clicking the top-toolbar \"Save\" button produced a success toast (\"Model Saved - Your changes have been saved successfully.\"). After reloading the page (http://localhost:6097/) and reopening the Software Engineer model, the Level Change value had reverted to \"0\" (and Weight Change to \"0%\") instead of persisting the \"+1\" that was present before reload/save. The edited state was not restored, meaning Save does not actually persist changes.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage/progress update not correctly tied to real edits; progress bar not determinate
    - Actual: Clicking a competency's enable/disable switch does not toggle its visual checked state (stays aria-checked="true"/data-state="checked") yet the Coverage % increments by ~1 on almost every click regardless of which node is clicked, including repeat clicks on the same already-"enabled" switch (68%→69%→70% across 3 unrelated clicks) - i.e. the change is not a correct reflection of enabling/disabling a specific competency. Additionally the progress indicator element has role=progressbar but data-state="indeterminate" with no aria-valuenow, so it never reflects the coverage percentage numerically.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [X] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical Path recalculation is not grounded in actual weight/enabled changes
    - Actual: Running the Promotion scenario's preset simulation (Weight Change -5% for \"technical\", +10% for \"behavioral\") changed the top \"Critical Path\" metric from 3 competencies (Programming Languages, Problem Solving, System Design) to 4 (Problem Solving, Programming Languages, Collaboration, +1), dropping System Design and adding Collaboration. However, a DOM query of the actual graph nodes confirmed every competency's displayed Weight percentage was completely unchanged (Programming Languages 90%, System Design 80%, Code Review 60%, Problem Solving 90%, Collaboration 70%, Agile 50% - identical to pre-simulation values). Since no underlying competency weight was ever actually modified (per FT-4 finding that weight controls are non-functional, and this simulation run also failed to persist any weight change to the nodes), the Critical Path list change has no valid basis and cannot be considered a correct recalculation reflecting a real weight/enabled edit.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: No risk-gap warning triggered when lowering weight of high-weight/critical competencies
    - Actual: Running the Promotion scenario's preset simulation, which explicitly decreases Weight Change by -5% for the \"technical\" category (containing the two highest-weight \"Critical\"-flagged competencies, Programming Languages 90% and System Design 80%), produced no risk-gap warning: the \"Risk Gaps\" metric remained \"0 identified\" both before and after running the simulation. This was also confirmed true for every other job model/template tested (Product Manager, HR Manager) which all showed \"0 identified\" by default. No mechanism was found anywhere in the app that raises a risk-gap warning in response to lowering a critical competency's weight.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: Risk-gap alert with affected competency names and corrective suggestion never appears
    - Actual: No risk-gap alert was ever observed anywhere in the application across all tested job models (Software Engineer, Product Manager, HR Manager) and after multiple simulation runs including one specifically designed to reduce a critical competency's weight. The \"Risk Gaps\" summary card consistently shows \"0 identified\" with no expandable detail, list of affected competency names, or corrective action suggestions anywhere in the DOM. Since the alert never triggers (see CT-17), its required content (affected competency names + corrective action suggestion) cannot exist or be verified.