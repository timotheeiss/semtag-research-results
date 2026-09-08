# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship (edge) weight cannot be edited; competency weight control does not update its displayed value
    - Actual: Edges expose no editing affordance: single click only selects the edge, double-click and right-click open no editor/menu, no dialog or input appears, and all 5 edge weight labels (80/70/90/60/50%) and stroke-widths stayed fixed throughout the session. The only weight control is the per-node weight slider; clicking its track did change the underlying model (Coverage jumped 63%→69% after clicking System Design's weight track) but the slider's aria-valuenow and "Weight 80%" label remained unchanged, so the displayed weight never updates.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state never shown in graph; repeated toggles do not act on current state (re-enable impossible)
    - Actual: Clicking the Agile Methodology node switch did exclude it from calculations (Avg Level 3.0→3.4, consistent with 5 of 6 competencies), but the node kept aria-checked="true", full opacity, unchanged styling, and its edge (agile→collab) kept normal stroke/opacity — no disabled indication. Clicking the same switch a second time to re-enable produced no change at all (Coverage stayed 69%, Avg Level stayed 3.4), so the toggle operates on stale state and the competency cannot be re-enabled.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph; graph shows the previous model's competencies and relationships
    - Actual: After applying the "Data Scientist" template, the header (Data Scientist / Analytics), summary metrics (Coverage 71%, Avg 3.5, Critical Path: Machine Learning, Python Programming, Statistical Analysis) and scenario category counts (Technical 4 / Behavioral 1 / Domain 1) all switched, but the graph still rendered the Software Engineer nodes (prog-lang, sys-design, code-review, problem-solve, collab, agile) with the old edges (80/70/90/60/50%). Graph and summary describe different models.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: No custom-template capture or My Templates collection; "Save Current" is a no-op toast
    - Actual: Clicking "Save Current" shows only a toast "Template Saved – Current model configuration has been saved as a template." with no dialog or field for a name/description (0 input elements on the page). The Template Library still contains exactly the 5 built-in templates with no new entry, no "My Templates" section exists anywhere in the page text, and localStorage stays empty (0 keys), so nothing can be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Edited model is not persisted; Save does not store state and reopening resets to defaults
    - Actual: Model was edited via simulation (Coverage 85%, Avg Level 4.2), then the header "Save" button was clicked. localStorage remained completely empty (0 keys). Navigating back to the list and reopening Software Engineer restored the original defaults: Coverage 68%, Avg Level 3.3, and every node back at its original level/weight (prog-lang Advanced/90%, sys-design Intermediate/80%, etc.). No levels, weights or enabled states were restored.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Scenario simulation produces invalid (NaN) competency levels; weight/level values are not kept in range
    - Actual: Direct edits respect ranges (level bars 1-5, weight slider min 10 / max 100, and a +2 level simulation clamped at 5 → Avg 4.2). But dragging a category's "Weight Change" to its minimum (-30%) corrupts that category's Level Change to aria-valuenow="NaN" (label renders blank) — reproducible after Reset. Running the simulation then writes NaN into the model: summary shows "Coverage NaN%", "Avg Level NaN / N/A"; repeated runs kept NaN. So simulated levels fall outside 1-5 and weights are not validated.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario is not initialized: defaults show as zero and running it changes nothing
    - Actual: On opening a model, the pre-selected Recruitment tab showed all category adjustments as Level Change 0 / Weight Change 0%. Clicking "Run Simulation" without switching tabs only produced a "Simulation Applied" toast while Coverage stayed 68%, Avg Level 3.3, Critical Path and Risk Gaps unchanged. After switching to Training and back, the Recruitment tab revealed its real defaults (Technical -1, Behavioral +10% weight, Domain -1), proving the initial tab state was never populated.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls never re-render after an edit; displayed value/state diverges from model
    - Actual: After clicking Code Review's level-5 bar the summary updated (Coverage 68%→74%, Avg 3.3→3.7) but the node still displayed "Level Intermediate" with 3/5 bars filled. Setting level 1 next moved Avg to 3.0 (edit did start from current state) yet the node still showed "Intermediate". A weight-track click changed coverage while the node still read "Weight 80%", and after disabling Agile the switch still read aria-checked="true"; a subsequent toggle then had no effect at all, so later edits do NOT start from the current displayed state. Gap/Critical indicators on nodes therefore cannot agree with the summary.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path includes a disabled competency, is not a valid directed path, and graph highlighting disagrees with the summary
    - Actual: After disabling Programming Languages, the summary Critical Path changed to "5 competencies: Problem Solving, System Design, Programming Languages, +2" — it still contains the disabled competency, and the listed order traverses sys-design→prog-lang although the only edge between them is prog-lang→sys-design. Meanwhile the graph still highlighted exactly the pre-change trio (border-node-critical on prog-lang, sys-design, problem-solve; animated edges unchanged), so highlighting (3 nodes) contradicts the summary (5). Critical path also changed spontaneously earlier (to "Code Review, Programming Languages, Problem Solving" and back) with no model edit.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.