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
    - Issue: Invalid date range (start after end) accepted with no explanation
    - Actual: Set From=2024-12-31 and To=2024-01-01. The app accepted the range silently, returned 0 rows and showed only the generic empty state "No transactions found / Try adjusting your filters". No validation error, toast, or message explaining that the start date is later than the end date; the date inputs have no min/max constraint or aria-invalid state.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created with no category; no validation or error shown
    - Actual: Submitted Add Transaction with Description "QA No Category Test", Amount 77.77 and the Category combobox still showing the "Select category" placeholder. The dialog closed and the transaction was created with a blank Category cell (row: "Aug 28, 2026 | QA No Category Test | (empty) | — | -$77.77"). The category select has required=false, so nothing identified the missing category.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is neither blocked nor requires reassignment; leaves transactions with blank category
    - Actual: Deleted the "Marketing" expense category which had 2 transactions ($1,150). The confirmation dialog only warned "Transactions using this category will not be deleted." with no block and no reassignment option. After deletion, Expense Categories went 8→7 and the transactions "QA Beta Ad Spend EDITED" (Aug 28, 2026) and "Google Ads Campaign" (Dec 12, 2024) now show an empty Category cell.


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
    - Issue: Chart Y-axis labels lack precision; non-zero values render as "$0k"
    - Actual: Dashboard "Income vs Expenses" chart plots Aug 2026 income $250 and expenses $300, but all five Y-axis tick labels read "$0k" (SVG text: "Mar, Apr, May, Jun, Jul, Aug, $0k, $0k, $0k, $0k, $0k"). The axis formatter divides by 1000 with no decimals, so amounts below $500 are indistinguishable from zero.