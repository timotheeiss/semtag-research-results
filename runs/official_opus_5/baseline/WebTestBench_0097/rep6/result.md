# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to change a relationship (edge) weight; competency weight edits are not displayed
    - Actual: Edges expose only selection: clicking, double-clicking or right-clicking an edge (e.g. "Edge from prog-lang to code-review", label 80%) only marks it selected — no weight editor, dialog, popover or input appears anywhere, and all 5 edge labels stayed 80/70/90/60/50% throughout. Changing a competency's own weight slider (Collaboration 70%→55% in React state, coverage 61%→68%) leaves the node's displayed "Weight 70%" and slider aria-valuenow=70 unchanged, and it does not affect any relationship weight.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not reflected in graph; disabled competency still counted in critical path; toggle cannot re-enable
    - Actual: Clicking a node's enable switch (e.g. Agile, Programming Languages) updates the analysis (coverage 68%→65%) but the switch stays aria-checked="true", the node keeps opacity:1 with no disabled styling and its edges are unchanged. The summary Critical Path still lists the disabled "Programming Languages". Clicking the same switch a second time does not re-enable (metrics stay identical, React state stays enabled:false), and a later level edit silently reverts the competency back to enabled.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph
    - Actual: Applying the "Data Scientist" template updated the header (Data Scientist / Analytics), metrics (Coverage 71%, Avg 3.5, Critical Path: Machine Learning, Python Programming, Statistical Analysis), the "Current" badge and the scenario category counts (Technical 4 / Behavioral 1 / Domain 1). The graph however still shows the previous Software Engineer competencies (prog-lang, sys-design, code-review, problem-solve, collab, agile) and the identical 5 Software Engineer relationships (80/70/90/60/50%), so the graph contradicts the rest of the editor.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No name/description input and no My Templates collection; saved template is not stored
    - Actual: Clicking "Save Current" shows only a toast ("Template Saved — Current model configuration has been saved as a template."). No dialog, name field or description field is ever presented (no input/textarea elements exist on the page). The Template Library still lists only the 5 built-in role templates, there is no "My Templates" section anywhere, and localStorage remains empty, so after leaving the editor and reopening the model no custom template can be retrieved.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Choosing a minimum category adjustment corrupts the sibling adjustment and produces NaN outputs
    - Actual: The happy path works: setting Technical Level Change to +2 and running the Recruitment scenario updated Coverage 68%→82%, Avg Level 3.3→4.0 and showed "Model Healthy"; Reset restored the original model (68%, 3.3, original levels/weights) and zeroed the adjustments. But choosing the minimum of a category slider (Technical Level Change = -2, or Behavioral Weight Change = -30%) immediately turns the sibling adjustment into "NaN%"/blank (aria-valuenow=NaN), and running the simulation then reports Coverage "NaN%" with weight=NaN on the affected competencies.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted or restored
    - Actual: Set System Design to level 5 (Coverage 68%→75%, Avg 3.3→3.7) and clicked Save; a "Model Saved — Your changes have been saved successfully." toast appeared but localStorage stayed empty. Navigating back to the model list and reopening Software Engineer showed the original model again (Coverage 68%, Avg 3.3, sys-design level 3), so the saved levels/weights/enabled states were not restored.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Simulation can drive competency weights out of the 10–100% range (NaN)
    - Actual: Levels stay clamped (1–5: Programming Languages 4→5 capped on +2, technical levels bottomed at 1 on repeated -2) and the weight slider enforces min 10/max 100 (aria-valuemin=10, aria-valuemax=100; Problem Solving capped at 100%). However, setting the Recruitment "Technical → Level Change" slider to its minimum (-2) sets the sibling "Weight Change" to NaN%, and running the simulation then writes weight=NaN to all three technical competencies (prog-lang, sys-design, code-review) and the Coverage metric renders as "NaN%".


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario is not initialized with its default adjustments
    - Actual: On opening the detail editor the pre-selected Recruitment tab showed every adjustment as "Level Change 0 / Weight Change 0%". Clicking Run Simulation without switching tabs left all outputs unchanged (Coverage 68%, Avg 3.3, Critical Path 3, Risk Gaps 0; React model identical). Only after visiting another tab and returning did Recruitment display its real defaults (Technical -1, Behavioral +10%, Domain -1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls show stale values/state after edits and subsequent edits do not start from current state
    - Actual: After clicking level segment 2 on Programming Languages the summary updated (Coverage 69%→60%, Avg 3.4→3.0, Risk Gaps 1) but the node control still displayed "Level Advanced" with 4 filled segments and "Weight 90%", and showed no Gap indicator despite the summary reporting a gap for that competency. After disabling a competency the node switch remained aria-checked="true". A subsequent level edit was applied on the stale data and silently re-enabled the previously disabled competency (React state agile enabled:false → true).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path summary and graph highlighting disagree; path includes disabled competency and is not a valid graph path
    - Actual: After disabling Programming Languages, the summary Critical Path changed from 3 to "5 competencies: Problem Solving, System Design, Programming Languages +2" — it still contains the disabled competency, and no 5-node path exists in the graph (edges are prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab; longest path is 3). Graph highlighting (border-node-critical) stayed on the original 3 nodes, so it no longer agrees with the summary.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.