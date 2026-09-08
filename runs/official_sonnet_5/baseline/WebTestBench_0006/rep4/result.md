# Test Result

## Functionality
- [X] FT-1: Users can create financial transactions with a type, date, description, amount, and category, and may also record a reference, notes, and tax-related status.

- [X] FT-2: Users can view a summary of their financial status, including total balance, current-month income and expenses, net profit or loss, financial trends, spending by category, and recent transactions.

- [X] FT-3: Users can search transactions by keywords contained in their descriptions or references.

- [X] FT-4: Users can review an annual tax summary containing taxable income, deductible expenses, net taxable income, category breakdowns, and the corresponding tax-related transactions.

- [X] FT-5: Users can filter financial transactions by income or expense type, category, and date range.

- [X] FT-6: A valid newly created transaction is retained and shown with the date, description, category, reference, amount, and tax-related status supplied by the user.

- [X] FT-7: Users can update or delete existing financial transactions, and the resulting records reflect those changes.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected with explanation
    - Actual: Setting From=2024-12-31 and To=2024-11-01 (start after end) does not trigger any validation error or explanation. Instead it silently shows 'No transactions found / Try adjusting your filters', indistinguishable from a valid empty-result search, giving no indication that the date range itself is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction can be created without a category
    - Actual: Filled Description and Amount but left Category as "Select category" (unselected) and clicked Add Expense. The transaction was created successfully and appears in the table with a blank Category cell (not "Unknown", just empty) instead of being blocked with a validation error.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category deletion not blocked/reassigned when transactions are assigned
    - Actual: Deleted "Office Supplies" category (3 assigned transactions) via Categories page. Confirmation dialog only stated "Are you sure you want to delete this category? Transactions using this category will not be deleted." with no option to reassign or block. Deletion proceeded immediately. On Transactions page, the 3 previously "Office Supplies"-categorized transactions (Office Supplies - Printer Paper & Ink -$156.00, IX18 Test Expense -$100.00, Office Equipment -$890.00) now display empty/blank Category cells (no badge, no "Uncategorized" label) - orphaned category references. App neither blocks deletion of in-use categories nor prompts/requires reassignment of affected transactions.


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
    - Issue: Chart Y-axis lacks precision for small non-zero values
    - Actual: After adding a current-month $75 expense, the Income vs Expenses chart's Y-axis tick labels all read "$0k" (evaluated via DOM: ["$0k","$0k","$0k","$0k","$0k"]), failing to distinguish the non-zero $75 value from zero.