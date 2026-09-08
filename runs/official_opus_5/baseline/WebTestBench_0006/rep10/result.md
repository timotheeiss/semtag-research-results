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
    - Issue: Assigned category not retained when updating an income transaction; edit dialog fails to prefill category and silently clears it on save
    - Actual: Type/category compatibility itself is correct (Expense lists only expense categories, Income only income categories). However the assigned category is not retained on update for income transactions: "QA Beta Consulting Income" was created with category Consulting (table showed "Consulting"; Categories page counted it under Consulting 4 txns/$11,800), then I edited it changing only the Tax Related switch — afterwards its Category cell is blank. Root cause confirmed: the Edit Transaction dialog does not prefill the category for income transactions — seeded "Consulting Fee - Strategy Session" (table: Consulting) and "Product Sales - Q4 Order" (table: Sales Revenue) both open with the category showing "Select category", while expense transactions ("Electric Bill"/Utilities, "QA Gamma Utility Bill"/Utilities) prefill correctly. Saving any income transaction therefore silently wipes its category.

- [X] FT-11: Users can mark or unmark a transaction as tax-related when creating or updating it.


## Constraint
- [X] CS-12: When search, type, category, and date criteria are combined, every resulting transaction satisfies all active criteria; removing the criteria restores the full set of transactions.

- [ ] CS-13: Financial transactions without a description or amount cannot be created, and the missing required information is identified to the user.
  - Bug Report:
    - Issue: Missing required fields block submit silently with no error message shown to user
    - Actual: Submitting with empty Description (amount 99.99, category Software) and separately with empty Amount (description "QA No Amount") both left the dialog open and did not create the transaction (row count stayed 19), but no validation text, toast, or aria-invalid/field-level indicator was rendered anywhere. The user is given no indication of which information is missing.

- [ ] CS-15: A transaction date range whose start date is later than its end date must be rejected with an explanation of the invalid range.
  - Bug Report:
    - Issue: Inverted date range accepted silently with no validation message
    - Actual: Set From=2024-12-31 and To=2024-01-01 (start later than end). The app accepted the range and simply rendered an empty result: "No transactions found / Try adjusting your filters". No rejection, error, or explanation that the date range is invalid was shown.

- [ ] CS-23: Financial transactions without an assigned category cannot be created, and a missing category is identified to the user.
  - Bug Report:
    - Issue: Missing category not validated; transaction created with blank category
    - Actual: Submitted Add Transaction with Description "QA Missing Category Test", Amount 123.45 and Category left as "Select category". The dialog closed and the transaction was saved. The new row (Aug 28, 2026) shows an empty Category cell. No error message, toast, or field-level indication of the missing category was shown.

- [ ] CS-24: Deleting a category assigned to existing transactions must be blocked or require reassignment so no transaction is left with a blank or Unknown category.
  - Bug Report:
    - Issue: Deleting an in-use category is neither blocked nor offers reassignment; transactions are orphaned with blank category
    - Actual: Deleted the "Marketing" category which showed "2 transactions". The confirm dialog was identical to that for an unused category and even stated "Transactions using this category will not be deleted" — no block and no reassignment option. After deletion, "QA Alpha Marketing Spend EDITED" (-$400.00) and "Google Ads Campaign" (-$850.00) both display an empty Category cell in the transactions table.


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
    - Issue: Chart y-axis labels lack precision; non-zero value indistinguishable from zero
    - Actual: Dashboard "Income vs Expenses" chart plots an Aug-2026 expense of $400 (bar rendered at full 216px height), but all 5 y-axis ticks are labelled "$0k". The axis uses whole-thousands ($Xk) formatting, so a $400 value and $0 are labelled identically and cannot be distinguished.