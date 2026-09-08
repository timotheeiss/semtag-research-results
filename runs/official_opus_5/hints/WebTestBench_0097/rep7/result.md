# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship weight is not editable, and the only weight control (competency weight slider) does not update its displayed value after a change
    - Actual: Edge weights (80%/70%/90%/60%/50%) are read-only: clicking an edge or its label opens no editor and there is no relationship weight control anywhere in the detail editor. Using the competency weight slider (prog-lang, drag from 90% toward mid-track) changed the analysis (coverage 68%→67%, critical path 3→5) proving the underlying weight moved to ~50, but the node still displays "Weight 90%" and the slider still reports aria-valuenow=90; no edge label or stroke width changed.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown on the node/edges, cannot be undone, and disabled competency still appears in the critical path
    - Actual: Toggling prog-lang off did exclude it from coverage/avg/gap math (56%→65%, avg 2.8→3.2, gaps 1→0), but: (a) the node still renders opacity:1 with no disabled styling and its Switch stays aria-checked="true"; (b) its two outgoing edges keep full colour/animation with no dimming; (c) the Critical Path summary still lists "Programming Languages" (count 3→5) while disabled; (d) clicking the toggle a 2nd and 3rd time never re-enables — metrics stay 65%/3.2/0, so repeated toggles do not act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates the header/metrics/scenario categories but leaves the competency graph showing the previous model's competencies and relationships
    - Actual: Applied the "Data Scientist" template from the Software Engineer model. Title→"Data Scientist", department→"Analytics", metrics→71%/3.5, Current badge moved to data-scientist, and scenario categories changed to Technical 4 / Behavioral 1 / Domain 1 (the DS distribution). But the graph still renders the six Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology with the same levels/weights) and the identical five SE relationships (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%), which also contradicts the new Technical=4/Behavioral=1 category counts.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" does nothing — no name/description capture, no custom template created, and no My Templates collection exists
    - Actual: Clicking templates.save-current ("Save Current", action=save-as-template) opened no dialog or form: zero [role=dialog] elements, no text/textarea inputs, no toast. The Template Library still contains only the 5 built-in templates, the string "My Templates" appears nowhere in the app, and localStorage remains completely empty.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted — state reverts to the original on reopening the model
    - Actual: In Software Engineer, set Programming Languages to level 2 (coverage 68%→60%, avg 3.3→3.0, Risk Gaps 0→1), then clicked Save and got the toast "Model Saved – Your changes have been saved successfully." Navigating back and reopening Software Engineer showed coverage 68%, avg 3.3, gaps 0 — the original values. Both localStorage and sessionStorage were empty after saving, so nothing survives a reload either.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized: its default adjustments show as zero and running it does nothing
    - Actual: On first load the Recruitment tab is active but all four Category Adjustments read Level Change 0 / Weight Change 0%. Clicking Run Simulation without switching tabs left every output unchanged (coverage 68%, avg 3.3, critical path 3, gaps 0). Only after clicking Training/Promotion and returning to Recruitment did its real defaults appear (Technical -1, Behavioral +10%, Domain -1), proving the defaults were never loaded for the initially selected tab.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never reflect the current value/state after an edit, and subsequent edits do not start from the current state
    - Actual: After clicking level 2 on Programming Languages the summary updated (coverage 68%→60%, avg 3.3→3.0, gaps 0→1) but the node still displayed "Level Advanced"; after dragging its weight slider the summary changed (68%→67%) but the node still displayed "Weight 90%" and the slider reported aria-valuenow=90; after disabling it the Switch stayed aria-checked="true" and the node kept opacity:1. Because the control keeps the stale state, the next toggle click re-applies "disable" instead of re-enabling (metrics stayed 65%/3.2/0 over three consecutive clicks). Also the node's Critical badge and the summary disagree: Programming Languages is listed in the Critical Path summary while disabled.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path summary and graph highlighting disagree after a weight change, and the reported path is not a valid path through the graph
    - Actual: Baseline: summary Critical Path = 3 (Programming Languages, Problem Solving, System Design) and 3 nodes carry the critical highlight — agreeing. After lowering Programming Languages' weight (coverage 68%→67%), the summary jumped to 5 competencies (Problem Solving, System Design, Programming Languages, +2) while the graph still highlighted only prog-lang, sys-design, problem-solve — no recalculation of highlighting and a 3-vs-5 mismatch. A 5-node path is impossible in this graph (only 5 edges; longest chain agile→collab→code-review is 3 nodes), and even the baseline trio is not a connected path (no prog-lang→problem-solve edge). Disabling prog-lang also left it listed in the critical path.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.