# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to edit a relationship (edge) weight; competency weight edits do not update the displayed weight and are silently reverted
    - Actual: The graph exposes no control for editing edge weights (edge labels 80%/70%/90%/60%/50% are read-only; edge hit areas are covered by node markup and no editor opens). The only weight control is the per-competency slider: dragging/clicking it changed React state weight 0.9->0.5 and coverage 68%->67%, but the node kept displaying "Weight 90%" and the value was reverted to 0.9 by the next node edit. Edge labels and stroke widths never changed.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown, disabled competency still counted in critical path, and re-enabling is impossible because the toggle always acts on stale initial state
    - Actual: Toggling Programming Languages set enabled=false in React state (coverage 68%->65%, avg 3.3->3.2, correctly excluded), but the node still renders with switch aria-checked="true", opacity 1 and no disabled styling, and its edges are unchanged. Critical Path went to 5 and still lists "Programming Languages" although it is disabled. Clicking the toggle a second time left enabled=false (coverage stayed 65%) - the competency can never be re-enabled. The toggle also reverted the competency's level from 2 back to 4.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph; the graph keeps the previous model's nodes and relationships
    - Actual: Applied the "HR Manager" template from the Software Engineer model. Title -> "HR Manager", department -> Human Resources, metrics -> 71%/3.5/CP 3, scenario category counts -> Technical 0 / Behavioral 1 / Leadership 2 / Domain 3, and the Current badge moved. But the graph still renders the Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the identical 5 relationships (prog-lang->code-review 80%, prog-lang->sys-design 70%, problem-solve->sys-design 90%, collab->code-review 60%, agile->collab 50%), which do not belong to the HR Manager template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save-as-template does nothing: no name/description form, no custom template created, and no My Templates collection exists
    - Actual: Clicked "Save Current" (templates.save-current, action save-as-template). No dialog, prompt, form, toast or confirmation appeared (0 [role=dialog] elements), the Template Library still lists only the same 5 built-in templates, and the string "My Templates" does not exist anywhere in the page.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted; state resets to defaults when the model is reopened
    - Actual: In Data Scientist, disabled "Data Storytelling" (coverage 71%->73%) and clicked Save. No confirmation appeared and localStorage/sessionStorage remained completely empty. Navigating back to the model list and reopening Data Scientist restored the untouched defaults (storytelling enabled again, coverage back to 71%). Also observed that the level edit made just before the toggle was silently reverted.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized; its default adjustments show 0 and running it changes nothing
    - Actual: On first load the Recruitment tab was active but all four category adjustments read Level 0 / Weight 0%. Clicking Run Simulation left the model completely unchanged (coverage 68%, avg 3.3, critical path 3, gaps 0; React state levels/weights identical). Only after switching to Training and back to Recruitment did the real defaults appear (technical -1, behavioral +10% weight, domain -1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never re-render after an edit; they show stale values and no Gap indicator, and the next edit starts from the stale state
    - Actual: Clicked Programming Languages level bar 2. Summary updated (coverage 68%->60%, avg 3.0, Risk Gaps 1 naming Programming Languages) and React state shows prog-lang level=2, but the node still displays Level "Advanced" with 4/5 bars filled and Weight 90%, and shows only a "Critical" badge with no Gap indicator. Earlier a weight change to 0.5 (state confirmed, coverage moved 68%->67%) also left the node showing 90%, and the subsequent level click reverted weight back to 0.9 because the handler reads the stale node data.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path summary and graph highlighting disagree; path includes a disabled competency and is not a valid directed path
    - Actual: After disabling Programming Languages the summary shows Critical Path = 5 competencies listing "Problem Solving, System Design, Programming Languages, +2", while the graph still highlights only the original 3 nodes (border-node-critical on prog-lang, sys-design, problem-solve). The listed path includes the disabled prog-lang, and there is no edge System Design -> Programming Languages, so the sequence is not a valid directed path. Node highlighting never changed across level/weight/enable edits.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.