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

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing description/amount blocks submission but gives no user-facing error message
    - Actual: Submitting the Add Transaction form with empty Description (amount and category filled) kept the dialog open (creation blocked), but no error text, aria-invalid attribute, red/destructive styling, or toast notification identified the missing Description field to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start after end) not rejected or explained
    - Actual: Setting From=2024-12-31 and To=2024-12-01 was accepted without any validation error; the app silently displayed "No transactions found / Try adjusting your filters" instead of rejecting the range with an explanation that the start date is after the end date.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction can be created without a category, with no validation or identification to user
    - Actual: Filled Description="CS-23 Test" and Amount=50 but left Category unselected ("Select category"), then submitted. The transaction was created successfully and appears in the table with a blank Category cell, instead of being blocked with a missing-category message.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category deletion not blocked and no reassignment offered, leaving transactions with blank category
    - Actual: Deleted the 'Software' expense category (which had 1 assigned transaction) via a confirmation dialog stating "Transactions using this category will not be deleted." After deletion, transaction tx-10 'Software Subscription - Adobe CC' now shows an empty/blank category value in the Transactions table instead of being blocked, reassigned, or labeled 'Unknown'.


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
    - Issue: Chart axis labels lack precision for small non-zero amounts
    - Actual: With an Aug (current month) expense of $120.50 present, the 'Income vs Expenses' chart's y-axis gridline labels all read '$0k' (5 labels, all $0k), giving no visual distinction between the $120.50 bar and zero.