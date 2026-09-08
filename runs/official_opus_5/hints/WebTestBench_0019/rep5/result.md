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
    - Issue: No persistence across page reload — registered accounts and session are lost
    - Actual: Registered qatester1@example.com and confirmed login worked before reload. After a page reload (navigating to /new-thread then /login), signing in with the exact same registered credentials was rejected — stayed on /login with logged-out nav (Sign up / Sign In), identical to the behavior for a never-registered email. The account no longer exists, so signed-in status, profile changes, threads, replies and votes cannot survive a reload either.


## Constraint
- [ ] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.
  - Bug Report:
    - Issue: Missing/blank required fields are not identified — silent no-op
    - Actual: Submitting the empty form and then a form with category "General Discussion" + whitespace-only title ("   ") and whitespace-only content correctly created no discussion (stayed on /new-thread), but no missing information is identified: no error text in the form, no toast/[role=alert], no aria-invalid or required attributes on the title/content inputs. The user gets zero feedback about what is missing.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in
    - Actual: Signed out, then signed in with qatester1@example.com and the incorrect password "wrongpass123" (registered password was "passw0rd"). Login succeeded: redirected to home and nav showed logged-in state (nav.profile = "qatester1", nav.logout present). No error message shown.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in screen for unauthenticated discussion creation; blank page
    - Actual: While logged out there is no "New Thread" entry point in the UI at all (nav shows only Home/Login/Register). Opening the discussion-creation route /new-thread while unauthenticated renders a completely blank page (document.body.innerText === "", accessibility snapshot contains only an empty notifications region) — no redirect to login, no sign-in form, and therefore no way to return to discussion creation after authenticating.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed
    - Actual: thread-1 displays "23 replies" (list and detail view) but its detail view lists only 2 actual replies (reply-1, reply-2). After posting one reply the counter became "24 replies" while only 3 replies are listed. The seeded totals are fabricated, so the counter and the listed replies are inconsistent for every seeded thread.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or applied for Trending / Most Active
    - Actual: The Trending and Most Active views show only "Trending Discussions" / "Most Active Discussions" headings with the subtitle "Join the conversation with our community" — no time period (e.g. "this week", "last 7 days") is stated anywhere. Ranking uses lifetime totals: the top trending item is thread-6 posted "over 2 years ago" with 156 all-time upvotes, and every seeded thread (all 1-3 years old) is ranked purely on its cumulative upvote/reply counters, with no restriction to interactions in any recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded and do not reflect real records or updates
    - Actual: Sidebar shows General Discussion (156), Technology (243), Creative Corner (89), Lifestyle (167), Gaming (312), News (78) but each category page lists only 1-2 actual threads (general = 2, tech = 2). Community stats show 1,045 threads / 4,823 replies / 892 members while the forum contains only 8 seeded threads and a handful of members. After registering a new member the members count stayed 892, and after publishing a new General Discussion thread both stats.threads (1,045) and the General Discussion category count (156) were unchanged.