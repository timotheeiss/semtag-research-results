# Test Result

## Functionality
- [X] FT-1: Searching by a case-insensitive job-title, keyword, or description fragment returns exactly the matching competency models; an optional department filter further narrows the results, and no matches produce a clear empty state.

- [X] FT-2: Selecting a model from the search results opens the corresponding detail editor with the matching job title, summary metrics, and competency graph.

- [X] FT-3: Users can drag competency nodes to rearrange the graph layout while existing directed relationships remain attached to the correct competencies and their meaning does not change.

- [ ] FT-4: Users can change the weight of a competency relationship, after which the displayed relationship weight, its visual emphasis, and any dependent analysis update immediately.
  - Bug Report:
    - Issue: Relationship/competency weight cannot be edited visibly; graph never shows the new weight and the change is silently lost
    - Actual: Edge (relationship) weights (80%,70%,90%,60%,50%) have no editing affordance at all — clicking an edge opens nothing. The only weight control is the per-node weight slider: dragging Programming Languages' weight thumb from 90% toward mid-track recomputed coverage 68%->67% (consistent with weight 55) but the node still displayed "Weight 90%" and the slider fill stayed at 90% (style right:11.1111%). Edge labels/stroke widths did not change. A following level edit reverted coverage math to weight 90 (60% = 13.2/22), i.e. the weight change was discarded.

- [ ] FT-5: Users can disable and later re-enable a competency; the node and its relationships clearly show the disabled state, disabled competencies are excluded from coverage, gap, and critical-path calculations, and repeated toggles act on the current state.
  - Bug Report:
    - Issue: Disabled state is not shown, disabled competency still appears in critical path, and re-enabling does not work
    - Actual: Toggling Programming Languages off updated coverage (60%->65%, i.e. excluded from coverage/avg/gaps), but the node kept opacity 1, its switch stayed aria-checked="true", and its edges kept full opacity/colour — no visible disabled state. The Critical Path summary still listed "Programming Languages" among 5 competencies while disabled. Clicking the same toggle a second time did not re-enable it: metrics stayed 65% / Avg 3.2 (still excluded) because the node re-sends its stale enabled=true value.

- [X] FT-6: Users can browse the template library and compare each available job-role template by its role, department, competency count, relationship count, and representative keywords.

- [ ] FT-7: Applying a template replaces the edited model consistently, so the title, metrics, competency graph, relationships, and scenario-adjustment categories all represent the selected template.
  - Bug Report:
    - Issue: Applying a template does not replace the competency graph; graph keeps the previous model's competencies and relationships
    - Actual: Applying the "Data Scientist" template updated the title (Data Scientist), department (Analytics), metrics (Coverage 71%, Avg 3.5, critical path naming Machine Learning / Python Programming / Statistical...), the "Current" badge and simulation category counts (Technical 4, Behavioral 1, Domain 1), but the graph still rendered the Software Engineer nodes (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) with the identical old edges (prog-lang->code-review 80%, prog-lang->sys-design 70%, problem-solve->sys-design 90%, collab->code-review 60%, agile->collab 50%).

- [ ] FT-8: Users can save the current edited model as a custom template with a name and description, then retrieve the same configuration from a My Templates collection after leaving and returning.
  - Bug Report:
    - Issue: Saving the current model as a custom template is not implemented
    - Actual: Clicking "Save Current" in the Template Library produced no dialog, no name/description form, no toast and no new entry: no [role=dialog] in the DOM, the template list still contains only the 5 built-in roles, no "My Templates" section exists anywhere in the page text, and localStorage remains empty.

- [X] FT-9: Users can switch among Recruitment, Training, and Promotion simulation scenarios, and the scenario description and category adjustments update to match the selected scenario.

- [X] FT-10: Users can choose category-level competency and weight adjustments for a scenario, run the simulation to update coverage, average level, critical-path and risk-gap outputs, and reset the model to its original state.

