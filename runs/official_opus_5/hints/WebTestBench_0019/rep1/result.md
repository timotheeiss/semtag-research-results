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
    - Issue: No persistence across page reload; all user data and session are lost and state resets to seed data
    - Actual: After reloading /thread/thread-4: the session was gone (header showed Login/Register, no nav.profile), my reply "Dave the Diver" had disappeared, thread.reply-count reverted 68 -> 67, my upvote on reply-4 reverted 32 -> 31 and lost its selected state, and localStorage was empty (no storage keys). The registered account itself was destroyed: signing in again with qatester1@test.com / secret123 failed with "Invalid credentials. Try: tech@example.com". Profile edits and both authored threads were also gone.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not validated at sign-in; any password grants a session
    - Actual: Signed in at /login as qatester1@test.com with the wrong password "wrongpassword" (registered password is "secret123"). The app redirected to the home page and the header showed the logged-in state (nav.profile = "qatester1", nav.logout and nav.new-thread present) instead of rejecting the attempt.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated discussion creation renders a blank page instead of a usable sign-in screen
    - Actual: While logged out, reaching discussion creation at /new-thread rendered a completely blank page: #root contained only an empty notifications section, document.body.innerText was empty, and the state persisted after waiting 1.5s. No sign-in screen, no redirect to /login, and no message were shown, so there is no way to authenticate and return to discussion creation. Additionally the logged-out UI exposes no entry point at all (no nav.new-thread control and no "start a discussion" call to action), leaving the URL as the only route.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the number of replies listed on the detail view
    - Actual: /thread/thread-4 showed thread.reply-count = "67 replies" while the detail view listed only 1 reply ("Replies (1)"). After posting one reply both incremented by 1, giving "68 replies" vs "Replies (2)" — the totals remain inconsistent (68 vs 2). Seeded threads carry inflated reply counts unrelated to their actual reply lists.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; trending/most-active rank by all-time totals rather than interactions within a window
    - Actual: The Trending view is labelled only "Trending Discussions" with the subtitle "Join the conversation with our community", and the browse filters are just "Trending / Recent / Most Active" — nowhere is a time window (e.g. "this week", "last 24 hours") stated. Ranking uses lifetime counters: the top trending threads are all timestamped "over 2 years ago" (156, 124, 92 upvotes), while a thread created today whose only upvote occurred today ranks last with 1. Most Active likewise ranks on all-time reply totals (89, 68, 45...).

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded placeholders that neither match actual records nor update on creation
    - Actual: Community Stats show 1,045 Threads / 4,823 Replies / 892 Members while the forum actually contains 10 threads. Category counts claim Technology (243) and General Discussion (156) although /category/tech listed only 3 threads and /category/gaming only 1. None of the counts changed after my session created 1 new member (qatester1), 2 new threads (1 Technology, 1 General Discussion) and 6 new replies: Threads stayed 1,045, Replies stayed 4,823, Members stayed 892, Technology stayed 243, General Discussion stayed 156.