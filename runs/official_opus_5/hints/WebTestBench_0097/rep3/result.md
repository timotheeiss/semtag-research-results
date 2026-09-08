# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Weight change is not reflected in the graph display; edge/relationship weight visuals never update
    - Actual: Dragging the Agile Methodology weight slider changed the underlying model (coverage 66%→67%→69%, consistent with weight 50%→40%→10%), but the node kept showing "Weight 50%" and aria-valuenow=50, and all edge labels/stroke widths stayed unchanged (80%/2.4, 70%/2.1, 90%/2.7, 60%/1.8, 50%/1.5). There is also no UI at all to edit an edge's weight (clicking an edge only selects it). The same weight drag additionally reverted the competency level from 2 back to 3 (avg 3.2→3.3).

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown in the graph and a competency cannot be re-enabled
    - Actual: Clicking the "Problem Solving" toggle excluded it from calculations (avg 3.3→3.2 = 16/5, coverage 69%→65%, critical path 3→4), but the switch stayed data-state="checked"/aria-checked="true", the node kept opacity:1 with no disabled styling, and its edge problem-solve→sys-design stayed fully visible and highlighted. Clicking the same toggle a second time to re-enable produced no change at all (coverage 65%, avg 3.2, critical path 4 unchanged), so the toggle acts on stale state and the competency can never be re-enabled.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph or its relationships
    - Actual: Applying the "Data Scientist" template updated the header (title "Data Scientist", department "Analytics"), metrics (coverage 71%, avg 3.5, critical path [Machine Learning, Python Programming, Statistical Analysis]) and scenario categories (Technical 4 / Behavioral 1 / Domain 1), but the graph still renders the previous Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) with the identical 5 old relationships (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%). None of the competencies named in the critical-path summary exist in the displayed graph.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No save-as-template flow and no My Templates collection
    - Actual: Clicking "Save Current" in the Template Library produced no dialog, no name/description form, no toast and no DOM change: the template list still contains only the 5 built-in roles (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) and there is no "My Templates" section anywhere on the page. localStorage is completely empty, so nothing is persisted.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted
    - Actual: Set Code Review to level 5 (coverage 68%→74%, avg 3.3→3.7) and clicked Save; a "Model Saved — Your changes have been saved successfully." toast appeared but localStorage stayed empty. After going back to the model list and reopening Software Engineer, the model was back to its defaults: coverage 68%, avg 3.3, Code Review level "Intermediate". Nothing was restored.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized with its default adjustments
    - Actual: On first load the Recruitment tab is active but every Category Adjustment reads Level Change 0 / Weight Change 0%. Clicking Run Simulation without switching tabs changed nothing (coverage 68%, avg 3.3, critical path 3, risk gaps 0 before and after). Only after switching to Training/Promotion and back does Recruitment reveal its real defaults (Technical -1, Behavioral +10% weight, Domain -1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls show stale values after edits and a later edit does not start from the current state
    - Actual: Clicking level-4 then level-2 on "Agile Methodology" changed the summary (avg 3.3→3.5→3.2, coverage 68%→70%→66%) but the node kept displaying "Level Intermediate" with 3/5 bars filled (bg-level-intermediate on bars 1-3) the whole time. Weight display likewise stayed "50%" after the weight was actually reduced to ~10% (coverage 69%). A subsequent weight edit reset the level from 2 back to 3 (avg returned to 3.3), i.e. the edit used the stale node state rather than the current state.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph highlighting never recalculates and the critical-path summary is not a valid directed path
    - Actual: Baseline critical path = [Programming Languages, Problem Solving, System Design], but no edge connects Programming Languages to Problem Solving, so it is not a valid directed path. Disabling Problem Solving changed the summary to 4 competencies [Programming Languages, System Design, Collaboration, +1] — also not a connected path (System Design has no outgoing edge) — while the graph highlighting stayed byte-identical: the same 3 nodes (prog-lang, sys-design, problem-solve) kept border-node-critical, including the just-disabled one, and the same 3 edges (prog-lang→code-review, problem-solve→sys-design, collab→code-review) kept the animated class both before and after. Summary and graph therefore disagree.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.