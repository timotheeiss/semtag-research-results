# Test Result

## Functionality
- [X] FT-1: The document list identifies each document's module; choosing a module shows only documents from that module, and choosing all modules restores the complete list.

- [X] FT-2: Selecting a document displays its title, description, version and update date, content sections, and related documents.

- [X] FT-3: From an expanded content section or the related-documents list, choosing a valid document reference makes the referenced document the visible document.

- [X] FT-4: Collapsing an expanded content section hides that section's body and expanded reference links while leaving the section available to reopen.

- [X] FT-5: Expanding a collapsed content section restores that section's body and any reference links it contains.

- [X] FT-6: Users can select a knowledge-graph node and receive a visible selection indication, and can change the graph zoom and reset the graph view with visible zoom feedback.

- [X] FT-7: The knowledge graph represents every available document across all modules and their related-document connections, and its view controls allow the complete graph to be brought into view.

- [X] FT-8: The application provides Documents, Knowledge Graph, and Favorites views, and users can switch among all three.

- [X] FT-9: Switching among Documents, Knowledge Graph, and Favorites preserves the selected document, each section's expanded or collapsed state, and the graph's zoom and pan state.

- [ ] FT-10: Favorites are retained after reload, their navigation count updates immediately, their markers survive document filtering and sorting, and the Favorites view continues to expose every favorited document regardless of a module filter used in Documents.
  - Bug Report:
    - Issue: Favorites view is filtered by the Documents module filter, hiding favorited documents
    - Actual: Favorites persisted across reload (2 favorites restored) and the nav badge updated immediately to 'Favorites 2'; markers survived module filtering (AUTH/UI) and sorting by Title/Last Updated/Module, and search. However, after selecting module filter AUTH in Documents and switching to Favorites, the Favorites list showed only 'JWT Authentication' — the favorited 'Design Tokens' (UI Components) was hidden — while the badge still read 2. Setting the filter back to All made both appear.


## Constraint
- [X] CS-12: A document never lists itself as either a content-section reference or a related document.

- [X] CS-13: The knowledge graph contains one node for every available document, associates each node with the document's module, and draws connections for the available related-document relationships.


## Interaction
- [X] IX-15: Each content section has a clear collapsed or expanded indicator, and the indicator's state agrees with whether the section body and expanded links are visible.

- [ ] IX-16: Choosing a document node in the knowledge graph opens that document in the document view without requiring a separate tab change, while preserving the graph view for later return.
  - Bug Report:
    - Issue: Graph node click does not open the document view; a manual tab change is still required
    - Actual: Clicking the 'Component Library' node (and earlier 'JWT Authentication') only highlighted the node; the app stayed on the Knowledge Graph tab (active tab still 'Knowledge Graph', no document panel rendered in main, waited 1.2s). The document is only shown after the user manually clicks the Documents tab. Graph zoom/selection is preserved on return, but the 'without requiring a separate tab change' behavior is not met.

- [ ] IX-17: Choosing a reference link shown for a collapsed section leaves the current document open and displays a prompt to expand the section; after expansion, choosing the same link opens its target document.
  - Bug Report:
    - Issue: No prompt displayed when clicking a reference link of a collapsed section
    - Actual: Clicking the 'HTTP Methods Guide' link shown under collapsed section 'Endpoint Naming Conventions' kept the current document (correct) but produced no visible prompt/toast/message (verified 3 clicks, waited 30s for any 'Expand section' text). Only a static HTML title="Expand section to access link" tooltip attribute exists, which requires hover and is not shown on click. After expanding the section, clicking the same link correctly opened HTTP Methods Guide.

- [X] IX-18: When search or module criteria match no documents, the document list displays a clear no-results message, and clearing those criteria restores matching documents.