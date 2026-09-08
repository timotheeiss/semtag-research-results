# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Competency weight slider inside a graph node does not respond to interaction
    - Actual: The per-competency "Weight" slider (data-semtag-id model.graph.item.<key>.weight, range 10-100) inside each React Flow graph node does not change value on click, drag, or keyboard arrow-key input. Verified via real Playwright click/drag and native JS .click() on multiple competencies (prog-lang in Software Engineer model, python/ml in Data Scientist model) across two different job models — aria-valuenow/displayed "Weight" text remained unchanged (e.g. stayed at 90%) after every interaction attempt, with elementFromPoint confirming clicks/drags land exactly on the intended control (no overlay interception) and no console errors were thrown. Since the weight can never actually be changed, no edge weight, visual emphasis, or dependent analysis update was observed either.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Competency enable/disable switch inside a graph node does not respond to interaction
    - Actual: Clicking the enabled toggle switch (data-semtag-id model.graph.item.<key>.enabled, role=switch) on multiple competency nodes (Programming Languages, Python Programming, Machine Learning) across two models never changed aria-checked/data-state from "true"/"checked". Tested with real Playwright clicks and native .click(); elementFromPoint confirmed no overlay was blocking the click. Since the toggle never actually flips, the node/edges never show a disabled state and nothing is excluded from coverage, gap, or critical-path calculations.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template updates the title/metrics but not the competency graph, causing an inconsistent model
    - Actual: Starting from the Data Scientist model and clicking Apply on the HR Manager template card: model.title changed to "HR Manager", model.department to "Human Resources", and metrics (coverage 71%, avgLevel 3.5) updated to HR Manager's values — but the competency graph still rendered the Data Scientist's 6 nodes (Machine Learning, Statistical Analysis, Python Programming, Data Visualization, Business Acumen, Data Storytelling) instead of HR Manager's competencies (Talent Acquisition, HR Compliance, etc.), verified both via semantic snapshot and raw DOM query of .react-flow__node data-id values. Title/metrics and the graph therefore represent two different models simultaneously.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Save Current template flow has no name/description input and no "My Templates" collection to retrieve from
    - Actual: Clicking "Save Current" immediately showed a toast "Template Saved: Current model configuration has been saved as a template" with no prompt to enter a custom name or description. The Template Library list still only shows the same 5 built-in role templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) — no new custom template entry appeared, and no "My Templates" section exists anywhere on the page (verified body text does not contain "My Templates"). localStorage is also empty, confirming nothing was actually persisted for later retrieval.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: No persistence of edited model state; Save button and reload/reopen lose all changes
    - Actual: Clicked set-level-1 on Negotiation competency (metrics changed to Coverage 63%, Avg Level 3.2, Gaps 1 with a Negotiation gap alert, confirming an underlying state change occurred despite the node's displayed 'Level: Expert' label not visually updating). Clicked Save (model.save) — no confirmation/toast observed, and localStorage/sessionStorage remained empty ({}) both immediately after Save and after a full page reload. Navigated back to Models list and reopened the Sales Manager model: metrics reverted to original defaults (Coverage 79%, Avg Level 3.8, Gaps 0), and the graph item list matches the pristine initial state. No mechanism persists edits across navigation or reload.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Coverage/Avg Level calculations produce NaN under scenario weight/level adjustments instead of staying within valid clamped ranges
    - Actual: Direct competency level/weight editing controls in the graph do not respond to input at all, so their range-clamping cannot be exercised through the primary edit path. Using the (only working) scenario category-adjustment sliders, applying a domain Weight Change of -30% or even a technical Level Change of -1 caused the model's Coverage metric to become "NaN%" (and once Avg Level became "NaN"/"N/A") rather than a value clamped to a sane range, and individual competency weights (10-100% slider bounds) could never be pushed to confirm they clamp correctly since they never changed via any interaction path. This indicates the app does not reliably keep computed coverage/level/weight values within valid bounds when adjustments are applied.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Competency level/weight/enabled controls do not register changes, so coverage never updates
    - Actual: Because the graph node's level buttons, weight slider, and enabled switch do not respond to any interaction (see FT-4/FT-5), attempting to change a competency's level, weight, or enabled state produces no change to the underlying model, and consequently the Coverage percentage/progress bar (metrics.coverage) remained at its original value (e.g. 68%) after each attempted edit.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Default scenario (Recruitment) has all-zero category adjustments, so running it without switching tabs never changes any model output
    - Actual: On every freshly opened model (tested HR Manager, Software Engineer, Data Scientist, Sales Manager), the initially selected "Recruitment" scenario tab shows Level Change "0" and Weight Change "0%" for all four categories (technical/behavioral/leadership/domain) by default. Clicking "Run Simulation" immediately (without switching tabs) shows a "Simulation Applied" toast, but Coverage (79%), Avg Level (3.8), Critical Path count (4) and every competency's level/weight remained byte-for-byte identical before and after the run, because every applied delta was 0. Since the default scenario ships with no-op adjustments, running it cannot "change the corresponding model outputs" as required.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls do not reflect any edit because edits do not apply
    - Actual: Since level buttons, weight slider, and enabled switch inside competency nodes do not register clicks/drags (verified across multiple nodes/models), the graph control never displays a new value/state after an "edit" attempt, so there is nothing for the Gap/Critical indicators to agree with, and no changed starting state for a subsequent edit.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path recalculation can produce corrupted/NaN summary metrics after a weight change
    - Actual: On the Sales Manager model, setting the "domain" category Weight Change to -30% (its minimum) and clicking Run Simulation updated the Critical Path list (3 competencies: Negotiation, Customer Relationships, Team Leadership — different from the pre-run set), but the Coverage metric became "NaN%" and Avg Level became "NaN"/"N/A" instead of valid recalculated numbers. A critical-path/summary recalculation that yields NaN values is not a valid, trustworthy recalculation and cannot be said to agree with graph highlighting.

- [ ] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.
  - Bug Report:
    - Issue: No working control can actually lower an individual competency's level/weight to trigger a named risk-gap warning
    - Actual: Direct in-graph level/weight controls do not register any change (see FT-4/FT-5). Scenario simulation adjustments do update aggregate Coverage/Avg Level/Critical Path (and often break into NaN), but never change any individual competency's displayed Level or Weight in the graph (e.g. CRM Proficiency stayed "Intermediate" after applying a technical Level Change of -1 and running the simulation), and Risk Gaps remained "0 identified" across every test (including a domain Weight Change of -30% and a technical Level Change of -1). Since no UI path can actually lower an enabled high-weight competency's level, no risk-gap warning naming that competency was ever produced.

- [ ] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.
  - Bug Report:
    - Issue: No risk-gap alert was ever produced because no reachable interaction actually creates a risk gap
    - Actual: Across all tested models and interactions (direct node edits, which are non-functional, and scenario simulations with extreme deltas such as domain Weight Change -30% and technical Level Change -1), the Risk Gaps metric stayed at "0 identified" and no text-and-icon alert naming affected competencies ever appeared near the summary metrics.