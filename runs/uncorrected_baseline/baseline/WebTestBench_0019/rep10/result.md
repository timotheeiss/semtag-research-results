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
    - Issue: Reply history not shown
    - Actual: Profile page only displays a "Threads by qatester2026 (1)" section listing threads the user started. Despite the user having posted 3 replies (verified in earlier steps), there is no "Replies by" section, tab, or any other UI element showing the user's reply history. Only thread-starting history is viewable, not the full posting and reply history.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit feature for posts/replies
    - Actual: Inspected the thread and reply DOM for edit/delete controls (buttons, icons, menus) on content authored by the logged-in user; none exist. Only an upvote button, reply-count icon, and view-count icon are present per post. Since there is no way to edit a published thread or reply, there is no "Edited" mark or last-edit-time feature to verify.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing support
    - Actual: The thread composer is a plain textarea with no formatting toolbar (no bold/link/list buttons). Entered markdown-style content "**bold**", "[link](https://example.com)", and a "- item" list rendered as literal raw text on the published thread page (e.g. "This is a **bold** statement with a [link](https://example.com) and a list: - item one - item two - item three") instead of being rendered as bold text, a clickable hyperlink, or a bulleted list.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate submission prevention
    - Actual: Rapidly clicking "Post Reply" twice with identical unchanged reply text ("Great topic! Thanks for starting this discussion.") created two separate duplicate reply entries and fired the "Reply posted!" toast twice; reply count went from 1 to 3 total after the duplicate submission. The system did not detect or block the repeated identical content.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.