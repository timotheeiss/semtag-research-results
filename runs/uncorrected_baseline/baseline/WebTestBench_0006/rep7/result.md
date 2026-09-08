# Test Result

## Functionality
- [X] FT-1: Users can create financial records by writing text input fields.

- [X] FT-2: Users can view the financial dashboard.

- [X] FT-3: Users can search for financial information by entering keywords.

- [ ] FT-4: Users can view and print basic financial summary tables.
  - Bug Report:
    - Issue: Summary tables are viewable but no functional print capability is present/verifiable.
    - Actual: Reports page (/reports) displays financial summary data (Total Income, Total Expenses, Net Profit/Loss, Daily Activity, Income/Expenses by Category) confirming users CAN view basic financial summary tables. However, the Reports page itself has no print button/control, and the only print-related control found in the app (Tax Summary's \"Print Report\" button, tested under IX-19) produced no observable in-app print preview, document output, or confirmation when clicked. Therefore the \"print\" half of this requirement could not be verified as functional.

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
    - Issue: App allows creating a transaction with a future date with no validation or warning.
    - Actual: Set Date to 2027-01-01 (future relative to current date 2026-09-02), filled Description/Amount/Category, and clicked "Add Expense". The transaction was created successfully and appeared in the table as "Jan 1, 2027 Future Test Expense -$100.00" with no error or blocking message.

- [ ] CS-15: The start time of the filtering dates must be earlier than the end time.
  - Bug Report:
    - Issue: Date filter does not validate or warn when the "From" date is later than the "To" date.
    - Actual: Set From: 2024-12-31 and To: 2024-12-01 (start later than end). The app silently returned "No transactions found - Try adjusting your filters" with no explicit error message explaining the invalid date range, and did not prevent or correct the invalid input.


## Interaction
- [X] IX-16: Dashboard information changes in real time when financial information is added, edited, or deleted.

- [X] IX-17: When a user marks a project as tax-related, the corresponding page should provide feedback.

- [X] IX-18: When users create and manage financial records, custom accounts show the already assigned records.

- [ ] IX-19: Click "Print Report," and the printed report will appear in the document selection menu.
  - Bug Report:
    - Issue: Print Report produces no observable in-app document/print preview or selection UI.
    - Actual: Clicked \"Print Report\" button on Tax Summary page. No dialog, modal, new tab, or any change appeared in the accessibility tree/DOM. Console messages showed no print-related output, only pre-existing unrelated warnings. There is no in-app \"document selection menu\" showing the printed report — the button appears to only trigger a native OS print dialog (if anything), which is not verifiable or observable within the application's DOM.


## Content
- [X] CT-20: Users can view their spending and expenditure details.

- [X] CT-21: The information on the financial dashboard must match the actual financial information entered.