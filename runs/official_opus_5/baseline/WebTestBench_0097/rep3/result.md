# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI to edit relationship (edge) weights
    - Actual: Single-click, double-click and right-click on graph edges / edge weight labels only mark the edge as selected — no weight editor, dialog, popover or input appears anywhere on the detail page (page text never contains "relationship"; no inputs/sliders exist outside the per-competency Level/Weight controls). Edge weights stay fixed at 80/70/90/60/50% and cannot be changed, so the label, stroke emphasis and dependent analysis can never update.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not reflected in graph; competency cannot be re-enabled
    - Actual: Toggling the "Agile Methodology" switch off did update the analysis (Coverage 68%→69%, Avg Level 3.3→3.4, i.e. agile excluded), but the node kept aria-checked="true", full opacity and unchanged edge styling — no visible disabled state on node or its edge. Clicking the same switch a second time did not re-enable it (Coverage stayed 69%, aria-checked still "true"), so repeated toggles do not act on the current state and the competency is stuck disabled.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph
    - Actual: After clicking Apply on the "Data Scientist" template the header/metrics/scenario panel switched to Data Scientist (Analytics, Coverage 71%, Critical Path: Machine Learning, Python Programming, Statistical Analysis; Technical 4 / Behavioral 1 competencies), but the graph still renders the previous Software Engineer model: nodes Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology and edges prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab with the old 80/70/90/60/50% weights. Graph and summary therefore represent different models.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No custom-template saving: no name/description prompt, no My Templates collection, nothing persisted
    - Actual: Clicking "Save Current" in the Template Library shows only a toast ("Template Saved — Current model configuration has been saved as a template."); no dialog for a name or description appears. The Template Library still lists only the 5 built-in role templates, the page contains no "My Templates" section, and localStorage stays empty, so the saved configuration cannot be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Edited model is not persisted; Save does not restore state on reopening
    - Actual: Set Code Review level to 5 (Coverage 68%→74%, Avg 3.3→3.7) and clicked the header Save button. Navigating back to the model list and reopening Software Engineer shows the original values again (Coverage 68%, Avg Level 3.3, Code Review level Intermediate). Both localStorage and sessionStorage remain empty, so nothing can survive a reload either.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario tab is not initialized with its default adjustments
    - Actual: On first opening the detail page the pre-selected Recruitment tab shows Level Change 0 / Weight Change 0% for every category, and clicking Run Simulation only shows the "Simulation Applied" toast while Coverage stays 68%, Avg Level 3.3, Critical Path 3, Risk Gaps 0 (no output change). Recruitment's real defaults (Technical -1, Behavioral Weight +10%, Domain -1) only appear after switching to another tab and back.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never re-render; they keep showing stale values and edits start from stale state
    - Actual: After clicking Code Review's 5th level bar the summary updated (Coverage 74%, Avg Level 3.7) but the node still displays "Level Intermediate" with only 3 filled bars. Same for weight (slider drag lowered prog-lang weight 90→50 per coverage 68%→67% while node kept showing 90%) and for enable state (agile excluded from analysis while switch stayed checked). Worse, a subsequent edit starts from the stale node data: after disabling agile (Coverage 69%, Avg 3.4), the next level edit silently re-enabled agile (Avg Level 3.7 = average over all 6 competencies incl. agile).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path is not a valid path, includes disabled competencies, and has no graph highlighting
    - Actual: After disabling Programming Languages the summary critical path became ["problem-solve","sys-design","prog-lang","collab","agile"] (read from component props) — it still contains the disabled prog-lang, and the sequence is not traversable: only edges are prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab, so sys-design→prog-lang, prog-lang→collab and collab→agile do not exist. The graph provides no critical-path highlighting at all: node border colors stay category-based and the set of "animated"/colored edges (e1,e3,e4) is identical to the initial state and never changed when the critical path went from 3 to 5 competencies, so summary and graph cannot agree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.