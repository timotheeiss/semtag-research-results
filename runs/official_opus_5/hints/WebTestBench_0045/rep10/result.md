# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task is fully non-editable; description and priority cannot be changed
    - Actual: For blocked task 4 "Complete tax filing", the edit button is disabled=true and clicking it (programmatic click, bypassing pointer-events) does not open task.form. A DOM scan of the blocked task card shows only three controls: complete (disabled), edit (disabled), delete (enabled) — no inline description or priority editor. Therefore the required "description and priority remain modifiable while blocked" behavior is absent; title/deadline are correctly unchangeable only because nothing is changeable.

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
    - Issue: Reopening a prerequisite leaves its completed dependent simultaneously completed and blocked, with no way to recover
    - Actual: Setup: "Complete tax filing"(4) has prerequisite "Review project proposal"(1); both were completed. Reopening task 1 was allowed (it returned to active, counts 4 active/4 completed → 5 active/3 completed). Task 4 stayed in the COMPLETED view while simultaneously showing blocked-warning 'Prerequisite "Review project proposal" incomplete', with complete.enabled=false and edit.enabled=false. The system neither blocked the reopen nor returned task 4 to an incomplete state, and because its toggle is now disabled the user cannot reopen task 4 to fix it.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Dependency connector lines are not hoverable and their tooltip never renders visibly
    - Actual: Dashed connectors exist (SVG overlay in tasks.list with 3 connections, each a visible dashed path plus a 16px-wide transparent hit path wrapped in a Radix tooltip trigger <g> with onMouseEnter/onPointerMove). Two failures: (1) Not reachable — the overlay svg has pointer-events:none and sits at z-0 behind the task cards; sampling 120 points along every connector, document.elementFromPoint never returned the connector path (0 reachable points), so a real mouse always hits the card instead. Playwright hover also refuses the path/g because their bounding boxes are zero-width. (2) Even when the trigger is activated programmatically, the popup does not display: its content measured 0x0 with computed opacity 0, while on the same page the blocked-warning tooltip rendered visibly at 248x70 with opacity 1. The chain text ("Dependency Chain / Complete tax filing pending / Review project proposal pending") exists in the DOM but is never visibly presented.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.