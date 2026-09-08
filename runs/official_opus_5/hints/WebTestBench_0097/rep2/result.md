# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: No UI to edit relationship (edge) weights; competency weight control does not update its displayed value
    - Actual: Edge weight labels (85%/70%/80%/60%/70%) are static. Single-click on an edge only selects it (no editor); double-click produces no dialog/popover/input (0 dialogs, 0 popovers, 0 inputs on page). No data-semtag element for any edge/relationship exists. The nearest analogue, the competency weight slider, is also broken: dragging model.graph.item.negotiation.weight from 95% toward mid-track changed coverage 79%→77% (implying model weight ~55), yet the node's displayed Weight stayed "95%", aria-valuenow stayed 95 and the thumb stayed at left:calc(94.4444% - 8.88889px). Reproduced twice; Reset restored 79%.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is never rendered on the node or its relationships, and a disabled competency cannot be re-enabled (repeated toggles do not act on current state)
    - Actual: Disabling works only in the analysis: disabling CRM Proficiency changed coverage 79%→81% and avg level 3.8→4.0, and disabling Negotiation removed it from the critical-path summary — so exclusion from coverage/gap/critical-path is correct. However (a) the node keeps class "competency-node bg-card p-4 min-w-[220px] border-node-critical", its "Critical" badge and its original Level/Weight text, with no dimming or disabled styling, and its incident edges keep the same stroke/opacity; (b) the switch stays aria-checked="true"/data-state="checked"; (c) a second click does NOT re-enable — after disabling CRM, coverage stayed 81% (79% expected if re-enabled), and after disabling Customer Relationships a second click left it still excluded from the critical path. The competency is permanently stuck disabled until Reset.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not update the competency graph — it still renders the previous model's competencies and relationships
    - Actual: Applied the Data Scientist template while editing Sales Manager. Title→"Data Scientist", department→"Analytics", metrics→71%/3.5/CP 3/gaps 0, scenario categories→Technical 4/Behavioral 1/Leadership 0/Domain 1, and the "Current" badge moved to Data Scientist — all correct. But the graph still shows the six Sales Manager nodes (Negotiation, Customer Relationships, Pipeline Management, Market Knowledge, Team Leadership, CRM Proficiency) with the Sales relationships (customer-rel→negotiation 85%, market-knowledge→pipeline-mgmt 70%, crm→pipeline-mgmt 80%, team-leadership→pipeline-mgmt 60%, negotiation→customer-rel 70%). Live app state confirms the model is really Data Scientist (Machine Learning, Statistical Analysis, Python Programming, Data Visualization, Business Acumen, Data Storytelling), so the graph contradicts every other part of the page.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: "Save Current" is a no-op — no name/description capture, no custom template created, and no My Templates collection exists
    - Actual: Clicking templates.save-current produced no dialog (0 [role=dialog]), no popover/portal, no input or textarea anywhere on the page, and no toast. The Template Library still lists exactly the 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with no new entry. The string "My Templates" appears nowhere in the app (only "Template Library"), and nothing was written to localStorage/sessionStorage/IndexedDB, so no custom template can be retrieved after leaving and returning.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Save does not persist edits; reopening the model discards all level/weight/enabled changes
    - Actual: Edited Sales Manager until coverage 54% / avg 2.3 / 3 risk gaps, clicked Save (no confirmation toast shown), navigated Back to the model list and reopened Sales Manager: metrics returned to the pristine coverage 79% / avg 3.8 / critical path 4 / gaps 0. Nothing is written anywhere — localStorage, sessionStorage, cookies are all empty and indexedDB.databases() returns [] both before and after Save — so a page reload cannot restore anything either.


## Constraint
- [X] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized — its default adjustments are missing and running it has no effect
    - Actual: On first load of the detail editor the Recruitment tab is active but every category shows "Level Change 0 / Weight Change 0%". Clicking Run Simulation without switching tabs left all outputs unchanged (coverage 79%, avg 3.8, critical path 4, gaps 0). Only after visiting Training/Promotion and returning to Recruitment do its real defaults appear: Technical -1, Behavioral +10%, Leadership -2 / -10%, Domain -1 — proving defaults exist but were never loaded for the initial tab.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph controls never re-render with the current value/state after an edit, so indicators disagree with the summary and subsequent edits start from a stale state
    - Actual: Weight edit: dragging negotiation weight slider changed coverage 79%→77% (model weight ~55%) while the control still showed "95%", aria-valuenow=95 and thumb left:calc(94.4444% - 8.88889px). Enabled-state edit: disabling CRM changed coverage to 81%/avg 4.0 while the node kept its "Critical" badge and border-node-critical class and the switch stayed aria-checked="true"; graph highlighting therefore disagrees with the summary (summary excluded it from coverage, graph still shows it as an active Critical node). Subsequent edits start from the stale state: a 2nd click on the toggle re-issued "disable" instead of re-enabling (coverage stayed 81%).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph highlighting does not recalculate and disagrees with the critical-path summary
    - Actual: After disabling Negotiation and lowering its level to 2, the summary critical path correctly recalculated to "Customer Relationships, Pipeline Management, CRM Proficiency, +1" (Negotiation excluded), but the graph still rendered the Negotiation node with class border-node-critical, its "Critical" badge, and stale "Level Expert / Weight 95%". No graph node highlighting changed at all (node classes identical to the pre-edit baseline). Summary and graph therefore contradict each other.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.