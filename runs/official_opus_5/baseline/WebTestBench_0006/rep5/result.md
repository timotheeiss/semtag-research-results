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
    - Issue: Missing required-field validation feedback
    - Actual: Submitting the Add Transaction form with empty description/amount (and again with description+category but no amount) leaves the dialog open and creates nothing, but no error message, toast, or field-level indication of the missing information is shown anywhere in the DOM (body text contains no "required"/"missing"/"please" text; toast regions empty).

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range accepted without validation message
    - Actual: Setting From=2024-12-31 and To=2024-01-01 was accepted silently; the table just showed "No transactions found / Try adjusting your filters" with no explanation that the start date is later than the end date.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Category not required; transaction created with blank category
    - Actual: Submitted Add Transaction with description "No Category Test", amount 42.75 and category left as "Select category". The dialog closed and the transaction was created; its row shows an empty Category cell (["Aug 27, 2026","No Category Test","","—","-$42.75","",""]). No missing-category message shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: In-use category deleted without block or reassignment, orphaning transactions
    - Actual: Deleted the "Software" expense category which showed "1 transactions". The confirm dialog only warns "Transactions using this category will not be deleted" and deletion proceeded (Expense Categories 8→7). The transaction "Software Subscription - Adobe CC" (Dec 15, 2024) now displays an empty Category cell; no reassignment was offered.


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
    - Issue: Chart Y-axis labels rounded to $k, losing precision for sub-$500 values
    - Actual: With a single Aug-2026 expense of $300, the Reports "Daily Activity" chart Y-axis shows five identical labels "$0k, $0k, $0k, $0k, $0k", and the Dashboard "Income vs Expenses" chart likewise shows "$0k" for every tick, so the non-zero $300 amount is indistinguishable from zero on the axis.