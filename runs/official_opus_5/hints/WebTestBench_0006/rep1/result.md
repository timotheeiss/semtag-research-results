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
    - Issue: Assigned category is not retained when a transaction is edited
    - Actual: Category options are correctly filtered by type (income: cat-1..cat-4; expense: cat-5..cat-12) and are kept on creation. But opening Edit on "QA Consulting Income Aug" (category Consulting) showed the Category select reset to "Select category" while every other field was prefilled. Saving an unrelated change (amount 250.75 → 300) silently wiped the category: the row became "Aug 27, 2026 | QA Consulting Income Aug EDITED | (blank) | INV-QA-001 | +$300.00".

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start &gt; end) is accepted silently with no validation message
    - Actual: Set From=2024-12-31 and To=2024-01-01. Both values were accepted, the table simply rendered 0 rows with the generic empty state "No transactions found / Try adjusting your filters". No error, toast, or explanation identifying the invalid range was shown.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not validated; transaction saved with blank category
    - Actual: Filled only Description "No Category Test" and Amount 123.45, left Category as "Select category", clicked "Add Expense". The dialog closed and a new row was created: "Aug 27, 2026 | No Category Test | (empty category) | — | -$123.45". No error message was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category in use can be deleted without blocking or reassignment, orphaning transactions
    - Actual: Deleted expense category "Utilities" which had 1 transaction ($285). The confirm dialog only said "Transactions using this category will not be deleted" and offered no reassignment; after confirming, the category was removed and the transaction row now reads "Dec 5, 2024 | Electric Bill | (blank category) | — | -$285.00".


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
    - Issue: Chart Y-axis labels lack precision; non-zero values are labelled $0k
    - Actual: Dashboard "Income vs Expenses" chart for Aug 2026 renders two non-zero bars (income $250.75, expense $80.00 — bar heights 208px and 66px), but all five Y-axis tick labels read "$0k", so the plotted amounts are indistinguishable from zero.