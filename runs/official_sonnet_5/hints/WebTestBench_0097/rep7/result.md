# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship/competency weight cannot be changed via any UI interaction
    - Actual: Edges are only selectable (click toggles a "selected" class) with no editing UI exposed for their weight. The per-competency "Weight" slider (e.g. System Design weight=80%, Collaboration weight=70%) does not respond to: focused keyboard arrow keys (ArrowLeft/ArrowRight/ArrowDown pressed while slider thumb has real DOM focus - aria-valuenow unchanged), clicking the track/filled-bar (unchanged), or a genuine Playwright drag from the thumb to a different track location (aria-valuenow stayed at 70 for Collaboration despite trusted mouse drag across ~25-150px). Metrics (coverage) also did not change after these weight-adjustment attempts, confirming no underlying state change occurred, unlike the level buttons which did successfully alter avgLevel.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disable/enable toggle doesn't visually reflect state and repeated toggles don't act on current state
    - Actual: Clicking Agile Methodology's enabled switch excluded it from calculations (coverage 68%→69%, avgLevel 3.3→3.4), confirming disable worked internally, but the switch stayed aria-checked="true"/data-state="checked" and node showed no disabled styling (no clear disabled visual). Clicking the same switch again (intended re-enable) left coverage/avgLevel unchanged at 69%/3.4 instead of reverting to baseline 68%/3.3, showing the second click did not re-enable the competency - repeated toggles did not act on the actual current (disabled) state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates title/metrics/scenario categories but not the competency graph
    - Actual: Applying the "Product Manager" template while viewing Software Engineer correctly updated the title ("Product Manager"), department ("Product"), metrics (coverage 78%, avgLevel 3.8, criticalPath 4), and scenario Category Adjustments counts (technical 1, behavioral 1, leadership 1, domain 3 - a genuinely different distribution). However, the competency graph still displayed the original Software Engineer nodes and edges unchanged (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology with their original weights/edges) instead of Product Manager's competencies - the graph and relationships were not replaced.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" does not prompt for a name/description and does not create a retrievable custom template
    - Actual: Clicking "Save Current" immediately showed a toast "Template Saved: Current model configuration has been saved as a template." with no dialog to enter a name or description. The Template Library list still shows exactly the same 5 built-in templates afterward (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) - no new custom entry was added, and the homepage "Templates" stat remained "5 Templates". There is no "My Templates" collection anywhere in the app to retrieve a saved custom template from.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted across page reload
    - Actual: Disabled the 'Agile Methodology' competency (toggle switch), which correctly updated Coverage 68%->69% and Avg Level 3.3->3.4, then clicked Save. After reloading the page (http://localhost:6097/) and reopening the Software Engineer model, Coverage/Avg Level reverted to the original baseline (68%/3.3) and the Agile Methodology toggle's aria-checked/data-state showed 'true'/'checked' (enabled) again, meaning the saved edit was lost and the original unedited model was reloaded instead.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight (and possibly level) values are not clamped to the 10%-100% / 1-5 bounds during scenario simulation, causing computed metrics to break
    - Actual: Direct edits are constrained by UI construction (only 5 discrete level buttons 1-5 exist; competency weight slider declares aria-valuemin=10/aria-valuemax=100). However, via Scenario Simulation: pushing Technical Weight Change to its max (+30%, applied to Programming Languages at 90% and other technical items) and clicking Run Simulation broke both Coverage ('NaN%') and Avg Level ('NaN') -- i.e. weights were pushed past the 100% ceiling (e.g. 90%+30%=120%) without clamping, corrupting downstream calculations instead of capping at 100%. A milder case (Technical Level Change=+1 only, tested under IX-19) similarly broke Coverage to 'NaN%'. Reset correctly restored baseline (68%/3.3/3/0) afterward.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Running default (Recruitment) scenario without switching tabs applies adjustments but corrupts an output metric
    - Actual: Recruitment tab is selected by default and shows visible neutral adjustments (Level Change '0', Weight Change '0%' for all 4 categories). Running simulation with all adjustments at 0 correctly leaves outputs unchanged (68% / 3.3 / 3 / 0). However, after setting Technical Level Change to +1 on the default Recruitment tab (without switching tabs) and clicking Run Simulation, Avg Level updated correctly (3.3->3.8) but Coverage broke to 'NaN%' instead of a valid percentage, and Critical Path/Risk Gap counts stayed unchanged. Reset correctly restored baseline (68%/3.3) and slider back to neutral.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node control does not visually reflect the current level/state after an edit
    - Actual: Clicked "System Design" set-level-5 (Expert) button: Avg Level metric correctly jumped 3.3→3.7 (proving the underlying level changed), but the node's level segment bar remained showing 3 filled segments and the level label still read "Intermediate" instead of "Expert". Earlier, setting Collaboration to level-1 dropped Avg Level 3.3→3.0 and produced a Risk Gap alert naming "Collaboration", yet Collaboration's node still displayed "Intermediate"/3 segments. The graph control thus disagrees with the summary analysis after an edit.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path summary and graph node "Critical" highlighting disagree after a state change
    - Actual: Disabling "Problem Solving" updated the Critical Path summary to a new 4-node path: Programming Languages, System Design, Collaboration, +1 (Problem Solving correctly excluded since disabled). However, the graph's per-node "Critical" badge still marked Programming Languages, System Design, and the disabled Problem Solving as Critical (unchanged from before the edit), and did not mark Collaboration as Critical - the node highlighting did not recalculate and disagrees with the summary.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.