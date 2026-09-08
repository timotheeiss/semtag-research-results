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
    - Issue: Missing required field validation lacks user-facing message
    - Actual: Submitting the Add Transaction form with empty Description and Amount does not create a transaction (blocked), but no error message, toast, aria-invalid flag, or visual indicator identifies the missing required fields to the user. The dialog just stays open silently.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not explained to user
    - Actual: Setting From=2024-12-31 and To=2024-01-01 (start after end) is not rejected; the app silently shows a generic "No transactions found / Try adjusting your filters" message instead of an explanation that the date range itself is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not blocked
    - Actual: Submitted Add Transaction form with Description and Amount filled but no category selected; the transaction was created successfully (tx-1787743028884) with a blank/empty category field instead of being rejected.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an assigned category leaves transactions with blank category
    - Actual: Deleting the "Software" category (assigned to tx-10) was not blocked and required no reassignment; the confirm dialog even states "Transactions using this category will not be deleted." After deletion, tx-10 "Software Subscription - Adobe CC" now shows an empty/blank category instead of a valid category or "Unknown".


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
    - Issue: Chart axis labels lack precision for sub-$500 amounts
    - Actual: With a $245.50 expense in August on the "Income vs Expenses" trend chart, all 5 y-axis gridline labels read "$0k" (rounded to nearest whole $k), making the non-zero amount indistinguishable from zero. Even with a larger $500 value, only the top gridline shows "$1k" while intermediate gridlines still read "$0k", confirming the axis uses whole-$k precision that cannot represent amounts under $500.