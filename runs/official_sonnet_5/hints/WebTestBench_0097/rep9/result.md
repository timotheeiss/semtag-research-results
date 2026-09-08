# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Competency/relationship weight is not actually editable via the UI
    - Actual: The "Weight" slider on each competency node (role=slider, aria-valuenow=90 for Programming Languages) does not respond to any interaction: a fully trusted Playwright drag from the thumb to a target x-position (verified via captured pointerdown/pointermove/pointerup event log at correct coordinates) left aria-valuenow at "90" and the displayed "90%" label unchanged; keyboard ArrowLeft/ArrowRight after a real focus click also left the value at 90; clicking the weighted relationship edges (e.g. "Edge from prog-lang to code-review", labeled 80%) only toggles a "selected" visual state (stroke width change) and opens no editor, slider, or input for the edge weight. No mechanism to change any weight value was found, so the checklist item's premise (weight update reflected in display/emphasis/analysis) cannot be exercised.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable toggle does not visually reflect state and does not toggle back
    - Actual: Baseline for Software Engineer model: Coverage 68%, Avg Level 3.3, Critical Path 3. Clicking the Programming Languages "enabled" switch once dropped Coverage to 65%, Avg Level to 3.2, Critical Path to 5 (so the click clearly registered and altered calculations), but the switch's aria-checked stayed "true" and the node received no disabled styling (opacity 1, no "disabled" class) — i.e. the node never visibly shows a disabled state, violating "the node ... clearly show the disabled state". Clicking the same toggle a second and third time produced no further change and did not restore the original enabled metrics (stuck at Coverage 65%/Avg 3.2/Critical Path 5), violating "repeated toggles act on the current state" (re-enabling did not work).

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates only title/department/metrics text, not the actual competency graph
    - Actual: Clicked Apply on the "Product Manager" template while viewing the Software Engineer model. model.title changed to "Product Manager", model.department to "Product", metrics.coverage to 78%, metrics.avg-level to 3.8, metrics.critical-path.count to 4, and templates.list correctly marked product-manager as "Current" with Apply disabled. However, the competency graph nodes were NOT replaced: model.graph / .react-flow__node still list the original Software Engineer competencies unchanged (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) instead of Product Manager-specific competencies (e.g. Product Strategy, Roadmap, etc. implied by the template's own keyword tags). The header/metrics and the underlying competency data are inconsistent with each other after applying a template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" as template shows a success toast but does not actually save/add a retrievable template
    - Actual: Clicked "Save Current" (templates.save-current) on the Software Engineer model. A toast appeared: "Template Saved - Current model configuration has been saved as a template." However, templates.list still contains exactly the original 5 items (software-engineer, product-manager, hr-manager, data-scientist, sales-manager) both before and after the click, verified via direct DOM query (list.children.length === 5) and repeated after clicking Save Current twice. No new template card appears in the Template Library, so there is nothing to retrieve. Additionally localStorage is empty (Object.keys(localStorage) === []), confirming no persistence mechanism exists at all, so the saved template cannot survive leaving/returning to the page either.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Category adjustment slider's displayed value desyncs/lags behind its actual underlying value during interaction
    - Actual: On the Recruitment scenario (baseline Technical Level Change displayed "0"), focused the Technical category's Level Change slider thumb and pressed ArrowRight repeatedly. The slider's own aria-valuenow correctly advanced 1→2→3 (confirming the control itself is interactive here, unlike the node Weight slider), but the adjacent displayed "Level Change" text label lagged one step behind: after the first ArrowRight (valuenow 1→2, which per the 0-4/-2 offset scale represents a change from -1 to 0) the label still showed "0" (stale/unchanged), and only after the second ArrowRight (valuenow 2→3, i.e. actual delta now +1) did the label update to show "+1" — one interaction behind the real value. Clicking Run Simulation while the label showed "+1" (already reflecting the true state at that point) did correctly recalculate metrics (Coverage 68%→76%, Avg Level 3.3→3.7), confirming the underlying calculation uses the real slider value, but the visible UI feedback shown to the user during adjustment does not reliably reflect the value about to be applied, which is misleading and fails the requirement that category-level adjustments update reliably/consistently. Reset afterward correctly restored Coverage to 68% and Avg Level to 3.3.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: "Save" shows a success toast but does not actually persist model state; edits are lost on navigating away and back
    - Actual: On Software Engineer model, set Code Review level to 5, which correctly updated metrics (Coverage 68%→74%, Avg Level 3.3→3.7). Clicked Save (model.save); a toast appeared: "Model Saved - Your changes have been saved successfully." Checked localStorage and sessionStorage: both empty (no persistence mechanism at all). Navigated back to the model list (model.back) and re-opened the same Software Engineer model without reloading the browser (in-session SPA navigation) — metrics reverted to the original baseline (Coverage 68%, Avg Level 3.3), i.e. the "saved" Code Review level-5 edit was lost. Save does not actually persist state even within the same browser session, let alone across a full page reload.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight bound (10-100%) cannot be enforced because the weight slider control does not accept any input
    - Actual: Level bound is satisfied by design: model.graph.item.prog-lang exposes exactly 5 discrete level buttons (set-level-1..set-level-5), so level can never be set outside 1-5. However, for the Weight constraint (declared in DOM as aria-valuemin="10" aria-valuemax="100" on the competency weight slider), interaction testing showed the control is entirely non-functional: focused the Programming Languages weight slider thumb (starting valuenow=90) and pressed "End" (should jump to max=100) — value stayed at 90; pressed "ArrowRight" (should increment) — value stayed at 90. Since no input method (click-focus+keyboard, drag — per prior FT-4 findings) can change the weight value at all, the 10-100% bound cannot be exercised or confirmed as enforced; the constraint is effectively unverifiable/non-functional rather than demonstrably respected.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node control does not visually reflect the new value after an edit
    - Actual: After clicking Agile Methodology's "set level 5" button, the aggregate Coverage metric correctly updated (68%→73%), proving the edit was applied internally, but the node's own displayed Level label still read "Intermediate" and its 5-segment level indicator still showed 3 filled segments (bg-level-intermediate) and 2 unfilled (bg-muted) — i.e. the graph control itself never shows the current value/state after the edit, so a user cannot see it reached level 5 by looking at the node. The same staleness was observed earlier for weight and enabled-state edits. This violates "the graph control displays the current value or state" and makes it impossible to confirm Gap/Critical indicators on the node agree with the summary from the node alone.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph "Critical" highlighting disagrees with the Critical Path summary after a relevant change
    - Actual: Baseline Critical Path summary = 3 competencies (Programming Languages, Problem Solving, System Design), matching the 3 nodes carrying a "Critical" badge in the graph. After disabling Problem Solving (an enabled-state change), the summary immediately recalculated to Critical Path = 4 (Programming Languages, System Design, Collaboration, +1), correctly dropping Problem Solving from the list. However, the Problem Solving node in the graph still displays its "Critical" badge (unchanged), so the graph highlighting and the summary analysis disagree about which competencies are critical after the edit.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Lowering weight does not trigger a risk-gap warning; instead it breaks the metrics calculation (NaN)
    - Actual: Since the per-node Weight slider is completely non-functional (see CS-12/FT-4), weight was lowered via the Scenario Simulation panel's Technical category Weight Change slider (keyboard-adjustable), pushed to its minimum (-30%) via the Home key, then Run Simulation was clicked. Expected: a risk-gap warning should appear/clear reflecting the now-underweighted Technical competencies. Actual: metrics.coverage displayed "NaN%" and metrics.avg-level displayed "NaN"/"N/A" — the calculation broke entirely — while metrics.gaps.count remained "0" throughout. No risk-gap indicator was created despite the model being in a clearly invalid/degenerate state. Reset afterward restored Coverage to 68% and Avg Level to 3.3.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: No risk-gap alert is ever displayed near the summary metrics, even when the model is in a clearly broken/degenerate state
    - Actual: Throughout all testing (baseline, disabling critical competencies, applying scenario simulations, and forcing an extreme -30% Technical weight delta that produced NaN Coverage/Avg Level), metrics.gaps.count ("Risk Gaps") stayed at "0 identified" and no alert/banner/warning element appeared near model.metrics or elsewhere on the page (checked via full innerText dump of the metrics/model container — only a "Model Healthy" message was ever observed under normal conditions, never a gap warning). Since the Risk Gaps counter never leaves 0 under any tested condition, including a state where the underlying metrics are literally NaN, there is no evidence the risk-gap alert UI is implemented/functional at all.