# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI control exists to edit a relationship (edge) weight
    - Actual: Inspected CompetencyGraph.tsx source: edges have no onEdgeClick/edge-edit handler, only the default React Flow onEdgesChange (selection). Clicking an edge (e.g. "Edge from prog-lang to code-review") only applies the default "selected" CSS class; there is no dialog, popover, or inline control to change its weight. The edge label (e.g. "80%") never changes as a result of any click/drag interaction on the edge itself.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable switch on competency node does not change state
    - Actual: Clicked the enabled/disabled Switch on the "Agile Methodology" node (trusted click, correct ref). Before and after: switch data-state stayed "checked", aria-checked stayed "true", node opacity stayed 1 (should become opacity-50/dimmed per source className logic when disabled), and the top summary panel's Coverage (69%) and Avg Level (3.4) values were unchanged, indicating the toggle produced no effect anywhere in the app.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates header/summary metrics but not the competency graph itself
    - Actual: Clicked "Apply" on the "Product Manager" template. The page header correctly changed to "Product Manager" / "Product" / "6 competencies", and summary metrics updated (Coverage 78%, Avg Level 3.8, Critical Path 4: Product Strategy/User Research/Communication/+1). However, the graph visualization still rendered the OLD Software Engineer competency nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and old edge weights (80/70/90/60/50%) instead of the Product Manager competencies referenced by the updated Critical Path list — the graph never re-synced with the newly applied model.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" claims success but does not persist or create an observable new template
    - Actual: Clicked "Save Current" in the Template Library panel. A toast "Template Saved - Current model configuration has been saved as a template." appeared, but the Template Library list still shows the exact same 5 templates as before (Software Engineer [Current], Product Manager, HR Manager, Data Scientist, Sales Manager) with no new/updated entry. Checking browser localStorage immediately after confirmed it is completely empty (no keys at all), so nothing was actually persisted anywhere despite the success toast.

- [ ] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.
  - Bug Report:
    - Issue: Running a scenario simulation corrupts summary metrics (NaN%)
    - Actual: In the Recruitment tab, increased "Level Change" for the "technical" category from 0 to +2 via the slider, then clicked "Run Simulation". A toast "Simulation Applied - Competency levels and weights have been adjusted based on the scenario." appeared, and Avg Level changed from 3.3 to 4.2 (so some underlying data did update), but the Coverage metric became "NaN%" instead of a valid percentage, and the technical category's own "Weight Change" slider (which was never touched) also started displaying "NaN%" instead of "0%". This is a clear functional defect.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: No changes persist across a page reload
    - Actual: Clicked the "Save" button on the Software Engineer detail page, then inspected localStorage/sessionStorage immediately after (both were completely empty, no keys), and reloaded the app via full navigation. After reload, the Models list is unchanged and all 5 job models are back to their original defaults. Since the app writes to no browser storage (localStorage/sessionStorage empty) and there is no backend call observed, any in-session changes (template applies, simulations, saves) are lost on reload — there is no working persistence mechanism at all.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Competency's own Level and Weight controls on the graph node do not change their values
    - Actual: Tested Level indicator buttons and Weight slider on two different nodes ("Programming Languages" weight 90%/level Advanced, "Code Review" weight 60%/level Intermediate, "Agile Methodology" weight 50%/level Intermediate) using trusted clicks (via snapshot refs and CSS selectors), trusted drag on the slider thumb, and trusted keyboard ArrowLeft/ArrowRight after confirmed DOM focus on the slider thumb. In every case the displayed Level and Weight text remained completely unchanged (e.g. Programming Languages stayed "Advanced"/"90%" after clicking the first Level dot which should set Level to 1/Novice).


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Level (1-5) / Weight (10-100%) constraints not reliably enforced
    - Actual: The per-node Level and Weight controls on the graph (intended range 1-5 levels, 10-100% weight per source code Slider min=10/max=100/step=5) cannot be exercised at all since they are non-functional (see FT-11), so boundary enforcement cannot be verified there. Additionally, running a scenario simulation with a +2 Level Change on the "technical" category caused the Coverage metric to become "NaN%", strongly suggesting a competency's level/weight was pushed out of its valid numeric range (e.g. above 5 or resulting in a divide-by-zero/undefined) without being clamped, rather than being capped at the documented maximum.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage/critical-path/graph do not correctly recalculate in real time
    - Actual: After running a scenario simulation that changed Avg Level (3.3->4.2), the Coverage metric broke to "NaN%" rather than recalculating to a valid number, and the graph nodes (e.g. "Programming Languages" still showing Level "Advanced" / Weight "90%") did not visually update to reflect the new underlying values at all, despite the Avg Level summary card showing a change. Critical Path list also failed to update contents (same 3 competencies, only DOM order changed) even though category-wide level changes should plausibly promote/demote critical-path membership.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Risk-gap detection does not react to significant competency level drops
    - Actual: Set the "technical" category Level Change slider to its minimum (-2) in the Recruitment scenario tab and clicked "Run Simulation". Avg Level dropped substantially (3.3 -> 2.3, "Beginner"), confirming the level decrease was applied to the underlying model, yet the "Risk Gaps" summary card remained at "0 identified" both before and after — no gap was flagged despite a 2-level drop across all technical competencies (including a "Critical" one, Programming Languages). Additionally Coverage broke to "NaN%" in the same action.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [X] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.