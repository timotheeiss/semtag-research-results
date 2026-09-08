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

- [ ] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.
  - Bug Report:
    - Issue: Income transactions lose their category when edited (Edit dialog does not prefill category)
    - Actual: Create-time behavior is correct (type-compatible category lists, category retained). But opening Edit on an INCOME transaction shows Category as the "Select category" placeholder (verified for "QA Consulting Income Alpha"/Consulting and "November Sales Revenue"/Sales Revenue), while expense transactions (e.g. Electric Bill/Utilities) prefill correctly. Saving an income edit without touching Category cleared it: the row's Category cell became blank and Reports groups it under "Unknown".

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: No validation feedback for missing required fields
    - Actual: Clicking "Add Expense" with empty Description and Amount does not create the transaction, but no error message, toast, or field-level indication of the missing required information appears anywhere (dialog text and notification region contain no error).

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range accepted without validation message
    - Actual: Setting From=2024-12-31 and To=2024-12-01 (start after end) is accepted silently; the table simply shows "No transactions found / Try adjusting your filters" with no rejection or explanation that the range is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created without a category; no validation
    - Actual: Submitted "QA Test No Category" ($123.45 expense) with Category left as "Select category". The dialog closed and the transaction was saved with an empty Category cell (row: Aug 27, 2026 | QA Test No Category | (blank) | — | -$123.45). No error message was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category in use can be deleted with no block or reassignment
    - Actual: Deleting the "Consulting" income category (3 transactions, $10,800) was allowed after a generic confirm ("Transactions using this category will not be deleted"). Afterwards the 3 affected transactions (Consulting Fee - Strategy Session, Client Project - Website Redesign, Client Retainer Fee) show a blank Category cell, and Reports groups them under "Unknown".


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
    - Issue: Chart Y-axis labels lack precision for sub-$500 values
    - Actual: With Aug 2026 income $420 and expenses $123.45 plotted, the "Income vs Expenses" chart Y-axis ticks read "$0k, $0k, $0k, $0k, $1k" — every tick below $500 collapses to "$0k", so a non-zero $420 bar cannot be distinguished from zero.