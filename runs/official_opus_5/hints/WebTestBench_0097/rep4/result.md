# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship/competency weight change is not reflected in the displayed weight or its visual emphasis
    - Actual: Edge (relationship) weights are read-only: clicking/selecting an edge (e.g. "Edge from user-research to prod-strategy", label 90%) only selects it, no weight editor appears anywhere. The only editable weight is the node weight slider; changing it (Home key on Product Strategy) recalculated coverage (78%→77%) but the displayed weight stayed "95%" (aria-valuenow=95) and the edge stroke-widths (2.7/2.4/2.55/2.1/1.8) did not change.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown and a competency can never be re-enabled
    - Actual: Toggling Communication off correctly excluded it from calculations (coverage 78%→73%, avg 3.8→3.6), but the switch still reads data-state="checked", the node keeps full opacity with no disabled styling and its edge ("Edge from communication to stakeholder") is unchanged. Clicking the toggle a 2nd and 3rd time left coverage at 73% / avg 3.6 — the competency stays disabled forever, so repeated toggles do not act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph/relationships
    - Actual: Applying the "Software Engineer" template updated the title (Software Engineer / Engineering), metrics (coverage 68%, avg 3.3, critical path 3), scenario categories (technical 3, behavioral 2, leadership 0, domain 1) and the "Current" badge, but the graph still renders the Product Manager competencies (Product Strategy, Prioritization, User Research, Stakeholder Management, Data Analysis, Communication) with the same PM edges even after waiting 1.5s — so the graph contradicts the rest of the editor.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save-as-custom-template is non-functional; no name/description prompt and no My Templates collection
    - Actual: Clicking "Save Current" (data-semtag-action=save-as-template) twice produced no dialog (0 [role=dialog]), no toast, no new entry in templates.list (still the same 5 built-in roles) and no "My Templates" section anywhere in the page text; localStorage remains empty, so nothing can be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted; reopening restores factory defaults
    - Actual: Set Data Analysis to level 5 (coverage 78%→83%), clicked Save (toast "Model Saved - Your changes have been saved successfully."), went Back and reopened Product Manager: coverage 78%, avg 3.8, Data Analysis back to "Intermediate". localStorage is completely empty (Object.keys(localStorage) === []), so nothing survives leaving the model or a reload.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized with its default adjustments
    - Actual: On opening the detail editor, the Recruitment tab showed all category adjustments as Level 0 / Weight 0%. Clicking Run Simulation without switching tabs left every metric unchanged (coverage 78%, avg 3.8, critical path 4, gaps 0). Only after switching to Training and back did Recruitment reveal its real defaults (technical -1, behavioral +10%, leadership -2/-10%, domain -1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls do not display the current level/weight after an edit (stale render)
    - Actual: Clicking User Research "set level 5" changed summary coverage 78%→84% and avg level 3.8→4.2, but the node still displays Level "Intermediate" with only 3 of 5 level bars filled. Likewise pressing Home on Product Strategy's weight slider changed coverage 78%→77% (consistent with weight 95→10) while the node still showed Weight 95% and aria-valuenow=95, so a subsequent slider edit restarts from the stale 95 rather than the current value.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path does not recalculate after an enabled-state change and includes a disabled competency
    - Actual: After disabling Communication (coverage recalculated 78%→73%), the Critical Path summary still shows "4 competencies: Product Strategy, User Research, Stakeholder Management +1" and the disabled Communication node still carries the border-node-critical highlight. The listed set is also not a connected path: edges are user-research→prod-strategy, prod-strategy→prioritization, stakeholder→prioritization, communication→stakeholder, so {prod-strategy, user-research, stakeholder, communication} forms two disconnected chains rather than one valid path.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.