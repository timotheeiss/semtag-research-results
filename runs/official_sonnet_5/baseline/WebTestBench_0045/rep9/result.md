# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task's edit control is fully disabled instead of allowing partial editing
    - Actual: For blocked task "Complete tax filing" (prerequisite incomplete), the edit button is rendered with the disabled attribute and clicking it produces no response (no dialog opens, no fields become editable). Expected behavior per spec is that description and priority remain editable while title and deadline are locked; instead, the entire edit action is unavailable, so description and priority cannot be changed either.

- [X] FT-4: Users can create a task with a non-empty title; the new task is retained in the active list and the active-task count increases.

- [X] FT-5: When creating a task, users can provide a title, optional description, optional deadline, priority, and zero or more prerequisites, and the created task retains the submitted details.

- [X] FT-6: Users can update an unblocked task's title, description, deadline, priority, and prerequisites, and can delete a task; saved changes are retained and a deleted task is removed from the task views.

- [X] FT-7: Users can mark an eligible active task as completed; it leaves the active view, appears in the completed view, and the active and completed counts update.

- [X] FT-8: Users can switch between active and completed task views; each view shows only tasks in the corresponding state and displays the corresponding count.

- [X] FT-9: Users can assign one or more existing tasks as prerequisites of another task, and the saved task retains and displays the resulting dependency state.

- [X] FT-10: A task with any incomplete prerequisite is visibly distinguished as blocked, and its completion action remains unavailable until all prerequisites are complete.

- [X] FT-11: A blocked task displays a prominent warning that names its single incomplete prerequisite or reports the number of incomplete prerequisites when there are several.


## Constraint
- [X] CS-12: A task cannot be marked completed while any prerequisite is incomplete; after all prerequisites are completed, it becomes eligible for completion.

- [X] CS-13: Users cannot create a direct or indirect circular dependency between tasks; any task whose selection would create a cycle is unavailable as a prerequisite.

- [X] CS-14: A new task cannot be created from an empty or whitespace-only title, and attempting to do so leaves the task list unchanged.

- [X] CS-17: Completing a task does not delete it: it remains available in the completed view until the user reopens or deletes it.

- [ ] CS-22: Reopening a prerequisite of a completed dependent task cannot leave that dependent task simultaneously completed and blocked; the system either prevents the prerequisite from being reopened or returns affected dependents to a valid incomplete state.
  - Bug Report:
    - Issue: Reopening a prerequisite of a completed dependent leaves the dependent simultaneously completed and blocked
    - Actual: Completed "Write unit tests" (prerequisite: "Buy groceries", which was completed). Reopened "Buy groceries" from the Completed view — the app allowed this without warning. Afterward, "Write unit tests" remained in the Completed view (Completed count stayed part of the 3 completed tasks, active count 4 did not include it) while simultaneously displaying "Prerequisite \"Buy groceries\" incomplete" and having its reopen/edit buttons disabled. This is exactly the invalid state the item forbids: the dependent task is both completed (still listed under Completed) and blocked (shows unmet-prerequisite warning) at the same time. The system neither prevented reopening "Buy groceries" nor moved "Write unit tests" back to an incomplete state.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Hovering the visual dashed connection line between dependent tasks does not trigger any dependency-chain popup
    - Actual: The dashed connector between dependent task cards is rendered as an SVG path with class "stroke-warning/50" inside an <svg> that has CSS "pointer-events: none", so real mouse hovering over the visual line passes through to whatever is behind it (verified via document.elementFromPoint, which returned unrelated content divs, not the line). A small companion stub div (class "bg-dashed-line") near each card edge is the only real hoverable element in that area, but hovering it (verified with a real Playwright hover plus checking for any [role=tooltip] element) produced no popup at all. The dependency-chain popup only appears when hovering the blocked task's own warning label (see IX-20), not when hovering the connecting line itself, so the described interaction is not implemented.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.