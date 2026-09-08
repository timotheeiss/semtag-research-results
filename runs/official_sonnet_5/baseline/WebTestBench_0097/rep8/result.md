# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No functional control exists to change a competency relationship (edge) weight
    - Actual: Each edge (e.g. "Edge from talent-acq to perf-mgmt", 70%) is rendered as a clickable/focusable element with role=button, but clicking or double-clicking it only toggles a "selected" CSS state — no editable input, slider, or dialog appears anywhere on the page to change its weight value. No CSS selector search found any dialog/popover/input related to "relationship" or edge weight editing. The only "Weight" sliders present are per-node (competency) sliders, which are a different value than the edge weight (e.g. Talent Acquisition node weight=90% while its outgoing edge weight=70%), and thus do not represent relationship weight.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disable/re-enable competency: no visual disabled state, and switch cannot re-enable competency
    - Actual: Toggling the Talent Acquisition switch off correctly affects calculations once (Coverage 71%->68%, Avg Level 3.5->3.4, Talent Acquisition removed from Critical Path list), confirmed via React fiber data.competency.enabled flipping semantics in the calc engine. However: (1) the switch itself shows NO visual disabled indication whatsoever - aria-checked stays "true", data-state stays "checked", the thumb stays in the ON position, and the node card shows no opacity/greyscale/disabled styling, so a user cannot tell the competency was disabled by looking at the switch/node; (2) clicking the switch again (repeated toggle, tested via real browser_click, direct onClick invocation via React props, and multiple selector strategies) does NOT re-enable the competency - Coverage/Avg Level/Critical Path remain stuck at the disabled values (68%/3.4/list without Talent Acquisition) even after 2+ additional clicks. This violates both the "clear visual disabled state" and "repeated toggles should work correctly based on current state" requirements of FT-5.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the graph; inconsistent state between summary/metrics and visualized nodes
    - Actual: Clicked "Apply" on the Data Scientist template while HR Manager model was open. Result: title correctly changed to "Data Scientist", department to "Analytics", a "Template Applied" toast appeared, Coverage/Avg Level metrics recalculated, Critical Path list updated to name Data-Scientist-specific competencies ("Machine Learning", "Python Programming", "Statistical Analysis"), scenario Category Adjustments competency counts updated (technical 4, behavioral 1, leadership 0, domain 1), and the Template Library card now shows "Applied"/disabled with a "Current" badge on Data Scientist. However, the actual competency graph (React Flow nodes) was NOT replaced: querying the DOM directly (document.querySelectorAll('.react-flow__node-competency')) shows the node cards are still the original HR Manager competencies ("Talent Acquisition", "HR Compliance", "Performance Management", "Employee Relations", "Leadership", "Organizational Development") — none of which match the Data Scientist competencies named in the Critical Path summary (e.g. "Machine Learning" does not even appear as a node). This creates an inconsistent, broken state where the graph, edges, and relationships were never actually replaced despite the header/metrics/scenario data claiming the new template was applied.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not retrievable; no "My Templates" section exists
    - Actual: Clicked "Save Current" while HR Manager model was open. A toast appeared: "Template Saved - Current model configuration has been saved as a template." However, there is no "My Templates" section/tab/filter anywhere on the page (verified via full-page text search: no occurrence of "My Templates" string in the DOM), and the Template Library list still shows exactly the same 5 original templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with no new custom template card added. The save action produces a success notification but the saved template cannot be found or retrieved anywhere in the UI.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Run Simulation produces NaN Coverage value; Weight Change slider shows NaN%
    - Actual: On the Recruitment tab, adjusted the "domain" category's Level Change slider via keyboard (ArrowRight) from 0 to +1 (confirmed via visible label update to "+1"). Immediately after this single interaction, without touching the Weight Change slider at all, the domain category's "Weight Change" label spontaneously displayed "NaN%" (aria-valuenow also became the string "NaN"). Clicking "Run Simulation" showed a "Simulation Applied" toast and correctly updated Avg Level (3.5 -> 4.0, "Advanced") reflecting the +1 level change, but the Coverage metric became "NaN%" instead of a valid percentage - a broken/undefined value directly caused by the NaN weight-change slider being included in the calculation. Clicking "Reset" did correctly restore Coverage to 71% and Avg Level to 3.5, so Reset itself works, but Run Simulation produces invalid (NaN) output for Coverage under normal category-adjustment usage.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Model edits (level changes) are not persisted when leaving and reopening the model
    - Actual: Changed Talent Acquisition's Level to 2 via the level dot buttons; confirmed the change took effect (Coverage 71%->63%, Avg Level 3.5->3.2, Risk Gaps 0->1 naming "Talent Acquisition"). Clicked the back-arrow button to return to the Browse Job Models list, then re-selected "HR Manager". Upon reopening, all values reverted to the original defaults (Coverage 71%, Avg Level 3.5 "Advanced", Risk Gaps 0, Critical Path unchanged) - the Level 2 change was completely lost. The model does not persist edits across leave/reopen navigation, failing the FT-11 requirement that levels/weights/enabled states be saved and restored after leaving and reopening.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight value/constraint is broken: node weight slider is non-interactive and scenario Weight Change slider can produce NaN, violating the 10%-100% constraint
    - Actual: Level constraint (1-5) is structurally enforced for direct edits: the Level control exposes exactly 5 discrete dot buttons; clicking dot 1 correctly set level to minimum (verified via Coverage 71%->59%, Avg Level 3.0, Risk Gap flagged) with no way to go below level 1 or above level 5 through the UI - this part is acceptable. However, Weight constraint (10%-100%) is broken: (1) the per-node Weight slider (aria-valuemin=10, aria-valuemax=100) is completely non-interactive under every tested interaction method (native click, keyboard arrows, pointer/mouse event dispatch, drag, and direct React onClick/handler invocation all previously verified to have zero effect on aria-valuenow), making it impossible to verify the constraint is enforced during direct edits since the control cannot be moved at all; (2) in the Scenario Simulation's Category Adjustments panel, after interacting with a sibling Level Change slider, the "Weight Change" slider's value spontaneously became "NaN" (both aria-valuenow and the displayed "NaN%" label), which is clearly outside any valid 10%-100% (or -X%/+X%) range, and this invalid value propagated into Run Simulation, corrupting the Coverage metric to "NaN%". This demonstrates the weight constraint is not reliably enforced.


