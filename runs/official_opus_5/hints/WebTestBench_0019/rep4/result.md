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

- [X] FT-13: Within the same loaded session, a user's saved profile details, authored discussions, replies, and vote state remain intact after logging out and logging back in.

- [ ] FT-18: Registered accounts, signed-in status, profile changes, authored discussions, replies, and vote state survive a page reload and remain available when the user returns.
  - Bug Report:
    - Issue: No persistence: all application state is in-memory and is destroyed by a page reload
    - Actual: After reloading http://localhost:7019/, the session was logged out and every created record was gone: both threads authored by qatester1 disappeared from the list, thread-1 reverted to 47 upvotes / 23 replies and thread-3 to 31 replies (my reply and 4 replies gone), and my upvotes were cleared. Logging in again with the registered qatester1@test.com / secret123 returned "Invalid credentials. Try: tech@example.com", i.e. the registered account itself no longer exists. localStorage and sessionStorage are both empty (0 keys).


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in; any password is accepted
    - Actual: Signed in with email qatester1@test.com and the deliberately wrong password "wrongpass999" (registered password was "secret123"). The app redirected to / and the navbar showed "New Thread / qatester1", i.e. a fully logged-in session was created instead of rejecting the attempt.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated discussion creation renders a blank page instead of a sign-in screen
    - Actual: When logged out, the header exposes no "New Thread" entry point at all, and visiting the discussion-creation route /new-thread renders a completely empty document (document.body.innerText === "", semantic snapshot contains zero elements). No sign-in prompt, redirect to /login, or any way to return to discussion creation after authenticating.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed on the discussion detail view
    - Actual: /thread/thread-1 shows "23 replies" but the detail view lists only 2 replies (reply-1, reply-2). After posting one reply the header showed "24 replies" while only 3 replies are listed. The same discrepancy exists across seeded threads (e.g. thread-6 "89 replies").

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied for Trending / Most Active
    - Actual: Headings read only "Trending Discussions" / "Most Active Discussions" with subtitle "Join the conversation with our community"; the filter bar shows only "Trending / Recent / Most Active". No period label (e.g. "this week", "last 7 days") appears anywhere. Both lists rank by all-time totals: the top entries are threads posted "over 2 years ago" (thread-6, 156 upvotes / 89 replies), so ranking is not limited to interactions in any recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are static placeholders that never reflect or update with actual records
    - Actual: Community Stats stayed at Threads 1,045 / Replies 4,823 / Members 892 after registering a new member, creating a thread and posting a reply. Sidebar category counts also unchanged: "Creative Corner (89)" after adding a Creative Corner thread, while /category/creative actually lists only 3 threads. Site-wide only 9 threads exist versus the claimed 1,045.