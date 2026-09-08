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
    - Issue: Reply history not shown on profile
    - Actual: Profile page only shows a "Threads by QATester99" section listing threads created by the user. The reply posted earlier ("This is a test reply from the QA automation agent.") on the thread does not appear anywhere on the profile page — there is no replies/history section, so users cannot view their full posting and reply history, only their own threads.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality for posts/replies
    - Actual: On the thread page (as the post's own author), there is no Edit control on the thread post or its reply — DOM inspection confirms no buttons/links related to "edit" exist anywhere on the page. Since posts and replies cannot be edited at all, there is no way for an "Edited" mark or last-edit-time to ever appear.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing support
    - Actual: The thread/reply content field is a plain textbox with no bold/link/list toolbar buttons (verified via DOM query — only Back/Category/Cancel/Create Thread buttons exist). Typed markdown-style content "This is **bold text** and a [link](https://example.com) and a list: - item one - item two" was posted and displayed as literal plain text on the thread page (asterisks and brackets shown as-is, list not rendered), confirming no rich text formatting (bold, links, lists) is supported or rendered.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate submission prevention
    - Actual: Posted the reply "Exact duplicate reply content check." successfully, then immediately re-typed and re-submitted the exact same text on the same thread. The system accepted it without any warning or block, resulting in two identical consecutive replies from the same user in the Replies list (Replies count went 2->3->4 with the same text appearing twice). No duplicate-content detection or rate-limiting was observed.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.