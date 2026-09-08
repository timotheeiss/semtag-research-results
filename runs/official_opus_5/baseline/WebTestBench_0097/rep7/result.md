# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI exists to edit relationship (edge) weights
    - Actual: Edge weights (80%/70%/90%/60%/50%) are rendered as read-only labels. Single-click only adds the "selected" class; double-click opens no dialog/editor and no weight control appears anywhere. The detail view contains no relationship/edge list or editor (page text contains no "relationship" string) — only per-competency (node) level and weight controls. Edge labels remained 80%,70%,90%,60%,50% after all interaction attempts, so relationship weight cannot be changed at all.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown on the node/edges, and a competency cannot be re-enabled
    - Actual: Clicking Code Review's switch excluded it from calculations (Coverage 68%→69%, Avg Level 3.3→3.4), but the node kept its normal styling and its switch still reported data-state="checked"; its edges (prog-lang→code-review, collab→code-review) kept full opacity and unchanged stroke, so no disabled state is visible anywhere. Clicking the same switch a second time to re-enable had no effect at all — Coverage/Avg Level stayed 69%/3.4 — so repeated toggles do not act on the current state and the competency can never be re-enabled.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph — nodes and relationships remain from the previous model
    - Actual: Applying the "Product Manager" template updated the header (Product Manager / Product / 6 competencies), metrics (Coverage 78%, Avg 3.8, Critical Path 4: Product Strategy/User Research/Communication +1) and scenario category counts (Technical 1, Behavioral 1, Leadership 1, Domain 3), and the underlying model became the PM competencies (Product Strategy, Stakeholder Management, Data Analysis, Communication, Prioritization, User Research) with PM edges. However the rendered graph still showed the old Software Engineer nodes — Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology — and the old edges (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%). Graph and summary therefore represent different models.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" custom-template feature is non-functional and no My Templates collection exists
    - Actual: Clicking "Save Current" in the Template Library (twice, verified the button received focus) produced no dialog or form for a template name/description ([role=dialog]/[data-state=open] count 0, zero input/textarea elements on the page), no toast, and no new entry — the library still lists only the same 5 built-in templates. The string "My Templates" does not appear anywhere in the app, so a saved configuration can never be retrieved.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted or restored
    - Actual: Edited Software Engineer (Code Review level 3→5; Coverage 68%→74%, Avg Level 3.3→3.7) and clicked Save — a "Model Saved / Your changes have been saved successfully." toast appeared, but localStorage and sessionStorage were both completely empty (0 keys). Navigating back to the model list and reopening Software Engineer showed the original values restored: code-review L3, Coverage 68%, Avg Level 3.3. The edit is lost on leaving the model, and nothing is stored to survive a reload.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario is not initialized with its default adjustments and running it has no effect
    - Actual: On a fresh load with Recruitment active by default, all Category Adjustments show Level Change 0 / Weight Change 0% for Technical, Behavioral, Leadership and Domain. Recruitment's real defaults (seen only after switching away and back) are Technical Level -1, Behavioral Weight +10%, Domain Level -1. Clicking "Run Simulation" without switching tabs left every output unchanged: Coverage 68%, Avg Level 3.3, Critical Path 3 (Programming Languages/Problem Solving/System Design), Risk Gaps 0.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls show stale values and a subsequent edit discards the previous edit instead of starting from current state
    - Actual: Set Programming Languages level Advanced→Beginner: summary updated (Coverage 68%→56%, Risk Gaps 0→1 naming Programming Languages) but the graph node still displayed "Level Advanced" with 4/5 level bars filled and showed no Gap indicator (class only "border-node-critical"), contradicting the summary. Then setting System Design to Beginner reverted the earlier edit: Coverage became 61% and Risk Gaps returned to 1 naming only "System Design", with Programming Languages back at Advanced. Each edit is applied to the original model snapshot, so the graph control neither reflects the current value/Gap state nor builds on it.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path includes disabled competencies, is not a valid directed path, and graph highlighting does not agree with or recalculate with the summary
    - Actual: After disabling Programming Languages the summary Critical Path changed from 3 to 5 competencies = [problem-solve, sys-design, prog-lang, collab, agile], which still contains the disabled prog-lang (enabled=false). The sequence is not a valid directed path: the model's only edges are prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab, so there is no sys-design→prog-lang, prog-lang→collab or collab→agile connection. Meanwhile the graph highlighting did not recalculate at all — exactly the same 3 nodes (prog-lang, sys-design, problem-solve) keep class "border-node-critical" and the same 2 edges keep the red critical stroke rgb(220,40,40) — so graph and summary disagree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.