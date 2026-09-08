# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: No discoverable UI mechanism to modify an edge's weight value
    - Actual: Clicking, double-clicking, keyboard Enter, and context-menu on edge elements produced no editing dialog/modal/input (dispatching a synthetic click on the edge SVG only toggled a "selected" CSS class with no UI surfaced). The only visible "Weight" control found is a per-node Radix UI slider; repeated real trusted browser_drag attempts on the slider thumb (various endpoints) completed without error but aria-valuenow never changed from its initial value, so no working method to change an edge's weight and see the graph refresh was found.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Enable/disable toggle switch on competency nodes is non-functional
    - Actual: Clicking the per-node enable/disable switch (role="switch") on multiple nodes (Agile Methodology, Programming Languages) via real trusted clicks and native .click() consistently left aria-checked="true" / data-state="checked" unchanged, and the node's opacity/class showed no "disabled" or grayed-out styling. No console errors were logged. The node was only visually "selected" (blue outline), not disabled.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template updates header/metrics but not the actual graph/editing interface
    - Actual: Clicking "Apply" on the Product Manager template showed a success toast ("Applied \"Product Manager\" template successfully"), changed the page title to "Product Manager", and updated Coverage (78%)/Avg Level/Critical Path (Product Strategy, User Research, Communication) stats — but the actual graph nodes and edges in the editing canvas remained unchanged, still showing the original Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) and their edges instead of Product Manager's competencies.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Saving current model as custom template lacks name/description input and is not retrievable
    - Actual: Clicking "Save Current" only showed a toast ("Current model configuration has been saved as a template") with no dialog/modal prompting for a template name or description. After saving, the homepage "Templates" stat still read "5" (unchanged) and no new custom template card appeared in the Template Library list or anywhere else on the site (no "My Templates" section exists).

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [X] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: Explicit "Save" does not actually persist edited model state
    - Actual: After running a Promotion simulation (which changed Critical Path from 3 to 4 competencies) and clicking the header "Save" button (which showed a "Model Saved - Your changes have been saved successfully" toast), navigating away to the home page and back into the Software Engineer model reverted Critical Path back to the original 3 competencies (Programming Languages, Problem Solving, System Design) and Weight Change/Level Change sliders reset to 0. The save action gives a false success confirmation but does not actually persist state.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Cannot verify prevention of invalid operations because prerequisite disable functionality is broken
    - Actual: The constraint requires preventing invalid operations such as adding dependency edges to disabled capabilities, but the enable/disable switch on competency nodes does not work at all (confirmed in FT-5: aria-checked never toggles). Since no node can ever actually become disabled, there is no mechanism in the app that could enforce this constraint — the safety net is effectively absent. Additionally, attempting to draw a new edge between nodes via connection handles (dragging from a source handle to a target handle) did not create any new edge (edge count remained 5), suggesting the app may not support user-initiated edge creation via the graph UI at all, further preventing verification of edge-related validation rules.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [X] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.

- [ ] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.
  - Bug Report:
    - Issue: Risk Gaps indicator never updates and underlying calculation is unstable
    - Actual: The "Risk Gaps" stat consistently showed "0 identified" across all tested structural/scenario changes, including running Recruitment simulation with +5% technical weight change and Promotion simulation with -5% technical/+10% behavioral weight changes — no risk gap was ever surfaced even though such adjustments substantially changed Coverage and Critical Path. Additionally, repeatedly pressing ArrowLeft on the "Level Change" slider (attempting a negative adjustment) corrupted the slider's value to aria-valuenow="NaN", and after running a simulation, the Coverage and Avg Level stats also displayed "NaN%"/"NaN"/"N/A", indicating the underlying calculation engine is unstable and the Risk Gaps warning analysis does not reliably function.

- [ ] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.
  - Bug Report:
    - Issue: No functional prompt/warning UI ever surfaces for detected problems
    - Actual: The only candidate UI for problem prompts is the "Risk Gaps" stat card (with a warning-style icon) in the top summary bar, but it remained "0 identified" throughout all tests regardless of significant model changes (simulations, weight/level adjustments), so no actual text/icon warning was ever observed to appear. The "Analytics" nav link (which might otherwise host a dedicated warnings/insights view) is a non-functional placeholder (href="#") that does not navigate anywhere or reveal any additional problem-detection UI.