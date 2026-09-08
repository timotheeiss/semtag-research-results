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
    - Actual: Opening Edit on tx-1787704173322 (category 'Consulting') showed the Category dropdown reset to placeholder 'Select category' instead of the current value. Saving the edit (description/amount/tax changed, category untouched) resulted in the transaction's category becoming blank in the table/collection ('' instead of 'Consulting'). Delete function worked correctly and updated totals.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start after end) not rejected or explained
    - Actual: Set From=2024-12-31 and To=2024-12-01 in transaction filters. No validation error appeared; the app silently accepted the invalid range and displayed a generic 'No transactions found / Try adjusting your filters' message instead of identifying that the start date is later than the end date.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created without a category assigned
    - Actual: Filled Description and Amount but left Category unselected, then clicked Add Expense. Transaction 'CS23 Test No Category' was created successfully with a blank category field and no error message shown to the user.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in use leaves transactions with a blank category instead of being blocked or requiring reassignment
    - Actual: Deleted 'Utilities' category (had 1 transaction, 'Electric Bill'). Confirmation dialog only warned 'Transactions using this category will not be deleted' with no reassignment option and allowed deletion. After deletion, transaction tx-4 'Electric Bill' now shows an empty/blank category field instead of 'Utilities' or a placeholder like 'Unknown'.


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
    - Issue: Chart y-axis labels round to thousands and cannot represent non-zero sub-$500 values
    - Actual: Added an income transaction of $300.25 in August 2026, making the 'Income vs Expenses' dashboard chart's August bar non-zero. All 5 y-axis tick labels on that chart still read '$0k' (values formatted in whole thousands with 0 decimals), making the non-zero $300.25 value indistinguishable from zero on the axis.