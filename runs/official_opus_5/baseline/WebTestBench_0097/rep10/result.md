# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship weights cannot be edited; competency weight edits are not reflected in the graph control
    - Actual: Edges (e.g. prog-lang→sys-design 70%) expose no editing affordance: clicking/selecting an edge opens nothing and the ReactFlow instance has no onEdgeClick/edge-update handler; edge labels stayed 80/70/90/60/50% through every edit. The only weight control is the per-competency slider, and dragging Problem Solving's slider changed the underlying weight 0.9→0.45 (Coverage 65%→67%) while the node still displayed "Weight 90%" and the slider still read aria-valuenow=90.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown and the competency cannot be re-enabled
    - Actual: Toggling Programming Languages off excluded it from analysis (Coverage 65%, Avg Level 3.2, gap alert cleared), but the node kept aria-checked="true", no dimming/disabled styling, and its edges kept full opacity/colour. Clicking the switch a second time did not re-enable it (metrics stayed 65% / 3.2) because the control still reflects the stale enabled=true state. A later unrelated edit silently reverted the competency to enabled=true.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph or its relationships
    - Actual: After Apply on the Product Manager template the header (Product Manager / Product), metrics (Coverage 78%, Avg 3.8, Critical Path: Product Strategy, User Research, Communication +1), scenario category counts (technical 1, behavioral 1, leadership 1, domain 3) and the "Current" badge all switched to Product Manager, but the graph still rendered the Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the old edges prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50% (unchanged after waiting).

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No custom-template capture (no name/description prompt) and no My Templates collection
    - Actual: "Save Current" only shows a toast "Template Saved – Current model configuration has been saved as a template."; no dialog or name/description fields appear, no new entry is added to the Template Library (still the same 5 built-in templates), there is no "My Templates" section anywhere in the app, and localStorage remains empty so nothing can be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted or restored
    - Actual: In Software Engineer, Code Review was raised to level 5 (Coverage 74%, Avg 3.7) and "Save" reported the toast "Model Saved – Your changes have been saved successfully.", but nothing was written to localStorage/sessionStorage. After going back to the model list and reopening Software Engineer the model was back to its defaults (Code Review level 3, Coverage 68%, Avg 3.3), so the levels/weights/enabled states were not restored.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized with its default adjustments
    - Actual: On opening the editor the Recruitment panel showed Level Change 0 / Weight Change 0% for all four categories, and clicking Run Simulation without switching tabs only produced the toast "Simulation Applied" with every output unchanged (Coverage 67%, Avg Level 3.3, Critical Path 4, Risk Gaps 0). Only after visiting another tab and returning did Recruitment display its real defaults (technical -1, behavioral +10%, domain -1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls show stale values; Critical/Gap indicators disagree with summary; edits do not start from current state
    - Actual: After setting Programming Languages to level 2, node still rendered "Level Advanced" with 4/5 bars filled and data isGap=false while the summary reported Risk Gaps 1 naming Programming Languages. After the weight drag the node showed "Weight 90%" though the model value was 0.45. Node isCritical flags (prog-lang, sys-design, problem-solve) disagreed with the summary critical path (Programming Languages, System Design, Collaboration, +1). Because controls hold stale data, a subsequent edit reverts earlier edits (disabled + level-2 Programming Languages reverted to enabled level 4 after editing another node).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path is not a valid path and graph highlighting disagrees with the summary
    - Actual: Summary critical path listed "Programming Languages, System Design, Collaboration, +1" after a weight change, but no edge connects System Design→Collaboration (edges: prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab), so it is not a traversable path; the initial "Programming Languages, Problem Solving, System Design" is likewise not connected. Meanwhile the graph still highlighted prog-lang/sys-design/problem-solve as critical, contradicting the summary list. Counts also fluctuated (3→5→4) without corresponding model changes.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.