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
    - Issue: Missing reply history on profile page
    - Actual: Profile page only shows a "Threads by qatester2026" section listing created threads. The user's reply (posted earlier on the QA test thread) is not shown anywhere on the profile - there is no "Replies" or "Reply history" section at all.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality for posts/replies
    - Actual: Searched the thread page (own thread, own reply) for any Edit control - none exists. DOM search for buttons/links containing "edit" text returned empty (aside from unrelated "Edit Profile" on the profile page). Since threads/replies cannot be edited at all, there is no way to trigger or verify an "Edited" mark or last-edit timestamp.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing support
    - Actual: The "New Thread" content field is a plain textbox with no bold/link/list toolbar. Entering markdown syntax "**bold text**", "[link](https://example.com)", and a "- list" was posted and rendered as literal raw text (not bold, not a hyperlink, not a bulleted list) on the thread page.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [X] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.