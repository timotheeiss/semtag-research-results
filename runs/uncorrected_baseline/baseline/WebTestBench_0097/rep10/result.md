# Test Result

## Functionality
- [X] FT-1: Human resources professionals can search for competency models by job title or keywords, and the system can accurately return a list of matching models.

- [X] FT-2: Users can click on any model in the search results to enter the model's details page.

- [X] FT-3: Users can directly adjust the hierarchy of capabilities by dragging and dropping nodes on the front end, and the model structure remains correct after layout changes.

- [ ] FT-4: Users can modify the weight value of any edge, and the graph will be refreshed immediately after the modification, visually reflecting the weight change.
  - Bug Report:
    - Issue: Edge weight is not editable
    - Actual: Edges only display a static weight percentage badge (e.g. "80%") with no associated slider/input control in the accessibility tree (unlike nodes, which each expose a "Weight" slider). Selecting an edge (via click or keyboard Enter/double-click) only toggles a "selected" CSS state on the edge; no weight editor, popover, or dialog appears anywhere on the page. There is no discoverable way to change an edge's weight value.

- [ ] FT-5: Users can temporarily enable or disable certain capability nodes in the graph. When disabled, the node and its related edges will be visually grayed out or hidden, and will no longer participate in subsequent calculations.
  - Bug Report:
    - Issue: Disabled node not visually grayed out/hidden; switch state not reliably toggling
    - Actual: Clicked the "Programming Languages" node's enable/disable switch three times in succession. After every single click, the switch's aria-checked and data-state attributes remained "true"/"checked" (never showed unchecked/disabled), the node's inner div opacity stayed at 1, and its className never gained any disabled/grayed CSS class (stayed "competency-node bg-card p-4 min-w-[220px] border-node-critical" throughout). The node's connected edges (prog-lang→code-review, prog-lang→sys-design) also never received any grayed-out/hidden styling. Stats did shift after the first click (Coverage 68%→65%, Avg Level 3.3→3.2, Critical Path composition changed from 3 items to 5 items) suggesting some recalculation occurred, but subsequent clicks produced no further change/reversion, and critical path gaining items after supposedly disabling a critical node is inconsistent with expected disable behavior. There is no visible indication anywhere in the UI that the node is actually disabled.

- [X] FT-6: The system provides a "General Job Competency Model Template Library," which users can browse and preview by job type.

- [ ] FT-7: Users can retrieve any template from the template library, which will be loaded as a new model into the editing interface.
  - Bug Report:
    - Issue: Applying a template updates header/stats but not the actual graph nodes/edges
    - Actual: Clicked "Apply" on the "Product Manager" template from the Software Engineer model page. A toast confirmed "Applied 'Product Manager' template successfully," the page heading changed to "Product Manager", category to "Product", and summary stats updated (Coverage 78%, Avg Level 3.8, Critical Path listing "Product Strategy, User Research, Communication, +1"). However, the actual graph in the editor was NOT replaced: querying the DOM showed the 6 rendered node data-ids were still "prog-lang, sys-design, code-review, problem-solve, collab, agile" — the original Software Engineer competencies (Programming Languages, System Design, Code Review, Problem Solving, Collaboration, Agile Methodology) — none of which match the Product Manager competencies referenced in the new Critical Path text (Product Strategy, User Research, Communication). The node cards, edge labels, and weights all remained unchanged from the Software Engineer model. This is a data-consistency bug: the template does not actually load as a new model into the graph editor, only certain summary UI text is updated.

- [ ] FT-8: Users can save the edited model as a custom template, name it, and add a description. After saving, they can retrieve it from "My Templates" at any time.
  - Bug Report:
    - Issue: No name/description input for saved template, and no "My Templates" retrieval location exists
    - Actual: Clicked "Save Current" in the Template Library panel. A toast appeared ("Template Saved - Current model configuration has been saved as a template.") but no dialog, form, or input field ever appeared to let the user name or describe the new custom template (confirmed via DOM query: 0 elements with role="dialog" before/after the click). After saving, the Template Library list still showed only the original 5 fixed templates (Software Engineer, Product Manager, HR Manager, Data Scientist, Sales Manager) — no new sixth "custom" template entry appeared. A page-wide text search found no "My Templates" section/tab anywhere in the UI to retrieve the supposedly-saved template. The save action appears to be non-functional/cosmetic (toast-only), with no actual persistence or retrieval mechanism.

- [X] FT-9: The system supports scenario-based simulation modes such as "recruitment," "training," and "promotion," and users can switch between different scenarios.

