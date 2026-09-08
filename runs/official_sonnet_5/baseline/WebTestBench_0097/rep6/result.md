# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI control to edit relationship (edge) weight
    - Actual: Clicking an edge (e.g. "Edge from prog-lang to sys-design" 70%) only toggles a 'selected' CSS class and edge color/width slightly, with no dialog, input, or slider appearing to change its weight. Double-click, keyboard arrow keys after selection, and simulated drag on the edge label all left the weight unchanged at 70%. Each competency node has its own "Weight" slider, but that adjusts the node's own importance weight, not the weight of a specific directed relationship/edge between two competencies. No accessible mechanism exists to change an edge's relationship weight.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable switch on competency nodes is unresponsive
    - Actual: Clicking the enabled/disabled switch on a competency node (tested on "Agile Methodology" and "Code Review" nodes) via real mouse click, CSS-selector click, and low-level pointer events consistently left aria-checked="true" (still enabled); instead the click only selected the parent graph node (class "selected" added to the node). The switch never toggled to the disabled/unchecked state through any interaction method tried, so competencies cannot be disabled, and consequently no disabled visual state, coverage/gap/critical-path exclusion, or re-enable behavior could be produced or verified.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates metrics/header but not the competency graph
    - Actual: Clicking "Apply" on the Product Manager template showed a "Template Applied" toast, updated the header title to "Product Manager", department to "Product", Coverage to 78%, Avg Level to 3.8, Critical Path to "Product Strategy, User Research, Communication, +1", and the Scenario Simulation category competency counts changed. However, the competency graph itself still displayed the old Software Engineer nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the same 5 edges (prog-lang→code-review, prog-lang→sys-design, etc.) — none of which match the new Critical Path names shown in the metrics. The graph, relationships, and node panel are inconsistent with the applied template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not retrievable/visible anywhere after saving
    - Actual: On the HR Manager model detail page, clicking "Save Current" in the Template Library panel produced a "Template Saved - Current model configuration has been saved as a template." toast notification, implying success. However, after navigating away to the Models list (home page still showed "Templates: 5", unchanged from before saving) and back into the HR Manager model, the Template Library panel still shows only the same original 5 templates (Software Engineer, Product Manager, HR Manager [Current], Data Scientist, Sales Manager) - no new custom/"My Templates" entry appears anywhere, and no separate "My Templates" section exists in the UI. The save action's confirmation toast is not backed by any persisted or retrievable template.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Running scenario simulation corrupts Coverage metric to NaN%
    - Actual: On HR Manager model's Recruitment tab, setting the "domain" category Level Change slider to +2 (via click+ArrowRight) also caused the sibling Weight Change display to incorrectly show "NaN%" instead of "0%" (unrelated slider value corrupted just by touching Level Change). Clicking "Run Simulation" then applied the adjustment: Avg Level correctly changed from 3.5 to 4.2, but the Coverage metric became "NaN%" (previously 71%) instead of a valid computed percentage - a clear calculation bug caused by the NaN weight-change value propagating into the coverage formula. Risk Gaps and Critical Path count remained unchanged. Clicking "Reset" did correctly restore Coverage to 71% and Avg Level to 3.5, so Reset itself works, but Run Simulation produces a broken/invalid Coverage output.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Levels/weights/enabled states cannot be edited at all, and the Save action shows no verifiable persistence
    - Actual: Node-level competency controls (enable switch, level pips, weight slider) are entirely non-interactive in this build (confirmed root-cause bug from FT-4/FT-5/CS-12/IX-13/IX-20: they lack the "nodrag" class so React Flow intercepts all pointer/keyboard input), so it is impossible to actually change any competency's level, weight, or enabled state through the UI - there is nothing genuine to persist. Clicking the header "Save" button did show a "Model Saved - Your changes have been saved successfully." toast, but after a full page reload and re-opening the HR Manager model, all values are simply back to the original pristine defaults (Coverage 71%, Avg Level 3.5, Talent Acquisition Weight 90%/Level Advanced, etc.) - identical to the state before any interaction. Since no edit could ever be made, there is no way to verify the save/restore round-trip actually preserves user changes; the "Save" confirmation is unverifiable and the feature this checklist item targets is unreachable due to the underlying broken graph controls.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Level (1-5) and weight (10-100%) constraints cannot be exercised/verified because the underlying node controls are non-interactive
    - Actual: The Talent Acquisition node's weight slider correctly declares aria-valuemin="10" and aria-valuemax="100" in markup, and each node exposes exactly 5 level-pip buttons (consistent with a 1-5 level scale), so the constraint bounds are declared correctly. However, attempting to actually exercise the constraint (e.g., increment the weight slider via keyboard after programmatically focusing it, or click a level pip) produces no value change at all: aria-valuenow remained "90" after an ArrowRight keypress with the slider focused, and clicking level pips does not change the displayed level (per FT-4/FT-5 root-cause bug: node controls lack the "nodrag" class so React Flow intercepts all pointer/keyboard interaction meant for them). Because the controls cannot be moved at all, it is impossible to verify that the system actually clamps/enforces values within 1-5 or 10-100% - the constraint is unverifiable/unusable in practice.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage does not update on graph node level/weight/enabled changes because those controls are non-functional
    - Actual: Clicking a level-pip button inside the "Talent Acquisition" graph node (to attempt changing its level) produced no change to the node's displayed level and no change to the Coverage metric (remained 71%) or Avg Level (remained 3.5). This is consistent with the previously confirmed root-cause bug (FT-4/FT-5): in-graph node controls (switch, level pips, weight slider) lack the "nodrag" CSS class needed to escape React Flow's pointer-event interception, so clicks are captured by node selection/drag handling instead of reaching the control. Since the underlying level/weight/enabled changes never actually take effect, Coverage cannot update immediately (or at all) in response to them.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Default simulation scenario produces no observable effect when run
    - Actual: On HR Manager model, Recruitment tab is selected by default with all Category Adjustments (technical/behavioral/leadership/domain) at Level Change 0 and Weight Change 0%. Clicking "Run Simulation" without switching tabs produced no change to any model output: Coverage stayed 71%, Avg Level stayed 3.5, Critical Path stayed 3 competencies, Risk Gaps stayed 0. No toast/notification appeared in the Notifications region confirming the simulation executed. Since the default adjustments are all zero and produce zero observable output change or confirmation feedback, there is no evidence the initial scenario is meaningfully initialized/applied - the same behavior would be observed whether Run Simulation was clicked or not.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Cannot verify "subsequent edit starts from current state" because graph node controls are non-interactive
    - Actual: Two of the three sub-behaviors check out: (1) graph controls correctly display current value/state - e.g. Talent Acquisition node shows "Weight 90%" text matching the underlying slider's aria-valuenow="90", "Level Advanced" text matches its level-pip fill state, and the switch's [checked] state matches the visually enabled node; (2) Gap/Critical indicators agree with the summary - the header "Critical Path" card lists Talent Acquisition, Employee Relations, HR Compliance, which exactly matches the three nodes carrying a "Critical" badge in the graph, and "Risk Gaps: 0 identified" agrees with no risk indicators shown on any node. However, the third requirement - that a subsequent edit starts from the currently displayed state - cannot be verified because the node's editing controls (weight slider, level pips, enable switch) are non-interactive (confirmed root-cause bug from FT-4/FT-5/CS-12: controls lack "nodrag" class so React Flow intercepts all pointer/keyboard input intended for them). Since no edit can be initiated at all, this requirement is unmet/unverifiable in the current build.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical Path summary recalculation disagrees with graph node "Critical" badge highlighting
    - Actual: After applying a -30% Weight Change to the "domain" category and running the simulation, the header "Critical Path" card changed from {Talent Acquisition, Employee Relations, HR Compliance} (3) to {Employee Relations, HR Compliance, Leadership, +1} (4) - i.e. Talent Acquisition was dropped and Leadership plus one more competency were added. However, in the graph canvas, the Talent Acquisition node still displays a "Critical" badge and unchanged Weight 90% - the graph was not updated at all by the simulation run. This means the Critical Path summary card and the in-graph Critical highlighting now directly contradict each other (summary says Talent Acquisition is no longer critical; graph still marks it Critical), and the newly-critical nodes (e.g., Leadership) show no Critical badge in the graph either.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: No risk-gap warning generated when a high-weight critical competency's weight is drastically lowered
    - Actual: Applying a -30% Weight Change to the "domain" category (which includes the Critical, 90%-weighted Talent Acquisition competency) and running the simulation caused Talent Acquisition to drop out of the Critical Path list entirely (per CT-16 finding), yet the "Risk Gaps" metric remained "0 identified" both before and after - no warning naming Talent Acquisition (or any competency) appeared. Across all simulation runs performed in this session (including one that corrupted Coverage/Avg Level to NaN), Risk Gaps never changed from 0, indicating the risk-gap detection/warning feature does not trigger even under conditions that plausibly should produce a gap (a previously critical, high-weight competency losing enough weight to fall out of the critical path).

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: No risk-gap alert UI observed; feature appears never to trigger, so its existence/prominence cannot be confirmed
    - Actual: No text-and-icon alert naming affected competencies was ever observed near the summary metrics in any test performed on the HR Manager or Software Engineer models, including after simulation runs that reduced a critical competency's weight by 30% and dropped it from the critical path (see CT-17), and after a run that corrupted Coverage/Avg Level to NaN/N/A. The "Risk Gaps" metric card stayed at "0 identified" throughout, with no accompanying alert element appearing in the DOM. Since Risk Gaps could not be driven above 0 through the available interaction paths (in-graph node controls are broken per FT-4/FT-5/CS-12; scenario simulation weight/level changes did not increment it either), it is not possible to confirm that a prominent alert naming affected competencies exists at all when risk gaps are detected.