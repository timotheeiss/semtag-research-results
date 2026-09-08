# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No way to change a relationship (edge) weight
    - Actual: The 6 competency nodes expose only competency-level and competency-weight controls (only 6 sliders exist in the graph, all `competency.weight`). Edges have no data-semtag ids, no editable label, and clicking/double-clicking edge rf__edge-e1 opens no editor or dialog (dialog count 0 before and after, page text unchanged). Edge weight labels stayed fixed at 80/70/90/60/50% with stroke widths 2.4/2.1/2.7/1.8/1.5 through every competency-weight edit, so relationship weight cannot be changed nor its emphasis updated.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is never shown and a competency cannot be re-enabled
    - Actual: Disabling Problem Solving correctly excluded it from analysis (Coverage 56%→65%, Avg 2.8→3.2, Risk Gaps 1→0, critical path changed), but the node kept identical styling/text ("Level Advanced | Weight 90%", class "competency-node bg-card ... border-node-critical") and its edge problem-solve→sys-design kept full opacity and colour, so no disabled state is shown. The toggle itself stayed data-state="checked", and clicking it a second time changed nothing (Coverage still 65%, toggle still "checked"), so the competency can never be re-enabled and repeated toggles do not act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph or its relationships
    - Actual: After applying the Data Scientist template, the header (Data Scientist / Analytics), metrics (Coverage 71%, Avg 3.5, critical path Machine Learning / Python Programming / Statistical Analysis) and scenario category counts (Technical 4, Behavioral 1, Domain 1) all switched to the new template, but the graph still renders the six Software Engineer nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the five Software Engineer edges (prog-lang→code-review 80%, prog-lang→sys-design 70%, problem-solve→sys-design 90%, collab→code-review 60%, agile→collab 50%), so the model is not represented consistently.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save-as-custom-template is not implemented
    - Actual: Clicking "Save Current" (templates.save-current, action save-as-template) opens no dialog and no name/description form (0 elements with role=dialog), shows no toast, adds nothing to localStorage (Object.keys(localStorage) is empty) and creates no "My Templates" collection — the template list still contains only the 5 built-in role templates, so a custom template can neither be saved nor retrieved later.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Choosing a category level adjustment corrupts that category's weight adjustment to NaN and breaks the simulation output
    - Actual: Setting Technical "Level Change" to +2 simultaneously changed Technical "Weight Change" from 0% to "NaN%". Running the simulation then produced Coverage "NaN%" with the progress bar collapsed to translateX(-100%) (Avg Level 4.2, Critical Path 3, Risk Gaps 0). Running a scenario's own default adjustments works (Recruitment: Coverage 68%→56%, Avg 3.3→2.7, Risk Gaps 0→1 naming System Design) and Reset does restore 68% / 3.3 / 3 / 0 with all adjustments back to 0, but user-chosen category adjustments are not usable.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits are not persisted or restored
    - Actual: Set System Design to level 5 (Coverage 68%→75%, Avg 3.3→3.7) and clicked Save, which showed a "Model Saved — Your changes have been saved successfully." toast, yet nothing was written to storage (localStorage and sessionStorage both empty). Navigating back to the model list and reopening Software Engineer showed the original Coverage 68% / Avg 3.3, so the levels, weights and enabled states were not restored.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Simulation can drive competency weights outside the 10%-100% range (to NaN)
    - Actual: Levels are correctly bounded (only set-level-1..5 buttons exist; Technical +2 raised level 3 and 4 competencies to a max of 5, Avg 4.2) and the direct weight slider is bounded aria-valuemin=10 / aria-valuemax=100. But choosing Technical Level Change +2 set that category's Weight Change to "NaN%", and running the simulation produced Coverage "NaN%" — i.e. competency weights became NaN, outside the supported 10%-100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized
    - Actual: On first load the Recruitment tab is active but every Category Adjustment reads Level Change 0 / Weight Change 0%, and clicking Run Simulation left all outputs unchanged (Coverage 68%, Avg 3.3, Critical Path 3, Risk Gaps 0). Only after switching to another tab and back does Recruitment reveal its real defaults (Technical -1, Behavioral +10% weight, Domain -1).

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Weight slider does not display the current value; next edit restarts from the stale original value
    - Actual: Focused sys-design weight slider (80) and pressed Home: summary recalculated (Coverage 68%→70%, Critical Path 3→4) proving the model weight became 10, but the node card still showed "Weight 80%" and the thumb still reported aria-valuenow=80. Same on prog-lang: Home→Coverage 66% while card stayed "90%"; a following ArrowRight then moved from the stale 90 (Coverage back to 68%) instead of from 10→11, so the subsequent edit did not start from the current state.


## Content
- [ ] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.
  - Bug Report:
    - Issue: Panning the graph view is not possible
    - Actual: Nodes, arrowed directed edges, visible weight labels (80/70/90/60/50%), zoom in/out/fit-view controls and wheel-zoom (scale 0.88→1.17) and node rearranging all work. But dragging the empty canvas never pans: viewport transform stayed "translate(0px, 37.6552px) scale(0.882922)" after a real mouse drag across the empty pane, after a synthetic pointer drag, and after focusing the pane and pressing ArrowRight. Only node positions changed when a node was under the cursor.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph critical-path highlighting does not recalculate and contradicts the summary; reported path is not a connected path
    - Actual: Summary critical path changed from 3 (Programming Languages, Problem Solving, System Design) to 4 (Programming Languages, System Design, Collaboration, +1) after disabling Problem Solving, and to 4/5 after weight edits, but graph highlighting never changed: only prog-lang, sys-design and problem-solve keep class "border-node-critical" — including the now-disabled Problem Solving — while Collaboration, which the summary lists in the path, has no highlight. The listed set is also not a connected path: with edges prog-lang→sys-design, prog-lang→code-review, problem-solve→sys-design, collab→code-review, agile→collab, there is no edge from System Design to Collaboration.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.