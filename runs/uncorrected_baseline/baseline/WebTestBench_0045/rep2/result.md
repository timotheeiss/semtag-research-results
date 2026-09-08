# Test Result

## Functionality
- [X] FT-1: Users can sort the list by priority.

- [X] FT-2: Users can sort the list by deadline.

- [ ] FT-3: Users can modify the content and priority of any task.
  - Bug Report:
    - Issue: Edit fully disabled for tasks with incomplete prerequisites, contrary to spec
    - Actual: Editing an unblocked task (Schedule dentist appointment) works correctly - description changed to "Updated via QA test." and priority changed to High, persisted after Save. However, per the app's own stated behavior, a task with an incomplete prerequisite (e.g. "QA Test Task", blocked by "Buy groceries") should still allow modification of description and priority fields. In practice the Edit button for such blocked tasks is fully disabled (disabled, pointer-events-none, opacity-50), so content/priority cannot be modified for "any task" as required - blocked tasks are entirely unmodifiable.

- [X] FT-4: Users can add to-do items

- [X] FT-5: Users can fill in the details of the to-do item (title, optional description, and due date).

- [X] FT-6: Users can edit, complete, or delete tasks.

- [X] FT-7: Users can mark tasks as completed.

- [X] FT-8: Users can view ongoing tasks.

- [X] FT-9: Users can bind one or more prerequisite tasks to any task.

- [X] FT-10: The system will automatically gray out tasks that have not completed their prerequisite tasks.

- [X] FT-11: The system will automatically mark tasks whose prerequisite tasks have not been completed as "Dependency on xxx task not completed".


## Constraint
- [X] CS-12: Tasks with dependencies cannot be marked as completed until the dependencies are completed.

- [X] CS-13: If task A has become a prerequisite task for task B, then task B cannot be chosen as a prerequisite task for task A.

- [X] CS-14: If a title is not entered when creating a new to-do item, it cannot be created.

- [X] CS-15: The date selection must be in the correct date format.

- [ ] CS-16: The deadline cannot be a date that has already passed.
  - Bug Report:
    - Issue: Past deadline dates are accepted without restriction
    - Actual: In the Create Task dialog, entering a past deadline date (2026-08-01, while today is 2026-09-02) did not disable the "Create Task" button nor show any validation error. Clicking "Create Task" successfully created the task, which then displayed "Overdue: Aug 1" on the card instead of being blocked at creation time.

- [X] CS-17: Tasks marked as completed should not be automatically deleted; they should remain visible.


## Interaction
- [ ] IX-18: A details pop-up window will appear when the user hovers over the task.
  - Bug Report:
    - Issue: No details pop-up on general task hover
    - Actual: Hovering over a task's card or its title (e.g. "Review project proposal", "Schedule dentist appointment", "Complete tax filing") produces no pop-up/tooltip at all. A pop-up ("Dependency Chain") only appears when hovering the specific small "Prerequisite ... incomplete" badge/icon on blocked tasks - this is the dependency-hover feature covered by IX-20, not a general "hover over the task shows details" feature.

- [X] IX-19: Provide visual feedback when creating a new to-do item without adding any title content.

- [X] IX-20: Visual feedback when the user hovers over the dependency list.


## Content
- [X] CT-21: The main page must display the task title, due date, and completion status.