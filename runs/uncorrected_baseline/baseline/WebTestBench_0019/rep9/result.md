# Test Result

## Functionality
- [ ] FT-1: Users can successfully register new accounts and log in to the community forum, and their login status is maintained correctly.
  - Bug Report:
    - Issue: Login status not persisted; newly registered credentials cannot log in
    - Actual: Registering a new account (qatester2026@example.com) auto-logs the user in, but reloading the page (http://localhost:7019/) reverts to logged-out state (Login/Register links shown). Attempting to explicitly log in with the just-registered credentials returns 'Invalid credentials. Try: tech@example.com', proving the new account cannot actually be used to log in. Login itself works only with pre-seeded demo accounts (e.g. tech@example.com), and that session likewise does not survive a page reload.

- [X] FT-2: Registered users can create or edit their profiles, including personal introductions and hobbies, and the information will display correctly after being saved.

- [X] FT-3: Logged-in users can create new discussion threads in different discussion categories, including titles and content, and successfully publish them.

- [X] FT-4: Users can post replies to any discussion thread, and the replies will be displayed in the thread's reply list in real time.

- [X] FT-5: Users can like or vote on posts or replies made by other users, and the count of likes/votes is updated in real time.

- [X] FT-6: Users can use the keyword search function to find posts, and the search results accurately match the title and body content.

- [X] FT-7: Users can filter the post list by discussion category, displaying only posts from the selected category.

- [X] FT-8: The forum homepage or specific sections can display a list of the most active discussions, sorted by recent interaction frequency.

- [ ] FT-9: Users can view their entire posting and reply history.
  - Bug Report:
    - Issue: Profile only shows thread history, no reply history
    - Actual: Visited own profile (/profile/user-1) after posting 3 threads and 2 replies as TechExplorer. The page only displays a 'Threads by TechExplorer (3)' section listing threads created; there is no section, tab, or list showing the replies the user has posted (the 2 replies posted earlier in this session do not appear anywhere on the profile).

- [X] FT-10: When browsing posts, users can view the poster's public profile information.

- [ ] FT-11: After a user edits a published post or reply, the page will display an "Edited" mark or the last edit time.
  - Bug Report:
    - Issue: No edit functionality exists for threads or replies
    - Actual: On the thread page (as the author of both the thread and its replies), no Edit button/control is present anywhere on the thread article or on individual replies - only an upvote button is shown per post. Since posts/replies cannot be edited at all, there is no way to trigger or verify an 'Edited' mark or last-edit timestamp.

- [ ] FT-12: The forum supports basic rich text editing features, such as bolding, links, and lists, and renders them correctly after posting.
  - Bug Report:
    - Issue: No rich text rendering support
    - Actual: Thread content submitted with markdown syntax '**bold**', '[helpful link](https://example.com)' and a '- ' bulleted list rendered as raw literal text on the thread page ('This is a **bold** statement with a [helpful link](https://example.com) and a list: - Item one - Item two - Item three') instead of being rendered as bold text, a clickable hyperlink, and a bullet list. The content textbox is a plain textarea with no formatting toolbar (no bold/link/list buttons).

- [ ] FT-13: When a user logs out and logs back in, their profile, posting history, and interaction status are correctly restored.
  - Bug Report:
    - Issue: No persistence of session or user data across reload/logout-login
    - Actual: After logging in as tech@example.com, editing the profile bio/interests, creating a thread, posting replies, and applying upvotes, then navigating away and back (simulating logout/re-login via page reload), the session was lost (Login/Register shown instead of logged-in state) and all changes reverted: profile bio is back to the original 'Full-stack developer passionate about open source...' text, interests reset to original 'Programming, Open Source, AI/ML, DevOps', the new thread and its replies/upvotes no longer exist anywhere in the app, and 'Threads by TechExplorer' count dropped back from 3 to 2. None of the profile, posting history, or interaction state was restored.


## Constraint
- [X] CS-14: When a user attempts to post a thread with an empty title or empty body, the system will block the submission and provide a warning.

- [X] CS-15: The system can prevent duplicate submissions of the same content (such as repeatedly and rapidly posting the same reply).


## Content
- [X] CT-16: The forum homepage or specific sections can display a list of recently popular discussions, sorted by the number of likes, replies, or activity levels.