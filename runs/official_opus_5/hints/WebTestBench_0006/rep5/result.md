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
    - Issue: Update silently corrupts the transaction's category
    - Actual: Delete works (confirmation dialog, row removed, 19->18 rows). Update applies edited description/amount/tax flag, but because the Edit dialog never loads the existing category, saving an unrelated edit blanked the category of the record (Consulting -> empty), so the resulting record does not faithfully reflect only the user's changes.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Edit form does not preload the transaction's category; saving silently wipes it
    - Actual: Category options correctly switch with type (Income -> Sales Revenue/Consulting/Interest Income/Other Income; Expense -> Office Supplies/Rent/... ). However, opening Edit Transaction on a transaction whose category is Consulting shows the category select as "Select category" (value not retained). Saving the edit (changing only description/amount) stored the transaction with an empty category: row became "Aug 20, 2026 | QA Consulting Income Alpha Edited | (blank) | QA-REF-001 | +$400.00".

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing required fields blocked silently with no error message
    - Actual: Submitting the Add Transaction form with empty description and amount kept the dialog open and created nothing (row count stayed 18), but no validation message, toast, field error, or native validation bubble was shown anywhere on the page (no [role=alert]/toast, no `required` attributes on the inputs). The user is given no indication of what is missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted silently with no explanation
    - Actual: Setting From=2024-12-31 and To=2024-01-01 was accepted; the table simply showed 0 rows with the generic message "No transactions found / Try adjusting your filters". No validation error identifying the invalid range, no [role=alert]/toast, and the date inputs carry no min/max constraints (checkValidity() true).

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created without a category
    - Actual: With Description="Validation Test Expense", Amount=25 and Category left at "Select category", clicking "Add Expense" saved the transaction (table rows 18 -> 19). New row renders as "Aug 27, 2026 | Validation Test Expense | (blank category) | — | -$25.00". No error message was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category in use can be deleted, orphaning its transactions
    - Actual: Deleting the "Software" category (shown as "1 transactions / $54.99") was allowed with no block and no reassignment option; the confirm dialog only says "Transactions using this category will not be deleted". After confirming, the category disappeared and the transaction "Software Subscription - Adobe CC" (Dec 15, 2024, -$54.99) now displays with a blank Category cell.


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
    - Issue: Chart Y-axis labels rounded to thousands, all read "$0k" for a non-zero value
    - Actual: After adding a $250 income for Aug 2026, the dashboard "Income vs Expenses" chart renders a visible bar (rect height 207.7px) but all five Y-axis tick labels read "$0k" ($0k,$0k,$0k,$0k,$0k), so the $250 amount is indistinguishable from zero.