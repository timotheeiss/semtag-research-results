# Test Result

## Functionality
- [X] FT-1: Users can create financial records by writing text input fields.

- [X] FT-2: Users can view the financial dashboard.

- [X] FT-3: Users can search for financial information by entering keywords.

- [ ] FT-4: Users can view and print basic financial summary tables.
  - Bug Report:
    - Issue: Reports page cannot display data for periods where actual transactions exist, and has no print function
    - Actual: The Reports page's month selector only offers the trailing 12 months from the current date (September 2026 back to October 2025). All actual transaction data in the system is dated October–December 2024, which is unreachable in the selector, so every selectable month shows '$0' totals and 'No data for this period' / 'No income this period' / 'No expenses this period' — the summary tables never show real data. Additionally, unlike the Tax Summary page, the Reports page has no 'Print' button or any print-triggering control (verified via DOM query for buttons/links), so users cannot print the financial summary from this page.

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
    - Issue: Future-dated transactions are accepted
    - Actual: Set transaction date to 2027-01-01 (future relative to current date 2026-09-02), submitted the Add Transaction form, and it was accepted without error. New row 'Jan 1, 2027 QA Future Date Test Software — -$25.00' appears in the transactions table and Total Expenses updated to -$36,510.99, confirming no validation prevents future dates.

- [ ] CS-15: The start time of the filtering dates must be earlier than the end time.
  - Bug Report:
    - Issue: No validation prevents start date being after end date
    - Actual: Set From=2024-12-10 and To=2024-12-01 (start later than end) in the Transactions date filter. The system did not show any validation error or block the input; it silently displayed 'No transactions found' with no indication that the date range itself was invalid.


## Interaction
- [X] IX-16: Dashboard information changes in real time when financial information is added, edited, or deleted.

- [X] IX-17: When a user marks a project as tax-related, the corresponding page should provide feedback.

- [X] IX-18: When users create and manage financial records, custom accounts show the already assigned records.

- [X] IX-19: Click "Print Report," and the printed report will appear in the document selection menu.


## Content
- [X] CT-20: Users can view their spending and expenditure details.

- [ ] CT-21: The information on the financial dashboard must match the actual financial information entered.
  - Bug Report:
    - Issue: Dashboard 'Total Balance' does not match actual transaction data
    - Actual: Transactions page footer shows Total Income = $48,092.50 and Total Expenses = -$36,485.99, giving a net of $11,606.51. However, the Dashboard 'Total Balance' card shows $74,931, which does not match this computed net from the actual entered transactions and did not change after adding the $500 income transaction (remained $74,931 before and after), indicating it is not derived from the real transaction data.