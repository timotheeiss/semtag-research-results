# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship weight not editable; competency weight edit leaves displayed weight and visual emphasis stale
    - Actual: Edges (relationships) offer no weight editor — clicking an edge only selects it, no dialog/panel appears, and edge labels stay 80/70/90/60/50%. The only weight control is the per-competency slider; dragging prog-lang's weight thumb changed the underlying model state to 0.55 (React state read: prog-lang:0.55) and the analysis updated (coverage 68%→67%, critical path 3→5), but the node's displayed weight still reads "90%", the slider still reports aria-valuenow=90, and edge stroke widths were unchanged (2.4/2.1/2.7/1.8/1.5).

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not shown in graph and competency cannot be re-enabled
    - Actual: Clicking the Agile Methodology toggle excluded it from the metrics (coverage 75%→69%, avg 3.7→3.4), but the toggle still reports aria-checked="true", the node keeps normal styling (class "competency-node bg-card p-4 min-w-[220px]", no disabled markers) and its edge (agile→collab) is unchanged (opacity 1, no dashing). Clicking the same toggle a second time did not re-enable it: metrics stayed at 69% / 3.4. Also, toggling reverted a previously applied level edit (sys-design level 5 reverted), so toggles do not act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph
    - Actual: After clicking Apply on the HR Manager template, the header/metrics switched to HR Manager (Human Resources, coverage 71%, avg 3.5, critical path "Talent Acquisition, Employee Relations, HR Compliance") and the template card was marked Current, but the graph still rendered the previous Software Engineer competencies (nodes prog-lang, sys-design, code-review, problem-solve, collab, agile) and the Software Engineer relationships (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%). Graph and summary therefore describe two different models.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No name/description entry and no My Templates collection; custom template is never stored
    - Actual: Clicking "Save Current" in the Template Library only shows a toast "Template Saved — Current model configuration has been saved as a template." No dialog or input appears to enter a name or description (0 dialogs, no text inputs on the page). The template list still contains exactly the same 5 built-in templates, no "My Templates" section exists anywhere in the app (Models or Analytics view), and localStorage remains empty, so nothing can be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Category adjustment sliders corrupt each other, producing NaN simulation outputs
    - Actual: Setting Technical "Level Change" to +2 immediately turned the same category's "Weight Change" into "NaN%" (slider aria-valuenow="NaN"); Run Simulation then produced "Coverage NaN%" (Avg Level did update 3.5→4.3). After Reset, setting Technical "Weight Change" to +25% blanked the same category's "Level Change"; Run Simulation produced "Coverage NaN%" and "Avg Level NaN / N/A". Reset itself works correctly (metrics returned to 71% / 3.5 and all adjustments back to 0).

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted
    - Actual: On Sales Manager, setting CRM Proficiency to level 5 changed metrics to Coverage 83% / Avg Level 4.2; clicking Save showed a "Model Saved — Your changes have been saved successfully." toast, but localStorage stayed empty. Navigating back to the model list and reopening Sales Manager restored the original values (Coverage 79%, Avg Level 3.8, CRM Proficiency "Intermediate"), so the edit was not restored.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Scenario simulation can drive competency weights out of range (NaN)
    - Actual: Direct edits are bounded correctly (level buttons only offer 1–5; weight slider aria-valuemin=10 / aria-valuemax=100) and simulated levels clamp at 5 (Data Scientist Technical +2 on levels 4,4,4,3 gave Avg Level 4.3 = (5+5+5+5+3+3)/6, not the unclamped 4.8). However, a scenario weight adjustment produced NaN: after setting Technical Level Change +2 the Technical Weight Change became "NaN%" and Run Simulation yielded "Coverage NaN%", i.e. the applied competency weights were NaN and therefore outside the supported 10%–100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized — its defaults are missing and running it is a no-op
    - Actual: On a freshly loaded app, opening Sales Manager shows the Recruitment tab already active but all Category Adjustments read "Level Change 0 / Weight Change 0%". Clicking Run Simulation without switching tabs left every output unchanged (Coverage 79%, Avg Level 3.8, Critical Path 4, Risk Gaps 0). After clicking Training and returning to the same Recruitment tab, its real defaults appeared (Technical -1, Behavioral +10%, Leadership -2 / -10%, Domain -1) and Run Simulation then changed the outputs to Coverage 65%, Avg Level 3.0, Risk Gaps 2 (Team Leadership, Market Knowledge).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph controls never reflect the current value/state; each edit restarts from the original model
    - Actual: Weight drag on prog-lang changed analysis (68%→67%) but node still showed "90%" and slider aria-valuenow=90. Clicking sys-design set-level-5 changed metrics (avg 3.3→3.7) but the node still showed "Intermediate". Toggling agile excluded it from metrics but the toggle stayed aria-checked="true". Subsequent edits do not start from the current state: after sys-design was set to level 5, toggling agile produced avg 3.4 / coverage 69%, which corresponds to sys-design back at level 3; a later prog-lang level-1 click produced avg 2.8 (=17/6), i.e. agile re-enabled again. Graph Gap/Critical badges likewise never change (prog-lang stayed unbadged for Gap while the summary reported a gap on Programming Languages).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path summary and graph highlighting disagree; path includes a disabled competency and is not a valid directed path
    - Actual: After disabling Programming Languages, the summary showed "Critical Path 5 competencies: Problem Solving, System Design, Programming Languages, +2" — i.e. it still contains the disabled competency and grew from 3 to 5 — while the graph still highlighted exactly the original 3 nodes (prog-lang, sys-design, problem-solve) with "Critical" badges, so the two do not agree and the graph never recalculates. The listed order is also not a valid directed path: the only edges are prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab, so there is no edge from System Design to Programming Languages. Same divergence seen after a weight change (summary went 3→5 while graph badges stayed at 3).

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.