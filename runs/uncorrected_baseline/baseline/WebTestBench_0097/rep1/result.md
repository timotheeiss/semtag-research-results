# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: Edge weight cannot be modified through any exposed UI control
    - Actual: Clicking a graph edge only toggles a "selected" CSS state (no editor/input appears, no dialog opens on click or double-click). The only visible "Weight" control is a slider on each competency node card, but it does not respond to real mouse drag (tested via trusted Playwright drag from the slider thumb to multiple track positions), click-on-track, or keyboard arrow keys while focused — aria-valuenow remained stuck at 90% (Talent Acquisition) through every interaction method tried. No mechanism was found to change any edge's or node's weight value, so the graph never refreshes to reflect a weight change.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Disabling a competency node does not visually gray out/hide it or its edges, and the toggle's own state indicator is inconsistent
    - Actual: Clicking the Talent Acquisition node's enable/disable switch caused Coverage (71%→68%), Avg Level (3.5→3.4) and Critical Path composition to change (Talent Acquisition dropped, Performance Management added) — suggesting the node was excluded from calculations — but the switch's aria-checked attribute remained "true" throughout, the node card kept opacity:1/filter:none (no graying), and all 5 edges kept opacity:1 with no disabled styling. A second click on the same switch produced no further change (metrics stayed at 68%), so the control could not be toggled back either. Users get no visual confirmation of which competencies are disabled.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template partially updates the model, leaving graph/metrics inconsistent
    - Actual: Clicked "Apply" on the Software Engineer template while HR Manager model was loaded. Page header updated to "Software Engineer" / "Engineering", the template card now shows "Current" on Software Engineer, and Critical Path card shows Software Engineer-specific competencies (Programming Languages, Problem Solving, System Design). However the graph canvas still renders the OLD HR Manager nodes/edges unchanged (Talent Acquisition, HR Compliance, Performance Management, Employee Relations, Leadership, Organizational Development with their original weights), and Coverage/Avg Level (68%/3.3) don't correspond to a fresh 6-node Software Engineer model. The model state is left inconsistent between header/metrics and the visible graph.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Save Current template feature does not persist or surface a new template
    - Actual: Clicked "Save Current" button in the Template Library panel. No dialog/prompt appeared to name the new template, no toast/confirmation notification was shown (aria-live regions remained empty), and the Template Library list still shows only the original 5 templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) with no new custom template added or any "My Templates" section to retrieve it from.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [ ] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.
  - Bug Report:
    - Issue: Simulation results don't reflect the configured scenario adjustments in quantitative metrics
    - Actual: Ran Promotion simulation with technical Weight Change -5%, behavioral Weight Change +10%. A toast appeared ("Simulation Applied - Competency levels and weights have been adjusted based on the scenario.") and the Critical Path list changed slightly (Collaboration added, +1 indicator), but Coverage (68%) and Avg Level (3.3) remained exactly unchanged, and the individual node "Weight" values in the graph (90%, 80%, 85%, 75%, 70%, 80%) were identical before and after running the simulation — i.e. the claimed level/weight adjustments are not reflected anywhere quantitatively.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: Explicit Save does not persist model state across navigation
    - Actual: On the HR Manager model, toggled node switches, applied the Software Engineer template, ran a simulation, then clicked "Save" (toast confirmed "Model Saved - Your changes have been saved successfully"). Navigated away via the "Models" nav link to the home list, then re-opened the HR Manager card. The model reverted entirely to its pristine original state (header "HR Manager", Coverage 71%, Avg Level 3.5, Critical Path: Talent Acquisition/Employee Relations/HR Compliance, all switches checked) — none of the prior edits or the "saved" state were restored, indicating Save does not actually persist changes.


## Constraint
- [X] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [X] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.

- [ ] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.
  - Bug Report:
    - Issue: Risk gap analysis does not detect/flag gaps when critical competencies are disabled
    - Actual: Disabled two "Critical"-flagged competencies (Talent Acquisition, then HR Compliance) via their node switches. Coverage, Avg Level, and Critical Path composition all changed in response (e.g. Coverage went 71%→68%→73%, Critical Path count changed 3→4 with a "+1" indicator), proving the model does recalculate on structural change. However "Risk Gaps" remained "0 identified" throughout both disables, even though disabling Critical-domain competencies is exactly the scenario that should surface a risk gap. No risk warning was ever generated.

- [ ] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.
  - Bug Report:
    - Issue: No prominent alert/notification is shown when a potential problem (risk gap) exists
    - Actual: Checked all [role=status], [role=alert] and [aria-live] regions after disabling two Critical-flagged competencies (Talent Acquisition, HR Compliance): both live-region containers (aria-live="polite" and aria-live="assertive") were empty, no toast/banner appeared, and the Risk Gaps card just static-displays "0". Since CT-17 already shows risk gaps aren't even being computed, there is no icon/text alert surfaced anywhere in the UI for this potential problem.