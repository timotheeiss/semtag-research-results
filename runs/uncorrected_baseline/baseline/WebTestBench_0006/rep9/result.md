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
    - Issue: Future-dated transactions are accepted without any validation error
    - Actual: Created a transaction dated 2026-12-25 (future relative to current date 2026-09-02); it was accepted and added to the transactions table with no warning or rejection.

- [ ] CS-15: The start time of the filtering dates must be earlier than the end time.
  - Bug Report:
    - Issue: No validation preventing/warning about an inverted date range (From later than To)
    - Actual: Set From=2024-12-20 and To=2024-12-01 (start after end); no error/warning was shown, filter silently returned "No transactions found" instead of blocking the invalid input or alerting the user.


## Interaction
- [X] IX-16: Dashboard information changes in real time when financial information is added, edited, or deleted.

- [X] IX-17: When a user marks a project as tax-related, the corresponding page should provide feedback.

- [X] IX-18: When users create and manage financial records, custom accounts show the already assigned records.

- [ ] IX-19: Click "Print Report," and the printed report will appear in the document selection menu.
  - Bug Report:
    - Issue: Clicking "Print Report" produces no observable/verifiable output
    - Actual: On the Tax Summary page (2024, populated with data), clicked the "Print Report" button. No new browser tab/window opened (confirmed via tab list before/after), no DOM change occurred, and no print dialog or document selection menu appeared in the accessible page tree. There is no observable evidence that a printable report was generated or that any document selection UI appeared.


## Content
- [X] CT-20: Users can view their spending and expenditure details.

- [ ] CT-21: The information on the financial dashboard must match the actual financial information entered.
  - Bug Report:
    - Issue: Dashboard summary figure does not match actual entered financial data
    - Actual: Dashboard "Total Balance" showed $74,931 ("Across all accounts"), but the Transactions page (source of truth) shows Total Income +$47,592.50 and Total Expenses -$36,585.99, yielding an actual net of $11,006.51 — not $74,931. This is a large, unexplained discrepancy between the Dashboard's headline figure and the actual sum of entered transactions.