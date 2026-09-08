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
    - Issue: Edit dialog does not pre-populate Category; updating silently wipes the transaction's category
    - Actual: Opened Edit on "QA Income Alpha Invoice" (category Consulting). The Category select showed the placeholder "Select category" instead of Consulting, while Date/Description/Amount/Reference/Notes/Tax were pre-filled. Changed only description + amount + tax switch and clicked Update Income: the row became "QA Income Alpha Invoice EDITED / +$320.00" but its Category cell is now EMPTY — the previously assigned Consulting category was lost. (Delete itself works: an earlier transaction was removed via the confirm dialog.)

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing required fields are not identified to the user
    - Actual: Submitting the Add Transaction form with empty Description and Amount does nothing: the dialog stays open, no transaction is created, but no inline error, aria-invalid, toast, or native validation message is shown anywhere (DOM scan for 'required/invalid/Please/error' returned nothing).

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted with no validation message
    - Actual: Set From=2024-12-31 and To=2024-01-01. The app silently accepted the range and simply rendered "No transactions found / Try adjusting your filters". No error, warning, aria-invalid, or explanation that the start date is after the end date was shown, and the inputs kept the invalid values.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created with no category
    - Actual: Filled Description="QA No Category Test" and Amount=12.34 leaving Category as "Select category", clicked Add Expense. The transaction was created and appears in the table (Aug 27, 2026 / -$12.34) with an empty Category cell. No error was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is allowed and orphans its transactions
    - Actual: The "Utilities" expense category showed "2 transactions". Its delete was neither blocked nor did it offer reassignment — the confirm dialog only said "Transactions using this category will not be deleted." After confirming, Expense Categories went 8→7 and both affected transactions ("QA Expense Beta Utilities" Aug 27 2026 -$1,234.56 and "Electric Bill" Dec 5 2024 -$285.00) now display a BLANK Category cell in the Transactions table.


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
    - Issue: Chart Y-axis labels lack precision; non-zero value indistinguishable from zero
    - Actual: With current-month income of $320.00, the Dashboard "Income vs Expenses" chart renders a visible non-zero income bar (rect height 216px) but all five Y-axis tick labels read "$0k" ($0k, $0k, $0k, $0k, $0k). The axis formatter rounds to whole thousands, so $320 is indistinguishable from $0. (Also seen earlier at $2,500.75: ticks $0k, $1k, $1k, $2k, $3k with duplicate labels.)