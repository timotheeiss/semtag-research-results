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
    - Issue: Missing description/amount is not identified to the user
    - Actual: Submitting the Add Transaction form with empty Description and Amount silently failed to create the transaction (table still has 18 rows), but no error message, inline validation text, or aria-invalid indicator appeared anywhere in the dialog to tell the user what is missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (start after end) is not rejected or explained
    - Actual: Setting date-from=2024-12-20 and date-to=2024-12-01 (start later than end) was silently accepted; the app just showed a generic 'No transactions found. Try adjusting your filters.' message with no validation error or explanation that the range itself is invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not identified to the user
    - Actual: Submitting the Add Transaction form with no category selected (still 'Select category' placeholder) silently failed to create the transaction, with no error text or visual indicator identifying the missing category to the user.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in use leaves transactions with a blank category instead of being blocked or requiring reassignment
    - Actual: Deleted the 'Marketing' category which had 2 assigned transactions (tx-8, tx-1787663354143). The delete confirmation only warned 'Transactions using this category will not be deleted' with no reassignment option. After deletion, both transactions now show a blank category value in the Transactions table instead of being blocked, reassigned, or shown as 'Unknown'.


## Interaction
- [ ] IX-16: Within the current session, adding, updating, or deleting a current-month transaction immediately updates the corresponding financial summaries, reports, and recent activity.
  - Bug Report:
    - Issue: Dashboard/report summaries do not update after adding a current-month transaction
    - Actual: Created an expense transaction dated 2026-08-25 (current month) via Transactions page; the Transactions page correctly updated its Total Expenses. However Dashboard month.income/month.expenses/month.profit remained $0/$0/$0 (unchanged), Dashboard 'Recent' list still shows only the old Dec 2024 items (new tx dated Aug 25 2026 should be most recent but is absent), and Reports page for August 2026 shows $0 income / $0 expenses / No income or expenses this period despite the new transaction existing.

- [X] IX-17: Marking or unmarking a transaction as tax-related updates its recorded tax status and its inclusion in the corresponding annual tax summary.

- [X] IX-18: The transaction count and total amount for each category update when categorized transactions are created or changed.

- [X] IX-19: Users can print the tax summary for a selected year without an application error.


## Content
- [X] CT-20: Users can view each transaction's date, description, category, reference, amount, and tax-related status, together with income and expense totals for the current result set.

- [ ] CT-21: Current-month income, expenses, net profit or loss, recent activity, and monthly report totals accurately reflect the underlying transactions.
  - Bug Report:
    - Issue: Current-month figures and monthly report totals do not reflect actual underlying transactions
    - Actual: A -$250.00 expense transaction dated Aug 25, 2026 exists (confirmed in Transactions table), but Dashboard shows month.expenses=$0 and month.profit=$0, Dashboard recent activity omits it, and the Reports page for August 2026 shows $0 income/$0 expenses and 'No income/expenses this period'.

- [ ] CT-22: When a financial chart represents a non-zero amount, its axis labels must use enough precision to distinguish that amount from zero, including for values below $500.
  - Bug Report:
    - Issue: Chart axis labels lack precision to distinguish small non-zero amounts from zero
    - Actual: The only chart with numeric axis labels in the app is the Dashboard's "Income vs Expenses" trend chart (Recharts bar/line chart). Its Y-axis tick formatter renders all values in thousands with a fixed "$Xk" format and zero decimal precision (observed ticks: "$0k" x5 across the current 0-value range). Because the formatter divides by 1000 and rounds to an integer with no decimals, any non-zero value below $500 (e.g. $250, $499) would also render as "$0k", making it visually indistinguishable from an actual $0 value. Note: due to a separate reactivity bug (Transactions changes not reflected on Dashboard/Reports, see IX-16), I was unable to get the app into a state showing genuinely non-zero current-period data to screenshot directly, but the deterministic "$Xk"-only formatter observed on all rendered ticks (no chart anywhere in the app - Dashboard, Reports, Categories, Tax Summary - showed any other axis label format) is sufficient to conclude the precision requirement is not met for sub-$500 amounts.