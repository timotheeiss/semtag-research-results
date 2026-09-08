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
    - Issue: Future date accepted when creating a transaction
    - Actual: Set Date field to 2027-01-01 (a future date relative to current date 2026-09-02) in the Add Transaction dialog and submitted. The system accepted it without any validation error and created the record 'QA Test Office Supplies Purchase' dated Jan 1, 2027 in the transactions table.

- [ ] CS-15: The start time of the filtering dates must be earlier than the end time.
  - Bug Report:
    - Issue: No validation preventing/warning about an inverted date range
    - Actual: Set From: 2024-12-15 and To: 2024-12-01 (start later than end) in the Transactions date filters. The system did not show any validation error or prevent the invalid range; it silently displayed 'No transactions found' with no indication that the date range itself is invalid.


## Interaction
- [X] IX-16: Dashboard information changes in real time when financial information is added, edited, or deleted.

- [X] IX-17: When a user marks a project as tax-related, the corresponding page should provide feedback.

- [X] IX-18: When users create and manage financial records, custom accounts show the already assigned records.

- [X] IX-19: Click "Print Report," and the printed report will appear in the document selection menu.


## Content
- [X] CT-20: Users can view their spending and expenditure details.

- [X] CT-21: The information on the financial dashboard must match the actual financial information entered.