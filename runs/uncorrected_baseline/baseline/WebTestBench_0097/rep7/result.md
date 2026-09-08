# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: Edge/competency weight cannot actually be modified by the user
    - Actual: There is no direct UI to edit an edge's weight (clicking or double-clicking an edge, e.g. "Edge from prog-lang to code-review" 80%, only selects/highlights it — no editable field or popover appears). The closest control is each node's own "Weight" slider (e.g. Programming Languages Weight 90%), but this slider is non-functional: clicking, dragging (verified via Playwright's real dragTo and via focused keyboard ArrowLeft presses), and clicking directly on the slider track all leave aria-valuenow at 90 unchanged. Inspecting the DOM shows the Radix slider thumb/track lack the React Flow "nodrag" class, so pointer interactions on it are captured by the node's own drag handler (dragging it moves the whole node instead) rather than reaching the slider control, making weight adjustment impossible via mouse or keyboard.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Competency enable/disable switch does not toggle
    - Actual: Clicking the on/off switch on a competency node (e.g. "Programming Languages", "Collaboration") via Playwright click, and even a native DOM .click() call, leaves the switch's aria-checked/data-state as "true"/"checked" — it never flips to unchecked. The node also shows no visual graying (opacity stays 1, no filter applied) after the click. Since the switch never actually disables, there is no way to verify grayed-out/hidden state or exclusion from calculations.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template does not actually load its competency graph
    - Actual: Clicking "Apply" on the "Product Manager" template shows a toast "Applied Product Manager template successfully", updates the page header to "Product Manager" and updates the summary stats (Coverage 78%, Critical Path listing "Product Strategy, User Research, Communication"), and marks it "Current" in the template list — but the actual graph canvas still renders the previous Software Engineer nodes/edges (data-id list remained ["prog-lang","sys-design","code-review","problem-solve","collab","agile"]). The visible graph is inconsistent with the applied template's stats, so the new model is not actually loaded into the editing interface.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: Custom template saving lacks naming/description and no "My Templates" retrieval exists
    - Actual: Clicking "Save Current" in the Template Library immediately shows a toast "Template Saved - Current model configuration has been saved as a template" with no dialog to enter a name or description. After saving, the Template Library list still shows only the same 5 built-in templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) — no new custom entry was added. A site-wide text search confirms there is no "My Templates" section anywhere on the page to retrieve saved custom templates.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [X] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: No persistence: Save produces no confirmation and model state is not restored after navigating away and back
    - Actual: After running the Training simulation (which set Coverage=60%, Avg Level=2.7, Critical Path=4 competencies) and clicking the "Save" button, no toast/confirmation appeared (the notifications <ol> list remained empty), and localStorage/sessionStorage remained completely empty (no keys) - indicating Save does not actually persist any data. Navigating to "Models" and back into the "Software Engineer" model showed Coverage=68%, Avg Level=3.3, Critical Path=3 competencies (Programming Languages, Problem Solving, System Design) - none of which match the simulated state (60%/2.7/4) nor even the original session-start default (74%/3.7/3), confirming the app has no working state-restoration or save/load mechanism; the model view simply re-initializes to an unpredictable baseline on each visit.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Cannot verify prevention of invalid operations (e.g., dependency edges to disabled competencies) because prerequisite features are non-functional
    - Actual: The canonical invalid-operation scenario (adding a dependency edge to/from a disabled competency) cannot be exercised at all: node disabling via the enable/disable switch is confirmed non-functional (see FT-5 FAIL - data-state never changes from "checked"). Attempted to test edge-creation validation directly by dragging from Programming Languages' source handle to Code Review's target handle (an already-existing edge, to check duplicate-edge prevention) via both browser_drag and manual PointerEvent (pointerdown/pointermove/pointerup) sequences - edge count stayed at 5 (no duplicate created). However, an identical drag attempt to create a brand-new, valid, non-duplicate edge (Programming Languages -> Agile Methodology, no pre-existing edge) ALSO produced no new edge (still 5 edges, unchanged), proving that edge creation via drag does not work at all in this environment - the "duplicate prevented" result is inconclusive because valid edge creation is equally broken. Since neither prerequisite mechanism (disabling a node, or creating any new edge) functions, the invalid-operation-prevention behavior this checklist item targets cannot be demonstrated or confirmed to work as intended.


