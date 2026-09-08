# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to change a relationship (edge) weight — edges are read-only
    - Actual: Single-click, double-click and right-click on the edge and on its weight label ("90%" for problem-solve→sys-design) only toggle React Flow selection: no inline editor, popover, dialog, context menu or side panel appears (0 dialogs/menus; the only sliders on the page are the 6 node weight sliders and 8 scenario sliders). Edge weights stayed 80/70/90/60/50% throughout. Source confirms CompetencyGraph.tsx builds edges from model.edges with no edge-update handler.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown on the node/edges, and a disabled competency cannot be re-enabled
    - Actual: Toggling System Design off correctly excluded it from analysis (Coverage 72%→70%, Avg 3.4 over 5 comps, critical path dropped System Design, gap cleared), but the node's switch still reads aria-checked="true", the card keeps its normal styling (no opacity-50/disabled class) and its edges are unchanged. Clicking the same switch a second time to re-enable had no effect at all: coverage stayed 70%, avg 3.4, critical path still excluded System Design — repeated toggles always recompute from the original state instead of the current one.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph — graph shows the previous model's competencies and relationships
    - Actual: From Software Engineer, applying the "HR Manager" template updated the title ("HR Manager" / Human Resources / 6 competencies), metrics (Coverage 71%, Critical Path: Talent Acquisition, Employee Relations, HR Compliance), scenario category counts (Technical 0 / Behavioral 1 / Leadership 2) and the "Current" badge. But the graph still renders the six Software Engineer nodes — Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology — and the five Software Engineer edges (prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab), while the underlying model prop is HR Manager (talent-acq, emp-relations, hr-compliance, leadership, perf-mgmt, org-dev).

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" is a no-op toast: no name/description entry and no My Templates collection
    - Actual: Clicking "Save Current" in the Template Library opened no dialog or form (0 [role=dialog], 0 text inputs) — it only showed the toast "Template Saved / Current model configuration has been saved as a template.". The Template Library still lists only the 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager); after going back to the model list and re-opening a model there is no "My Templates" section and no saved custom template anywhere on the page.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Choosing a category adjustment corrupts the paired weight adjustment to NaN, and running the simulation makes Coverage "NaN%"
    - Actual: Running a scenario with its loaded defaults works (Recruitment → Coverage 68%→56%, Avg 3.3→2.7, Risk Gaps 0→1, critical path reordered) and Reset restores 68% / 3.3 / 0 gaps. But after Reset, changing Technical "Level Change" to +1 immediately made that category's "Weight Change" display "NaN%", and clicking Run Simulation set Coverage to "NaN%" (Avg Level 3.8). The adjustment entry is created without a weightDelta, so the simulated weights become NaN.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted — reopening the model restores the original values
    - Actual: Raised System Design to level 5 (Coverage 68%→75%, Avg Level 3.3→3.7) and clicked Save, which showed "Model Saved / Your changes have been saved successfully.". Navigating back to the model list and reopening Software Engineer showed Coverage 68% and Avg Level 3.3 again — the edit was lost. localStorage and sessionStorage are both empty, so nothing survives a reload either.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Simulation can drive competency weights outside 10–100% (to NaN)
    - Actual: Level/weight clamping works for scenario defaults (repeated Training runs capped levels at 5: Avg 4.5 with prog-lang/sys-design/code-review/agile all L5; repeated Promotion runs capped behavioral weights at 1.000 and code-review at 0.100; direct controls expose only levels 1–5 and a weight slider with aria-valuemin=10/aria-valuemax=100). However, choosing only a category "Level Change" (Technical −1) leaves the paired Weight Change as "NaN%", and running the simulation set all three technical competencies' weights to NaN (prog-lang WNaN, sys-design WNaN, code-review WNaN) with Coverage rendering "NaN%" — weights outside the supported 10–100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized — its default adjustments are neither shown nor applied
    - Actual: On a freshly opened model the Recruitment tab shows Level Change 0 / Weight Change 0% for all four categories, although the scenario's defaults are technical −1 level, behavioral +10% weight, leadership −2/−10%, domain −1 level. Clicking "Run Simulation" without switching tabs showed a "Simulation Applied" toast but left every output unchanged (Coverage 68%, Avg Level 3.3, Risk Gaps 0, critical path identical). Source: SimulationPanel initializes adjustments to {} and only fills them in handleScenarioChange.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never re-render after an edit; they keep showing the pre-edit value and stale Gap/Critical indicators
    - Actual: Clicking System Design's level-2 bar updated the summary (Coverage 68%→65%, Avg 3.3→3.2, Risk Gaps 0→1 "System Design"), but the node still displays "Level Intermediate" with 3 of 5 level bars filled and its class is "competency-node ... border-node-critical" with no border-node-warning (gap) styling, so the node's Gap indicator contradicts the summary. Source: CompetencyGraph.tsx passes initialNodes only to useNodesState() with no sync effect, so node data is frozen at mount.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph critical-path highlighting does not recalculate and contradicts the summary; the "path" is a disconnected set
    - Actual: Lowering Programming Languages' weight to 55% changed the summary Critical Path from "3 competencies: Programming Languages, Problem Solving, System Design" to "5 competencies: Problem Solving, System Design, Programming Languages +2", but the graph still highlighted exactly the original 3 nodes (border-node-critical on prog-lang, sys-design, problem-solve) and the same 2 red edges — no recalculation. Same disagreement after disabling System Design (summary dropped it from the path while the node kept its critical highlight). The 5-item result is also not a valid connected path: it includes Collaboration/Agile, whose only edges (agile→collab, collab→code-review) do not connect to the prog-lang/problem-solve/sys-design part of the set.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.