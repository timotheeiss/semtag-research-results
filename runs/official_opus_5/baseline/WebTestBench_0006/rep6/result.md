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
    - Issue: Update silently clears the transaction's category
    - Actual: Delete works (confirm dialog, row removed). Update changed description/amount/tax status correctly, but because the Edit dialog does not prefill Category, saving an otherwise untouched transaction wiped its category (Consulting → blank), so the resulting record does not reflect the user's changes only.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Assigned category is not retained when a transaction is edited
    - Actual: Type-compatible category lists are correct and the category is retained on create, but the Edit Transaction dialog does not prefill the existing category (shows "Select category"). Editing only the description/amount of "QA Consulting Invoice Alpha" (category Consulting) saved the row with an empty Category cell: "Aug 27, 2026 | QA Consulting Invoice Alpha Edited | (blank) | QA-REF-001 | +$3,000.00".

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start > end) accepted without any validation message
    - Actual: Set From=2024-12-31 and To=2024-01-01. Both values were accepted, no error/warning/toast appeared; the table simply showed "No transactions found / Try adjusting your filters" with no explanation that the range is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not validated; transaction created with blank category
    - Actual: Filled only Description "QA No Category Test" and Amount 12.34, left Category as "Select category", clicked "Add Expense". The dialog closed and the row was created: "Aug 27, 2026 | QA No Category Test | (blank category) | — | -$12.34". No error message identified a missing category.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is allowed with no block or reassignment, leaving transactions with a blank category
    - Actual: Deleted the "Software" category, which had 1 transaction ($54.99). The confirm dialog only said "Transactions using this category will not be deleted" — no block and no reassignment option. After deletion the transaction "Dec 15, 2024 Software Subscription - Adobe CC" shows an empty Category cell.


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
    - Issue: Chart Y-axis labels formatted as "$Xk" with no decimals, so non-zero values under $500 are indistinguishable from zero
    - Actual: With an Aug-2026 expense of $120.75 as the only current-window value, the Dashboard "Income vs Expenses" Y-axis ticks are all "$0k" ($0k,$0k,$0k,$0k,$0k) even though the chart plots a non-zero $120.75 bar. Earlier, with a $2,500.50 value, the axis showed duplicated ticks "$0k,$1k,$1k,$2k,$3k"; the Reports "Daily Activity" axis uses the same $k formatting.