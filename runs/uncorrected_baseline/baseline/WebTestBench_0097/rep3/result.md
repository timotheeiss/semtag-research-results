# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: No UI mechanism to modify edge weight
    - Actual: Edges display a weight percentage label (e.g. "Edge from python to ml" 90%) but clicking/double-clicking the edge only toggles a "selected" CSS class with no editor, slider, or dialog appearing (dialog count and slider count unchanged before/after). Real mouse clicks on the edge/label are also blocked because overlapping node cards intercept pointer events, making the edge unreachable via normal UI interaction. Only per-node "Weight" sliders exist, which modify the node's own weight, not edge weights.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Node enable/disable switch does not toggle
    - Actual: Clicking the switch control on any competency node (tested Data Visualization and Business Acumen, via real UI click and via programmatic .click()) leaves aria-checked="true" and node opacity=1 unchanged. No node ever becomes visually grayed out/hidden or excluded, so disabling cannot be verified to affect calculations.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template does not actually load its competency graph
    - Actual: Clicking "Apply" on the "Software Engineer" template updated the page header to "Software Engineer"/"Engineering" and changed the Coverage/Avg Level/Critical Path summary text (critical path list showed "Programming Languages, Problem Solving, System Design" - Software Engineer competency names), but the actual rendered graph nodes remained unchanged, still showing the original Data Scientist competencies (Machine Learning, Statistical Analysis, Python Programming, Data Visualization, Business Acumen, Data Storytelling) with their original ids (ml, statistics, python, data-viz, business-acumen, storytelling). The model was not truly loaded into the editing interface.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: No naming/description capture and no retrievable 'My Templates' location
    - Actual: Clicking 'Save Current' immediately showed a toast ('Template Saved' / 'Current model configuration has been saved as a template.') with no dialog to name the template or add a description first. After saving, the Template Library list still showed only the same original 5 templates (Software Engineer marked 'Current', Product Manager, HR Manager, Data Scientist, Sales Manager) - no new custom template entry appeared, and no 'My Templates' section exists anywhere on the page to retrieve it.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [X] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: Edited state / explicit save does not persist across navigation
    - Actual: On the Software Engineer model, raised Code Review's level to Expert (5th star), which correctly updated Coverage 68%→74% and Avg Level 3.3→3.7 (Advanced). Clicked the 'Save' button in the header (no dialog/confirmation appeared other than a toast). Then navigated back to the Models list via the back arrow and re-opened Software Engineer. The model reloaded with Coverage back to 68%, Avg Level back to 3.3 (Intermediate), and Code Review's level back to 'Intermediate' - the edit was lost. There is no working persistence mechanism: neither implicit state restoration nor the explicit 'Save' button retains user edits when leaving and returning to a model.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Cannot verify constraint enforcement; underlying edit operations are non-functional
    - Actual: Node disable (switch) does not work (see FT-5), so a "disabled capability" state can never be reached to test edge-to-disabled-node prevention. Additionally, attempting to create new edges via handle-to-handle drag (both a self-loop on Machine Learning and a normal new connection from Data Visualization to Statistical Analysis) produced no new edge and no warning/error message - edge creation appears unimplemented, so there is no observable validation/prevention mechanism for invalid dependency edges.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [ ] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.
  - Bug Report:
    - Issue: Critical path does not recalculate after structural/level changes
    - Actual: The Critical Path metric consistently listed the same 3 competencies (Machine Learning, Python Programming, Statistical Analysis) with count "3" before and after multiple level changes (raising Machine Learning to Expert, lowering Python Programming to Beginner) that did successfully change Coverage/Avg Level/Risk Gaps. Edge-weight editing and node disabling (the other structural levers) are non-functional (see FT-4/FT-5), so no structural change could be applied at all to verify real-time critical-path recalculation; the value observed never changed, suggesting it is a static/precomputed list rather than dynamically recalculated.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.