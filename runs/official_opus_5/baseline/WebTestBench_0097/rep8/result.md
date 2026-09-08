# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI to change relationship (edge) weight
    - Actual: Single-click and double-click on edges (e.g. "Edge from prog-lang to code-review", 80%) only select the edge — no inline input, popover, or dialog appears (0 dialogs, 0 inputs inside the graph). The graph exposes no edge-weight control anywhere, and edge weights stayed 80/70/90/60/50% throughout. ReactFlow is rendered without any edge-update handler and edges come from a frozen useEdgesState(initialEdges), so relationship weights can never be changed or re-rendered.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not shown and competency cannot be re-enabled
    - Actual: Clicking the Agile Methodology switch set enabled=false in the model (Coverage 68%→69%), but the node kept aria-checked="true", unchanged styling ("competency-node bg-card p-4"), and its edge agile→collab was unchanged — no disabled state is shown for node or relationships. Clicking the switch a second time did NOT re-enable it (still enabled=false, Coverage still 69%). Graph nodes are created once via useNodesState(initialNodes) and never re-synced, so their handlers close over the original competency array; a later edit (level click) silently reverted agile to enabled=true.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Template applied inconsistently — graph keeps the previous model
    - Actual: Applying the "Product Manager" template updated the header (Product Manager / Product), metrics (Coverage 78%, Avg 3.8, critical path Product Strategy/User Research/Communication) and scenario category counts (technical 1, behavioral 1, leadership 1, domain 3), but the competency graph still displayed the old Software Engineer nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the old relationships (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%).

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saving a custom template is not implemented
    - Actual: Clicking "Save Current" showed only a toast ("Template Saved — Current model configuration has been saved as a template."). No dialog or form appeared to enter a name/description (0 dialogs), no "My Templates" section exists anywhere in the page, the Template Library still lists only the 5 built-in role templates, and localStorage is empty (no keys), so nothing is persisted or retrievable.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted or restored
    - Actual: Set Collaboration level 3→5 (Coverage 68%→75%), clicked Save (toast "Model Saved — Your changes have been saved successfully."), went back to the model list and reopened Software Engineer: competencies were back to prog-lang L4 W90, sys-design L3 W80, problem-solve L4 W90, collab L3 W70, code-review L3 W60, agile L3 W50 and Coverage 68%. localStorage and sessionStorage are both empty, so nothing survives leaving the model or a reload.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario is not initialized with its default adjustments
    - Actual: On opening the editor the Recruitment tab is selected but every category shows "Level Change 0 / Weight Change 0%" instead of its defaults (Recruitment defaults are technical −1 level, behavioral +10% weight, leadership −2/−10%, domain −1). Clicking "Run Simulation" without switching tabs showed the "Simulation Applied" toast but changed nothing: competencies stayed prog-lang L4 W0.9, sys-design L3 W0.8, problem-solve L4 W0.9, collab L3 W0.7, code-review L3 W0.6, agile L3 W0.5 and Coverage stayed 68%. Adjustments state is only populated by a tab-change handler.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph controls show stale values; edits do not start from current state
    - Actual: After setting Agile Methodology level to 5 (summary Avg Level 3.3→3.7, Coverage 68%→73%), the node still displayed "Level Intermediate" with 3/5 bars filled and slider aria-valuenow=50 unchanged; node data via React fiber remained {level:3, weight:0.5, enabled:true}. Gap/Critical flags on nodes (isCritical/isGap) are also frozen at their initial values. A subsequent edit resets prior edits: with Agile disabled, clicking a level bar restored enabled=true, i.e. edits are applied to the original snapshot, not the current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph highlighting never recalculates and critical path includes a disabled competency
    - Actual: After disabling Programming Languages, the Critical Path summary changed to 5 competencies listing "Problem Solving, System Design, Programming Languages, +2" — it still includes the disabled Programming Languages, so it is not a valid path through enabled competencies. Meanwhile the graph highlighting did not recalculate at all: node isCritical flags stayed at the initial prog-lang/sys-design/problem-solve set and edge stroke colours were unchanged (rgb(220,40,40) still on the original two edges), so summary and graph disagree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.