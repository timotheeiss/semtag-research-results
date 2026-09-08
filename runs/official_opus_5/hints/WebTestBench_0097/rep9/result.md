# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to edit a competency relationship's weight; relationship weights are read-only and never update
    - Actual: The detail editor exposes no control for relationship/edge weights: no edge is clickable for editing (clicking edge e1 opened nothing), the ReactFlow instance wires only onNodesChange/onEdgesChange (no onEdgeClick/onEdgeUpdate/edgesUpdatable), and no semantic element or DOM control matches edge/relationship/link/connection. The 5 edge weight labels (80%, 70%, 90%, 60%, 50%) stayed fixed through every interaction, including competency-weight changes. Only per-competency weight sliders exist, which are a different quantity.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown on the node/edges, and a disabled competency cannot be re-enabled (repeated toggles do not act on current state)
    - Actual: Disabling "Programming Languages" correctly excluded it from analysis (coverage 66%→65%, avg 3.3→3.2, gap alert cleared), but the UI showed no disabled state: the toggle kept data-state="checked", the node wrapper kept opacity 1 and unchanged classes (competency-node ... border-node-critical), and all 5 edges kept opacity 1 with unchanged stroke/dash. Clicking the same toggle a second time to re-enable had no effect — coverage stayed 65%, avg 3.2, gaps 0, toggle still "checked" — so the competency remained disabled and could not be restored.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph; the graph keeps showing the previous model's competencies and relationships
    - Actual: Applying the "HR Manager" template updated the title to "HR Manager", department "Human Resources", metrics (coverage 71%, avg 3.5, critical 3), the "Current" badge, and the scenario category adjustments (Technical 0 / Behavioral 1 / Leadership 2 / Domain 3). However the graph still rendered the Software Engineer nodes — Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology (node ids prog-lang, sys-design, code-review, problem-solve, collab, agile) — with the unchanged edge weights 80%, 70%, 90%, 60%, 50%. Underlying state confirms the model is HR Manager with competencies talent-acq, emp-relations, hr-compliance, leadership, perf-mgmt, org-dev, so the graph contradicts the applied template.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saving the current model as a custom template is not implemented; no name/description prompt and no My Templates collection
    - Actual: After editing the model (Collaboration → level 5, coverage 75%), clicking "Save Current" (templates.save-current) produced no visible response: no dialog or alertdialog element, no name/description inputs, no toast/status message, and no new semantic elements. The template list still contained only the 5 built-in role templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager). The text "My Templates" does not appear anywhere in the page, and localStorage remained completely empty, so there is no custom template to retrieve after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted; leaving and reopening the model discards all changes despite a "Model Saved" confirmation
    - Actual: Edited Collaboration to level 5 (coverage rose 68%→75%, avg 3.3→3.7) and clicked Save, which showed the toast "Model Saved — Your changes have been saved successfully." However nothing was written to storage (localStorage and sessionStorage both empty). After clicking back to the model list and reopening Software Engineer, the model was fully reverted to defaults: collab back to L3 (state prog-lang L4 W0.9, sys-design L3 W0.8, problem-solve L4 W0.9, collab L3 W0.7, code-review L3 W0.6, agile L3 W0.5, all enabled) with coverage 68% and avg 3.3. Since nothing is persisted, a page reload cannot restore the edits either.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: The initially selected Recruitment scenario is not initialized with its default adjustments, so running it without first switching tabs does nothing
    - Actual: On a fresh load of the Software Engineer model, the pre-selected "Recruitment Simulation" tab showed every category adjustment as Level Change 0 / Weight Change 0% (Technical, Behavioral, Leadership, Domain). Clicking Run Simulation without switching tabs left the model completely unchanged: competencies stayed prog-lang L4 W0.9, sys-design L3 W0.8, problem-solve L4 W0.9, collab L3 W0.7, code-review L3 W0.6, agile L3 W0.5 and outputs stayed coverage 68%, avg 3.3, gaps 0. Recruitment's real defaults only appear after navigating to another tab and back (Behavioral +10% weight, Leadership -2 level/-10% weight, Domain -1 level), and only then does running it change outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never re-render: they display stale level, weight and enabled state, disagree with the summary analysis, and subsequent edits do not start from the current state
    - Actual: After setting Programming Languages to level 2 (summary avg fell 3.3→3.0, gaps 0→1) the node still displayed "Level: Advanced" and showed no Gap indicator even though the summary alert named it as the gap competency. After dragging its weight slider (coverage 68%→67%→66%, matching model weights 0.55 then ~0.15) the node still displayed "Weight 90%" with aria-valuenow=90 and the thumb pinned at x=749 (the 90% position), so the next drag started from the stale 90% position rather than the current value. After disabling it, the toggle still read data-state="checked", and clicking it again failed to re-enable (coverage remained 65%).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path summary and graph highlighting disagree after a weight change; graph highlighting does not recalculate, and the listed path is not a valid path through the edges
    - Actual: Before the change: summary "3 competencies: Programming Languages, Problem Solving, System Design" and 3 nodes highlighted (border-node-critical) — consistent. After lowering Programming Languages' weight 0.9→0.55, the summary immediately became "5 competencies: Problem Solving, System Design, Programming Languages, +2" but the graph still highlighted only the same 3 nodes (prog-lang, sys-design, problem-solve), so summary (5) and highlighting (3) disagree. The listed sequence is also not a valid directed path: edges are prog-lang→sys-design and problem-solve→sys-design, with no edge from Programming Languages to Problem Solving.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.