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
    - Issue: Update silently discards unedited category data
    - Actual: Delete works (confirm dialog removes the row) and edits to description/amount/category/tax status are applied. But the edit dialog does not preload the transaction's category: editing only the amount (250 → 275) saved the record with a blank category, so the resulting record does not faithfully reflect only the user's change.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Assigned category not retained/prefilled when editing
    - Actual: Category options do correctly match the type (Income → Sales Revenue/Consulting/Interest Income/Other Income; Expense → the 8 expense categories). However, opening Edit on the Consulting income transaction shows Category = "Select category" instead of Consulting; saving after changing only the amount wiped the category — the row now reads ["Aug 10, 2026","QA Consulting Income Alpha","","QA-INC-001","+$275.00","✓"] with a blank category.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected or explained
    - Actual: Setting From=2024-12-31 and To=2024-01-01 was accepted silently; the table simply shows "No transactions found / Try adjusting your filters" with no message that the start date is after the end date.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not validated
    - Actual: With description "CS23 No Category Test" and amount 12.34 but no category selected, the form submitted successfully with no error message. The new row appears in the table with an empty category cell: ["Aug 28, 2026","CS23 No Category Test","","—","-$12.34","",""].

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category orphans transactions
    - Actual: Deleting the "Utilities" category (1 transaction, $285) was allowed with only a generic confirm ("Transactions using this category will not be deleted") and no reassignment option. The "Electric Bill" transaction now shows a blank category: ["Dec 5, 2024","Electric Bill","","—","-$285.00","✓"].


## Interaction
- [ ] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.
  - Bug Report:
    - Issue: Total Balance summary never updates
    - Actual: Monthly income/expenses/profit, the monthly report and Recent Transactions do update immediately on add/edit/delete (e.g. add +$250 income & -$100 expense → income $250, expenses $100, profit $150; edit to $275 and delete the expense → income $275, expenses $0, profit $275). However the dashboard "Total Balance" card stayed at $74,931 through every add, edit and delete, including after adding a $100,000 expense — it is static and ignores transactions.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [X] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Chart axis labels lack precision (rounded to $0k)
    - Actual: Dashboard "Income vs Expenses" chart plots Aug 2026 income $250 and expenses $100, but all five Y-axis tick labels read "$0k", so non-zero values below $500 are indistinguishable from zero.