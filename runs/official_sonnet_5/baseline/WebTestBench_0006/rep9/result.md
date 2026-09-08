# Test Result

## Functionality
- [X] FT-1: Users can create financial transactions with a type, date, description, amount, and category, and may also record a reference, notes, and tax-related status.

- [ ] FT-2: Users can view a summary of their financial status, including total balance, current-month income and expenses, net profit or loss, financial trends, spending by category, and recent transactions.
  - Bug Report:
    - Issue: Total Balance does not reconcile with underlying transaction data
    - Actual: Dashboard displays Total Balance='$74,931', but the sum of all visible transactions (Total Income +$47,592.50 minus Total Expenses -$36,485.99, from the Transactions page with all 18 baseline records) equals net $11,106.51 — a large unexplained discrepancy of ~$63,824. There is no starting-balance field, accounts page, or other UI element anywhere in the app disclosing this offset. All other Dashboard summary elements work correctly: Monthly Income/Monthly Expenses/Net Profit-Loss correctly compute to $0 for the current month (Aug 2026, no transactions) and update live when current-month transactions are added; Income vs Expenses chart and Expenses by Category chart are present and rendered; Recent Transactions list correctly shows the most recent transactions with description/category/date/amount.

- [X] FT-3: Users can search transactions by keywords contained in their descriptions or references.

- [X] FT-4: Users can review an annual tax summary containing taxable income, deductible expenses, net taxable income, category breakdowns, and the corresponding tax-related transactions.

- [X] FT-5: Users can filter financial transactions by income or expense type, category, and date range.

- [X] FT-6: A valid newly created transaction is retained and shown with the date, description, category, reference, amount, and tax-related status supplied by the user.

- [ ] FT-7: Users can update or delete existing financial transactions, and the resulting records reflect those changes.
  - Bug Report:
    - Issue: Editing a transaction silently clears its category
    - Actual: Opened Edit dialog for an existing transaction whose category was 'Sales Revenue'. The edit dialog's category combobox visually displayed the placeholder 'Select category' instead of the actual selected category (though the underlying hidden select still held the correct value). Updated only Description, Amount, and unmarked Tax Related without touching Category, then submitted. The resulting transaction retained the description/amount/tax changes correctly, but its Category was wiped to blank. Delete function worked correctly (row removed after confirmation).

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing required fields not identified to user
    - Actual: Submitting Add Transaction with empty Description and Amount blocks creation (dialog stays open, row count unchanged) but shows no error message, no red/invalid field styling, and no toast notification identifying the missing description or amount.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (From > To) not rejected with explanation
    - Actual: Set From=2024-12-31 and To=2024-12-01 (From after To). The app did not reject the range or show any message explaining the range is invalid. Instead it silently applied the filter and displayed a generic 'No transactions found / Try adjusting your filters' empty state, indistinguishable from a valid filter that simply matches no rows.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created without a category
    - Actual: Filled Description and Amount, left Category as default placeholder 'Select category' (not selecting any option), submitted 'Add Expense'. The transaction was created successfully (row added: 'Aug 25, 2026 CS23 Test Expense No Category — -$25.00') with a blank Category cell, and no error was shown identifying a missing category.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in use is not blocked and leaves transactions with blank category
    - Actual: Deleted the 'Software' category which had 1 assigned transaction ('Software Subscription - Adobe CC'). The confirmation dialog explicitly stated 'Transactions using this category will not be deleted' with no reassignment option. After confirming deletion, the transaction now shows a blank Category cell instead of the original 'Software' or any 'Unknown' placeholder.


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
    - Issue: Chart axis labels lack precision to distinguish non-zero sub-$500 values from zero
    - Actual: On the Reports page 'Daily Activity' chart for August 2026 (containing one real data point: a $175 expense on Aug 25), all 5 Y-axis tick labels read '$0k', identical to what a chart with zero data would show. Since the axis is scaled/labeled in whole thousands ('$Xk') with no decimal precision, any value under $500 (rounds to $0k) is visually indistinguishable from no data at all, making the chart misleading for typical small transactions.