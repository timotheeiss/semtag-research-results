# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: Edge weight is not editable; edge weight label is also not reliably clickable via real pointer due to overlapping node cards.
    - Actual: Real mouse clicks on the edge (e.g. "Edge from talent-acq to perf-mgmt", labeled 70%) are intercepted by the overlapping node card panel (class "space-y-3") even on a pristine, un-modified layout, so a normal user cannot reach the edge. Even after forcing selection via a synthetic click event (which does add a "selected" CSS class to the edge), no weight-editing UI, input, slider, or dialog appears anywhere in the DOM (searched for "Edge Weight"/"Edit Edge" text and role=dialog - none found). The displayed "Weight" sliders inside node cards only control each node/competency's own weight, not edge weights.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Disabling a competency node does not visually gray out or hide the node/edges, and the toggle switch UI does not reflect state changes correctly.
    - Actual: Clicking the "Talent Acquisition" node's enable/disable switch once caused Coverage to drop from 71% to 68% and the Critical Path list to change (Talent Acquisition removed, Performance Management added), indicating the underlying calculation did register a disable. However: (1) the switch's data-state/aria-checked remained "checked"/"true" after the click and after multiple subsequent clicks - it never visually shows as OFF; (2) the node's CSS class/opacity/filter remained unchanged (opacity:1, no disabled styling) - not grayed out; (3) the related edge (talent-acq→perf-mgmt) also kept opacity:1 with no visual change; (4) further clicks on the switch had no effect (coverage stayed at 68%, could not toggle back to 71%), meaning the control is not reliably usable to re-enable the node either.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template updates the page header/title and some metrics but does not update the actual interactive graph (nodes/edges) shown in the editor.
    - Actual: Clicking "Apply" on the "Software Engineer" template showed a "Template Applied" toast, changed the page title to "Software Engineer"/Engineering, and updated Critical Path labels to "Programming Languages, Problem Solving, System Design". However, the React Flow graph nodes still had data-ids ["talent-acq","hr-compliance","perf-mgmt","emp-relations","leadership","org-dev"] - i.e. the previous HR Manager model's competency nodes/edges were still displayed instead of the Software Engineer template's competencies, so the editing interface itself was not actually loaded with the new template's structure.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Save-as-template flow does not collect name/description and saved template is not retrievable
    - Actual: Clicking 'Save Current' immediately shows a 'Template Saved' toast with no dialog/input for a name or description. The Template Library list still shows only the original 5 built-in templates (Software Engineer, Product Manager, HR Manager [Current], Data Scientist, Sales Manager) with no new custom entry added. document.body.innerText.includes('My Templates') returned false, confirming no 'My Templates' section exists anywhere on the page to retrieve saved custom templates from.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [X] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: No actual state persistence despite 'Save' claiming success
    - Actual: After running a Promotion simulation (Coverage 71%→70%, Critical Path 3→4 competencies) and clicking the 'Save' button (which showed a 'Model Saved' toast: 'Your changes have been saved successfully'), localStorage and sessionStorage remained empty (Object.keys both []) confirming no client-side persistence was written. Reloading the page via browser_navigate and re-opening the HR Manager model showed Coverage reverted to 71% and Critical Path reverted to 3 competencies (Talent Acquisition, Employee Relations, HR Compliance) — the original unmodified state. The 'saved' changes were not restored, meaning the Save button and 'Model Saved' confirmation are non-functional/misleading and no state restoration mechanism exists across page loads.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Cannot verify invalid-operation prevention; underlying mechanisms (edge creation, node disable) are non-functional/unreliable
    - Actual: Tested the checklist's own example (adding dependency edges to disabled competencies) by attempting edge creation via React Flow connection handles (.react-flow__handle, 12 found, all class 'connectable'). Dragging from a node's source handle to (a) its own target handle (self-loop) and (b) a different, unconnected node's target handle both resulted in NO new edge being created — document.querySelectorAll('.react-flow__edge') remained at exactly 5 edges (e1-e5) before and after both attempts. Since even a valid, non-invalid new connection cannot be created via the UI, there is no way to verify the system specifically prevents invalid connections (e.g., to disabled nodes) as opposed to edge creation simply being broken entirely. Additionally, the node 'disable' switches (precondition for this test) do not reliably reflect state: clicking all 6 competency switches left every switch showing aria-checked='true'/data-state='checked' in the DOM (same bug documented in FT-5), so a genuinely 'disabled' competency state could not be reliably established to test against. No validation messages, blocked actions, or error feedback were observed for any invalid-operation scenario attempted.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [ ] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.
  - Bug Report:
    - Issue: Editing interactions are not consistently smooth/responsive; some are blocked or unreliable
    - Actual: While simple interactions (level-star clicks, tab switching, Run Simulation) responded immediately, several core editing operations were not smooth or reliably actionable: (1) Edge label clicks (for weight editing) were consistently blocked by pointer-event interception from an overlapping node panel element ('<div class=\"space-y-3\">' intercepts pointer events), causing real Playwright clicks to time out and require synthetic MouseEvent dispatch as a workaround to register any interaction at all — a real user could not smoothly click an edge. (2) Node dragging was unreliable: an initial drag-and-drop attempt (dragging a node onto a background/control element) timed out after repeated pointer-interception retries and left the node's CSS transform corrupted (translate(0px, -11758.6px)), requiring a full page reload to recover; drags only succeeded reliably when targeting another node instead of background/control elements. These overlap and hit-testing issues indicate editing operations are not consistently smooth or responsive.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [ ] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.
  - Bug Report:
    - Issue: Graph visual highlighting does not sync with recalculated critical path
    - Actual: The 'Critical Path' metric correctly recalculates in real time (initially 3 competencies: Talent Acquisition, Employee Relations, HR Compliance; after running the Promotion simulation it changed to 4: Employee Relations, HR Compliance, Leadership +1 — Talent Acquisition dropped out, Leadership was added). However, the graph's node highlighting (CSS class 'border-node-critical' applied to each node's .competency-node div) did NOT update to reflect this: both before and after the simulation, exactly the same 3 nodes (talent-acq, hr-compliance, emp-relations) carried the 'border-node-critical' class, while leadership, perf-mgmt, and org-dev never did — even after Leadership became part of the newly-computed critical path and Talent Acquisition left it. This class is actually tied to each competency's static 'Critical' importance badge (a fixed field, unrelated to the dynamic critical-path computation), not to the live critical-path result, so the graph never visually highlights the currently-computed critical path.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.