# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to edit relationship weights; competency weight edits are not reflected in the graph UI
    - Actual: Edges (e.g. "Edge from agile to collab", 50%) can only be selected — single click, double click and right click produce no editor, dialog, menu or input, and no "relationship" editing UI exists anywhere in the page. The only weight controls are the per-node competency weight sliders; after clicking the Programming Languages weight slider track the underlying model state changed to weight 0.55 (coverage moved 68%→67%) but the node still displayed "Weight 90%" and its slider stayed at aria-valuenow=90, so no displayed weight or edge emphasis updated.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state not shown in graph and competency cannot be re-enabled
    - Actual: Toggling "Problem Solving" off updated the analysis (Coverage 60→65%, Critical Path list dropped Problem Solving, Risk Gaps 0), but the node kept aria-checked="true", normal (non-dimmed) card styling and its edge to System Design stayed fully styled/animated — no disabled indication. Clicking the switch a second time did not re-enable it: model state stayed enabled=false and the summary was unchanged (still 65%), so repeated toggles do not act on the current state. The toggle also silently reset the competency's edited level from 2 back to 4.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Template application does not replace the competency graph
    - Actual: Applying the "Product Manager" template updated the header (Product Manager / Product), summary metrics (Coverage 78%, Critical Path: Product Strategy, User Research, Communication), scenario category counts (Technical 1, Behavioral 1, Leadership 1, Domain 3) and the "Current" badge, but the graph still rendered the previous Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the identical old relationships (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%).

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No custom-template saving: "Save Current" is inert and no My Templates collection exists
    - Actual: Clicking "Save Current" in the Template Library opened no dialog/form for a name or description, showed no toast, added no template to the list (still only the 5 built-in role templates), and wrote nothing to localStorage/sessionStorage (both empty). The string "My Templates" does not exist anywhere in the app.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Choosing a category adjustment corrupts the paired adjustment and yields NaN simulation outputs
    - Actual: Running a scenario with its untouched defaults works (Coverage 65%→49%, Avg 3.2→2.4, Critical Path/Risk Gaps updated) and Reset correctly restores the original model (Coverage back to 68%, all levels/weights/enabled restored, sliders back to 0). But as soon as the user chooses an adjustment, the sibling slider for that category becomes NaN (Technical Level +1 → Technical Weight "NaN%"), and Run Simulation writes NaN into the model: Coverage "NaN%", Avg Level "NaN / N/A", competency weights and levels NaN.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved edits are not persisted or restored
    - Actual: Set Code Review to level 5 (Coverage 68%→74%) and clicked Save — toast "Model Saved: Your changes have been saved successfully." appeared, but nothing was written to localStorage/sessionStorage (both empty). Navigating back to the model list and reopening Software Engineer showed the original model again (code-review level 3, Coverage 68%), so the saved levels/weights/enabled states were lost.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Simulation produces NaN levels/weights, outside the 1-5 and 10%-100% ranges
    - Actual: Changing one category adjustment corrupts its sibling to NaN (e.g. Technical Level Change +1 → Technical "Weight Change NaN%"; Behavioral Weight Change +5% → Behavioral Level Change blank/NaN). Running the simulation then wrote invalid values into the model: weights became NaN for prog-lang/sys-design/code-review (Coverage rendered "NaN%") and levels became NaN for problem-solve/collab (Avg Level "NaN", "N/A"). Repeated default-scenario runs did clamp correctly (levels floor 1, weights ceiling 1.0), but the NaN case breaks the 1–5 / 10–100% constraint.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario is not initialized with its default adjustments
    - Actual: On first opening the detail view the pre-selected Recruitment tab showed every category at "Level Change 0 / Weight Change 0%", and clicking Run Simulation changed nothing (model state and Coverage 65%, Avg 3.2, Critical Path 4, Risk Gaps 0 all identical). Only after switching to another tab and back did Recruitment reveal its real defaults (Technical -1, Behavioral +10%, Domain -1), which then did change the outputs (Coverage 65%→49%, Avg 3.2→2.4, Risk Gaps 0→1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls are stale — they never show the edited value/state
    - Actual: Clicking a level bar on "Problem Solving" changed the model (state level 4→2; summary Coverage 68→60%, Avg 3.3→3.0, Risk Gaps 0→1 "Problem Solving"), but the node card still displayed "Level Advanced" with 4 advanced bars filled and no Gap indicator, while the summary lists it as a gap. Likewise a weight slider change (model weight 0.9→0.55) left the node showing "Weight 90%" (slider aria-valuenow=90), and that weight change was silently reverted to 0.9 on the next edit, so a subsequent edit does not start from the current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path is not a valid path and graph highlighting disagrees with the summary
    - Actual: After disabling Problem Solving the summary critical path became 4 competencies "Programming Languages, System Design, Collaboration, +1", but the only relationships are prog-lang→code-review, prog-lang→sys-design, problem-solve→sys-design, collab→code-review, agile→collab — no 4-node path exists and Programming Languages→System Design→Collaboration is not connected. Graph highlighting still marked prog-lang, sys-design and the DISABLED problem-solve as isCritical (Collaboration not highlighted), contradicting the summary. The same model state also produced different outputs at different times (68%/3-node path vs 66-67%/5-node path with identical levels, weights and enabled flags).

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.