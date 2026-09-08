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
    - Issue: No persistence — all data and the session are lost on page reload
    - Actual: After reloading the page, the app reset entirely to seed data. The signed-in session was gone (nav showed Login/Register instead of qa_tester_alpha). Both threads authored this session ('Watercolor techniques for absolute beginners', 'Second QA thread for ranking checks') disappeared — the home list is back to the original 8 seeded threads. thread-4 reverted from '68 replies' to '67 replies' (posted reply gone) and thread-3 back to 31; upvote state was reset (thread-4 at 124, reply-4 at 31, deselected). localStorage and sessionStorage are both empty, so nothing is persisted and the registered account no longer exists.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not validated at sign-in; any password grants a session
    - Actual: Registered qa_alpha@example.com with password 'secret123', then signed in with 'wrongpass999'. Login succeeded: redirected to home and nav.profile shows 'qa_tester_alpha' as the logged-in user. No rejection or error.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated discussion-creation route renders a blank dead-end page instead of a sign-in screen
    - Actual: While logged out there is no in-app way to attempt starting a discussion — the 'New Thread' control is removed from the nav entirely. Reaching the creation route directly (/new-thread) renders a completely empty page: document.body.innerText is '' and the accessibility snapshot contains only an empty notifications region, with no header, no sign-in form, and no link back. The user is neither taken to a sign-in screen nor able to return to discussion creation after authenticating.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed on the detail view
    - Actual: thread-4 header metadata showed '67 replies' while the detail view listed 'Replies (1)' with a single reply. After posting one reply the metadata became '68 replies' but the list showed 'Replies (2)'. The two totals differ by the same fabricated offset (66) and never agree. Same pattern on other seeded threads (e.g. thread-6 advertises 89 replies).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; trending/most-active rank by all-time totals
    - Actual: The 'Trending Discussions' and 'Most Active Discussions' views state no time window anywhere — the only subtitle is 'Join the conversation with our community', and the sidebar filters are bare labels 'Trending / Recent / Most Active'. The only 'week' text on the page comes from a thread body, not a window label. Ranking uses lifetime counters: the #1 trending thread (thread-6, 156 upvotes) and #1 most-active thread (89 replies) are both dated 'over 2 years ago', so posts with zero interactions in any recent period still top the lists.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded placeholders that neither match reality nor update on new records
    - Actual: Community Stats show 1,045 Threads / 4,823 Replies / 892 Members, but the forum actually contains 10 threads total. Category counts (General 156, Technology 243, Creative Corner 89, Lifestyle 167, Gaming 312, News 78) likewise far exceed the 1-3 threads each category actually lists. None of the values changed after creating records this session: registering qa_tester_alpha left Members at 892; publishing 2 threads (Creative Corner + General Discussion) left Threads at 1,045 and those categories at 89 and 156; posting 5 replies left Replies at 4,823.