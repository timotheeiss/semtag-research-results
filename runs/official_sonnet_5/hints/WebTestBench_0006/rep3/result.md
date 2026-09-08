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
    - Issue: Edit transaction form does not pre-fill the Category field
    - Actual: Opening Edit on the QA transaction (category=Consulting) showed the Category dropdown reset to placeholder 'Select category' instead of the existing value. Updating only the Amount field and saving silently cleared the category (table row Category cell became blank) even though update succeeded and amount changed correctly. Amount update itself worked (+$2,000.00), but the unintended category loss means updates do not reliably preserve/reflect correct record state.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing description/amount blocks creation but gives no user-facing error message
    - Actual: Submitting the Add Transaction form with Category set but Description and Amount left empty did not create a row (blocked, dialog stayed open, row count unchanged) - so creation is correctly prevented. However, no error text, toast, red border, or any other indication identifying the missing description/amount was shown anywhere on the page or in the dialog, leaving the user without explanation for why nothing happened.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start > end) not rejected with explanation
    - Actual: Setting From=2024-12-31 and To=2024-01-01 was accepted silently; the UI just displayed 'No transactions found / Try adjusting your filters' with no validation error or explanation that the date range itself is invalid (start date later than end date).

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transactions without a category can be created; no validation or error shown
    - Actual: Filled Description='CS23 Test No Category' and Amount=99.00, left Category unset ('Select category'), and submitted. The transaction was created successfully (row count 19→20) with an empty Category cell in the table and no error message identifying the missing category.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in use is not blocked and does not require reassignment
    - Actual: Deleted 'Interest Income' category (which had 1 transaction, tx-9 'Bank Interest', assigned). The confirmation dialog only warned 'Transactions using this category will not be deleted' with no reassignment option. After confirming deletion, tx-9's Category cell in the Transactions table is now blank, leaving the transaction with an empty/unknown category.


## Interaction
- [ ] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.
  - Bug Report:
    - Issue: Dashboard 'Total Balance' does not update when current-month transactions are added/updated/deleted
    - Actual: Monthly Income, Net Profit/Loss, and Recent Transactions all updated immediately and correctly through add ($0→$1,235), update ($1,235→$2,000), and delete ($2,000→$0) of a current-month transaction. However, dashboard.stats.balance ('Total Balance') stayed at $74,931 throughout all three operations and never changed, even though a $2,000 income transaction was added and later removed - it should have reflected the change in overall balance.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [X] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Trend chart Y-axis labels round to the nearest $1k, indistinguishable from zero for small amounts
    - Actual: After creating a $45.00 expense in August 2026 (the only transaction that month), the 'Income vs Expenses' chart's Y-axis labels all read '$0k' (e.g. for the $0 months and for August which actually has $45 of expenses). The stat card correctly showed 'Monthly Expenses: $45', but the chart axis precision (rounded to whole $k) cannot distinguish a $45 non-zero value from an actual $0 value.