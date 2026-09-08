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

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing required field validation lacks user-facing error messages
    - Actual: Submitting the Add Transaction form with empty Description and Amount silently failed to create the transaction (no new row added, dialog stayed open) but no error message, red border, aria-invalid attribute, or toast notification appeared anywhere in the DOM identifying which fields were missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start > end) not rejected or explained
    - Actual: Set From=2024-12-31 and To=2024-12-01 (start later than end). No validation error or explanation was shown; the app silently treated it as a normal filter and displayed 'No transactions found - Try adjusting your filters', identical to a generic empty-result message, not an explanation that the date range itself is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category validation lacks user-facing error identification
    - Actual: Submitting the Add Transaction form without selecting a Category silently failed to create the transaction (no new row added) but no error message or visual indicator identified the missing category to the user.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category deletion not blocked and no reassignment offered for categories in use
    - Actual: Deleted 'Interest Income' category (cat-3) which had 1 assigned transaction (tx-9 'Bank Interest'). Confirmation dialog only warned 'Transactions using this category will not be deleted' with no reassignment option. After confirming delete, tx-9's category field became blank (empty string) in the transactions table, leaving the transaction with no category.


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
    - Issue: Chart Y-axis labels lack precision to distinguish small non-zero values from zero
    - Actual: Income vs Expenses chart Y-axis gridline labels read: '$0k','$0k','$1k','$1k','$1k' for what should be 5 evenly-spaced values between $0 and ~$1000 (e.g. 0, 250, 500, 750, 1000). Values like $250 round to '$0k', identical to the actual zero label, making it impossible to distinguish a non-zero value below $500 from zero on the axis.