## Interaction
- [ ] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.
  - Bug Report:
    - Issue: Weight changes cannot be verified to update coverage immediately because the weight control is non-functional
    - Actual: Level changes and enabled/disabled toggles DO immediately update Coverage % and the progress bar without any reload: clicking a Level dot (level 1) instantly changed Coverage from 71% to 59% and the progress bar's fill transform updated in the same render (translateX(-40.625%) matching the new 59% value); toggling the enable switch instantly changed Coverage from 71% to 68%. However, the per-node Weight slider is completely non-interactive (as established via exhaustive testing across click/keyboard/pointer/drag/direct-handler-invocation methods - aria-valuenow never changes from its initial value), so it is impossible to trigger or verify a weight-driven immediate coverage update at all. Since IX-13 requires level, weight, AND enabled changes to all immediately update coverage, and the weight case cannot be exercised due to this control being broken, the item fails overall.

- [X] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node does not reflect current level value or gap state; disagrees with summary indicators
    - Actual: After setting Talent Acquisition's Level to 1 (minimum) via the level dots, the summary metrics correctly reflect the change (Coverage 71%->59%, Avg Level 3.0, Risk Gaps 1 naming "Talent Acquisition", with an alert). However, the graph node itself does NOT show the current state: (1) the node's "Level" text label still displays "Advanced" instead of the actual new level (should show something like "Beginner"/"Novice"); (2) the node retains its green "border-node-critical" styling (rgb(33,196,93)) with no visual gap/warning indicator (e.g., red/orange border or icon) despite the Risk Gaps summary explicitly naming this exact competency as a gap. This means the graph control does not display the current value/state, and the node's Critical/Gap visual indicators do not agree with the Risk Gaps summary panel, directly violating IX-20.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Named "Critical Path" competencies do not form a connected path in the graph
    - Actual: The Critical Path summary lists "Talent Acquisition, Employee Relations, HR Compliance", and the graph correctly highlights the same 3 nodes with a distinct "border-node-critical" style (agreeing with the summary on WHICH nodes are flagged). However, the actual directed graph edges are: talent-acq->perf-mgmt, emp-relations->perf-mgmt, leadership->org-dev, hr-compliance->emp-relations, perf-mgmt->org-dev. Among the 3 "critical path" nodes, only one edge connects two of them (hr-compliance->emp-relations); Talent Acquisition has no edge to either Employee Relations or HR Compliance (it only connects to Performance Management, which is not flagged critical). Therefore the named "Critical Path" does not form a valid connected directed path through the graph - it is simply a list of individually-flagged critical competencies, not an actual path/sequence. Additionally, no edges are visually highlighted/styled to indicate critical-path membership (edge stroke colors/widths appear to encode weight/category, not critical-path status), so there is no graph highlighting of the path itself beyond the individual node borders.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.