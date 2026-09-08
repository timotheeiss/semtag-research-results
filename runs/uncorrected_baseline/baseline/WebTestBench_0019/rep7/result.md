# Test Result

## Functionality
- [X] FT-1: Users can successfully register new accounts and log in to the community forum, and their login status is maintained correctly.

- [X] FT-2: Registered users can create or edit their profiles, including personal introductions and hobbies, and the information will display correctly after being saved.

- [X] FT-3: Logged-in users can create new discussion threads in different discussion categories, including titles and content, and successfully publish them.

- [X] FT-4: Users can post replies to any discussion thread, and the replies will be displayed in the thread's reply list in real time.

- [X] FT-5: Users can like or vote on posts or replies made by other users, and the count of likes/votes is updated in real time.

- [X] FT-6: Users can use the keyword search function to find posts, and the search results accurately match the title and body content.

- [X] FT-7: Users can filter the post list by discussion category, displaying only posts from the selected category.

- [X] FT-8: The forum homepage or specific sections can display a list of the most active discussions, sorted by recent interaction frequency.

- [ ] FT-9: Users can view their entire posting and reply history.
  - Bug Report:
    - Issue: Reply history missing from profile
    - Actual: Profile page for QAtester99 shows a 'Threads by QAtester99' section listing threads created, but there is no section displaying the user's reply history, even though the user posted 3 replies in this session. Users cannot view their full posting AND reply history from the profile.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit feature exists for threads/replies
    - Actual: Inspected own thread and own reply (as author) on the thread page; no Edit/Delete/menu button or link is present anywhere in the DOM (checked via text content, aria-label, and title attributes). Since posts/replies cannot be edited at all, there is no way for an 'Edited' mark or last-edit time to ever appear.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: Rich text formatting not rendered
    - Actual: Composed thread content using markdown syntax for bold (**bold**), a link ([link](https://example.com)), and a bullet list (- item). After posting, the content displayed as raw literal text "This is a **bold** statement with a [link](https://example.com) and a list: - Item one - Item two - Item three" with no bold styling, no clickable link, and no list formatting. No rich text toolbar (bold/link/list buttons) was present in the compose form either.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate-content submission prevention
    - Actual: Posted the exact same reply text ("This is a test reply from QA automation. Great thread!") twice in succession via the reply form; both submissions succeeded and created two separate identical reply entries (Replies count went 1→2) with no warning or blocking. No duplicate-detection mechanism was triggered.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.