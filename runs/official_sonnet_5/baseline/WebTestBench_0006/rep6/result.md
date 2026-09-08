# Test Result

## Functionality
- [X] FT-1: Users can create financial transactions with a type, date, description, amount, and category, and may also record a reference, notes, and tax-related status.

- [ ] FT-2: Users can view a summary of their financial status, including total balance, current-month income and expenses, net profit or loss, financial trends, spending by category, and recent transactions.
  - Bug Report:
    - Issue: Total Balance does not accurately reflect underlying transactions
    - Actual: Dashboard displays Total Balance, Monthly Income, Monthly Expenses, Net Profit/Loss, trend indicators, Income vs Expenses chart, Expenses by Category chart, and Recent Transactions. However, after adding a new $300 income transaction, Total Balance remained unchanged at $74,931 (same as before the transaction was added), while Monthly Income and Net Profit/Loss correctly updated (+$300). This means Total Balance is not being computed/refreshed from the actual transaction ledger, an accuracy failure. Additionally, the 'vs last month' trend percentage showed '↑ 0%' despite Monthly Income going from $0 (July 2026, no prior data) to $300 (Aug 2026) — a change from zero should not display as 0%, indicating a flawed trend calculation.

- [X] FT-3: Users can search transactions by keywords contained in their descriptions or references.

- [X] FT-4: Users can review an annual tax summary containing taxable income, deductible expenses, net taxable income, category breakdowns, and the corresponding tax-related transactions.

- [X] FT-5: Users can filter financial transactions by income or expense type, category, and date range.

- [X] FT-6: A valid newly created transaction is retained and shown with the date, description, category, reference, amount, and tax-related status supplied by the user.

- [ ] FT-7: Users can update or delete existing financial transactions, and the resulting records reflect those changes.
  - Bug Report:
    - Issue: Edit form does not pre-populate the Category field for existing transactions
    - Actual: Opening the Edit dialog for the 'Aug Test Income Small' transaction (which has Category='Consulting' clearly shown in the table row) showed the Category combobox displaying the placeholder 'Select category' instead of 'Consulting'. All other fields (Type, Date, Description, Amount, Tax Related) pre-populated correctly. If a user edits amount/description without noticing/reselecting category, saving would silently blank the category (compounded by the previously-confirmed CS-23 bug that blank categories are accepted without validation). This was previously missed when FT-7 was recorded PASS using a different transaction where category had just been explicitly set via that same edit form; re-testing on an independently created transaction reveals the pre-population defect.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected with explanation
    - Actual: Setting From=2024-12-31 and To=2024-12-01 (start date later than end date) was accepted without any validation error. The page silently displayed a generic 'No transactions found / Try adjusting your filters' empty-state message identical to what appears for any valid-but-empty filter combination, with no indication that the date range itself is invalid. Checked full page text via DOM query for words like 'invalid', 'start date', 'end date', 'later than' — none found.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not blocked or identified
    - Actual: Submitted a new Expense transaction ("Test No Category", $100) with no category selected. The transaction was created successfully and appears in the table with a blank Category cell instead of being rejected or flagged to the user.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category with existing assigned transactions is not blocked or reassigned
    - Actual: Attempted to delete the 'Utilities' category which had 1 assigned transaction ('Electric Bill', -$285.00). The confirmation dialog only warned 'Transactions using this category will not be deleted' (i.e., transactions are kept) but did not block deletion or offer reassignment. Confirming the delete succeeded immediately — the category was removed from the Categories page. Checking the Transactions page afterward showed the 'Electric Bill' row now has a blank/empty Category cell (previously 'Utilities'), confirming the transaction was left with an orphaned/undefined category reference instead of the deletion being blocked or requiring reassignment to another category.


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
    - Issue: Chart axis labels lack precision to distinguish non-zero values from zero
    - Actual: After adding a $300 Aug 2026 income transaction, the 'Income vs Expenses' bar chart rendered a bar spanning nearly the full chart height (SVG path y from 9 to 221, i.e. near-maximum), confirming a real non-zero value is plotted. However, all 5 Y-axis gridline labels read '$0k' (verified via DOM query of .recharts-yAxis tick text nodes: [\"$0k\",\"$0k\",\"$0k\",\"$0k\",\"$0k\"]). The axis label formatter appears to divide by 1000 and round/floor without decimal precision, so any value under $500 (and in this case even $300 as the chart's max) is displayed identically to zero, making the chart unreadable/misleading for small-value periods.