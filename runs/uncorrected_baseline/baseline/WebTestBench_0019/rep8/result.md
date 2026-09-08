# Test Result

## Functionality
- [ ] FT-1: Users can successfully register new accounts and log in to the community forum, and their login status is maintained correctly.
  - Bug Report:
    - Issue: Login status not persisted across page reload
    - Actual: Registered a new account (qatester01), which logged the user in immediately (nav showed username, New Thread link, logout button). After navigating/reloading to the homepage URL, the user was logged out again — nav reverted to Login/Register links. Inspected localStorage, sessionStorage, and cookies via browser_evaluate: all empty, confirming no session persistence mechanism exists.

- [X] FT-2: Registered users can create or edit their profiles, including personal introductions and hobbies, and the information will display correctly after being saved.

- [X] FT-3: Logged-in users can create new discussion threads in different discussion categories, including titles and content, and successfully publish them.

- [X] FT-4: Users can post replies to any discussion thread, and the replies will be displayed in the thread's reply list in real time.

- [X] FT-5: Users can like or vote on posts or replies made by other users, and the count of likes/votes is updated in real time.

- [X] FT-6: Users can use the keyword search function to find posts, and the search results accurately match the title and body content.

- [X] FT-7: Users can filter the post list by discussion category, displaying only posts from the selected category.

- [X] FT-8: The forum homepage or specific sections can display a list of the most active discussions, sorted by recent interaction frequency.

- [ ] FT-9: Users can view their entire posting and reply history.
  - Bug Report:
    - Issue: Reply history is not shown on user profile
    - Actual: Visited /profile/user-1 (TechExplorer), who is known to have authored a reply on thread-2 ('The Pomodoro technique is great!...' visible on that thread page). The profile page only shows a 'Threads by TechExplorer (2)' section listing threads they started; there is no section, tab, or list showing the replies they've posted. Users can only view their thread-creation history, not their full posting AND reply history as required.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality for posts/replies exists
    - Actual: Viewed own thread and own replies (logged in as the author) on the thread detail page. No 'Edit' or 'Delete' button/icon is present anywhere in the thread article or reply items (verified via DOM query for all buttons/links — only 'Back' and 'Post Reply' have any text, others are unlabeled icon buttons for upvote). Since threads/replies cannot be edited at all, there is no mechanism to display an 'Edited' mark or last-edit time.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text rendering support
    - Actual: Entered content with markdown-style rich text syntax: '**bold**', '[link](https://example.com)', and a bullet list ('- item one / - item two / - item three'). After posting, the thread body displayed the raw literal text 'This is a **bold** statement with a [link](https://example.com) and a list: - item one - item two - item three' with no bold styling, no clickable hyperlink, and no rendered list — the editor is a plain textarea with no formatting toolbar and no rendering of markup on display.

- [ ] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.
  - Bug Report:
    - Issue: User state and interactions are not restored after logout/login
    - Actual: Logged in as GameMaster42 (gamer@example.com), upvoted the 'Discussion: The future of remote work' thread (which started at 156), then logged out via the logout button, then logged back in with the same account. After logging back in, the upvote button for that thread was reset to un-toggled/inactive state and the count showed '155' — neither matching the pre-interaction baseline (156) nor reflecting the user's vote (which should have made it 157 and stayed active). This, combined with the earlier confirmed absence of any localStorage/sessionStorage/cookie persistence, shows the app keeps all data in volatile in-memory state that is not tied to a real user session — profile edits, posting history, and interaction status (upvotes) are not reliably restored across logout/login cycles.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate submission prevention
    - Actual: Typed the identical reply text again ('This is my first test reply to this thread.') and rapidly clicked the Post Reply button twice (via two back-to-back click() calls). Both submissions succeeded: two 'Reply posted!' toasts appeared and the reply list grew to 'Replies (2)' with two duplicate entries containing the exact same text, author, and near-identical timestamp. The system does not detect or block rapid duplicate content submission.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.