# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked tasks have disabled edit button, preventing any editing of description, priority, title, or deadline. FT-3 requires editing description and priority for blocked tasks, but edit button is disabled.
    - Actual: For blocked tasks, the edit button is disabled, making it impossible to open the edit dialog. No editing is possible for blocked tasks in the current implementation.

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

- [ ] CS-13: Users cannot create a direct or indirect circular dependency between tasks; any task whose selection would create a cycle is unavailable as a prerequisite.
  - Bug Report:
    - Issue: Unable to verify circular dependency prevention. When adding prerequisites via edit dialog, no tasks appear disabled/unavailable to prevent cycles.
    - Actual: All available tasks appear as selectable checkboxes without any indication of which would create cycles. No UI mechanism visible to prevent circular dependencies.

- [X] CS-14: A new task cannot be created from an empty or whitespace-only title, and attempting to do so leaves the task list unchanged.

- [X] CS-17: Completing a task does not delete it: it remains available in the completed view until the user reopens or deletes it.

- [ ] CS-22: Reopening a prerequisite of a completed dependent task cannot leave that dependent task simultaneously completed and blocked; the system either prevents the prerequisite from being reopened or returns affected dependents to a valid incomplete state.
  - Bug Report:
    - Issue: Cannot test specific scenario: completing a prerequisite of a completed dependent task and checking if it returns to valid state. Blocked edit buttons prevent creating the necessary test scenario.
    - Actual: Due to disabled edit buttons on blocked tasks and inability to manipulate prerequisite states after completion, the specific scenario described cannot be fully tested.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: No visual connections (dashed lines) detected between dependent tasks. Either not implemented or not visible in the accessibility/DOM tree.
    - Actual: Examined task display multiple times; no visual connecting lines or graphics between tasks with dependencies are visible in the interface or DOM.

- [ ] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.
  - Bug Report:
    - Issue: Hovering over prerequisite warning text does not display a popup or tooltip showing dependency chain details.
    - Actual: Attempted to hover over prerequisite warning "Prerequisite \"Test Task FT5 Complete\" incomplete"; no popup, tooltip, or additional UI elements appeared in the accessibility tree.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.