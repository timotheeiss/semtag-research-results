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
    - Issue: Editing a transaction silently clears its category
    - Actual: Opened Edit Transaction for 'QA Test Income Transaction' (category was 'Consulting'). The Edit dialog's Category dropdown displayed 'Select category' (not pre-populated with 'Consulting') even though the underlying native select had defaulted to the first category 'Sales Revenue'. Changed Amount to 750 and toggled Tax Related off without touching the Category field, then clicked Update Income. Result: amount and tax status updated correctly, but the transaction's Category was wiped to blank ('Aug 20, 2026 | QA Test Income Transaction | (blank) | QA-REF-001 | +$750.00 | (blank tax)'), losing previously saved data.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (From date after To date) is not rejected with an explanatory message; the app silently treats it as a filter yielding zero results instead of flagging the input as invalid.
    - Actual: Set From=2024-12-15 and To=2024-12-01 (start > end). No validation error or explanation was shown. The transaction list simply displayed the generic empty state 'No transactions found / Try adjusting your filters', identical to what would be shown for a valid-but-non-matching range, giving the user no indication that the date range itself is logically invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category validation not enforced
    - Actual: Submitting the Add Transaction form with Description='Test Expense No Category' and Amount=25 but no Category selected succeeded: the dialog closed and a new transaction row 'Aug 24, 2026 | Test Expense No Category | (blank category cell) | — | -$25.00' was added to the table. No error was shown identifying a missing category.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category assigned to existing transactions is not blocked and does not require reassignment; affected transactions are left with a blank/orphaned category.
    - Actual: Deleted the 'Marketing' category (2 transactions assigned) via the confirm dialog, which itself warned 'Transactions using this category will not be deleted.' After deletion, navigating to Transactions showed 'Google Ads Campaign' (MKTG-001, -$850.00) and 'QA Test Expense Transaction' (QA-REF-EXP, -$120.00) both with an empty/blank Category cell instead of being blocked, reassigned to a default category, or labeled 'Unknown'.


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
    - Issue: Chart axis labels lack precision for values under $1000
    - Actual: On the Dashboard 'Income vs Expenses' chart, after adding a $500 August income transaction, the y-axis tick labels read '$0k, $0k, $0k, $0k, $1k' (5 ticks). Values are rounded to whole thousands, so any non-zero amount below $500 (and most gridline values between $0 and $1000) is displayed as '$0k', making it indistinguishable from an actual zero amount.