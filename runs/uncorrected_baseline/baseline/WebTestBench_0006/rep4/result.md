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
    - Issue: Future-dated transactions are accepted
    - Actual: Submitted an Add Transaction form with date 2026-12-31 (a future date relative to today 2026-09-02) and it was accepted without any validation error, creating a new row "Dec 31, 2026 Future Test Expense Office Supplies — -$100.00 ✓" at the top of the transactions table.

- [ ] CS-15: The start time of the filtering dates must be earlier than the end time.
  - Bug Report:
    - Issue: No validation preventing From date > To date
    - Actual: Set From=2024-12-10 and To=2024-12-01 (start after end). The app did not block or flag this invalid range; it silently rendered "No transactions found / Try adjusting your filters" with no error message indicating the date range itself is invalid.


## Interaction
- [X] IX-16: Dashboard information changes in real time when financial information is added, edited, or deleted.

- [X] IX-17: When a user marks a project as tax-related, the corresponding page should provide feedback.

- [X] IX-18: When users create and manage financial records, custom accounts show the already assigned records.

- [X] IX-19: Click "Print Report," and the printed report will appear in the document selection menu.


## Content
- [X] CT-20: Users can view their spending and expenditure details.

- [ ] CT-21: The information on the financial dashboard must match the actual financial information entered.
  - Bug Report:
    - Issue: Dashboard "Total Balance" does not reconcile with actual transaction data
    - Actual: Transactions page shows Total Income = +$48,092.50 and Total Expenses = -$36,485.99 (net = $11,606.51 across all recorded transactions), yet the Dashboard's "Total Balance" card displays a static $74,931 both before and after adding/removing test transactions — it never changed when transactions were added (e.g. after adding the $500 IX-16 test income, Monthly Income and Net Profit/Loss cards updated correctly to $500, but Total Balance remained exactly $74,931, unrelated to any combination of the actual entered income/expense totals). Additionally, the Reports page month selector only offers Sep 2026–Oct 2025, while all seeded transaction data is dated Oct–Dec 2024, so Reports never displays the real financial data for any selectable month.