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
    - Issue: No persistence whatsoever; a page reload wipes all accounts, sessions, content and votes back to seed data
    - Actual: After a page reload the app reset entirely to its seeded state. The registered account qa_tester_alpha was gone and the session was lost (header shows 'ThreadHive / Login / Register'). Both authored discussions disappeared — the thread list returned to exactly the 8 seeded threads with 'QA Alpha Zebra Thread about testing' and 'QA Beta Thread for trending rank test' absent. Replies were lost: thread-1 reverted from '24 replies' to '23 replies' and thread-3 from '35 replies' to '31 replies'. Vote state was lost: thread-...348120's upvote vanished with the thread, and all seeded upvote counts reverted to their originals. localStorage was completely empty ({}), confirming nothing is stored. Saved profile bio/interests were also unrecoverable.


## Constraint
- [ ] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.
  - Bug Report:
    - Issue: Missing/blank fields are silently rejected; no validation message identifies the missing information
    - Actual: Submitting the fully empty New Thread form left the page at /new-thread with no thread created, but the form text contained no error or field-level message. Repeating with category=Technology and whitespace-only title ('   ') and body ('   ') also blocked creation yet again showed no message — form text remained 'Category ... Title 0/200 ... Content Cancel Create Thread' with no indication of which fields were invalid.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in; any password grants a session
    - Actual: Registered qa_alpha@example.com with password 'secret123', signed out, then signed in using password 'wrongpassword999'. The app redirected to the home page and the nav showed the logged-in state (nav.profile = 'qa_tester_alpha', nav.logout present) instead of rejecting the credentials.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation renders a blank dead-end page instead of a sign-in screen
    - Actual: While logged out there is no 'New Thread' affordance anywhere (header shows only 'ThreadHive / Login / Register' and no create-thread CTA exists on the page). Navigating to the creation route /new-thread as an unauthenticated user renders a completely empty page: document.body.innerText is '' and #root contains only an empty notifications section (body HTML 256 chars), still at /new-thread after a 1.5s wait. There is no redirect to sign-in, no login form, no message, and therefore no way to authenticate and return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the number of replies actually listed on the detail view
    - Actual: /thread/thread-1 displayed '23 replies' while its detail view listed only 2 reply items (reply-1, reply-2). After posting a reply the header read '24 replies' but only 3 replies are listed. The seeded totals are inflated placeholders, so the displayed total and the listed replies disagree by 21 on this thread (other seeded threads show the same pattern, e.g. thread-6 '89 replies').

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied; Trending/Most Active rank by all-time totals
    - Actual: The browse filters are only 'Trending / Recent / Most Active' and the headings are 'Trending Discussions' / 'Most Active Discussions' — nowhere is a time window such as 'this week' or 'past 24 hours' stated (page text contains no window phrase). Ranking is by lifetime totals, not interactions in a window: the Trending list is topped by seeded threads that the profile pages date 'over 2 years ago' (thread-6 at 156 upvotes, thread-1 'over 2 years ago'), while threads created today rank last. Most Active likewise sorts on cumulative reply counts (89, 67, 45, ...) regardless of when those replies occurred.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hardcoded placeholders; they neither match actual records nor update when records are created
    - Actual: Only 9 threads exist in the forum (8 seeded + 1 created), yet stats.threads shows '1,045', stats.replies '4,823' and stats.members '892'. Category counts show Technology (243) while /category/tech lists just 3 threads, and Gaming (312) while /category/gaming lists exactly 1 thread. After registering a new member and publishing a new Technology thread, Technology remained (243), stats.threads remained 1,045 and stats.members remained 892 — no count updated.