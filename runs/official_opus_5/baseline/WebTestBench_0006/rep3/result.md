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
    - Issue: Editing a transaction silently wipes its assigned category (Edit dialog does not prefill Category)
    - Actual: Deleting works (confirm dialog → row removed, totals updated). Updating applies the edited fields (description/amount/category/tax all saved correctly when set), BUT the Edit Transaction dialog always shows Category as "Select category" instead of the current category: editing "QA Consulting Payment" to only turn Tax Related off saved the record with an empty Category (row became "Aug 27, 2026 | QA Consulting Payment | (blank) | QA-INV-100 | +$420.50"). Reproduced on seeded data too: Edit on "Google Ads Campaign" (Marketing) opens with combobox text "Select category".

- [X] FT-8: Users can create, edit, and delete income or expense categories, including the category name, type, and color.

- [X] FT-9: The categories available for a transaction are compatible with its income or expense type, and the assigned category is retained with the transaction.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [X] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted silently with no validation message
    - Actual: Setting From=2024-12-31 and To=2024-11-01 was accepted; the table simply showed "No transactions found / Try adjusting your filters". No error, toast, or explanation of the invalid range, and the date inputs carry no min/max constraints.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category is not validated; transaction created without a category
    - Actual: Filled Description="QA No Category Test", Amount=25, left Category as "Select category", clicked Add Expense. Dialog closed with no error/toast and the transaction was saved (row "Aug 27, 2026 | QA No Category Test | (blank category) | — | -$25.00"). Category select has required=false.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Category deletion is not blocked and offers no reassignment; leaves transaction with blank category
    - Actual: Category "QA Temp Expense" had 1 transaction ($100). Delete confirmation stated "Transactions using this category will not be deleted" and the delete succeeded. The transaction "QA Office Chair" (Aug 15, 2026) now displays an empty Category cell with no reassignment prompt.


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
    - Issue: Chart y-axis labels lack precision; non-zero value below $500 renders as $0k
    - Actual: Dashboard "Income vs Expenses" chart plots Aug 2026 income of $420.50, but the y-axis tick labels read "$0k, $0k, $0k, $0k, $1k" — four identical $0k labels, so the $420.50 bar cannot be distinguished from zero.