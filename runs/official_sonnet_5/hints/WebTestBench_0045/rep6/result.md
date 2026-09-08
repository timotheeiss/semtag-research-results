# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [X] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task editing not partially available
    - Actual: For blocked tasks (e.g. 'Complete tax filing', 'QA Full Test Task'), the Edit button is entirely HTML-disabled (not just title/deadline fields), so the edit form cannot be opened at all. There is no alternate inline control to modify description or priority while blocked; clicking directly on the priority badge does nothing. Expected: description and priority remain editable while title/deadline are locked.

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
    - Issue: Reopening a prerequisite leaves dependent tasks completed and blocked simultaneously
    - Actual: Completed task 'Submit final report' (7) which depends on 'Review project proposal' (1) and 'Complete tax filing' (4). Reopened (un-completed) task 1 via its toggle in the Completed view - the app allowed this without warning. Afterward, tasks 4 and 7 remained in the Completed view/count (Completed 5, not moved to Active) while simultaneously showing blocked-warning='Prerequisite ... incomplete' and complete.enabled=false, edit.enabled=false. They did not appear in the Active view. Expected: either prevent reopening task 1, or automatically revert tasks 4/7 to an incomplete/active state.


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: No interactive dependency-chain popup on the visual connector
    - Actual: Found small decorative elements with class 'bg-dashed-line' (2px x 12px, no title/tooltip attributes) positioned between consecutive task cards in the list, seemingly just generic separators between every adjacent card rather than a meaningful connection specifically linking Task A to its prerequisite Task B. Hovering over this element with Playwright produced zero '[role=tooltip]' elements in the DOM - no dependency chain or completion-state popup appears. The only working dependency-chain tooltip is triggered by hovering the textual blocked-warning badge (see IX-20), not by the dashed line connector itself.

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.