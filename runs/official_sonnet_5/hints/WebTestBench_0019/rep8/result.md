# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username and duplicate email are not rejected during registration
    - Actual: Register/sign-in/sign-out/sign-back-in flow for a unique account works correctly. However, registering a second account with username 'qatester1' (already taken) but a different email succeeded and logged in as a new/duplicate 'qatester1'. Similarly, registering a third account with a new username 'qatester2' but the email already used by 'qatester1' (qatester1@example.com) also succeeded without any rejection or error message.

- [X] FT-2: A registered user can edit their bio and comma-separated interests, save the changes, and see the saved bio and interests on their own public profile.

- [X] FT-3: A logged-in user can publish a discussion with a category, title, and body; the published discussion displays those values and appears in its selected category.

- [X] FT-4: A logged-in user can add a non-empty reply to a discussion, and the reply appears without a manual reload while the discussion's displayed reply total increases by one.

- [X] FT-5: A logged-in user can add or remove their single upvote on another user's discussion or reply, and the displayed count and selected state update immediately.

- [X] FT-6: Submitting a keyword search returns only discussions whose title or body contains the keyword, case-insensitively, and an unmatched keyword produces an empty result with clear feedback.

- [X] FT-7: Selecting a discussion category shows only discussions assigned to that category, and selecting a different category changes the result set to that category.

- [X] FT-8: The Most Active discussion list is ordered by reply count from highest to lowest and updates its order when reply activity changes the ranking.

- [X] FT-10: From a discussion or reply, users can open the author's public profile and view the username, bio, interests, join date, reputation, and authored discussions.

- [X] FT-13: Within the same loaded session, a user's saved profile details, authored discussions, replies, and vote state remain intact after logging out and logging back in.

- [ ] FT-18: Registered accounts, signed-in status, profile changes, authored discussions, replies, and vote state survive a page reload and remain available when the user returns.
  - Bug Report:
    - Issue: No state persists across a page reload
    - Actual: After a full page reload, all in-session state is lost: the logged-in user (qaengineer) is signed out (nav shows Login/Register instead of profile), the newly created 'QA Zebra Testing Thread' no longer appears in the thread list, thread-8's reply count reverted from 42 back to 38, thread-2's upvotes reverted from 93 back to 89, and thread-1's replies reverted from 24 back to 23. Registered accounts (qatester1, qatester2, voter1-4, qaengineer) are also no longer valid for login after a reload.


## Constraint
- [ ] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.
  - Bug Report:
    - Issue: No missing-field feedback shown on invalid submit
    - Actual: Submitting the New Thread form with no category, title, or body correctly does not create a discussion (page stays on /new-thread), but no error message, field highlighting, or other feedback identifies which fields are missing. DOM search for error/invalid/required text found no matches.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Login succeeds with an incorrect password
    - Actual: Logging in with email qaengineer@example.com (registered password 'test123') and an incorrect password 'wrongpassword' did not show any error and instead created a logged-in session as 'qaengineer' (nav.profile shows 'qaengineer', redirected to home page).

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation results in a blank page, not a usable sign-in screen
    - Actual: While logged out, navigating to /new-thread does not redirect to a usable Login screen. The page renders completely blank (only the notification region, no form, no message, no login prompt) and the URL stays at /new-thread. Console shows a React warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered', indicating a broken redirect attempt. There is no way to return to discussion creation after authenticating because the sign-in screen never appears.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Reply total displayed does not match actual replies listed
    - Actual: On thread-1 ('What programming language should I learn in 2024?'), the header/summary shows '23 replies' but the detail view's Replies section heading reads 'Replies (2)' and only 2 replies are actually rendered (reply-1 by GameMaster42, reply-2 by NewsHound). The displayed total (23) does not match the number of replies listed (2).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or enforced for Trending/Most Active
    - Actual: Neither the Trending nor Most Active view displays any stated time window (e.g. 'last 7 days', 'this week'). Both lists rank threads purely by all-time upvote/reply totals: threads timestamped 'over 2 years ago' rank at the very top alongside a thread created minutes ago, with no visible filtering or window label anywhere on the page.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category counts do not update to reflect actual created records
    - Actual: Community Stats sidebar shows stats.threads=1,045 and stats.members=892 unchanged even after creating 1 new discussion and registering 5 new accounts (qaengineer, voter1-4) in this session; stats.replies remains 4,823 despite 6 replies being added during testing. Additionally, category 'Technology (243)' claims 243 threads but the actual /category/tech view only lists 3 threads (thread-7, thread-1, and the newly created QA thread), showing the displayed counts do not reflect the discussions/replies/members actually present in the forum.