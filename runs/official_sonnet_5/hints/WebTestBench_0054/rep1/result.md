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
    - Issue: Favorites view incorrectly inherits the Documents module filter, hiding favorited documents
    - Actual: Favorites persisted after reload (count=3, correct docs marked on) — PASS on that sub-behavior. Count updated immediately when favoriting/unfavoriting — PASS. Favorite markers survived filtering and sorting in the Documents list — PASS. However, after setting the Documents module filter to 'UI' and switching to the Favorites tab, the Favorites list showed the 'No favorites yet' empty-state message even though nav.favorites.count still read '3' and none of the 3 favorited documents (database-schema, jwt-authentication, rest-api-design) are UI-module docs. Resetting the Documents filter back to 'All' made all 3 favorited documents reappear in Favorites. This shows the Favorites view does not expose every favorited document regardless of the Documents module filter, violating the requirement.


## Constraint
- [X] CS-12: A document never lists itself as either a content-section reference or a related document.

- [X] CS-13: The knowledge graph contains one node for every available document, associates each node with the document's module, and draws connections for the available related-document relationships.


## Interaction
- [X] IX-15: Each content section has a clear collapsed or expanded indicator, and the indicator's state agrees with whether the section body and expanded links are visible.

- [ ] IX-16: Choosing a document node in the knowledge graph opens that document in the document view without requiring a separate tab change, while preserving the graph view for later return.
  - Bug Report:
    - Issue: Clicking a graph node does not open the document view
    - Actual: Clicking a node (e.g. rest-api-design, then api-versioning) only sets that node's state to 'selected' with a visual highlight; the app remains on the Knowledge Graph tab and no document detail view opens (no navigation, no side panel, no document.title observable appears). Double-clicking the node also produced no navigation. The node's 'target: document.detail' hint is not actually wired to open the document on click.

- [X] IX-17: Choosing a reference link shown for a collapsed section leaves the current document open and displays a prompt to expand the section; after expansion, choosing the same link opens its target document.

- [X] IX-18: When search or module criteria match no documents, the document list displays a clear no-results message, and clearing those criteria restores matching documents.