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
    - Issue: No persistence layer whatsoever; all accounts, content, and session state are lost on page reload
    - Actual: After reloading http://localhost:7019/ every change made during the session was reverted: (1) signed-in status lost - nav returned to Login/Register; (2) the registered account is gone - signing in with the exact registered credentials qa.alpha@example.com / 'Passw0rd123' stayed on /login with no session created; (3) the authored discussion is gone - /thread/thread-1787905712246 renders 'Thread not found'; (4) profile changes (bio/interests) lost with the account; (5) replies reverted - thread-1 back to '23 replies' (was 24), thread-3 back to '31 replies' (was 35); (6) vote state reverted - thread-2 back to 89 upvotes (was 93). Storage probe shows localStorage=[], sessionStorage=[], cookies='' - state is in-memory only.


## Constraint
- [ ] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.
  - Bug Report:
    - Issue: Invalid submissions are silently ignored; missing information is never identified to the user
    - Actual: Submitting the completely empty New Thread form leaves the page unchanged with no error text anywhere in new-thread.form (innerText contains only the labels/counter/buttons). Submitting with category='Technology' but whitespace-only title ('   ') and whitespace-only content ('   ') is likewise silently ignored. No native constraint validation exists either: new-thread.title and new-thread.content have required=false and validity.valid=true, and new-thread.category is a BUTTON that does not participate in validation. Blocking works (no discussion is created), but the missing fields are never identified.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not validated at sign-in; any password authenticates an existing account
    - Actual: While logged out, submitted login.email='qa.alpha@example.com' with an incorrect password 'WrongPassword999' (registered password was 'Passw0rd123'). The app redirected to http://localhost:7019/ and the nav changed to the authenticated state (nav.profile='qa_tester_alpha', nav.logout and nav.new-thread present), i.e. a full logged-in session was created. No error was shown and the login was not rejected.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Discussion-creation route renders a completely blank page for unauthenticated users instead of a sign-in screen
    - Actual: While logged out, opening the discussion-creation route http://localhost:7019/new-thread renders an empty page: semantic_snapshot returns no elements at all ({"screen":"new-thread"} with no groups), document.body.innerText is '' and body HTML is only 256 chars. There is no sign-in form, no redirect to /login, no prompt and no way to return to discussion creation. Additionally the logged-out UI exposes no discussion-creation entry point at all (nav shows only Login/Register; nav.new-thread is absent), so the user is left at a dead end.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply totals are inflated seed numbers that do not match the replies actually listed on the detail view
    - Actual: /thread/thread-1 displays thread.reply-count='23 replies' but thread.replies lists only 2 actual replies (reply-1, reply-2). After posting one reply the label became '24 replies' while only 3 replies are listed. The +1 increment is internally consistent, but the absolute total never matches the listed count (24 vs 3). Same pattern on other seeded threads (e.g. thread-6 '89 replies', thread-4 '67 replies').

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied; 'Trending' and 'Most Active' rank by all-time totals
    - Actual: The Trending/Most Active views state no time window anywhere: filter labels are just 'Trending', 'Recent', 'Most Active', the heading is 'Trending Discussions'/'Most Active Discussions', and the subtitle is 'Join the conversation with our community' - no 'last 24 hours', 'this week' or equivalent. Ranking also ignores recency entirely: the top four Trending entries are all dated 'over 2 years ago' (thread-6 156 upvotes, thread-4 124, thread-7 92, thread-2 89), ordered purely by lifetime upvote totals. Most Active likewise sorts on cumulative lifetime reply counts.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded placeholder numbers that neither match actual records nor update when records are created
    - Actual: Category sidebar claims Technology (243) but /category/tech lists only 3 threads; Gaming (312) but /category/gaming lists only 1 thread (thread-4). Community stats claim stats.threads=1,045, stats.replies=4,823, stats.members=892, while the forum actually contains 9 threads total (8 seeded + 1 created). The counts also never update: Technology stayed at (243) and stats.threads stayed at 1,045 both before and after publishing 'Zebra QA Automation Patterns', and stats.members stayed at 892 after registering the new account qa_tester_alpha.