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
    - Issue: Update silently discards the transaction's category
    - Actual: Delete works correctly (confirm dialog, row removed, totals recalculated 20→19 rows). Update partially works: description and amount changed as entered (+$450→+$500) and tax status updated, BUT the Edit dialog never pre-fills the existing Category, and saving without touching it wiped the category — the row's Category changed from "Consulting" to blank, a change the user never made.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Assigned category is not retained when a transaction is edited
    - Actual: Category/type compatibility is correct (Income offers only the 4 income categories; Expense only the 8 expense ones) and creation retains the category. However opening Edit on the transaction assigned to "Consulting" showed the Category control as the placeholder "Select category" (not pre-filled). Saving the edit without touching Category silently cleared it: the row's Category cell went from "Consulting" to blank.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted without validation message
    - Actual: Set From=2024-12-31 and To=2024-11-01. The filter was accepted silently; the table simply showed "No transactions found / Try adjusting your filters". No error, warning, or explanation of the invalid range was displayed anywhere on the page.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing required category is not validated
    - Actual: Submitted Add Transaction with description "QA No Category Test", amount 25 and no category selected. The dialog closed, no error/toast appeared, and the transaction was created with an empty Category cell (row: Aug 27 2026 | QA No Category Test | (blank) | — | -$25.00).

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category orphans its transactions
    - Actual: Deleted category "Rent" which had 2 transactions ($4,400 total). The confirm dialog neither blocked the delete nor offered reassignment — it only said "Transactions using this category will not be deleted." After confirming, the deletion succeeded and both "Monthly Office Rent" rows (Dec 2 2024 / Nov 2 2024) now show a blank Category cell; Reports renders such rows under "Unknown".


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
    - Issue: Chart Y-axis labels rounded to $Xk, indistinguishable from zero for sub-$500 values
    - Actual: "Income vs Expenses" chart renders non-zero bars for Aug 2026 (income $450 = 162px tall, expenses $25 = 9px tall), but the Y-axis tick labels are "$0k", "$0k", "$0k", "$0k", "$1k". Every gridline below the top reads $0k, so the $450 and $25 values cannot be distinguished from zero on the axis.