- [ ] FT-11: After editing a model, users can save and later restore the same levels, weights, and enabled states after leaving and reopening the model or reloading the app.
  - Bug Report:
    - Issue: Edited model is not persisted; saved changes are lost when leaving and reopening the model
    - Actual: Set Code Review to level 5 (Coverage 68%->74%, Avg 3.3->3.7), clicked Save and got the toast "Model Saved — Your changes have been saved successfully". Navigating Back and reopening Software Engineer showed Coverage 68% / Avg 3.3 again and the Code Review node back at Level Intermediate / Weight 60%. localStorage is empty, so nothing is stored for a later reload either.


## Constraint
- [ ] CS-12: Direct edits and scenario simulations keep every competency level within 1 to 5 and every competency weight within the supported 10% to 100% range.
  - Bug Report:
    - Issue: Scenario simulation drives competency weights outside the 10%-100% range, producing NaN metrics
    - Actual: Direct edits are bounded (level buttons 1-5; weight slider aria-valuemin=10 / aria-valuemax=100) and repeated +2 level runs clamped at 5 (Avg 3.8 matched the clamped calculation). But setting the Technical "Weight Change" adjustment to its minimum (-30%) and running the simulation once from the freshly reset model turned Coverage into "NaN%" and Avg Level into "NaN" (label "N/A"); further runs stayed NaN, i.e. weights fell to/below 0 instead of stopping at 10%.


## Interaction
- [X] IX-13: Changing an enabled competency's level or weight, or changing which competencies are enabled, immediately updates the displayed coverage percentage and progress indicator without a reload.

- [ ] IX-19: The initially selected simulation scenario is fully initialized: its default adjustments are visible and running it without first switching tabs applies those adjustments and changes the corresponding model outputs.
  - Bug Report:
    - Issue: Initially selected Recruitment scenario is not initialized with its default adjustments
    - Actual: On opening the detail editor, the Recruitment tab showed Level Change 0 / Weight Change 0% for Technical, Behavioral, Leadership and Domain, and clicking Run Simulation left every model output unchanged (Coverage 68%, Avg 3.3, Critical Path 3, Risk Gaps 0). Only after switching to Training/Promotion and back did Recruitment display its real defaults (Technical -1, Behavioral +10%, Domain -1), which then did change outputs to 56% / 2.7.

- [ ] IX-20: After any competency level, weight, or enabled-state edit, the graph control displays the current value or state, its Gap and Critical indicators agree with the summary analysis, and a subsequent edit starts from that current state.
  - Bug Report:
    - Issue: Graph node controls show stale values after edits; subsequent edits do not start from current state
    - Actual: After clicking "set level 2" on Programming Languages, summary showed Coverage 60%, Avg 3.0 and a gap alert for that competency, but the node still displayed "Level Advanced" (=4) and its level bar/weight slider were unchanged. Likewise after a weight drag the node kept "Weight 90%" while metrics used 55. Because the node keeps stale data, the next edit re-sends the old values: a level click after the weight drag restored weight 90 (coverage 60% = 13.2/22 instead of 61.7% expected with weight 55).


## Content
- [X] CT-15: The detail editor presents the current model as an interactive directed weighted graph: nodes identify competencies, arrowed connections identify dependency or reinforcement relationships, relationship weights are visible, and users can pan, zoom, and rearrange the view.

- [ ] CT-16: After a relevant competency-weight or enabled-state change, the critical-path summary and graph highlighting recalculate immediately, form a valid path through enabled competencies, and agree with each other.
  - Bug Report:
    - Issue: Critical-path summary and graph highlighting disagree, and the summary is not a valid connected path
    - Actual: After disabling Problem Solving, the summary listed Critical Path = 4 (Programming Languages, System Design, Collaboration, +1) while the graph kept highlighting exactly prog-lang, sys-design and the now-disabled problem-solve (border-node-critical) and never highlighted Collaboration. Earlier, with Programming Languages disabled, the summary path still included "Programming Languages". The listed sequences are also not connected paths (no edge System Design -> Collaboration; edges are prog-lang->sys-design, prog-lang->code-review, problem-solve->sys-design, collab->code-review, agile->collab).

- [X] CT-17: Lowering an enabled high-weight competency below the recommended level immediately creates a risk-gap warning naming that competency; resolving the level or disabling the competency clears it.

- [X] CT-18: When one or more risk competency gaps are detected, the app prominently displays a text-and-icon alert near the summary metrics that names the affected competencies and suggests corrective action.