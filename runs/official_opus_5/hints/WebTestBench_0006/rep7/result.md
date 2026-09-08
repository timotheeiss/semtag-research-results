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
    - Issue: Update silently discards the transaction's existing category (edit form does not pre-load it)
    - Actual: Delete works correctly (confirm dialog; list went 20→19 rows and Total Income reverted $47,842.50→$47,592.50). Update is defective: opening Edit on "QA Income Test Alpha" (category Consulting) pre-filled date/description/amount/reference/notes/type/tax-related, but the Category select showed "Select category" instead of "Consulting". Changing only description and amount and saving produced "Aug 15, 2026 | QA Income Test Beta | (blank category) | QA-REF-001 | +$300.00" — the previously assigned Consulting category was silently destroyed even though the user never touched that field.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted silently with no validation or explanation
    - Actual: Set From=2024-12-31 and To=2024-12-01 (start after end). The filter was accepted as-is: no error message, no toast, no aria-invalid and no native validationMessage on either date input. The table simply rendered 0 rows with the generic empty state "No transactions found / Try adjusting your filters", which does not identify the invalid range.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not validated; transaction saved without a category
    - Actual: Filled only Description="QA No Category Test" and Amount=123.45, left Category as "Select category", and clicked "Add Expense". The dialog closed, no error/toast was shown, and the row was created: "Aug 28, 2026 | QA No Category Test | (blank category) | — | -$123.45".

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is neither blocked nor requires reassignment; transactions are orphaned
    - Actual: Deleted category "Software" which had 1 assigned transaction. The confirm dialog offered no reassignment control and its Delete button was enabled, stating only "Transactions using this category will not be deleted." After confirming, the category was removed and the transaction "Software Subscription - Adobe CC" (Dec 15, 2024, -$54.99) now displays an empty Category cell. The Reports page renders such orphans as category "Unknown".


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
    - Issue: Chart Y-axis labels lack precision; non-zero amounts are indistinguishable from zero
    - Actual: Dashboard "Income vs Expenses" chart for Aug 2026 renders an income bar (height 207.7px, $250.00) and an expense bar (height 102.6px, $123.45), but all five Y-axis ticks read "$0k". The axis uses a $Xk format with no decimals, so any value below $500 rounds to "$0k" and cannot be distinguished from zero.