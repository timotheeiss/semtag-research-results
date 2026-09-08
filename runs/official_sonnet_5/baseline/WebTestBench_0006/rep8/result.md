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
    - Issue: Editing a transaction without re-selecting its category silently drops the category, leaving the transaction uncategorized.
    - Actual: Opened Edit dialog for a transaction whose Category was 'Sales Revenue' (verified via underlying select value 'cat-1'). The Category combobox visually displayed 'Select category' instead of the current value. Without touching the category field, I changed only the Amount and clicked 'Update'. After saving, the transaction's Category cell became blank in the table and the underlying select value was confirmed empty (''). This shows the update operation does not preserve unmodified fields correctly and can silently remove required data (category) from an existing transaction.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range (From > To) not rejected with explanation
    - Actual: Set From=2024-12-31 and To=2024-12-01 (From after To). The app did not reject this invalid range or show any explanatory error message. Native input validationMessage was empty and checkValidity() returned true for both fields. Instead, the UI silently displayed a generic empty-state message 'No transactions found / Try adjusting your filters', identical to what would appear for any filter combination yielding zero results - giving the user no indication that the date range itself was logically invalid.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Transaction created successfully without an assigned category
    - Actual: Filled Description ('CS-23 Test No Category') and Amount (10) but left Category combobox at its 'Select category' placeholder (unselected) and clicked Add Expense. Unlike Description/Amount (which have native 'required' HTML validation blocking submission), the Category field has no such validation - the transaction was created successfully. DOM inspection of the new table row confirmed the Category cell is empty string (not even a placeholder dash), while Reference shows '—'. No error was shown to the user identifying a missing category.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting a category in use by transactions is not blocked and does not require reassignment; affected transactions are left with a blank category.
    - Actual: Deleted the 'Interest Income' category (which had 1 transaction, 'Bank Interest', assigned). The confirmation dialog itself stated 'Transactions using this category will not be deleted' with no reassignment option. After confirming delete, the 'Bank Interest' transaction row in the Transactions table now shows a blank Category cell (no name, no 'Unknown' placeholder) instead of being blocked or reassigned.


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
    - Issue: Income vs Expenses chart Y-axis lacks precision for values under $500, making non-zero amounts indistinguishable from zero.
    - Actual: With current-month income at $150 (non-zero), the chart's Y-axis tick labels all read '$0k' (five ticks: $0k,$0k,$0k,$0k,$0k) as read directly from the SVG text elements via DOM inspection. This makes it impossible to visually distinguish the $150 income bar from a $0 value using the axis labels.