# Test Result

## Functionality
- [ ] FT-1: Users can successfully register new accounts and log in to the community forum, and their login status is maintained correctly.
  - Bug Report:
    - Issue: Registration succeeds but account/session is not persisted; login status not maintained across page reload
    - Actual: Registered new account "QATester99" (qatester99@example.com), got "Account created! Welcome to ThreadHive!" toast and was logged in. After reloading http://localhost:7019/, the header reverted to showing "Login"/"Register" links (logged out). Attempting to log back in with the same registered email/password returned "Invalid credentials. Try: tech@example.com", proving the account was not actually persisted server-side and the session was not maintained.

- [X] FT-2: Registered users can create or edit their profiles, including personal introductions and hobbies, and the information will display correctly after being saved.

- [X] FT-3: Logged-in users can create new discussion threads in different discussion categories, including titles and content, and successfully publish them.

- [X] FT-4: Users can post replies to any discussion thread, and the replies will be displayed in the thread's reply list in real time.

- [X] FT-5: Users can like or vote on posts or replies made by other users, and the count of likes/votes is updated in real time.

- [X] FT-6: Users can use the keyword search function to find posts, and the search results accurately match the title and body content.

- [X] FT-7: Users can filter the post list by discussion category, displaying only posts from the selected category.

- [X] FT-8: The forum homepage or specific sections can display a list of the most active discussions, sorted by recent interaction frequency.

- [ ] FT-9: Users can view their entire posting and reply history.
  - Bug Report:
    - Issue: Profile page shows only thread history, not reply history
    - Actual: Visited own profile page (/profile/user-1) after posting a new thread and a reply. The page only shows a "Threads by TechExplorer (3)" section listing threads created by the user; there is no section, tab, or list showing the replies the user has posted (e.g. the reply "This is a QA test reply to verify real-time display." made earlier is not shown anywhere on the profile).

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality exists for posts/replies
    - Actual: On the thread page for a thread and reply authored by the currently logged-in user (TechExplorer), inspected all buttons/links (including via DOM query for aria-label/title containing "edit", "delete", "menu", "more") and found none. There is no UI affordance to edit a published thread or reply, so an "Edited" mark or last-edit-time feature cannot be tested/exists.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing support; markdown-style formatting is not rendered
    - Actual: The thread content textbox on /new-thread is a plain textarea with no formatting toolbar (no bold/link/list buttons). Entered content "This is a **bold** test with a [link](https://example.com) and a list: - item one - item two" and after posting, the thread page displays this text completely literally (asterisks and markdown link syntax shown as-is), with no bold styling, no clickable link, and no bulleted list rendering.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [X] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.