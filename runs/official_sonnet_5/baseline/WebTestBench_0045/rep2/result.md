# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task's edit action is fully disabled instead of allowing partial edits
    - Actual: For a blocked task ("Complete tax filing", blocked by incomplete "Review project proposal"), the Edit button is entirely disabled (clicking it does not open the Edit Task dialog at all, and no inline editing affordance exists for description/priority). Expected behavior per spec is that description and priority remain editable while title and deadline are locked; instead no editing of any field is possible while blocked.

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
    - Issue: Reopening a prerequisite leaves its completed dependent in an invalid completed+blocked state
    - Actual: "Buy groceries" was a prerequisite of the already-completed task "Review project proposal (Updated)". Reopening "Buy groceries" from the Completed view succeeded without any prevention or warning. Afterward, "Review project proposal (Updated)" remained in the Completed view/count (Completed 3, still counted as completed) while simultaneously displaying the blocked-task warning "Prerequisite \"Buy groceries\" incomplete" and a disabled reopen/complete toggle — i.e., the dependent task is simultaneously marked completed and blocked, which is exactly the invalid state the spec requires the system to prevent.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Hovering over the visible dashed connection line does not display the dependency chain popup
    - Actual: The app renders dashed SVG lines connecting dependent tasks with tooltip triggers (a &lt;g&gt; element with cursor-pointer and data-state) intended to show a "Dependency Chain" popup on hover. However, a decorative, non-interactive div (class "bg-dashed-line", the only visually rendered dash mark in the gap between cards) sits on top of the SVG connector with pointer-events:auto and default z-index, intercepting all pointer events at that location. A real Playwright hover on the visible dashed-line element (the only part of the connection a user can actually see and target) produced no tooltip/popup at all; document.elementFromPoint at the dash location resolves to the decorative div, never the SVG connector, so the dependency-chain popup described in the spec is unreachable via normal hovering.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.