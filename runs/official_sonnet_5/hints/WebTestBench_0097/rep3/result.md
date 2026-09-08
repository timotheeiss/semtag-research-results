# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship/competency weight slider is non-functional
    - Actual: The per-competency "Weight" slider (e.g. Code Review, 60%) does not respond to any interaction: real mouse click-and-drag (with genuine trusted pointer events verified via event listener, moving from 55.5% to 90% position), keyboard Arrow/Home/End keys after focusing the thumb, and Radix pointer-capture based dragging all left aria-valuenow, the displayed "Weight" percentage label, and the connected edge weight labels completely unchanged (stayed at 60%/80% etc.). No visual emphasis or dependent analysis (coverage, critical path) changed as a result of any weight-slider interaction, confirming the control does not update the underlying relationship weight.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Enable/disable switch does not properly toggle and visual state never syncs with backend state
    - Actual: Clicking Collaboration node's enable switch (data-semtag-id='model.graph.item.collab.enabled') repeatedly (3 clicks) shows aria-checked/data-state staying 'true'/'checked' the entire time (visual never shows disabled). Backend metrics changed on the 1st click (coverage moved to 70%, avgLevel 3.4, gaps 0) but the 2nd and 3rd clicks produced NO further change to coverage/avgLevel/gaps, meaning the toggle got stuck after a single state change instead of alternating enabled/disabled on each click. Node itself never shows any disabled/dimmed styling (className stays 'react-flow__node react-flow__node-competency nopan selected selectable', innerText undimmed).

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Graph does not update when applying a template, despite title/metrics/scenario categories updating
    - Actual: Clicking 'Apply' on the 'Product Manager' template correctly updated: page title (Software Engineer -> Product Manager), department (Engineering -> Product), coverage (70%->78%), avgLevel (3.4->3.8), critical path list (now names 'Product Strategy', 'User Research', 'Communication', '+1' - competencies that don't exist anywhere in the visible graph), and Scenario Simulation category adjustment competency counts (technical 3->1, behavioral 2->1, leadership 0->1, domain 1->3). However, the competency graph itself was NOT replaced: it still displays the exact same 6 Software Engineer nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and the same 5 edges with identical weights (80%,70%,90%,60%,50%) as before applying the template. This is a clear data inconsistency - the Critical Path metric references competencies (Product Strategy, User Research, Communication) that are absent from the rendered graph.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not persisted/retrievable; no 'My Templates' section exists
    - Actual: Clicking 'Save Current' in the Template Library produced a success toast ('Template Saved - Current model configuration has been saved as a template.'), but the Template Library list still shows only the same original 5 templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) - no 6th/new template was added. There is no 'My Templates' section anywhere on the page (verified via full-page text search for 'My Templates' - none found, and a scan of all h2/h3/h4 headings shows only the original 5 template names). The save action is purely cosmetic (toast only) with no actual persistence or retrieval mechanism.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Running a simulation produces a broken (NaN) coverage metric; slider values also glitch/change unexpectedly across tab switches
    - Actual: On the Recruitment tab, setting technical category's Level Change to +1 and clicking 'Run Simulation' triggered a 'Simulation Applied' toast and did update avgLevel (3.3->3.8) and Critical Path, BUT the Coverage metric broke to 'NaN%' (instead of a valid percentage) and the technical category's own Weight Change readout simultaneously became 'NaN%' despite never being touched. Additionally, after switching away to the Training tab and back to Recruitment, the category adjustment sliders displayed different, unexplained non-zero values (technical -1, behavioral weight +10%, domain -1) that did not correspond to any user interaction, indicating unstable/uncontrolled slider state across tab switches. Clicking Reset does correctly restore metrics and zero out sliders when done immediately, but the NaN coverage bug on Run Simulation is a functional defect.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Model edits are not persisted across page reload/navigation
    - Actual: Set Agile Methodology's level to 5 (Expert) via its level-set button, confirmed via metrics.avg-level rising 3.3->3.7. Then performed a full page reload (browser_navigate to the same URL), which returned to the Models list/home screen (no deep-link/URL state for the open model). Re-opening the 'Software Engineer' model showed metrics.avg-level back at 3.3 and metrics.coverage back at 68% (the original baseline), meaning the level change made before reload was lost/not persisted, and there is no unique URL or storage mechanism retaining the edited state.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Only level-set buttons reliably trigger immediate coverage updates; weight slider and enable/disable toggle do not work correctly
    - Actual: Directly changing a competency's Level via its level-set buttons DOES immediately update Coverage %/progress bar and Avg Level without a page reload (confirmed earlier: System Design level 3->1 dropped coverage 69%->61% instantly). However, the other two control types named in this requirement are broken: the Weight slider is completely non-functional (see FT-4 - no drag/keyboard interaction changes its value or any metric), and the enable/disable Switch does not properly toggle or visually sync (see FT-5) though it does move coverage once. Since 2 of the 3 named mechanisms (weight, enabled-state) fail to reliably/correctly drive live coverage updates, this requirement is not consistently met.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node's Level badge never visually reflects the current backend level after edits
    - Actual: Clicking System Design's level-set-1 button correctly updated backend state (avgLevel 3.3->3.0, Risk Gaps 0->1 naming 'System Design' - gap/summary indicators DO agree with each other), but the node's own Level badge text remained 'Intermediate' (unchanged) instead of reflecting the new lower level. The backend DOES correctly track the true current level for subsequent edits (clicking level-set-3 afterward correctly computed avgLevel 3.0->3.3 and cleared the gap, exactly matching a 1->3 level transition), so the underlying state machine is consistent - but the visual graph control never displays the competency's current value/state to the user, which is the specific requirement being tested.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph has no visual highlighting that corresponds to the Critical Path summary
    - Actual: The Critical Path summary panel does recalculate after an enabled-state change (disabling 'Problem Solving' changed the count 3->4 and the named list from [Programming Languages, Problem Solving, System Design] to [Programming Languages, System Design, Collaboration, +1]). However, there is no graph highlighting mechanism that agrees with this summary: node className is identical for critical-path and non-critical-path nodes (only difference observed was a 'selected' class from manual click-selection, not critical-path status), and while some edges carry an 'animated' CSS class, this does not correlate with critical-path membership - e.g. the edge 'prog-lang->sys-design' (both endpoints ARE critical-path/'Critical'-badged nodes) is NOT animated, while 'collab->code-review' (neither endpoint was originally critical-path) IS animated. Since the graph shows no verifiable, consistent visual indication of which nodes/edges form the critical path, the graph highlighting and the summary panel do not 'agree with each other' as required.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.