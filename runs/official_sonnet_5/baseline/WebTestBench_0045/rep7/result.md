# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Edit action fully disabled for blocked tasks instead of allowing description/priority edits
    - Actual: For blocked task "Submit final report" (prerequisite "Review project proposal" incomplete), the edit (pencil) icon button has the disabled attribute and clicking it does not open any edit dialog or inline editor. Clicking the task title also does nothing. There is no way to modify description or priority while the task is blocked — the entire edit capability is disabled, not just title/deadline.

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
    - Issue: Reopening a prerequisite leaves completed dependent task in an invalid completed+blocked state
    - Actual: Completed "Submit final report" (dependent on "Complete tax filing" and "Review project proposal"). Reopened prerequisite "Complete tax filing" via its reopen toggle in the Completed view. Result: "Complete tax filing" successfully returned to Active (not prevented). However "Submit final report" remained in the Completed tab (Completed count stayed 4, task still listed under Completed) while simultaneously displaying the blocked-state warning "Prerequisite \"Complete tax filing\" incomplete" and a disabled reopen/complete toggle — i.e., it is both marked completed (still in Completed view) and blocked at the same time, which violates the required behavior of either preventing the reopen or moving the dependent back to a valid incomplete/active state.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Dashed dependency connector lines are not interactive/hoverable
    - Actual: The dashed connections are rendered two ways: (1) an SVG overlay (class "... pointer-events-none z-0") containing dashed paths between dependent/prerequisite cards, and (2) small "bg-dashed-line" divs between adjacent cards. The SVG paths are geometrically zero-width (straight vertical lines) and are visually obscured by the solid task-card divs stacked above them (Playwright reports "<div> subtree intercepts pointer events" when attempting to hover the connector at any point along its length), so a real pointer can never reach them except at the small gap segment. Hovering that exposed small dashed-line div (between "Review project proposal" and its dependent "Complete tax filing") produced no tooltip or popup at all — no dependency chain information was shown. Only hovering the textual prerequisite warning badge on the blocked task (tested separately under IX-20) shows the dependency chain popup; the visual dashed connector itself does not.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.