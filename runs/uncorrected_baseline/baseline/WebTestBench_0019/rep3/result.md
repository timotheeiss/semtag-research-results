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
    - Issue: Reply history not shown on profile page
    - Actual: On own profile page (/profile/user-...), only a "Threads by QAEngineerTim (1)" section is shown, listing threads the user started. Despite having posted a reply earlier (visible on the thread itself), there is no "Replies" or reply-history section on the profile page, so users cannot view their entire posting and reply history — only their thread-starting history.

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality for posts/replies
    - Actual: Viewed own thread ("Empty Body Test Title") and own reply ("Reply to test editing feature.") as their author. Neither the thread article nor the reply item exposes any Edit control (no edit icon/button, no menu) — only an upvote button is present. Since posts/replies cannot be edited at all, there is no "Edited" mark or last-edit-time indicator to verify.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing/rendering support
    - Actual: Entered content with markdown-style bold (**bold text**), a link ([link to example](https://example.com)), and a bullet list (- item one/two/three) into the thread Content field. After posting, the thread body renders the raw text verbatim: "This is **bold text**, a [link to example](https://example.com), and a list: - item one - item two - item three" — the asterisks and brackets are shown literally, not rendered as bold, a clickable link, or a list. There is no rich text toolbar (bold/link/list buttons) in the composer either; the Content field is a plain textbox.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate-submission prevention
    - Actual: Typed "Duplicate submission test message." and submitted it as a reply, then retyped the identical text and clicked Post Reply again. The system accepted the second identical submission without any warning, resulting in two separate reply entries with the exact same content, author, and near-identical timestamps ("Replies (3)" list shows "Duplicate submission test message." twice). No duplicate/rate-limit check blocked the repeat post.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.