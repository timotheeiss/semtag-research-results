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
    - Issue: No persistence — all state is in-memory and lost on page reload
    - Actual: After a page reload the home page listed only the 8 seeded threads (thread-6, 4, 7, 2, 5, 8, 3, 1); both discussions I authored (thread-1787879974733 "QA Alpha test thread about zebra optimization" and thread-1787880187307 "QA Beta ranking control thread") were gone, along with the replies I posted and my upvotes (thread-1 back to its seeded values). The registered account qa_tester_alpha and its saved bio/interests were also destroyed, and the header reverted to logged-out "Login / Register". Both localStorage and sessionStorage were completely empty (0 keys), confirming nothing is persisted.


## Constraint
- [ ] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.
  - Bug Report:
    - Issue: Missing/blank fields are never identified to the user
    - Actual: Submitting the empty form, and then a form with category=Technology but whitespace-only title ("   ") and whitespace-only content, both correctly created no discussion and stayed on /new-thread. However no missing information was identified: full page innerText contained no error text, and a DOM query for [aria-invalid="true"], [role="alert"], .text-destructive, .text-red-500 returned an empty list. The Create Thread button simply does nothing, giving the user no indication of what is wrong.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not validated at sign-in
    - Actual: Signed in at /login as qa_alpha@example.com with the deliberately incorrect password "wrongpassword" (registered password was "secret123"). The app redirected to home and the nav showed the logged-in state (nav.profile = "qa_tester_alpha", nav.logout present), so an authenticated session was created despite the wrong password.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated discussion creation shows a blank page instead of a sign-in screen
    - Actual: While logged out there is no "New Thread" entry point anywhere (header shows only "ThreadHive / Login / Register" and no start-discussion CTA exists on the home page). Attempting discussion creation at /new-thread rendered a completely blank page: body innerText was "" and body innerHTML only 256 chars, with zero semantic elements. There was no sign-in form, no message and no redirect to /login, so the user is stranded and cannot authenticate or return to discussion creation. This is specific to the guarded route: loading /login directly renders the full sign-in screen (4161 chars, "Welcome Back / Sign in to continue the conversation").


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed
    - Actual: /thread/thread-1 displayed "23 replies" while its detail view listed only 2 reply items (reply-1, reply-2). After posting one reply the header read "24 replies" but only 3 replies were listed. The seeded counts are fabricated numbers independent of real reply records; the +1 increment is consistent but the absolute totals are not.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; ranking uses all-time totals
    - Actual: The Trending and Most Active views state no time window anywhere. Filter bar reads only "Trending / Recent / Most Active" and the heading area reads "Trending Discussions" / "Most Active Discussions" with the generic subtitle "Join the conversation with our community" — no "this week", "past 7 days", "24 hours" or similar. Ranking is by all-time cumulative counters, not interactions within any window: both lists are topped by thread-6 (156 upvotes / 89 replies) which is dated "over 2 years ago", and every seeded thread in the lists is over 2 years old.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hardcoded and do not reflect or update with real records
    - Actual: Community stats claim 1,045 threads, 4,823 replies and 892 members, but the forum actually contains only 9 threads (all listed on the home page) and a handful of replies (e.g. thread-1 lists 3). Category counts are equally fabricated: Technology shows (243) while /category/tech lists just 3 threads; Gaming shows (312) while /category/gaming lists 1. None of the counts updated after I created a real record: after registering a new member, publishing a Technology thread and posting a reply, stats remained exactly 1,045 / 4,823 / 892 and Technology remained (243).