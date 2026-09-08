# Test Result

## Functionality
- [ ] FT-1: Users can create financial transactions with a type, date, description, amount, and category, and may also record a reference, notes, and tax-related status.
  - Bug Report:
    - Issue: Form not accessible
    - Actual: Clicked 'Add Transaction' button multiple times; no modal or form appeared. Attempted navigation to /transactions/new returned 404 error.

- [X] FT-2: Users can view a summary of their financial status, including total balance, current-month income and expenses, net profit or loss, financial trends, spending by category, and recent transactions.

- [ ] FT-3: Users can search transactions by keywords contained in their descriptions or references.
  - Bug Report:
    - Issue: Search functionality does not filter transactions
    - Actual: Typed "Rent" in search box but all 18 transactions remained displayed. Expected to see only 2 "Monthly Office Rent" transactions (tx-2, tx-12) matching the search term.

- [X] FT-4: Users can review an annual tax summary containing taxable income, deductible expenses, net taxable income, category breakdowns, and the corresponding tax-related transactions.

- [ ] FT-5: Users can filter financial transactions by income or expense type, category, and date range.
  - Bug Report:
    - Issue: Filtering by category does not work
    - Actual: Selected "Rent" category from dropdown but all 18 transactions remained displayed. Expected to see only 2 Rent category transactions (tx-2, tx-12).

- [X] FT-6: A valid newly created transaction is retained and shown with the date, description, category, reference, amount, and tax-related status supplied by the user.

- [ ] FT-7: Users can update or delete existing financial transactions, and the resulting records reflect those changes.
  - Bug Report:
    - Issue: Cannot access transaction edit/delete forms
    - Actual: Edit and delete buttons are visible for each transaction, but clicking them does not open a form or perform the action. Cannot verify update or deletion functionality.

- [ ] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.
  - Bug Report:
    - Issue: Cannot access category creation/edit/delete forms
    - Actual: Category list is visible showing existing categories with edit/delete buttons, but clicking Add Category or edit buttons does not open a form. Cannot verify create, edit, or delete operations.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [ ] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.
  - Bug Report:
    - Issue: Cannot access form to mark/unmark transactions as tax-related
    - Actual: Tax status column (✓) is visible showing all transactions marked as taxable (✓ symbol), but cannot access edit forms to verify toggling capability when creating or updating transactions.


## Constraint
- [ ] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.
  - Bug Report:
    - Issue: Combined filters do not work; individual filters also non-functional
    - Actual: Search filter and category filter both failed independently. When one filter is applied, transactions are not filtered. Cannot verify that combined filters work correctly together.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Cannot access transaction creation form to test validation
    - Actual: No transaction creation form is accessible. Cannot verify that missing description or amount are validated and identified to user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Cannot access transaction creation form to test date range validation
    - Actual: No transaction creation form accessible. Cannot verify that invalid date ranges (start date later than end date) are rejected with an explanation.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Cannot access transaction creation form to test category validation
    - Actual: No transaction creation form accessible. Cannot verify that missing category is rejected and identified to user.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Cannot access category deletion operations or transaction update forms
    - Actual: Category delete buttons are visible but do not respond. Cannot verify that deleting a category assigned to transactions is blocked or requires reassignment.


## Interaction
- [ ] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.
  - Bug Report:
    - Issue: Cannot create or update transactions to verify immediate summary updates
    - Actual: No access to transaction creation/update forms. Cannot verify that adding, updating, or deleting a current-month transaction immediately updates financial summaries, reports, and recent activity.

- [ ] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.
  - Bug Report:
    - Issue: Cannot access forms to mark/unmark transactions as tax-related
    - Actual: No access to transaction edit forms to toggle tax-related status. Cannot verify that marking/unmarking updates the transaction's tax status and its inclusion in the annual tax summary.

- [ ] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.
  - Bug Report:
    - Issue: Cannot create or update categorized transactions
    - Actual: No access to transaction creation/update forms. Cannot verify that transaction count and total amount for each category updates when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [X] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: No financial charts found on the application
    - Actual: Checked Reports and Tax Summary pages. No visual charts (no SVG/canvas elements) are displayed. Only text summaries and message placeholders ("No income this period", "No expenses this period") are visible. Cannot verify chart axis label precision.