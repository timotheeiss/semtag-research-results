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
    - Issue: Edit form does not pre-populate the existing category; saving an edit silently wipes the transaction's category
    - Actual: Opening Edit on the transaction 'QA Consulting Income Alpha' (category Consulting) showed the Category select reset to 'Select category' while Type/Date/Description/Amount/Reference/Notes/Tax were pre-filled correctly. Editing only description, amount and the tax toggle and saving produced a record with a BLANK category: 'Aug 27, 2026 | QA Consulting Income Alpha EDITED | (blank) | QA-REF-001 | +$450.75'. Description, amount and tax status themselves updated correctly, and delete works (verified separately), but the update silently loses data the user never changed.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected and no explanation shown
    - Actual: Set From=2024-12-31 and To=2024-11-01. The app accepted the reversed range without any validation error; the table simply showed 0 rows with the generic message 'No transactions found / Try adjusting your filters'. No message identifies the range as invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing-category validation absent; transaction saved without a category
    - Actual: Filled description 'QA No Category Test' and amount 123.45, left Category as 'Select category', clicked 'Add Expense'. The dialog closed and the transaction was created and listed as 'Aug 27, 2026 | QA No Category Test | (blank category) | — | -$123.45'. No error message was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is neither blocked nor requires reassignment; leaves blank category
    - Actual: Deleted the 'Software' expense category which had 1 transaction. The confirm dialog only warned 'Transactions using this category will not be deleted' with no reassignment option and no block. After deletion the transaction row reads 'Dec 15, 2024 | Software Subscription - Adobe CC | (blank category) | — | -$54.99'.


## Interaction
- [ ] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.
  - Bug Report:
    - Issue: Dashboard 'Total Balance' summary does not update when current-month transactions are added, updated or deleted
    - Actual: Monthly Income/Expenses/Net Profit, Reports and Recent Transactions all updated immediately (add +$320.50 → income/profit $321 and top of recent list; edit to $450.75 → $451; delete → back to $0 and removed from recent). However 'Total Balance' stayed frozen at exactly $74,931 through all three mutations. It is also inconsistent with the underlying data (all transactions net to $11,106.51), indicating a static value rather than a derived summary.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [X] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Chart Y-axis labels lack precision; non-zero amount indistinguishable from zero
    - Actual: After adding a $320.50 Aug 2026 income, the dashboard 'Income vs Expenses' chart renders a full-height bar (height 203.6px) for Aug, but all five Y-axis tick labels read '$0k' (values are formatted in thousands with no decimals), so the $320.50 value cannot be distinguished from $0.