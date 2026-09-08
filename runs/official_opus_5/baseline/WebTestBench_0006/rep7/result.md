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

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Assigned category is silently lost when an income transaction is updated after an expense transaction's edit dialog was opened
    - Actual: Type-based category filtering itself works (Expense→8 expense categories, Income→Sales Revenue/Consulting/Interest Income/Other Income). But category retention fails: opened Edit on "Google Ads Campaign" (expense/Marketing), cancelled, then opened Edit on "Product Sales - Q4 Order" (income/Sales Revenue) — the category trigger displayed placeholder "Select category" — and clicked Update Income without touching the category. The saved row is now "Dec 3, 2024 | Product Sales - Q4 Order | (blank category) | SO-4521 | +$8,750.00". Same happened to "QA Test Invoice Alpha" (Consulting → blank) after editing an expense first.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range accepted silently with no error explanation
    - Actual: Set From=2024-12-31 and To=2024-01-01 on the Transactions filter. No validation error, warning, or toast appeared; the app simply rendered an empty table with the generic message "No transactions found / Try adjusting your filters". The invalid range is never identified to the user.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not validated; transaction created with blank category
    - Actual: Filled only Description="No Category Test" and Amount=99.5, left Category as "Select category", clicked "Add Expense". The dialog closed and a new row "Aug 28, 2026 | No Category Test | (blank category) | — | -$99.50" was added to the table. The category <select> has required=false and no error was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is neither blocked nor requires reassignment, leaving an orphaned transaction with a blank category
    - Actual: Deleted the "Utilities" expense category, which showed "1 transactions". The confirm dialog only warned "Transactions using this category will not be deleted" and offered no reassignment. After deletion the category disappeared (Expense Categories 8→7) and the transaction "Dec 5, 2024 Electric Bill -$285.00" now renders with an empty Category cell.


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
    - Issue: Chart Y-axis labels rounded to whole thousands, so non-zero values below $500 are labelled "$0k"
    - Actual: With an Aug-2026 expense of $300.25 and income of $1,234.56 plotted, the "Income vs Expenses" chart Y-axis ticks read ["$0k","$0k","$1k","$1k","$1k"] (recharts yAxis ticks). The $300.25 expense bar falls at a gridline labelled "$0k" and the ticks are duplicated, so a non-zero amount below $500 is indistinguishable from zero.