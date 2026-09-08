# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight slider control is non-interactive / does not respond to any standard input method
    - Actual: On the Software Engineer model detail page, each competency node has a "Weight" Radix UI slider (role=slider, aria-valuemin=10, aria-valuemax=100). For the Programming Languages node (initial aria-valuenow=90): (1) Clicking the slider thumb successfully focuses it (verified document.activeElement === thumb), but subsequent ArrowLeft, ArrowRight, Home, and End key presses left aria-valuenow unchanged at 90 in every trial. (2) Attempting to drag the slider thumb via Playwright's drag tool (both thumb-to-thumb and thumb-to-injected-marker-element techniques) did not change aria-valuenow either, and in one case the tool silently dragged/attempted-to-drag the parent node instead of the slider. (3) Playwright's structured slider-fill helper (browser_fill_form, type=slider) errored with \"Element is not an <input>... and does not have a role allowing [aria-readonly]\", confirming it is not a standard accessible range input. No alternative control (numeric input, +/- buttons, etc.) exists for editing this weight. The edges connecting competencies also display a percentage badge (e.g. \"80%\" on the prog-lang→code-review edge) but these edges are only focusable/selectable (clicking/Enter toggles a CSS \"active\"/selected state) with no discoverable UI to edit their value. As a result, there is no working way to change a competency relationship weight, so the expected downstream updates (displayed weight, visual emphasis, dependent analysis) could not be produced or verified.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable competency switch does not toggle
    - Actual: Each competency node has an enabled/disabled toggle switch (role="switch", initial aria-checked="true", data-state="checked", not disabled). Clicking the Programming Languages node's switch twice in succession (which should disable then re-enable it) left aria-checked/data-state as "checked" after both clicks — the switch never flips to the unchecked/disabled state, and the node itself shows no visual disabled indicator (opacity remained 1, no disabled class added) after either click. A synthetic click dispatched directly on the switch button element also produced no change. Separately, the very first click on the node area did cause the summary metrics to shift once (Coverage 68%→65%, Avg Level 3.3→3.2, Critical Path 3→5 competencies) but a second identical click produced no further/reverting change, indicating this was not a working disable toggle but an unrelated one-off selection side effect — the switch control itself is non-functional and there is no way to reliably enable/disable a competency.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates summary/metadata but not the competency graph
    - Actual: Clicking "Apply" on the Product Manager template showed a "Template Applied" toast, changed the page title to "Product Manager", department to "Product", Coverage to 78%, Avg Level to 3.8, Critical Path to 4 (listing Product Strategy, User Research, Communication, +1), and correctly updated the Template Library card states (Product Manager now shows "Applied"/disabled and is marked "Current"; Software Engineer's Apply button became enabled again) and the Category Adjustments competency counts. However, the competency graph itself was NOT updated: the six graph nodes still show the original Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) instead of Product Manager competencies like "Product Strategy" or "User Research" referenced in the new Critical Path list. This is a data/UI inconsistency — the graph the user visually edits does not reflect the applied template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not retrievable anywhere in the UI
    - Actual: On the Software Engineer detail page, clicking "Save Current" in the Template Library panel produced a toast: "Template Saved — Current model configuration has been saved as a template." However, immediately afterward the Template Library list still showed only the same original 5 templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) — no new/6th template card was added, and no "My Templates" or equivalent section exists anywhere in the app (verified via full-text search of the page and by inspecting all template card headings, which remained exactly the original 5). The homepage "Templates" stat also remained "5" instead of incrementing to 6. There is no discoverable way to view or apply the just-saved custom template.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Edited model state not persisted despite success confirmation
    - Actual: Dragged the Agile Methodology node from its default position (translate(600px,0px)) to overlap the Collaboration node (translate(253.423px, 201.15px)), confirmed via DOM inspection. Clicked "Save" button which produced a "Model Saved" toast ("Your changes have been saved successfully."). Navigated back to Models list and reopened the Software Engineer model. DOM inspection of .react-flow__node transforms showed the Agile node had reverted to its original default position translate(600px, 0px), not the saved/dragged position. All other node positions were also at defaults. The saved state was not actually restored.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Level and Weight bound enforcement cannot be verified because the editing controls themselves are non-functional
    - Actual: Attempted to change a competency's Level via the 5 Level buttons (1-5) on the Programming Languages node (starting at Level "Advanced"). Clicked Level button 1 (leftmost) and Level button 2 — both showed [active] pseudo-state on click but the node's Level label remained "Advanced" (verified via card.innerText both before and after each click). The Weight slider (role=slider, aria-valuemin=10, aria-valuemax=100) was already confirmed non-responsive in FT-4 (no change via keyboard, drag, or fill_form). Since neither the Level control nor the Weight control actually changes the underlying value, it is impossible to test whether the app enforces the 1-5 level bound or the 10%-100% weight bound (e.g., clamping at boundaries) — the constraint cannot be exercised at all.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage recompute is not reliably tied to actual competency edits; fires incorrectly on node selection instead
    - Actual: Clicked the enable/disable switch on the Programming Languages node. The switch's own aria-checked/visual state did NOT change (stayed checked=true, confirming the switch itself is non-functional per FT-5). However, Coverage instantly changed from 68% to 65% and Critical Path count changed from 3 to 5 (Problem Solving, System Design, Programming Languages +2) as a side effect of the click — even though no actual competency was disabled and no genuine edit took effect. This shows the "real-time" recompute is triggered by node selection/click, not by validated model edits, producing numerically arbitrary/incorrect metric changes disconnected from any real state change. Separately, adjusting the Scenario Simulation category sliders (Level Change) did NOT update Coverage/Avg Level in real time (unchanged at 68%/3.3) until Run Simulation was clicked, and this same interaction corrupted the paired Weight Change field to display "NaN%".

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [X] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path/coverage recalculation produces corrupted (NaN) output after running a simulation with a Level Change adjustment
    - Actual: Set the "technical" category Level Change slider to -2 in the Recruitment simulation tab (via keyboard ArrowLeft), which also corrupted the paired Weight Change field to display "NaN%" (a pre-existing bug also noted in IX-13). Clicked "Run Simulation". After running, the Critical Path count/members stayed the same (3: Programming Languages, System Design, Problem Solving) and the "Critical" badge highlighting on graph nodes remained consistent with the summary panel list. However, the Coverage metric in the summary panel changed to "NaN%" instead of a valid percentage — confirming that the recalculation triggered by Run Simulation propagates the corrupted Weight Change value into the model's derived metrics, producing an invalid, user-visible NaN result rather than a correct recalculation. This is a genuine functional defect in the recalculation pipeline that a user would observe directly in the Coverage card.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Risk Gaps warning never triggers even under conditions that should clearly represent competency gaps
    - Actual: Checked Risk Gaps ("0 identified") across multiple states: baseline load of Software Engineer, Product Manager, and HR Manager models (all showed "0"); after template apply; after switch-click side effects that dropped Coverage/Critical Path values; and after running a Recruitment simulation on HR Manager with an extreme Level Change of -2 applied to the "domain" category (which contains 2 of the 3 Critical competencies: Talent Acquisition and HR Compliance). After running this simulation, Avg Level dropped from 3.5 (Advanced) to 2.5 (Intermediate) and Coverage became invalid ("NaN%"), yet Risk Gaps still displayed "0 identified" — no warning was created despite a severe, critical-competency-affecting drop in levels. This indicates the risk-gap detection/warning mechanism does not function; it never produces a non-zero count or visible warning under any tested condition.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: No prominent content-driven alert (e.g., for risk gaps or critical issues) could be observed; only generic action-confirmation toasts exist
    - Actual: The only "prominent alert" style UI observed in the app is a bottom/top toast notification (region "Notifications (F8)") that appears after actions like Template Applied, Template Saved, Simulation Applied, and Model Saved — these are generic action-confirmation toasts, not content-driven alerts reflecting model risk state. Since the Risk Gaps metric never becomes non-zero under any tested condition (see CT-17, including an extreme -2 Level Change on a category containing 2 Critical competencies, which also corrupted Coverage to "NaN%"), no risk-gap/critical-alert banner or prominent warning UI element was ever triggered or observed, despite the model summary reaching an invalid/degraded state (NaN% Coverage, Avg Level dropped from Advanced to Intermediate). There is no visible alert mechanism surfacing this invalid NaN% state or any risk condition to the user prominently — the summary card silently displays "NaN%" with no accompanying warning.