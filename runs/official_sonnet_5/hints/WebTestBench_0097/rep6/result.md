# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Competency/relationship weight slider is not interactive
    - Actual: Attempted to change a competency's Weight slider (e.g. Programming Languages 90%, Code Review 60%) via keyboard (ArrowLeft/Home while focused), click on the track, and mouse-drag from thumb to track center. In every case aria-valuenow and the displayed weight-value% remained exactly unchanged (verified via DOM aria-valuenow and semantic weight-value observable). A console TypeError ("Cannot read properties of null (reading 'document') at nodrag_default... at HTMLDivElement.mousedowned") was captured from React Flow's node-drag handler firing on the slider, since the weight slider inside each graph node lacks the "nodrag" class needed to prevent React-Flow's node-drag from intercepting pointer interaction. By contrast, Level buttons (discrete clicks) on the same node do work and immediately update coverage/avg level. Clicking an edge (relationship) label also produced no weight-edit UI. Net effect: users cannot change a competency's weight (nor any edge/relationship weight) through the UI, so weight, visual emphasis, and dependent analysis never update.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Competency enable/disable toggle does not reflect state correctly and does not act on current state on repeated clicks
    - Actual: On the Software Engineer model (baseline Coverage 68%, Avg Level 3.3), clicking the Code Review competency's enabled switch once left aria-checked="true"/data-state="checked" (switch still visually shows enabled, node opacity stayed 1, no dimmed/disabled styling) yet Coverage/Avg Level changed to 69%/3.4 as if it had been excluded from calculations. Clicking the same switch a second time (expected to re-enable/restore baseline) left aria-checked="true" and metrics unchanged at 69%/3.4 instead of returning to 68%/3.3. So the switch never visibly shows a disabled state and does not toggle back and forth as clicked, violating the requirement that toggles clearly show disabled state and that repeated toggles act on the current state.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template leaves the competency graph out of sync with the rest of the model
    - Actual: Applying the "Product Manager" template correctly updated the title ("Product Manager"), department ("Product"), metrics (Coverage 78%, Avg Level 3.8, Critical Path 4 listing Product Strategy/User Research/Communication/+1), and the Scenario Simulation "Category Adjustments" competency counts (technical 1, behavioral 1, leadership 1, domain 3, matching Product Manager categories). However the competency graph itself still displayed the previous Software Engineer competencies and edges unchanged (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology with edges like "prog-lang to code-review"), i.e. the graph was not replaced with Product Manager's actual competencies/relationships. Title, metrics, and category adjustments represent the template, but the competency graph and relationships do not.

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saved custom template is not retrievable; no "My Templates" section exists
    - Actual: On the Software Engineer model, clicked \"Save Current\" in the Template Library panel. A toast appeared: \"Template Saved — Current model configuration has been saved as a template.\" However, after navigating back to the Models list and reopening the Software Engineer model (and separately after a full page reload to http://localhost:6097/), the Template Library still only listed the same original 5 built-in templates (Software Engineer marked \"Current\", Product Manager, HR Manager, Data Scientist, Sales Manager) with no new/duplicate saved template entry, and there is no separate \"My Templates\" section or tab anywhere in the UI. So the save action shows a success toast but does not actually persist a retrievable custom template.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [ ] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.
  - Bug Report:
    - Issue: Running a simulation with category-level adjustments produces an invalid Coverage metric (NaN%)
    - Actual: On Software Engineer model (baseline Coverage 68%, Avg Level 3.3, Critical Path 3, Gaps 0), set the Technical category's Level Change slider to +2 via keyboard (ArrowRight x2, verified value went 0→+1→+2) and clicked Run Simulation. Avg Level correctly updated to 4.2 and Critical Path stayed 3 (Gaps 0), but Coverage displayed as \"NaN%\" instead of a numeric percentage, and remained \"NaN%\" after waiting 1s (not a transient render glitch). Clicking Reset did correctly restore Coverage to 68%, Avg Level to 3.3, and the Technical Level Change slider back to \"0\", so reset-to-original works, but the simulation run itself produces a broken/invalid Coverage output.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Saved model edits (level changes) do not persist after leaving and reopening the model
    - Actual: On the Software Engineer model (baseline Coverage 68%), set Agile Methodology's level to 5, then clicked \"Save\". A \"Model Saved — Your changes have been saved successfully.\" toast appeared and Coverage updated to 73% (confirming the level change was applied and even acknowledged as saved). Navigated away via the Models nav link back to the job list, then reopened the Software Engineer model: Coverage showed 68% again (the original baseline), not the saved 73%, meaning the level-5 edit was lost. Re-verified with a full page reload (http://localhost:6097/) and reopening the model — still 68%, confirming edits and enabled/level/weight states are not actually persisted despite the app claiming the save succeeded.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Weight bounds (10-100%) cannot be verified as enforced because weight editing is non-functional both directly and via simulation; level bounds are enforced only via simulation math, not reflected in UI
    - Actual: Level bounds (1-5): Verified via category simulation on Software Engineer (baseline Avg Level 3.3). Applying Technical Level Change -2 twice cumulatively (net -4, which would drive prog-lang 4→0, sys-design 3→-1, code-review 3→-1 if unclamped) produced Avg Level 2.2, matching a floor-of-1 calculation (1+1+1+7+3)/6=2.17≈2.2, confirming levels are clamped at a floor of 1 in the underlying calc. Applying Technical Level Change +2 cumulatively twice (net +4) produced Avg Level 4.2 both after one and two applications, matching a ceiling-of-5 calculation (5+5+5+7+3)/6=4.17≈4.2, confirming a ceiling of 5. So level bounds ARE respected by the simulation math, though (per IX-20) the node's own displayed level never visually updates to reflect this. Weight bounds (10-100%): the direct per-competency Weight slider is non-functional (per FT-4, aria-valuenow/displayed weight never changes via click/drag/keyboard). Via the Technical category's Weight Change simulation slider, set to its max \"+30%\" (aria-valuenow 60 of range 0-60) and ran Run Simulation: the node's own weight-value displays for Programming Languages and System Design remained unchanged at 90%/80% (aria-valuenow on the node's weight slider also stayed 90), and no metric (Critical Path count, Gap count) changed as a result; Coverage additionally showed the invalid \"NaN%\" (same bug as FT-10). Since there is no way to actually move a competency's effective weight through any UI path, weight bound enforcement (min 10%, max 100%) cannot be confirmed at all — the feature is non-functional rather than bounded.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected scenario (Recruitment) has no meaningful default adjustments, so running it produces no output change
    - Actual: On first opening the Software Engineer model, the default-selected Recruitment tab's Category Adjustments (technical/behavioral/leadership/domain) all show Level Change "0" and Weight Change "0%". Clicking "Run Simulation" without switching tabs showed a "Simulation Applied" toast, but Coverage, Avg Level, Critical Path count, and Risk Gaps count remained exactly unchanged (68%, 3.3, 3, 0) before and after, and no other output/preview area appeared. So the initially selected scenario is not meaningfully initialized with non-zero defaults, and running it does not change any model outputs.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node control does not display the current level after an edit; desyncs from the summary analysis
    - Actual: On Software Engineer model (baseline Coverage 68%, Avg Level 3.3), clicking Agile Methodology's "set level to 5" button changed Coverage to 73% and Avg Level to 3.7 (consistent with the level actually changing to 5 in calculations), but the node's own Level label kept showing "Intermediate" and the level bar indicators stayed at 3/5 filled (bg-level-intermediate) instead of updating to reflect level 5. The graph control's displayed value therefore does not agree with the summary analysis after the edit, so a user cannot see the competency's actual current state, and a subsequent edit would appear to start from the wrong (stale) displayed level.


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Graph node "Critical" highlighting does not recalculate/sync with the Critical Path summary when enabled-state changes
    - Actual: Baseline (Software Engineer, all enabled): Critical Path summary listed exactly Programming Languages, Problem Solving, System Design (count 3), and in the graph those same 3 nodes carried a \"Critical\" badge while Code Review, Collaboration, and Agile Methodology did not — summary and graph agreed. After clicking Problem Solving's enabled toggle to disable it, the Critical Path summary count changed to 4 and the listed names became Programming Languages, System Design, Collaboration, +1 (i.e. Problem Solving was removed and Collaboration was newly added). However, in the graph itself, Problem Solving's node still displayed the \"Critical\" badge (unchanged) and Collaboration's node still displayed no \"Critical\" badge (unchanged) — the graph highlighting never updated to match the new summary. So after a weight/enabled-state change, the Critical Path summary and the graph's visual critical-path highlighting disagree with each other, violating the requirement that they recalculate together and stay consistent.

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.