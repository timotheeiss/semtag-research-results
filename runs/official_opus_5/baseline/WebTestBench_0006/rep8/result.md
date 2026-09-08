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
    - Issue: Editing a transaction silently wipes its category
    - Actual: The Edit Transaction dialog does not pre-populate the Category field — it shows the placeholder "Select category" even though the transaction had Category=Consulting (the hidden select defaults to cat-1). After changing only Description and Amount and clicking "Update Income", the saved row lost its category entirely: cells became ["Aug 20, 2026","QA Test Income Alpha EDITED","","QA-REF-001","+$500.75","",""] — Category is now blank. Description/amount/tax changes did apply, and Delete works correctly.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Assigned category not retained through an edit
    - Actual: Category options are correctly filtered by type (Income → Sales Revenue/Consulting/Interest Income/Other Income; Expense → Office Supplies/Rent/Utilities/etc.) and the category is retained on creation. However the Edit Transaction dialog does not load the transaction's existing category (shows "Select category"), and saving an unrelated field change dropped the assigned "Consulting" category, leaving the transaction's Category cell blank.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing required fields are not identified to the user
    - Actual: Submitting Add Transaction with empty description/amount silently does nothing: the dialog stays open, no toast, no inline error text, and no aria-invalid/error styling on any field. The user gets zero feedback about what is missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start &gt; end) accepted without rejection or explanation
    - Actual: Set From=2024-12-31 and To=2024-12-01. Both values were accepted, no validation error, warning, toast or explanation was shown. The table simply rendered an empty result with the generic message "No transactions found / Try adjusting your filters", which does not identify the invalid range.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created with no category; no validation and blank category displayed
    - Actual: Submitted Add Transaction with Description="Validation Test No Amount", Amount=10 and Category left at "Select category". The dialog closed and the transaction was created. Its Category cell renders as an empty string (blank), leaving a transaction with no category. No error message was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is neither blocked nor requires reassignment, orphaning transactions
    - Actual: Deleted the "Software" expense category which had 1 transaction. The confirm dialog only warned "Transactions using this category will not be deleted" — no block and no reassignment option. After deletion, the transaction "Software Subscription - Adobe CC" (Dec 15, 2024) now renders with an empty/blank Category cell.


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
    - Issue: Chart Y-axis labels lack precision; non-zero amounts render as "$0k"
    - Actual: After adding an Aug 2026 income of $420.50, the "Income vs Expenses" chart plots a non-zero bar for Aug, but 4 of its 5 Y-axis tick labels read "$0k" (the 5th reads "$1k"). A $420.50 value is indistinguishable from $0 on the axis — the "$Nk" formatting truncates all values below $500 to $0k.