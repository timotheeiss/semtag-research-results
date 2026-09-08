# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Registration/login session not persisted; duplicate username/email not rejected
    - Actual: Registered user 'qatester01' (qatester01@example.com / password123) successfully, was auto-signed-in and could sign out. However: (1) Re-registering with the same username 'qatester01' (different email) was NOT rejected as duplicate - it created a second account and signed in as a new user (profile url changed from /profile/user-1787598060060 to /profile/user-1787598075756), instead of showing a duplicate-username error. (2) After signing out, attempting to sign back in with the exact original registered credentials (qatester01@example.com / password123) failed with 'Invalid credentials. Try: tech@example.com', meaning a newly registered account cannot be logged back into.

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
    - Issue: Page reload wipes session and all in-memory data changes
    - Actual: Performing a full page reload (navigating to http://localhost:6019/) logged the user out entirely (Login/Register links reappeared instead of New Thread/profile link), and reverted all previously created/mutated state to the original seed data: the newly authored 'QA Test Discussion Title' discussion vanished from the Trending list entirely; the reply added to thread-6 was lost (count reverted from 90 back to 89); the 4 replies added to thread-3 'Digital art vs traditional art' were lost (count reverted from 35 back to 31); vote buttons became disabled (logged out) but counts remained at seed values; category counts (e.g. Creative Corner) and Community Stats (1,045 Threads, 4,823 Replies, 892 Members, 47 Online) remained at original seed values, unaffected by prior activity. None of session/login state, profile edits, authored discussions, replies, or vote state persisted across the reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted for login
    - Actual: Logged in as demo account tech@example.com using an incorrect password ('wrongpassword999'); the app showed a 'Welcome back!' toast and signed in successfully as TechExplorer instead of rejecting the invalid password.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion-creation route crashes/blanks the app instead of redirecting to a usable sign-in screen
    - Actual: While logged out, there is no visible 'New Thread'/'Start a discussion' control anywhere in the UI (banner only shows Login/Register links), so the only way to attempt starting a discussion is navigating directly to /new-thread. Doing so does not redirect to /login or show any sign-in prompt; instead the app renders a completely blank page (only the hidden notifications region is present in the DOM - document.body.innerText was empty). Console showed the warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered', indicating the route attempts to redirect during initial render, which fails and leaves the page blank rather than showing a usable sign-in screen. Reproduced twice consistently.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Reply total displayed on discussion header does not match actual replies listed
    - Actual: On thread-6, the header/article showed '89 replies' while the 'Replies' section below only listed 1 actual reply (heading said 'Replies (1)'). After adding a new reply, the header count became '90 replies' while the Replies section showed 'Replies (2)' - a constant offset of 88 remained, so the displayed total never matches the number of replies actually listed, even though it does increase by one when a reply is added.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recency window for Trending/Most Active, and ranked items are not actually recent
    - Actual: Neither the 'Trending Discussions' nor 'Most Active Discussions' views state any time window (e.g. 'this week', 'last 30 days'). Both headings share the identical generic subtitle 'Join the conversation with our community' with no mention of a recency scope. Additionally, every thread shown in both lists is timestamped 'over 2 years ago', confirming the ranking is not scoped/filtered to any recent window at all - it simply ranks all-time seed data by upvotes (Trending) or reply count (Most Active) regardless of age.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category/community counts do not update when new discussions are created
    - Actual: After creating a new discussion in the 'Creative Corner' category, the sidebar category count still showed 'Creative Corner (89)' (unchanged) instead of incrementing to 90, and the Community Stats 'Threads' counter remained at 1,045 instead of increasing to reflect the newly created thread.