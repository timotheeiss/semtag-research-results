# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship weights are not editable, and the competency weight slider never updates its displayed value
    - Actual: Edge (relationship) weights are render-only SVG labels (80/70/90/60/50%): clicking an edge only selects it, no dialog/panel/input appears, and no relationship-weight control exists anywhere on the page (no "Relationship"/"Connection"/"Edge Weight" UI). The only weight control is the per-competency slider, which is also broken: after click on track, real mouse drag of the thumb, and ArrowLeft keypress on the focused thumb, model.graph.item.prog-lang.weight-value stayed "90%" and aria-valuenow stayed 90. The underlying state did change once (Coverage 68%->67%, exactly the weighted-coverage value for weight 90->55), proving the display is stale; a second slider interaction produced no change at all. Edge labels and stroke widths never changed.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Competency cannot be re-enabled, no disabled visual state, and disabled competency still counted in critical path
    - Actual: 1st click on model.graph.item.prog-lang.enabled disabled it in state (Coverage 68%->65%, Avg 3.3->3.2, matching exclusion of weight 90). But: (a) the toggle stayed data-state="checked"; (b) the node kept full opacity, kept its "Critical" badge and showed no disabled styling, and its 2 attached edges (e1 80%, e2 70%) kept opacity 1 and unchanged colour; (c) the Critical Path summary still lists "Programming Languages" and grew 3->5 competencies; (d) 2nd and 3rd clicks on the toggle had no effect at all - Coverage stayed 65%, Critical Path stayed 5 - so the competency can never be re-enabled and repeated toggles do not act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph, leaving the model internally inconsistent
    - Actual: Applying the Data Scientist template updated the header (title "Data Scientist", department "Analytics"), the summary metrics (Coverage 71%, Avg 3.5, Critical Path: Machine Learning / Python Programming / Statistical Analysis), the "Current" badge, and the scenario category counts (Technical 4, Behavioral 1, Leadership 0, Domain 1). But the competency graph still renders the previous Software Engineer model unchanged: the same 6 nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology, ids prog-lang/sys-design/code-review/problem-solve/collab/agile) and the same 5 relationships with weights 80/70/90/60/50%. The graph therefore contradicts the summary, which names competencies (Machine Learning, Python Programming, Statistical Analysis) that appear nowhere in the graph.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save-as-custom-template is not implemented: no name/description entry, nothing saved, no My Templates collection
    - Actual: Clicking "Save Current" (templates.save-current, action save-as-template) produced no response whatsoever: no dialog or alertdialog, no toast/status element, no name or description text input anywhere on the page, and no error. The Template Library still lists only the 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with no new entry. There is no "My Templates" collection anywhere in the page text, and localStorage remains completely empty (0 keys), so nothing was persisted to retrieve after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted; state reverts to defaults on leaving/reopening or reloading
    - Actual: Edited Software Engineer (Agile Methodology -> level 5), which moved Coverage 68%->73% and Avg Level 3.3->3.7. Clicking Save showed a "Model Saved - Your changes have been saved successfully." toast, but localStorage and sessionStorage both stayed completely empty (0 keys). Navigating back to the model list and reopening Software Engineer restored the defaults: Coverage 68%, Avg Level 3.3, Agile Methodology back to "Intermediate". Reloading the app and reopening the model gave the same default state, so no levels, weights or enabled states are restored.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario loads with uninitialized (all-zero) adjustments, so running it does nothing
    - Actual: On first load the Recruitment tab is active but all four category adjustments display Level Change 0 / Weight Change 0% (sliders centred at aria-valuenow 2 of 0-4 and 30 of 0-60, i.e. zero deltas). Clicking Run Simulation without switching tabs left every output unchanged: Coverage 68%, Avg Level 3.3, Critical Path 3, Risk Gaps 0 - identical to before. Only after switching to another tab and back does Recruitment show its actual defaults (technical -1/0%, behavioral 0/+10%, leadership 0/0%, domain -1/0%), proving the initial scenario state was never initialized.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph controls never show the post-edit state; node Gap/Critical indicators disagree with the summary; subsequent edits start from stale state
    - Actual: (a) Value not displayed: setting Problem Solving to level 1 changed the summary (Coverage 68%->56%, Avg 3.3->2.8, Risk Gaps 1) but the node still displayed "Level: Advanced" with 4 of 5 level segments filled (bg-level-advanced x4); the same happened for level 5. Weight edits changed analysis (Coverage 68%->69%, computed weight 60->35) while the node label stayed "60%" and aria-valuenow stayed 60. Disabling a competency left its toggle at data-state="checked". (b) Indicators disagree: with the summary reporting a risk gap on Problem Solving, its node carried no gap indicator whatsoever (only Tailwind gap-* layout utilities; card classes "competency-node bg-card p-4 min-w-[220px] border-node-critical"). After disabling Programming Languages the summary listed Critical Path = 5 competencies including the disabled one, while only 3 nodes carried border-node-critical. (c) Stale baseline: after disabling Programming Languages, a later level edit on Collaboration silently re-enabled it (Coverage jumped to 75%, i.e. the full 440 weight base), and repeat toggle clicks re-applied "disable" instead of re-enabling.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path is not a valid path, includes disabled competencies, and disagrees with graph highlighting
    - Actual: Baseline critical path = "Programming Languages, Problem Solving, System Design", but the graph's only relationships are prog-lang->code-review, prog-lang->sys-design, problem-solve->sys-design, collab->code-review, sys-design->collab. No edge joins Programming Languages to Problem Solving, and no ordering of those three nodes forms a directed path (System Design is a sink for both), so the summary is a set of high-weight competencies rather than a path. After disabling System Design the summary recalculated to 4 competencies "Programming Languages, Problem Solving, Collaboration, +1" (and Coverage 68%->70%, correctly excluding System Design), yet the graph still highlighted prog-lang, sys-design and problem-solve with border-node-critical - i.e. it still highlights the DISABLED System Design and never highlights Collaboration. Summary and graph highlighting therefore do not agree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.