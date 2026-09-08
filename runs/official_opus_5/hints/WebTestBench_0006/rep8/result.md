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
    - Issue: Edit form does not pre-populate the existing category, so saving an edit silently clears it
    - Actual: Opening Edit on transaction "QA Consulting Income Alpha" (category Consulting) showed the Category select as "Select category" instead of "Consulting". Changing only the description and amount and saving produced a record with an empty Category cell: Aug 15, 2026 | QA Consulting Income Beta | (blank) | INV-QA-001 | +$600.00. Description/amount/tax changes were applied and delete works, but the update does not faithfully retain the record's category.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected or explained
    - Actual: Setting From=2024-12-31 and To=2024-11-01 was accepted silently; the table simply showed "No transactions found / Try adjusting your filters" with no validation message about the start date being after the end date.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not validated
    - Actual: Submitting the Add Transaction form with description "QA No Category Test" and amount 123.45 but no category selected created the transaction; the dialog closed with no error and the new row shows an empty Category cell.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is allowed and orphans transactions
    - Actual: The "Rent" expense category had 2 transactions. Delete was permitted (dialog only warned "Transactions using this category will not be deleted", no block or reassignment). After deletion, the Dec 2, 2024 and Nov 2, 2024 "Monthly Office Rent" transactions show an empty Category cell.


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
    - Issue: Chart axis labels lack precision for values under $500
    - Actual: Dashboard "Income vs Expenses" chart plots Aug 2026 income of $450.00, but the Y-axis tick labels render as "$0k, $0k, $0k, $0k, $1k" — the $450 value is indistinguishable from zero on the axis.