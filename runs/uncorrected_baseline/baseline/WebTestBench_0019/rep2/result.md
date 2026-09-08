# Test Result

## Functionality
- [ ] FT-1: Users can successfully register new accounts and log in to the community forum, and their login status is maintained correctly.
  - Bug Report:
    - Issue: Login session not persisted across page reload
    - Actual: Registered and logged in successfully as 'qatester_th1' (header showed profile link, New Thread, logout button). After reloading the homepage (http://localhost:7019/), the header reverted to showing 'Login'/'Register' links, indicating the user was logged out. Inspection of localStorage, sessionStorage, and cookies showed all empty, confirming no session/token persistence mechanism exists.

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
    - Actual: Visited own profile (/profile/user-1) after posting a new thread and a reply on that thread. The profile page only shows a 'Threads by TechExplorer (3)' section listing threads started by the user. There is no section, tab, or list showing the user's reply history (the reply posted earlier in this session, 'This is my first reply to test the reply functionality.', does not appear anywhere on the profile page).

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality for posts/replies
    - Actual: Inspected the thread page for the user's own thread and own reply (author = logged-in user TechExplorer). No 'Edit' button, pencil icon, or any affordance to edit a published thread or reply exists in the DOM (searched all buttons/links for 'edit' text — none found besides the unrelated 'Edit Profile' button on the profile page). Since posts/replies cannot be edited at all, there is no way for an 'Edited' mark or last-edit-time to ever appear.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text editing/rendering support
    - Actual: The thread content editor is a plain textarea with no formatting toolbar (no bold/link/list buttons). Entered markdown-style content '**bold**', '[link](https://example.com)', and a bullet list. After posting, the thread body rendered the raw text literally ('This is a **bold** statement, here is a [link](https://example.com), and a list: - item one - item two - item three') with no bold styling, no clickable hyperlink, and no list formatting.

- [X] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [ ] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).
  - Bug Report:
    - Issue: No duplicate content submission prevention
    - Actual: Posted a reply with text 'Duplicate submission test reply.'. Then typed the exact same text again into the reply box and clicked 'Post Reply' as a separate action. The system accepted it without any warning, creating two identical replies from the same user in the same thread ('Replies (3)' shows the text 'Duplicate submission test reply.' appearing twice). Note: rapid successive clicks on the same button press (3 synchronous clicks) were only counted once because the input cleared and the button disabled itself after the first submission — but this is only a side-effect of form-reset/button-disable, not an actual duplicate-content detection mechanism, since resubmitting identical text via a fresh click succeeded.


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.