## Interaction
- [ ] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.
  - Bug Report:
    - Issue: Real-time coverage recalculation is unreliable/not tied to verifiable model changes
    - Actual: All node-level graph-editing controls that should trigger recalculation are non-functional (weight slider - FT-4 FAIL, enable/disable switch - FT-5 FAIL, and the 5-segment "Level" indicator buttons under each node, which are clickable <button> elements but produce no change to the node's Level text or Weight slider value when clicked, e.g. clicking Agile Methodology's 5th level segment left it at Level=Intermediate, Weight aria-valuenow=50 unchanged). Despite no verifiable underlying data change, the Coverage stat itself fluctuated (74% -> 73%) immediately after that inert click, while re-clicking/selecting other nodes with no edits produced no further change (stable at 73%). This shows the Coverage recalculation is not deterministically driven by actual, verifiable graph/model edits - either it reacts to something other than real state changes, or the level control has a hidden/partial side-effect not reflected in its own displayed value. Since no working UI path exists to make a genuine, confirmed structural edit (node/edge weight, enable/disable, level) to cleanly validate correct real-time recalculation, this requirement cannot be considered met.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [ ] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.
  - Bug Report:
    - Issue: Critical path node highlighting on canvas desyncs from the Critical Path stat list after a structural update
    - Actual: Before running a simulation, the 3 nodes with the `border-node-critical` highlight class (Programming Languages, Problem Solving, System Design) exactly matched the Critical Path panel's listed 3 competencies. After running the Promotion simulation, the Critical Path stat panel updated to "4 competencies: Problem Solving, Programming Languages, Collaboration, +1", but DOM inspection of node classes showed only Programming Languages, System Design, and Problem Solving still carry `border-node-critical` - Collaboration (now listed as part of the critical path) has NO critical-path border highlight on the canvas. The visual highlighting failed to update in sync with the recalculated critical path list, leaving a stale/incorrect highlight state.

- [ ] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.
  - Bug Report:
    - Issue: Risk Gaps analysis never identifies any gaps even under severe simulated competency degradation
    - Actual: Ran the Training simulation after driving the "technical" category's Level Change slider to its minimum (-2) and Weight Change slider to its minimum (-30%) via keyboard (functional sliders, confirmed via aria-valuenow changes 30->0 and 4->0). This is applied to 3 technical competencies including two marked "Critical" (Programming Languages, System Design). Post-simulation stats: Coverage dropped 73%->60%, Avg Level dropped 3.7->2.7 (Advanced->Intermediate), Critical Path changed to 4 competencies - confirming the simulation was genuinely applied and had a severe negative impact. Despite this, "Risk Gaps" remained "0 identified" both before and after, indicating the risk-gap detection logic does not fire even when critical competencies are pushed to significantly degraded levels/weights, i.e., the feature does not functionally detect/warn about risk gaps.

- [ ] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.
  - Bug Report:
    - Issue: No prominent alert (text/icon) is ever surfaced for detected problems, because the underlying problem-detection (Risk Gaps) never activates
    - Actual: Inspected the Risk Gaps card DOM: it uses a static circle-alert icon with class "text-muted-foreground" that is not conditionally styled/colored based on the gap count (would presumably turn red/destructive only if count > 0, but this was never observed since Risk Gaps stayed "0 identified" even after driving the model to a severely degraded state via the Training simulation, see CT-17). A page-wide scan for destructive/alert-styled elements found only static "Critical" category badges and red-colored negative Weight Change deltas in the simulation panel - no dedicated prominent alert, toast, or banner tied to detected risk/problem conditions was found or triggered anywhere in the app. Since the risk-gap detection logic itself never fires, there is no demonstrable alert mechanism for surfacing detected problems to the user.