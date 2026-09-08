# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Competency weight slider inside graph nodes is non-functional
    - Actual: Attempted to change the "Programming Languages" node's Weight slider (90%) via: (1) native Playwright click+ArrowLeft/ArrowRight/Home keyboard, (2) Playwright drag from thumb to track center (different x-coordinates confirmed), (3) synthetic pointerdown/move/up events at a computed target position. In all cases aria-valuenow and the displayed "90%" value never changed, and no dependent metric changed. Clicking directly on an edge (e.g. "Edge from prog-lang to code-review") only selects it (adds [active] state) but opens no weight-editing UI. There is no way found in the UI to change a relationship/competency weight.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable toggle switch on graph competency nodes does not change state
    - Actual: Clicked the enabled/disabled switch (role="switch", not disabled, cursor:pointer) on two different, previously-untouched competency nodes ("Agile Methodology" and "System Design") using a real trusted Playwright click at correct, visible, unobstructed coordinates. In both cases data-state/aria-checked remained "checked" (true) after the click, and even a direct DOM .click() call produced no change. No visual "disabled" styling ever appeared and metrics (coverage/critical path/gaps) never reflected an exclusion. The toggle control is present and looks interactive but does not perform its stated action.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates title/metrics/category-adjustment counts but does NOT update the competency graph itself
    - Actual: Clicked \"Apply\" on the Product Manager template while viewing Software Engineer. Result: model.title changed to \"Product Manager\", department to \"Product\", metrics changed (Coverage 78%, Avg Level 3.8, Critical Path 4, Gaps 0), Critical Path list now names \"Product Strategy, User Research, Communication, +1\", and Category Adjustments competency counts changed (technical 1, behavioral 1, leadership 1, domain 3). However, the actual Competency Graph (React Flow nodes/edges) was NOT updated at all — it still displays the original Software Engineer nodes verbatim (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) with their original weights/levels/edges. This produces a direct data inconsistency: the Critical Path metric names competencies (\"Product Strategy\", \"User Research\", \"Communication\") that do not exist anywhere in the visible graph. The graph/relationships did not update consistently with the rest of the model as required.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save Current template does not prompt for name/description and the saved template is not retrievable anywhere
    - Actual: Clicked \"Save Current\" while viewing the (template-applied) Product Manager model. No dialog/prompt appeared asking for a name or description — the action fired immediately with no user input captured. Inspected the DOM afterward: the Template Library list still contains exactly the same 5 original template ids (software-engineer, product-manager, hr-manager, data-scientist, sales-manager) with no new entry added; no \"My Templates\" section or heading exists anywhere in the page (document.body.innerText does not contain \"My Templates\"); localStorage and sessionStorage are both empty (no client-side persistence). After navigating away to the Models list and back into Software Engineer's detail page, the Template Library still shows only the same original 5 templates. The saved custom template cannot be found or reapplied anywhere, so the described save-and-retrieve workflow does not work.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Coverage output becomes NaN% whenever a category Level Change slider is used, due to a weight-delta calculation bug
    - Actual: Repeated, deterministic reproduction (4 separate fresh-page trials): as soon as any category's "Level Change" slider is clicked/moved (e.g. Behavioral +1, or Technical -1), that same category's sibling "Weight Change" value immediately shows "NaN%" (before Run Simulation is even clicked), and it never self-corrects (moving the level back to 0 leaves weightDelta stuck at NaN%). Clicking "Run Simulation" afterward propagates this into the top-level Coverage metric, which renders as "NaN%" instead of a valid percentage, even though Avg Level (e.g. 3.3→3.7), Critical Path count, and Gap count do update to plausible numeric values. Reset does correctly restore Coverage/AvgLevel/CriticalPath/GapCount and all category deltas back to baseline (68%/3.3/3/0, 0/0% deltas). Because Coverage is a required output of this workflow and is broken (NaN) on essentially every realistic use of the Level Change slider, the "run simulation to update coverage... outputs" requirement is not reliably met. (Note: an earlier isolated run in this session coincidentally showed a valid, non-NaN Coverage value after a similar interaction sequence, but this could not be reproduced across 4 controlled fresh-state retries, confirming NaN corruption is the dominant, reproducible behavior.)

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: No persistence mechanism exists for saved model edits (levels/weights/enabled states)
    - Actual: Direct level/weight/enabled edits are already confirmed non-functional (see FT-4/FT-5), so there is no actual edit that could be persisted. Additionally tested the \"Save\" button itself (distinct from \"Save Current\" template action): clicking it produced no visible confirmation, no non-static network requests (verified via browser_network_requests — 0 non-static requests fired), and localStorage/sessionStorage remained completely empty both before and after the click. After a full page reload, the app returns to the Models list with no memory of which model/template had been open (e.g., a previously-applied Product Manager template reverts silently, since the underlying data is only in-memory React state that is discarded on reload). There is no working mechanism to save and later restore model state (levels, weights, or enabled states) across a reload or a leave/return cycle.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight computation produces NaN instead of staying within 10%-100%
    - Actual: Direct node-level edits are blocked entirely by the FT-4/FT-5 bugs (levels/weights/enabled state cannot be changed, so they trivially can't go out of range, but the constraint can't be positively verified either). During scenario simulation, moving the Technical category's "Level Change" slider caused the sibling "Weight Change" slider for the same category to display "NaN%" (instead of a value inside its own -60%..+60% range), and after clicking Run Simulation the model's Coverage metric itself became "NaN%", a value clearly outside the 0-100% (and thus outside any 10%-100% weight-derived) valid range.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Coverage does not update because the underlying edit controls (level/weight/enabled) are non-functional
    - Actual: Since the graph node's level buttons, weight slider, and enabled toggle do not change their underlying state (see FT-4/FT-5), the Coverage percentage and progress bar also never changed after any of these interaction attempts, even though a working edit should immediately update coverage per this requirement.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls do not reflect any edit because edits do not apply
    - Actual: Because level/weight/enabled edits on graph nodes never take effect (FT-4/FT-5), the node controls always continue to show the original value/state after an attempted edit, so there is nothing for the Gap/Critical indicators to agree with, and a subsequent edit cannot be shown to "start from" a new current state since no state change ever occurred.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical path cannot be verified to recalculate from a weight/enabled-state change because those edits are non-functional
    - Actual: Attempts to change a competency's weight or enabled state directly on the graph (prerequisite for this test) had no effect (see FT-4/FT-5), so the Critical Path summary and graph highlighting could not be observed to recalculate from such a change. (Note: selecting different nodes did change the displayed Critical Path count/list, e.g. 3→5 competencies, which appears to be tied to node selection rather than an actual structural/weight change, and no corresponding highlighting differences could be confirmed on the edges themselves.)

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: Cannot lower a competency's level via the UI, so no risk-gap warning can be produced or verified
    - Actual: The Level segmented-buttons on graph nodes (e.g. "Programming Languages" set-level-2, "Code Review" set-level-5) do not change the competency's level when clicked (level remained "Advanced"/"Intermediate" respectively after the click), and the enabled toggle also doesn't work, so a high-weight competency's level could not be lowered to test whether a risk-gap warning appears. Risk Gaps counter stayed "0" throughout all attempts.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.