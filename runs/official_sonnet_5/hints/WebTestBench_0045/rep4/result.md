# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Edit control fully disabled on blocked task instead of allowing partial edit
    - Actual: For blocked task 'Complete tax filing' (tasks.list.item.4), the Edit button is rendered with the disabled attribute (pointer-events:none, opacity 50%) and cannot be clicked (Playwright click timed out: 'element is not enabled'). There is no alternate inline control for editing description or priority — clicking the priority badge directly does nothing. So a blocked task's description and priority cannot be modified at all, contradicting the requirement that only title/deadline should be locked while description/priority remain editable.

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
    - Issue: Reopening a prerequisite leaves its completed dependent simultaneously completed and blocked
    - Actual: Setup: task1 'Review project proposal' completed → task4 'Complete tax filing' (depends on task1) completed. Then reopened task1 (toggled it back to active) with no warning or prevention. Result: task4 remains listed under the Completed filter with toggle state 'completed' (semantic_observe: state='completed', enabled=false), yet it also now displays the blocking warning 'Prerequisite "Review project proposal" incomplete' and has its edit/complete actions disabled — i.e. it is simultaneously marked completed and blocked, which the system neither prevented (reopening task1 succeeded unconditionally) nor resolved (task4 was not reverted to an incomplete/active state).


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: No hover tooltip on dependency connector line
    - Actual: The dashed vertical connector segments (class bg-dashed-line) rendered between dependency-chain tasks (task1→task4→task7) are plain non-interactive divs with no title/aria attributes or event listeners. Hovering over them (isolated from any other element) produces no popup/tooltip element in the DOM (checked for [role=tooltip], radix popper wrapper, .popover, [class*=tooltip] — none appeared).

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.