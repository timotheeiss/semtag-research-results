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
    - Issue: Missing required field validation lacks user-facing feedback
    - Actual: Submitting the Add Transaction form with empty description and amount kept the dialog open (transaction not created) but displayed no error text, no aria-invalid attributes, and no toast/notification identifying the missing description or amount to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid inverted date range is silently accepted with no user-facing explanation; app should reject or explain the invalid range rather than just showing empty results indistinguishable from a valid-but-empty filter.
    - Actual: Setting From=2024-12-31 and To=2024-12-01 (an inverted/invalid range) silently produces a &quot;No transactions found / Try adjusting your filters&quot; empty state. No validation error, warning, or explanation is shown to the user indicating the date range itself is invalid (e.g., that From is later than To).

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created without a category
    - Actual: Filled description and amount but left Category as "Select category" and clicked Add Expense; the transaction was created successfully and appeared in the table with a blank category cell instead of being rejected.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category that is actively in use by transactions is not blocked and does not require reassignment; it silently orphans the affected transactions' category field.
    - Actual: Deleting the &quot;Travel&quot; category (which had 1 transaction assigned, &quot;Business Trip - Conference&quot;) was allowed without being blocked and without prompting for reassignment. The confirmation dialog only states &quot;Transactions using this category will not be deleted&quot; as a generic warning (shown identically even for a 0-transaction category). After confirming deletion, the transaction row for &quot;Business Trip - Conference&quot; now shows a blank/empty category cell instead of &quot;Travel&quot;, leaving an orphaned reference with no reassignment step.


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
    - Issue: Chart axis labels lack precision for values under $500
    - Actual: Income vs Expenses chart Y-axis gridlines, scaled to a max of ~$999, displayed "$0k" for two different non-zero gridline levels (e.g. 0 and ~$250) in addition to zero, formatted only in whole $k units so amounts below $500 are indistinguishable from $0.