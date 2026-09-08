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

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start > end) is not rejected or explained
    - Actual: Set From=2024-12-31 and To=2024-01-01 (start later than end). The app did not reject the input or show any explanation of an invalid range; it silently accepted the values and displayed a generic 'No transactions found - Try adjusting your filters' empty state, giving no indication that the date range itself is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not blocked or identified
    - Actual: Submitted the Add Transaction form with Description and Amount filled but Category left as "Select category" (unselected). The transaction was created successfully with a blank category (table row shows empty Category cell) instead of being rejected with a validation message.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is not blocked and does not require reassignment
    - Actual: Deleted the 'Interest Income' category which had 1 assigned transaction (tx-9, Bank Interest). Confirmation dialog only warned 'Transactions using this category will not be deleted' with no reassignment step. After confirming, transaction tx-9's Category cell became blank instead of showing 'Unknown' or being reassigned/blocked.


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
    - Issue: Chart axis labels lack precision for values below $1,000 (including below $500)
    - Actual: Dashboard 'Income vs Expenses' chart Y-axis, with an actual data point of $600 (August), shows tick labels ['$0k','$0k','$0k','$0k','$1k'] - four of the five ticks display as '$0k' even though they represent distinct non-zero values (e.g. $150, $300, $450), making them indistinguishable from zero on the axis.