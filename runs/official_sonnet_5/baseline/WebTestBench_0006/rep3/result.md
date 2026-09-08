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
    - Issue: Editing a transaction does not preserve its existing category, silently clearing it on update
    - Actual: Opened Edit dialog for the "QA Test Consulting Income" transaction (previously category=Consulting); the Category dropdown showed placeholder "Select category" instead of the current value. Changed only Amount to 750 and clicked Update Income; the saved transaction row now shows amount +$750.00 correctly but the Category cell is blank, i.e. the category was lost even though it was not intentionally changed

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected with explanation
    - Actual: Setting From=2024-12-31 and To=2024-11-01 (start after end) is silently accepted by the form; no validation error is shown. The app simply treats it as a filter with no matches, showing generic "No transactions found / Try adjusting your filters" empty state, giving the user no indication that the date range itself is logically invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transactions can be created without an assigned category
    - Actual: Filled Description="CS-23 Test No Category" and Amount=100 but left Category unselected, then clicked Add Expense; the transaction was created successfully and appears in the table with a blank Category cell instead of being rejected with a missing-category message

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in-use is not blocked and does not require reassignment
    - Actual: Deleted "Insurance" category despite it having 1 assigned transaction; confirmation dialog explicitly stated "Transactions using this category will not be deleted", and afterward the "Business Insurance - Annual" transaction's Category cell became blank/empty instead of showing a category or being blocked/reassigned


## Interaction
- [X] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [ ] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.
  - Bug Report:
    - Issue: Financial summary figures inaccurate/inaccessible relative to underlying transaction data
    - Actual: Dashboard "Total Balance" shows $74,931, but summing all 18 ledger transactions gives Total Income $47,592.50 − Total Expenses $36,485.99 = Net $11,106.51, a discrepancy of ~$63,824 with no visible basis in the transaction data. Separately, the Reports page month selector only offers the 12 months trailing the current app date (Aug 2026 back to Sep 2025) and cannot be navigated to Oct/Nov/Dec 2024, where 100% of the actual transaction data resides — so the Monthly financial analysis report can never show real income/expense/net totals for any accessible month (always $0), making the "monthly report totals" feature effectively non-functional against the underlying ledger.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Chart Y-axis labels lack precision for small values, rounding to indistinguishable $0k
    - Actual: Added a $200 Income transaction dated Aug 24, 2026 (current month). On Dashboard's Income vs Expenses chart, all 5 Y-axis gridline labels display '$0k' (0, 0, 0, 0, 0) even though the Aug bar now represents $200 in income. The axis provides no numeric distinction between $0 and $200, making the chart scale unreadable/misleading for amounts under $1k.