# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship/competency weight control is non-functional
    - Actual: Neither the graph edges (labeled "Edge from X to Y NN%") nor each competency node's "Weight" slider respond to any interaction. Clicking an edge only toggles a "selected" CSS class with no value change. Clicking the weight slider thumb, clicking directly on the slider track (which should jump the value to click position), a full drag from the thumb to a distant point (different x and y, e.g. from x=253,y=114 to x=87,y=82), and keyboard ArrowLeft/ArrowDown after focusing the thumb all leave aria-valuenow unchanged (verified before/after: Code Review weight stayed at 60% through track-click, cross-row drag, and thumb click attempts). By contrast, the Level 1-5 buttons on the same node DID change underlying data (confirmed via avgLevel/coverage metrics shifting from 3.3/68% to 3.7/74% to 3.0/63%), proving the weight slider specifically is broken while other controls on the same node work - so weight cannot be changed by any discoverable means, and no dependent analysis or visual emphasis update was observed.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disable/re-enable toggle doesn't visually update and can't be re-enabled
    - Actual: Clicking Agile Methodology's enabled switch correctly excluded it from calculations the first time (coverage 68%->69%, avgLevel 3.3->3.4, matching average of the remaining 5 competencies), but the switch's rendered aria-checked/data-state stayed "true"/"checked" and the node card showed no dimming, badge, or other disabled indicator at all. Clicking the same switch two more times left metrics unchanged at 69%/3.4 instead of toggling back to 68%/3.3 - i.e. the toggle never re-enabled the competency and does not act on the current actual state (it appears to always read a stale "enabled" prop and re-disable). This fails the requirements that the disabled state be clearly shown and that repeated toggles act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates title/metrics/scenario categories but the competency graph keeps showing the previous model's nodes
    - Actual: Clicking "Apply" on the Product Manager template while viewing the Software Engineer model correctly updated model.title to "Product Manager", model.department to "Product", metrics (coverage 78%, avgLevel 3.8, criticalPath 4, gaps 0), the template list's "Applied"/"Current" markers, and the scenario Category Adjustments competency counts (technical 1, behavioral 1, leadership 1, domain 3 - different from Software Engineer's 3/2/0/1). However, the graph nodes still showed the old Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) instead of Product Manager competencies, so the graph and relationships do not represent the selected template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saving a custom template has no name/description input and the saved template cannot be retrieved anywhere
    - Actual: Clicking "Save Current" (templates.save-current) in the model editor immediately showed a toast "Template Saved / Current model configuration has been saved as a template." with no dialog or input fields for entering a name or description at any point before or after the click - so the user cannot name or describe the template. Additionally, after saving, the templates.list collection in the model editor still showed only the same 5 built-in templates (no new entry), and navigating back to the home page showed home.stats.templates still reading "5 Templates" (not 6) and no "My Templates" section or collection anywhere in the page (verified via document.body.innerText.includes('My Templates') -> "not found" on both the model detail page and the home page). The saved template is neither nameable/describable nor retrievable.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Running a simulation with a user-adjusted category slider produces an invalid "NaN%" coverage output
    - Actual: Category-level Level/Weight Change sliders in the Scenario Simulation panel are interactive (confirmed by focusing the Behavioral "Level Change" slider and pressing ArrowRight, which moved aria-valuenow 2->3 and the displayed label from "0" to "+1"). Running the Training scenario's own default adjustment (technical Level Change +1) alone correctly updated coverage to 81% and avgLevel to 4.0. However, after additionally setting Behavioral Level Change to +1 and clicking "Run Simulation" again, metrics.coverage displayed "NaN%" while avgLevel showed a plausible 3.7 - an invalid/broken output. Reset did correctly restore the original 68%/3.3 baseline afterward, so Reset itself works, but the simulation run with a combined category adjustment produced a broken coverage output.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted after leaving and returning, or after reload
    - Actual: Opened Software Engineer model (baseline metrics.coverage=68%), set Agile Methodology's level to 5 (level-value "Intermediate"->set-level-5), which correctly changed metrics.coverage to 73%. Clicked model.save (action "save-model"). Navigated back to the home page via model.back and reopened the Software Engineer card: metrics.coverage read 68% again, i.e. the saved edit was lost and the model reverted to its original default state. Repeated the same edit+save sequence and this time verified persistence via a full page reload (browser_navigate to http://localhost:6097/ then reopening the Software Engineer card): metrics.coverage again read 68%, confirming the level-5 edit was not restored either. In both leave/return and reload scenarios, the previously saved levels/weights/enabled states were not restored - Save does not persist changes for later retrieval.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage/progress update immediately for level and enable/disable changes, but weight changes can't trigger this since weight control is non-functional
    - Actual: Setting Agile Methodology's level to 5 immediately changed metrics.coverage from 68% to 73% and the progressbar fill from translateX(-31.8%) to translateX(-27.3%) with no reload, and disabling/enabling a competency similarly updated coverage instantly (per FT-5 testing). However, changing a competency's weight (the third trigger named in this item) is impossible: as documented in FT-4, the weight slider does not respond to click, track-click, drag, or keyboard input, so no weight-triggered coverage update could be produced or verified.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Default (initially selected) Recruitment scenario has all-zero adjustments, so running it produces no output change
    - Actual: On opening the Software Engineer model, the Recruitment tab is selected by default with Level Change=0 and Weight Change=0% for every category (technical/behavioral/leadership/domain). Clicking "Run Simulation" immediately without switching tabs or touching any slider left metrics.coverage (68%), metrics.avg-level (3.3), metrics.critical-path.count (3), and metrics.gaps.count (0) completely unchanged, and no simulation-results panel appeared elsewhere. Since the default scenario carries no non-zero adjustments, running it as-is cannot and does not change any model output, failing the requirement that it be "fully initialized" with visible, applicable default adjustments.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node control does not display the current value/state after an edit, and its Critical indicator disagrees with the summary analysis
    - Actual: Setting Code Review's level to 5 (via set-level-5) changed the underlying data (avgLevel 3.3->3.7, coverage 68%->74%) but the node card kept showing "Intermediate" with 3/5 filled level segments - the same as before the edit. The same node's level was then set to 1, and metrics updated again (avg 3.0) but the card still showed "Intermediate"/3-filled, never reflecting the true current level. Separately, disabling "Problem Solving" updated the critical-path summary but left the node's border-node-critical highlighting stale (see CT-16), so the graph's Critical indicator disagreed with the summary. Because the displayed value never reflects the actual current state, a user cannot visually confirm what "current state" a subsequent edit starts from.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph critical-path highlighting does not sync with the recalculated critical-path summary
    - Actual: Disabling "Problem Solving" (a critical-path member) immediately updated the Critical Path summary from 3 (Programming Languages, Problem Solving, System Design) to 4 (Programming Languages, System Design, Collaboration, +1), correctly excluding the disabled competency. However, the graph's node highlighting (border-node-critical class) still marked Programming Languages, System Design, AND Problem Solving as critical (the stale, pre-edit set) and did not highlight Collaboration, which the new summary lists as part of the critical path. The graph highlighting and the critical-path summary disagree after the edit.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.