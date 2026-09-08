# Test Result

## Functionality
- [X] FT-1: Users can create financial records by writing text input fields.

- [X] FT-2: Users can view the financial dashboard.

- [X] FT-3: Users can search for financial information by entering keywords.

- [X] FT-4: Users can view and print basic financial summary tables.

- [X] FT-5: Users can filter the product list by product category (such as expenditure, date, transaction).

- [X] FT-6: Users can create new financial records.

- [X] FT-7: Users can manage the financial records they have created.

- [X] FT-8: Users can customize account categories or accounts.

- [X] FT-9: Users can categorize the financial records they create.

- [X] FT-10: Users can view and adjust various items.

- [X] FT-11: Users can mark projects as tax-related.


## Constraint
- [X] CS-12: After a user applies filtering or sorting criteria, the product results displayed by the system must strictly match the selected criteria.

- [X] CS-13: Users cannot create financial records without any input.

- [ ] CS-14: When adding a new financial record, the selected time period cannot be a future time.
  - Bug Report:
    - Issue: Future-dated transaction accepted without validation
    - Actual: Set transaction date to 2026-12-31 (a future date relative to today 2026-09-02) and submitted; the app accepted it and created the transaction 'Test Future Expense' dated Dec 31, 2026 with no error or blocking message.

- [ ] CS-15: The start time of the filtering dates must be earlier than the end time.
  - Bug Report:
    - Issue: No validation preventing From date later than To date
    - Actual: Set From: 2024-12-15 and To: 2024-12-01 (From after To); the app accepted both values without any validation error, warning, or field constraint. It simply displayed 'No transactions found' with no indication that the date range itself was invalid/reversed.


## Interaction
- [X] IX-16: Dashboard information changes in real time when financial information is added, edited, or deleted.

- [X] IX-17: When a user marks a project as tax-related, the corresponding page should provide feedback.

- [X] IX-18: When users create and manage financial records, custom accounts show the already assigned records.

- [X] IX-19: Click "Print Report," and the printed report will appear in the document selection menu.


## Content
- [X] CT-20: Users can view their spending and expenditure details.

- [ ] CT-21: The information on the financial dashboard must match the actual financial information entered.
  - Bug Report:
    - Issue: Dashboard Total Balance does not reflect actual entered transaction data
    - Actual: Dashboard 'Total Balance' shows a static $74,931 that does not correspond to the sum of entered transactions. Total Income ($47,592.50) minus Total Expenses ($36,535.99) from the Transactions page equals $11,056.51, which does not match the $74,931 shown on the dashboard, and the balance figure did not change after adding/editing transactions.