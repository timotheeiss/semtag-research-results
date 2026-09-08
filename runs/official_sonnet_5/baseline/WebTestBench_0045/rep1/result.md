# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task's edit button is fully disabled instead of allowing partial edit
    - Actual: For the blocked task "FT5 Full Detail Task" (2 prerequisites incomplete), the edit (pencil) button is rendered disabled and clicking it has no effect — no edit dialog opens at all. Users cannot modify description or priority while blocked; only delete remains available. Expected: title/deadline locked but description/priority still editable.

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
    - Issue: Reopening a completed prerequisite leaves the completed dependent task simultaneously completed and blocked
    - Actual: Reopened prerequisite "Buy groceries" (previously completed) while its dependent "FT5 Full Detail Task" was completed. The system did not prevent the reopen and did not move "FT5 Full Detail Task" back to active/incomplete state — it remains in the Completed view (completed count still includes it) yet now displays "Prerequisite \"Buy groceries\" incomplete" with a disabled toggle/edit, an invalid simultaneously-completed-and-blocked state.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Hovering the visual dependency connector does not show any popup
    - Actual: Located the visual dashed connection element (div.bg-dashed-line) rendered between "Review project proposal" and its dependent "Cycle Test Task A" cards. Hovering over it (and waiting 1.5s for any delayed tooltip) produced no popup, tooltip, or dependency-chain detail in the DOM/accessibility tree.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.