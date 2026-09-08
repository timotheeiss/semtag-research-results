# Test Result

## Functionality
- [X] FT-1: Users can create financial transactions with a type, date, description, amount, and category, and may also record a reference, notes, and tax-related status.

- [X] FT-2: Users can view a summary of their financial status, including total balance, current-month income and expenses, net profit or loss, financial trends, spending by category, and recent transactions.

- [X] FT-3: Users can search transactions by keywords contained in their descriptions or references.

- [X] FT-4: Users can review an annual tax summary containing taxable income, deductible expenses, net taxable income, category breakdowns, and the corresponding tax-related transactions.

- [X] FT-5: Users can filter financial transactions by income or expense type, category, and date range.

- [X] FT-6: A valid newly created transaction is retained and shown with the date, description, category, reference, amount, and tax-related status supplied by the user.

- [ ] FT-7: Users can update or delete existing financial transactions, and the resulting records reflect those changes.
  - Bug Report:
    - Issue: Edit form does not prefill the existing category, so updating a transaction silently erases its category
    - Actual: Opening Edit on any transaction shows Category = "Select category" instead of the stored value (verified on seeded tx-10 "Software Subscription - Adobe CC", stored category "Software"). Editing "QA Consulting Retainer" (only toggling Tax Related off) saved the record with an empty CATEGORY cell, losing the previously assigned "Consulting". Delete and other field updates do work.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted with no validation message
    - Actual: Set From=2024-12-31, To=2024-11-01. No error, toast, or explanation of the invalid range; the table simply showed 0 rows with generic text "No transactions found / Try adjusting your filters".

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not validated; transaction created without a category
    - Actual: Filled only description "NoCategory Test" and amount 12.34, left Category as "Select category", clicked Add Expense. The dialog closed with no error/toast and the transaction was saved (row "Aug 27, 2026 | NoCategory Test | (blank category) | — | -$12.34").

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category deletion not blocked and no reassignment; transactions left with blank category
    - Actual: Category "QA Temp Expense" had 1 transaction ($250). Delete dialog only warned "Transactions using this category will not be deleted" and deletion succeeded. The transaction "QA Temp Expense Item" now displays an empty CATEGORY cell.


## Interaction
- [X] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [X] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Chart Y-axis labels rounded to whole thousands, so non-zero amounts are indistinguishable from zero
    - Actual: With August 2026 income $400 and expenses $300 plotted, the "Income vs Expenses" chart Y-axis ticks all read "$0k" ("$0k","$0k","$0k","$0k","$0k"). Earlier with a $2,500 value the ticks were "$0k","$1k","$1k","$2k","$3k" (duplicate labels, $300 still inside the "$0k" band).