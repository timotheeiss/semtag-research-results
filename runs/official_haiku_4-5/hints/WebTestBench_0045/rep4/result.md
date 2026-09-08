# Test Result

## Functionality
- [X] FT-1: Users can sort the tasks in the current view by priority, with high-priority tasks before medium-priority tasks and medium-priority tasks before low-priority tasks.

- [ ] FT-2: Users can sort the tasks in the current view by deadline, with the earliest deadlines first and tasks without deadlines after dated tasks.
  - Bug Report:
    - Issue: Sort by deadline not working
    - Actual: Combobox selection changes don't affect task order. After selecting 'Deadline' option, the combobox still displays 'Priority' and task order remains: Review project proposal (Aug 27), Complete tax filing (Sep 8), Submit final report (Aug 30), Schedule dentist appointment (Sep 1), Buy groceries (Tomorrow)."

- [ ] FT-3: When a task is blocked by an incomplete prerequisite, users can still change its description and priority, but cannot change its title or deadline.
  - Bug Report:
    - Issue: Blocked task edit button is completely disabled
    - Actual: Task 4 (blocked task) has edit button with enabled=false. The button is disabled and cannot be clicked. There are no inline editable fields for description or priority. Users cannot modify blocked tasks as required."

- [ ] FT-4: Users can create a task with a non-empty title; the new task is retained in the active list and the active-task count increases.
  - Bug Report:
    - Issue: Task creation with title not working
    - Actual: After filling title "Test Task Title" in the form, Create Task button becomes enabled. Clicking Create Task closes the dialog but the task is not added to the active list. Task count remains "5 active, 2 completed".

- [ ] FT-5: When creating a task, users can provide a title, optional description, optional deadline, priority, and zero or more prerequisites, and the created task retains the submitted details.
  - Bug Report:
    - Issue: Task creation with full details not working
    - Actual: Form accepts title, description, and other fields, but created task is not persisted. The task creation feature appears to be non-functional.

- [ ] FT-6: Users can update an unblocked task's title, description, deadline, priority, and prerequisites, and can delete a task; saved changes are retained and a deleted task is removed from the task views.
  - Bug Report:
    - Issue: Cannot test - task creation not working
    - Actual: FT-6 requires ability to create and modify tasks. Since FT-4 (task creation) is non-functional, this test cannot be completed. Task update/delete functionality cannot be verified."

- [ ] FT-7: Users can mark an eligible active task as completed; it leaves the active view, appears in the completed view, and the active and completed counts update.
  - Bug Report:
    - Issue: Marking task as completed not working
    - Actual: Clicking the complete toggle on Task 1 "Review project proposal" has no effect. Task remains in active state with no change in task counts. Task completion functionality appears non-functional."

- [ ] FT-8: Users can switch between active and completed task views; each view shows only tasks in the corresponding state and displays the corresponding count.
  - Bug Report:
    - Issue: Toggle between active and completed task views not working
    - Actual: Clicking the "Completed 2" button does not switch the view. The page continues to show the 5 active tasks, and the toggle state remains "unselected".

- [ ] FT-9: Users can assign one or more existing tasks as prerequisites of another task, and the saved task retains and displays the resulting dependency state.
  - Bug Report:
    - Issue: Cannot test - task creation not working
    - Actual: FT-9 requires ability to create tasks and assign prerequisites. Since task creation (FT-4) is non-functional, this feature cannot be tested."

- [X] FT-10: A task with any incomplete prerequisite is visibly distinguished as blocked, and its completion action remains unavailable until all prerequisites are complete.

- [X] FT-11: A blocked task displays a prominent warning that names its single incomplete prerequisite or reports the number of incomplete prerequisites when there are several.


## Constraint
- [X] CS-12: A task cannot be marked completed while any prerequisite is incomplete; after all prerequisites are completed, it becomes eligible for completion.

- [ ] CS-13: Users cannot create a direct or indirect circular dependency between tasks; any task whose selection would create a cycle is unavailable as a prerequisite.
  - Bug Report:
    - Issue: Cannot test - task creation not working
    - Actual: CS-13 requires ability to create multiple tasks and attempt to create circular dependencies. Since task creation is non-functional, this constraint cannot be verified."

- [X] CS-14: A new task cannot be created from an empty or whitespace-only title, and attempting to do so leaves the task list unchanged.

- [ ] CS-17: Completing a task does not delete it: it remains available in the completed view until the user reopens or deletes it.
  - Bug Report:
    - Issue: Cannot test - task completion not working
    - Actual: CS-17 requires ability to complete a task and verify it remains in completed view. Since task completion (FT-7) is non-functional, this constraint cannot be verified."

- [ ] CS-22: Reopening a prerequisite of a completed dependent task cannot leave that dependent task simultaneously completed and blocked; the system either prevents the prerequisite from being reopened or returns affected dependents to a valid incomplete state.
  - Bug Report:
    - Issue: Cannot test - task completion and prerequisite reopening not working
    - Actual: CS-22 requires ability to complete tasks and reopen prerequisites. Since task completion (FT-7) is non-functional, this complex constraint cannot be verified."


## Interaction
- [ ] IX-18: Hovering over a visual connection between dependent tasks displays the related dependency chain and the completion state of its tasks.
  - Bug Report:
    - Issue: Visual dashed line connections not found or not interactive
    - Actual: No interactive visual connections (dashed lines) between dependent tasks are accessible. While dependency chain tooltips appear on hovering over prerequisite warnings, the actual visual line connections cannot be identified or interacted with."

- [X] IX-20: Hovering over a blocked task's prerequisite warning displays the dependency chain and identifies which prerequisite tasks are still pending.


## Content
- [X] CT-21: Each task displays its title and priority, displays its description and deadline when supplied, exposes any blocking prerequisite state, and visibly distinguishes completed tasks from active tasks.