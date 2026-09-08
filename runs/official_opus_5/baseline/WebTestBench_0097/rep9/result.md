# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to edit relationship (edge) weight
    - Actual: Edges display weight labels (80%/70%/90%/60%/50%) but offer no editing affordance: clicking/keyboard-activating an edge only marks it .selected, opening no editor or dialog, and no relationship weight control exists anywhere in the detail editor (source CompetencyGraph.tsx defines no onEdgeClick/edge-weight handler; only competency node weight is editable). Relationship weight therefore cannot be changed.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not shown on node; competency cannot be re-enabled
    - Actual: Toggling the "Agile Methodology" switch off updated the summary (Coverage 68%→69%, Avg Level 3.3→3.4, i.e. it was excluded from calculations), but the node's switch stayed aria-checked="true" and the node/edges kept normal styling (class "competency-node bg-card p-4 min-w-[220px]", no disabled indicator). Clicking the switch a second time did NOT re-enable it — coverage stayed at 69% and the switch stayed checked, so repeated toggles do not act on the current state (CompetencyGraph uses useNodesState(initialNodes) and never re-syncs node data/callbacks).

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Graph does not update when a template is applied
    - Actual: Applying the "Data Scientist" template updated the title/badges (Data Scientist, Analytics, 6 competencies), metrics (Coverage 71%, Avg 3.5, Critical Path: Machine Learning, Python Programming, Statistical Analysis) and scenario categories (technical 4 / behavioral 1), but the competency graph still rendered the previous Software Engineer model: nodes Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology and edges prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab. Graph and relationships are inconsistent with the applied template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No custom-template save flow or My Templates collection
    - Actual: Clicking "Save Current" opened no dialog or form — there is no field for a template name or description anywhere (0 input/textarea elements on the page after clicking), and no "My Templates" section exists; the Template Library still lists only the 5 built-in role templates. The handler (handleSaveAsTemplate) only fires a toast and persists nothing (no localStorage use), so a saved configuration cannot be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: User-chosen category adjustment corrupts simulation output (Coverage NaN%)
    - Actual: Default scenario runs worked (Recruitment: Coverage 68%→56%, Avg 3.3→2.7, Risk Gaps 0→1 "System Design"; Reset restored 68%/3.3/0). However, choosing a custom adjustment — dragging Technical "Level Change" to +2 — made that category's Weight Change render "NaN%", and Run Simulation then produced "Coverage NaN%" (Avg Level 4.2). Coverage output is therefore invalid whenever a user picks their own category adjustment.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Edits are not persisted by Save; state lost on leaving the model
    - Actual: Set Programming Languages to level 2 (Coverage 60%, Avg 3.0, Risk Gaps 1) and clicked Save — only a "Model Saved" toast appeared. Navigating back to the list and reopening Software Engineer restored the original values (Coverage 68%, Avg 3.3, Risk Gaps 0), so the saved levels/weights/enabled states were not restored. handleSave writes nothing (no localStorage or shared store in the app).


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Custom scenario adjustment drives competency weight out of range (NaN)
    - Actual: Levels stay clamped 1–5 (repeated Recruitment runs bottomed out at Avg Level 1.8/Coverage 41%; technical +2 capped at level 5) and the direct weight slider is bounded min=10/max=100. But after dragging the Technical "Level Change" slider to +2, the same category's "Weight Change" displayed "NaN%", and running the simulation produced Coverage "NaN%" — the technical competencies' weights became NaN, i.e. outside the supported 10%–100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario not initialized; running it applies no adjustments
    - Actual: On first entering the editor, the pre-selected Recruitment tab showed every category adjustment as "Level Change 0 / Weight Change 0%" instead of its defaults (technical -1 level, behavioral +10% weight, leadership -2/-10%, domain -1 level). Clicking "Run Simulation" without switching tabs produced only a "Simulation Applied" toast while all outputs were unchanged (Coverage 68%, Avg Level 3.3, Critical Path 3, Risk Gaps 0) — adjustments state is only populated by handleScenarioChange.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never reflect current state; subsequent edits start from the original state, not the current one
    - Actual: After setting Programming Languages to level 2 the summary showed Coverage 60% / Risk Gaps 1, but the node card still read "Level Advanced", showed 4 filled level bars, "Weight 90%" and switch aria-checked=true; no Gap indicator appeared on the node. Also, after disabling Agile (Coverage 69%), a later level edit on Programming Languages silently re-enabled Agile (coverage math returned to the 6-competency basis: 64%), proving edits are applied to the stale initial model rather than the current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph critical-path highlighting does not recalculate and contradicts the summary
    - Actual: After lowering Programming Languages' weight, the Critical Path summary immediately changed from 3 competencies (Programming Languages, Problem Solving, System Design) to 5 (Problem Solving, System Design, Programming Languages, +2), but the graph still highlighted only the original 3 nodes (border-node-critical on prog-lang, sys-design, problem-solve; none on collab/agile) and every edge kept its original stroke colour (e.g. agile→collab still rgb(13,162,231), not the critical red). Summary and graph highlighting therefore disagree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.