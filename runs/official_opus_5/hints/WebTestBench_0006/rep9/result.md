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
    - Issue: Edit form does not pre-populate the existing category, so saving an update silently wipes the transaction's category
    - Actual: Opened Edit on tx-1787895671372 ("QA Consulting Income Aug", category Consulting): the form's Category select showed "Select category" instead of "Consulting" (date, description, amount, reference, notes, tax toggle were pre-filled). After changing only description (v2), amount (300) and the tax toggle and saving, the row became "Aug 28, 2026 | QA Consulting Income Aug v2 | (blank category) | INV-QA-001 | +$300.00" — the previously assigned Consulting category was lost. Delete does work correctly (confirmation dialog, row removed).

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start > end) is accepted with no validation message
    - Actual: Set From=2024-12-31 and To=2024-11-01. The filter was applied without any rejection; the table simply showed "No transactions found / Try adjusting your filters". No error, toast, or explanation of the invalid range was displayed.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not validated; transaction created with blank category
    - Actual: Submitted Add Transaction with description "QA No Category Test" and amount 123.45 but no category selected. The dialog closed and the transaction was saved (row tx-1787895613397, "Aug 28, 2026 | QA No Category Test | (blank category) | -$123.45"). No error message or field-level indication was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is allowed with no block or reassignment, orphaning transactions
    - Actual: Deleted category "Software" (cat-11) which had 1 transaction. Confirmation only warned "Transactions using this category will not be deleted" and offered no reassignment. After deletion the transaction "Software Subscription - Adobe CC" (Dec 15, 2024) now shows an empty Category cell.


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
    - Issue: Chart Y-axis uses whole-thousands ($Nk) precision, so sub-$500 amounts are labeled identically to zero
    - Actual: With a $420 income transaction in Aug 2026, the "Income vs Expenses" chart Y-axis ticks read "$0k, $0k, $0k, $0k, $1k". The $420 data point falls on a tick labeled "$0k", indistinguishable from zero; the four lower ticks are all identical "$0k" labels.