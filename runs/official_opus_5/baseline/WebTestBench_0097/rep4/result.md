# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI to edit relationship (edge) weights
    - Actual: Edges render a weight label (e.g. 80%) and stroke width scaled to weight, but single-click, double-click and right-click on the edge only add the React Flow "selected" class — no editor, dialog, popover, context menu or input appears. No relationship/connection editing control exists anywhere in the detail editor (page text contains no "Relationship" editor section), so relationship weight cannot be changed.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not shown and competency cannot be re-enabled
    - Actual: Clicking Problem Solving's switch excluded it from calculations (Coverage 60%→65%, Avg Level 3.0→3.2), but the node still renders aria-checked="true", full opacity, no disabled styling, and its edges stay fully opaque/coloured. Clicking the same switch a second time did NOT re-enable it: Coverage stayed 65% and Avg Level 3.2 (still excluding Problem Solving), because the toggle acts on the stale displayed state rather than the current state. Additionally the toggle reverted a previously set level (Programming Languages level 2 → back to 4).

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Template application does not update the competency graph
    - Actual: Applying the "Data Scientist" template changed the header (Data Scientist / Analytics), summary metrics (Coverage 71%, Avg 3.5, critical path listing Machine Learning, Python Programming, Statistical Analysis), the template "Current" badge and scenario category counts (Technical 4, Behavioral 1, Domain 1). However the graph still renders the previous Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology, ids prog-lang…agile) and the old relationships (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%) even after a fit-view re-render, so the model is not replaced consistently.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No custom-template save flow or My Templates collection
    - Actual: Clicking "Save Current" only shows a toast "Template Saved – Current model configuration has been saved as a template." No dialog or name/description fields appear (0 dialogs, 0 inputs on page). The Template Library still lists only the 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with no new entry, there is no "My Templates" section anywhere in the app, and localStorage stays empty, so nothing can be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted/restored
    - Actual: In Product Manager, setting Product Strategy to level 1 changed Coverage 78%→66%, Avg 3.8→3.3, Risk Gaps 0→1. Clicking Save showed toast "Model Saved – Your changes have been saved successfully.", but returning to the list and reopening Product Manager restored the original defaults (Coverage 78%, Avg 3.8, Risk Gaps 0, Product Strategy level Advanced/95%). Nothing is written to localStorage or sessionStorage, so the state is also lost on reload.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario not initialized with its default adjustments
    - Actual: On a fresh load with the Recruitment tab already selected, all category adjustments displayed 0 / 0%. Clicking Run Simulation showed the toast "Simulation Applied" but no output changed (Coverage stayed 68%, Avg 3.3, Critical Path 3, Risk Gaps 0). Only after switching to another tab and back does Recruitment show its real defaults (Technical -1, Behavioral +10%, Domain -1), which then do change the outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node control shows stale values; no Gap indicator; subsequent edits start from stale state
    - Actual: After setting System Design weight via its slider the summary recomputed (Coverage 68%→69%) but the node still showed "Weight 80%" (aria-valuenow=80). After setting Programming Languages to level 2 the summary showed Avg 3.0/Coverage 60% and Risk Gaps 1, yet the node still displayed "Level Advanced" with 4/5 dots filled and no Gap indicator. A later toggle on another node wrote the stale level back, reverting Programming Languages to level 4 — subsequent edits start from the stale, not the current, state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path not reflected in graph highlighting and not a valid path
    - Actual: Graph highlighting is static: only nodes flagged "Critical" (prog-lang, sys-design, problem-solve) carry border-node-critical and this never changed while the summary critical path changed 3→5→4→3 across weight/enable edits; the disabled System Design node stays highlighted. The summary path itself is not a connected path: e.g. "Programming Languages, Problem Solving, Collaboration, +1" although no edge links Programming Languages to Problem Solving (edges are prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab). Summary and graph therefore disagree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.