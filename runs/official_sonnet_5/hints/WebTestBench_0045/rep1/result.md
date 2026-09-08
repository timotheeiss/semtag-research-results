# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Edit button fully disabled for blocked tasks, preventing any field modification
    - Actual: Clicking the Edit button on a blocked task (e.g. 'Complete tax filing') timed out because the button element has the `disabled` attribute and is not clickable at all. No edit form opens, so description and priority cannot be modified as required, only title/deadline should have been restricted.

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
    - Issue: Reopening a prerequisite leaves a completed dependent task simultaneously completed and blocked
    - Actual: Completed 'Review project proposal v2' (prerequisite) then completed dependent 'Complete tax filing'. Reopening 'Review project proposal v2' succeeded without restriction, and 'Complete tax filing' remained in the Completed view (count stayed 4, line-through styling retained, complete toggle disabled) while simultaneously showing blocked-warning 'Prerequisite \"Review project proposal v2\" incomplete'. The system neither prevented the reopen nor reverted the dependent task to an incomplete state.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Dependency connector lines are not hoverable (pointer-events disabled)
    - Actual: The dashed dependency connector lines are rendered as SVG `<path>` elements (stroke-dasharray, arrowhead markers) inside an `<svg class="absolute top-0 left-0 w-full h-full pointer-events-none z-0">` overlay. Because the overlay has `pointer-events-none`, mouse events pass through the lines entirely, so they cannot be hovered. Directly hovering the small decorative connector div ('.bg-dashed-line') between task cards also produced no tooltip/popup in an isolated test (mouse moved to a neutral element first, then to the connector: no 'Dependency Chain' popup appeared). Only hovering the blocked-task's warning label (a separate element) shows the dependency chain tooltip (see IX-20), not the visual connection line itself.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.