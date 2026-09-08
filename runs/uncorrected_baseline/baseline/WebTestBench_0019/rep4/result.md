# Test Result

## Functionality
- [ ] FT-1: Users can successfully register new accounts and log in to the community forum, and their login status is maintained correctly.
  - Bug Report:
    - Issue: Login session not persisted across page reload
    - Actual: Registration works and auto-logs the user in (toast "Account created! Welcome to ThreadHive!" and nav shows username). However, the newly registered account's credentials cannot be used to log in afterward - attempting to sign in with the exact email/password just used to register returns "Invalid credentials. Try: tech@example.com", indicating registered accounts are not actually persisted for login. Additionally, after logging in successfully with the demo account (tech@example.com), reloading the page (navigating to http://localhost:7019/) reverts the header to show "Login"/"Register" links instead of the logged-in user, meaning login state is not maintained across a page reload.

- [X] FT-2: Registered users can create or edit their profiles, including personal introductions and hobbies, and the information will display correctly after being saved.

- [X] FT-3: Logged-in users can create new discussion threads in different discussion categories, including titles and content, and successfully publish them.

- [X] FT-4: Users can post replies to any discussion thread, and the replies will be displayed in the thread's reply list in real time.

- [X] FT-5: Users can like or vote on posts or replies made by other users, and the count of likes/votes is updated in real time.

- [X] FT-6: Users can use the keyword search function to find posts, and the search results accurately match the title and body content.

- [X] FT-7: Users can filter the post list by discussion category, displaying only posts from the selected category.

- [X] FT-8: The forum homepage or specific sections can display a list of the most active discussions, sorted by recent interaction frequency.

- [ ] FT-9: Users can view their entire posting and reply history.
  - Bug Report:
    - Issue: Profile page only shows thread history, not reply history
    - Actual: Navigated to own profile (TechExplorer, /profile/user-1). The page displays a "Threads by TechExplorer (3)" section listing all threads I created, but there is no section, tab, or list showing the replies I posted (I had posted 3 replies on the test thread: two duplicate replies and one markdown-test reply). Users therefore cannot view their entire posting AND reply history - only their thread-creation history is visible.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality for published threads or replies
    - Actual: Inspected the thread page (own newly-created thread and own replies) for any edit control. No "Edit" button, pencil icon, or menu exists on threads or replies - the only buttons present are the upvote/like buttons, "Back", and "Post Reply". Since users cannot edit a published post or reply at all, there is no way to trigger or observe an "Edited" mark or last-edit timestamp.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing/rendering support
    - Actual: The thread/reply composer is a plain HTML &lt;textarea&gt; with no formatting toolbar (no bold/link/list buttons). Posting content with markdown-style syntax "Testing **bold text**, a [link](https://example.com), and a list:\n- item one\n- item two" resulted in the raw text being displayed verbatim as plain text - the asterisks were not rendered as bold, the markdown link was not converted to a clickable hyperlink, and the list items were not rendered as a bulleted list.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate-submission protection on rapid repeated reply posts
    - Actual: After typing the reply text "Great thread! This is a QA test reply." and rapidly clicking Post Reply 3 times in succession, the system posted the exact same reply content twice (visible as two separate identical reply entries with the same text, author, and timestamp), and showed 3 "Reply posted!" toast notifications. The reply count went from 1 to 2 with duplicate content, showing no deduplication or rate-limiting/duplicate-content guard is implemented.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.