- [ ] FT-10: In simulation mode, users can adjust the model structure, and the system can simulate and demonstrate the quantitative or qualitative impact of this adjustment on key indicators in the scenario.
  - Bug Report:
    - Issue: Running a simulation does not visibly apply quantitative changes to node weights/levels or key indicators
    - Actual: On the Promotion tab (preset adjustments: technical Weight Change -5%, behavioral Weight Change +10%, leadership Weight Change +20%), clicked "Run Simulation". A toast appeared: "Simulation Applied - Competency levels and weights have been adjusted based on the scenario." However, direct DOM inspection of all 6 graph node cards showed their Weight values were completely unchanged from their pre-simulation values (Programming Languages 90%, System Design 80%, Code Review 60%, Problem Solving 90%, Collaboration 70%, Agile Methodology 50% — identical before and after). Top-level Coverage (78%), Avg Level (3.8), and Risk Gaps (0) stats also remained bit-for-bit identical before and after running the simulation. Only the Critical Path competency-name list text shuffled slightly (different item order/composition), which is not a reliable/clear quantitative or qualitative impact indicator. The simulation "Run" action appears to be non-functional beyond displaying a success toast — no real structural adjustment or impact is applied to or visible in the model.

- [ ] FT-11: When a user leaves the page and returns, the system should be able to restore the previously edited model state, or provide an explicit save/load mechanism.
  - Bug Report:
    - Issue: No state persistence: navigating away and back resets model to default; Save button is non-functional
    - Actual: Toggled the "Programming Languages" node's switch (Coverage changed 68%→65%), then clicked the header "Save" button, which displayed a toast: "Model Saved - Your changes have been saved successfully." Navigated to the Models list via the "Models" nav link, then re-opened the "Software Engineer" model. Coverage was back to the default 68% (not the saved 65%), confirming the change was NOT persisted despite the explicit "Save" confirmation toast. Separately, applying a template (Product Manager) and running a scenario simulation (see FT-7/FT-10) also did not survive navigating away and back — re-opening "Software Engineer" showed pristine default stats (Coverage 68%, Avg Level 3.3, Critical Path 3 items: Programming Languages/Problem Solving/System Design) and the "Software Engineer" template card was again marked "Current" in the Template Library, with no trace of any prior edits, applied template, or simulation. There is no working implicit (auto-restore) or explicit (Save button) state persistence mechanism in the app.


## Constraint
- [ ] CS-12: The system can prevent invalid operations that may lead to calculation errors, such as adding dependency edges to disabled capabilities.
  - Bug Report:
    - Issue: Disabled-node edge constraint not enforced / not verifiable due to broken disable feature
    - Actual: After toggling the "Programming Languages" node's disable switch multiple times (see FT-5), its connection handles still carried the full "connectable connectablestart connectableend" classes identical to fully enabled nodes (e.g. "sys-design", "collab") — there is no restriction placed on the handles of a supposedly-disabled node. Since the disable switch never actually changes aria-checked/data-state away from "true"/"checked" (per FT-5 finding), the app provides no functioning disabled state to test the constraint against, and the handles remain connectable regardless. Additionally, attempted drag-based edge creation (both a self-loop on prog-lang and a new edge from prog-lang to the unconnected "collab" node) produced no new edge and no error/warning message, indicating the app has no working add-edge interaction at all, so no invalid-operation guard (e.g. blocking edges to/from disabled competencies) could be observed or confirmed to exist.


## Interaction
- [X] IX-13: Each time the graph structure is modified, the system can recalculate and display the updated "capability coverage" percentage in real time.

- [X] IX-14: All graph structure editing operations are smooth and responsive, with no noticeable lag or delay when the model complexity is moderate.


## Content
- [X] CT-15: On the details page, the system presents the competency model in a clear, interactive, directed weighted graph structure, where nodes represent competencies and edges represent dependencies or reinforcement relationships.

- [ ] CT-16: After each structural change, the system can recalculate and highlight the "critical path" in the model in real time.
  - Bug Report:
    - Issue: Critical Path graph highlighting does not update to match the recalculated critical path list
    - Actual: After toggling the "Programming Languages" node's switch, the Critical Path stat panel text updated in real time from "3 competencies (Programming Languages, Problem Solving, System Design)" to "5 competencies (Problem Solving, System Design, Programming Languages, +2)". However, querying the graph node DOM showed only the same original 3 nodes (Programming Languages, System Design, Problem Solving) carry the "border-node-critical" highlight class — identical to before the change — while the 2 additional competencies now included in the recalculated Critical Path list (the "+2") are NOT visually highlighted in the graph at all. The graph's critical-path highlighting appears to be statically tied to each competency's fixed "Critical" category flag rather than the dynamically recalculated critical path, so the visual highlighting becomes inconsistent with the numeric/text Critical Path stat as soon as the model changes. Additionally, disabling a node paradoxically increasing the critical path count from 3 to 5 (rather than typically reducing it) further suggests the underlying critical-path calculation logic itself is unreliable.

- [X] CT-17: After each structural change, the system can analyze in real time and provide a "risk capability gap" warning.

- [X] CT-18: When a potential problem is detected, the system can immediately provide a clear text or icon prompt in a prominent position on the interface.