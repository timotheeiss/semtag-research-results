# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task cannot be edited at all; description and priority are not modifiable
    - Actual: On blocked task "Complete tax filing" (id 4) the edit button has DOM property disabled=true. Clicking it (Playwright and JS-dispatched click) opens no form: [data-semtag-id='task.form'] is absent. No alternative affordance exists on the blocked card to change description or priority, so the required partial-edit (description + priority editable, title + deadline locked) is unavailable.

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
    - Issue: Reopening a prerequisite leaves its completed dependent simultaneously completed and blocked, and the dependent becomes unrecoverable
    - Actual: Setup: "Complete tax filing" (id 4) depends on "Review project proposal" (id 1); both were completed. Reopening task 1 was allowed (active 3->4, completed 4->3) and task 4 was NOT returned to an incomplete state. Task 4 now simultaneously: stays in the Completed view with complete state="completed" and line-through title, AND renders the blocked warning 'Prerequisite "Review project proposal" incomplete'. Its complete toggle is disabled=true and edit is disabled=true, so the user cannot un-complete or edit it to escape the invalid state; only deletion remains.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Dashed dependency connection lines are not hoverable; no dependency-chain popup appears
    - Actual: The SVG holding the dashed connectors has computed pointer-events:none and z-index:0 and is painted behind the task cards. document.elementFromPoint at the midpoint of all 3 connection paths returns task-card DIVs, never the connection path, so the mouse can never reach the line's hover target. Real Playwright hover on the visible dashed connector stub (.bg-dashed-line) produced 0 tooltips and 0 "Dependency Chain" nodes. Playwright also refuses to hover the paths themselves (zero-width bounding box / "element is not visible").

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.