# Test Result

## Functionality
- [X] FT-1: Couples can create a personal wedding planning page and enter key information such as the wedding date.

- [X] FT-2: New users can update information such as dates and budgets on the planning page.

- [X] FT-3: The webpage allows users to filter venues/suppliers by location type, price range, and service type.

- [X] FT-4: Provides a keyword search function for venues/suppliers.

- [X] FT-5: The website allows users to tag their preferred venues/suppliers for easy access later.

- [X] FT-6: The website provides wedding planning tips and checklists for users to view.

- [X] FT-7: New users can save planning tips/lists to their personal planning page.


## Constraint
- [ ] CS-8: The wedding budget cannot be plural.
  - Bug Report:
    - Issue: No validation on budget field for negative/invalid numeric values
    - Actual: Entered -5000 into the Budget field and saved; dashboard displayed 'Budget $-5,000' with no error or rejection, confirming the field accepts invalid negative amounts.

- [ ] CS-9: The number of guests cannot be negative.
  - Bug Report:
    - Issue: No validation preventing negative guest count
    - Actual: Entered -10 into Expected Guests field and saved; dashboard displayed 'Guest Count -10' with no error or rejection.

- [X] CS-10: Tagged favorite options and saved planning content are persistently stored so they are not lost when accessed again.

- [ ] CS-11: The wedding date cannot be an invalid date in the past.
  - Bug Report:
    - Issue: No validation preventing past wedding dates
    - Actual: Entered 2020-01-01 (a past date) into Wedding Date field and saved; dashboard accepted it and displayed 'Wedding Date Jan 1, 2020' with no error, and the 'days until wedding' countdown text disappeared instead of showing an error.


## Interaction
- [ ] IX-12: When a user clicks the "View Details" button, they are smoothly redirected to the details page.
  - Bug Report:
    - Issue: View Details button non-functional
    - Actual: Clicked 'View Details' on venue cards both on /venues and /favorites pages; page URL remained unchanged (still /venues or /favorites) and no modal or details view opened.

- [X] IX-13: Planning Tips / The checklist can be marked as completed normally, and the status is updated in real time.

- [X] IX-14: After switching the filter/search criteria, the venue/supplier list is updated in real time.


## Content
- [X] CT-15: The personal planning page fully displays all key wedding information, with no content missing.

- [X] CT-16: The venue/supplier information includes key details (location, price, services offered).

- [X] CT-17: Planning tips / The list is comprehensive and has practical reference value.

- [X] CT-18: Filter/search results with precise matching criteria, displaying results without errors or omissions.