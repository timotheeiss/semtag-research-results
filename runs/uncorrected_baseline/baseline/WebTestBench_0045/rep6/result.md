# Test Result

## Functionality
- [X] FT-1: Users can sort the list by priority.

- [X] FT-2: Users can sort the list by deadline.

- [X] FT-3: Users can modify the content and priority of any task.

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
    - Issue: Missing validation for past deadline dates
    - Actual: Entered deadline 2020-01-01 (past date) in Create Task form; Create Task button remained enabled and the task "Date Format Test Task" was created successfully, displayed as "Overdue: Jan 1" instead of being blocked.

- [X] CS-17: Tasks marked as completed should not be automatically deleted; they should remain visible.


## Interaction
- [X] IX-18: A details pop-up window will appear when the user hovers over the task.

- [X] IX-19: Provide visual feedback when creating a new to-do item without adding any title content.

- [X] IX-20: Visual feedback when the user hovers over the dependency list.


## Content
- [X] CT-21: The main page must display the task title, due date, and completion status.