# Test Result

## Functionality
- [X] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.

- [X] FT-2: A registered user can edit their bio and comma-separated interests, save the changes, and see the saved bio and interests on their own public profile.

- [X] FT-3: A logged-in user can publish a discussion with a category, title, and body; the published discussion displays those values and appears in its selected category.

- [X] FT-4: A logged-in user can add a non-empty reply to a discussion, and the reply appears without a manual reload while the discussion's displayed reply total increases by one.

- [X] FT-5: A logged-in user can add or remove their single upvote on another user's discussion or reply, and the displayed count and selected state update immediately.

- [X] FT-6: Submitting a keyword search returns only discussions whose title or body contains the keyword, case-insensitively, and an unmatched keyword produces an empty result with clear feedback.

- [X] FT-7: Selecting a discussion category shows only discussions assigned to that category, and selecting a different category changes the result set to that category.

- [X] FT-8: The Most Active discussion list is ordered by reply count from highest to lowest and updates its order when reply activity changes the ranking.

- [X] FT-10: From a discussion or reply, users can open the author's public profile and view the username, bio, interests, join date, reputation, and authored discussions.

- [ ] FT-13: Within the same loaded session, a user's saved profile details, authored discussions, replies, and vote state remain intact after logging out and logging back in.
  - Bug Report:
    - Issue: Account credentials not persisted across logout/login within same session
    - Actual: Created testuser001 account with bio, interests, discussion, and replies. After logging out and attempting to log back in with same credentials (testuser001@test.com / password123), system rejected with 'Invalid credentials' error. Account data did not persist, or credentials were not saved properly for re-authentication within same session.

- [X] FT-18: Registered accounts, signed-in status, profile changes, authored discussions, replies, and vote state survive a page reload and remain available when the user returns.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [X] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.

- [X] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [X] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: Time window not stated in Trending/Most Active descriptions
    - Actual: Trending and Most Active view headings display 'Join the conversation with our community' without stating a specific time window. Discussions shown are from 'over 2 years ago', suggesting all-time rankings rather than recent-period rankings. No indicator of whether trends/activity metrics are from last 24h, 7d, 30d, etc.

- [X] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.