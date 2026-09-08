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
    - Issue: Missing required fields not identified to user
    - Actual: Submitting the Add Transaction form completely empty (no description, no amount, no category) did not create a transaction, but no error message, inline validation text, or aria-invalid indicator appeared anywhere in the dialog to tell the user what was missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range silently accepted, no explanation shown
    - Actual: Setting Date From=2024-12-31 and Date To=2024-12-01 (start after end) was accepted without any validation error; the table simply rendered 'No transactions found' with no explanation that the range itself was invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created with no category assigned
    - Actual: Submitting the form with description='No Category Test' and amount=50 but no category selected successfully created transaction tx-1787752526419 with a blank category cell in the table, instead of being blocked with an error.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in use leaves transactions with blank category
    - Actual: Deleted the 'Software' category which had 1 transaction (tx-10, 'Software Subscription - Adobe CC'). The confirmation dialog explicitly stated 'Transactions using this category will not be deleted', and after deletion tx-10's category cell became blank ('') in the Transactions table instead of being blocked or reassigned.


## Interaction
- [ ] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.
  - Bug Report:
    - Issue: Stale data after transaction change
    - Actual: After adding a new Aug 26, 2026 income transaction ($1,000, Sales Revenue), Dashboard Monthly Income/Profit and Recent Transactions updated correctly, and Categories page updated correctly, BUT Dashboard Total Balance remained unchanged at $74,931 (should increase by $1,000), and the Reports page for August 2026 still showed $0 income / $0 expenses / +$0 profit / "No income this period" — completely ignoring the new transaction. Not all summaries/reports update immediately after a transaction change.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [ ] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.
  - Bug Report:
    - Issue: Inaccurate monthly totals / stale balance
    - Actual: Reports page for August 2026 (the current month, containing the newly added $1,000 income transaction) shows Income $0, Expenses $0, Profit +$0, and "No income this period" — it does not include the new transaction at all, so monthly report totals do not accurately reflect underlying transactions. Additionally, Dashboard Total Balance ($74,931) does not reflect the new +$1,000 transaction, making the balance stat inaccurate. (Monthly Income/Expenses/Profit stats and Recent Transactions list on the Dashboard were accurate.)

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Chart axis labels lack precision for sub-$500 values
    - Actual: Income vs Expenses bar chart on Dashboard has an August bar for a $125.50 expense (confirmed via a visible rect in the chart SVG), yet all 5 Y-axis gridline labels read '$0k' instead of a value distinguishable from zero.