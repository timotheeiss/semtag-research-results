# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username/email not rejected
    - Actual: Registered qatester1/qatester1@example.com successfully, signed out and back in with same credentials worked fine. However, re-submitting the registration form with the identical username 'qatester1' and identical email 'qatester1@example.com' (different password) was accepted and created a second distinct account (user-1787640416146) instead of being rejected as a duplicate.

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
    - Issue: No data persistence across page reload — all in-memory state is lost
    - Actual: Before reload: logged in as qaengineer3 with edited bio/interests, an authored thread with 1 upvote and 1 reply. After navigating/reloading to the same profile URL: nav bar reverted to logged-out state (Login/Register links), and the profile page showed "User not found" (account itself gone, not just session). Navigating to homepage confirmed the authored thread "FT-13 Persistence Test Thread" no longer appears anywhere in the trending list — it was completely wiped, along with the account, session, profile edits, reply, and vote.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [X] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated user attempting to start a discussion is not taken to a usable sign-in screen
    - Actual: As an unauthenticated (logged-out) user, navigating to the new-thread route (/new-thread) does not redirect to a login/sign-in screen and does not show the "Start a New Discussion" form either. Instead the page renders completely blank (only the notification region is present in the DOM). Reproduced twice consistently.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Reply total badge does not match actual reply list for seeded discussions
    - Actual: On thread-1 ('What programming language should I learn in 2024?'), the header badge shows '23 replies' but the 'Replies' section heading and list show only 'Replies (2)' with exactly 2 replies rendered. Same mismatch observed on thread-3 ('Digital art vs traditional art'): badge shows '31 replies' but Replies section shows 'Replies (0)' / 'No replies yet.' This mismatch is present on seeded/pre-existing discussions across the app (newly created discussions in-session do stay consistent, e.g. 0→1 after adding a reply).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or enforced for Trending/Most Active views
    - Actual: Checked all three browse views (Trending, Recent, Most Active). Each shows only a heading ("Trending Discussions" / "Recent Discussions" / "Most Active Discussions") and a generic subtitle "Join the conversation with our community" — no text anywhere indicates a time window (e.g., "today", "this week", "last 24 hours"). Additionally, both Trending and Most Active views include threads timestamped "over 2 years ago" ranked alongside/above content posted "less than a minute ago" / "2 minutes ago", confirming ranking is not scoped to any recent window at all — it spans the entire history of threads regardless of age.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Sidebar category and community stat counts do not update when new data is created
    - Actual: Baseline Community Stats: 1,045 Threads / 4,823 Replies / 892 Members; Technology category showed (243). Actions performed in the same session: registered 3 new accounts (qaengineer5, 6, 7) — Members count remained "892" (no increment). Created a new thread in the Technology category — Threads count remained "1,045" and Technology category count remained "(243)" (no increment for either). Posted a new reply on that thread — Replies count remained "4,823" (no increment). None of the sidebar counts reflected the actual new data created during the session, despite the new thread/reply/accounts being visibly present elsewhere on the page (e.g., in the Trending list and profile).