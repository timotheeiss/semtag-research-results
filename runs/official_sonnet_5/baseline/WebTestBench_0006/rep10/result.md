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
    - Issue: Editing a transaction corrupts its category association
    - Actual: Opened Edit dialog for existing transaction (Category=Consulting). The Category combobox in the Edit dialog incorrectly displayed 'Select category' instead of 'Consulting' even though the underlying hidden select element had value 'cat-1'. Changed Amount to 750.50 and toggled Tax Related off, then clicked 'Update Income' without touching the category field. After saving, the transaction row's Category cell became completely blank (previously 'Consulting'), while Amount correctly updated to +$750.50 and Tax checkmark correctly removed. This shows the edit form's failure to visually pre-populate the category causes the save operation to wipe out the transaction's category on any edit, silently corrupting existing data.

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing description/amount not identified to user
    - Actual: Submitting the Add Transaction form with empty Description and empty Amount is silently blocked (no new row is added, row count stays at 19), but no error message, red border, aria-invalid, or any visible/inspectable indication is shown near the Description or Amount fields, and the aria-live region remains empty. The user is not told what is missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Invalid date range not rejected with explanation
    - Actual: Setting From=2024-12-31 and To=2024-11-01 (start after end) does not produce any validation error. The filter silently returns zero results with a generic "No transactions found / Try adjusting your filters" message, without ever informing the user that the date range itself is invalid (start > end).

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not identified to user
    - Actual: Submitting the Add Transaction form without selecting a Category (still showing "Select category") is silently blocked (no new row added), but no error message or field-level indication is shown to inform the user the category is required.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is not blocked and does not require reassignment
    - Actual: Deleted the 'Utilities' category which had 1 transaction assigned ('Electric Bill', -$285.00). The confirmation dialog itself stated 'Transactions using this category will not be deleted' — with no option to reassign or block the deletion. After confirming delete, the 'Electric Bill' transaction row was left with a blank/orphaned Category cell (verified via browser_evaluate: row text became 'Dec 5, 2024Electric Bill—-$285.00✓' with no category name between date and reference dash). This leaves data in an inconsistent state instead of blocking deletion or prompting for reassignment as required.


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
    - Actual: With a max monthly value of $500.25, both the Dashboard "Income vs Expenses" chart and Reports "Daily Activity" chart render 5 Y-axis tick labels as $0k, $0k, $0k, $0k, $1k. The intermediate non-zero tick values (~$125, $250, $375) are all displayed as "$0k", making them indistinguishable from an actual $0 value, and even the $500 top tick rounds to "$1k" rather than showing precise value.