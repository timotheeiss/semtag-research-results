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
    - Issue: Edit dialog does not pre-populate the category; saving silently clears it
    - Actual: Editing "QA Consulting Retainer Zeta" (category Consulting) opened the Edit Transaction dialog with Category showing "Select category" instead of "Consulting". Changing only description/amount and saving produced a record with an EMPTY category cell (row: "Aug 15, 2026 | QA Consulting Retainer Zeta EDITED | (blank) | QAREF-001 | +$450.00"), i.e. the update introduced an unintended data loss. Description/amount edits and deletion (with confirm dialog) otherwise worked.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted without validation message
    - Actual: Setting From=2024-12-31 and To=2024-01-01 was accepted; the table simply showed 0 rows with the generic "No transactions found / Try adjusting your filters" message. No aria-invalid, no alert, and no explanation that the start date is after the end date.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not validated
    - Actual: With description "QA No Category Test" and amount 12.34 but no category selected, submit succeeded: dialog closed and the transaction was added to the table with an empty CATEGORY cell. No error message was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is allowed and orphans transactions
    - Actual: Category "Utilities" (listed as "1 transactions / $285") was deleted after a confirm dialog that explicitly states "Transactions using this category will not be deleted". No block and no reassignment offered. The transaction "Electric Bill" (Dec 5, 2024, -$285.00) now displays a blank CATEGORY cell.


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
    - Issue: Chart axis labels lack precision for small values
    - Actual: After adding a $300 income for Aug 2026, the "Income vs Expenses" chart renders a non-zero bar (rect height=216) but every Y-axis tick label reads "$0k" ($0k, $0k, $0k, $0k, $0k), so a $300 amount is indistinguishable from zero.