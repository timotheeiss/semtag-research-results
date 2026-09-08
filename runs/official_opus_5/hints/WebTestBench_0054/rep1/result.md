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

- [ ] FT-9: Switching among Documents, Knowledge Graph, and Favorites preserves the selected document, each section's expanded or collapsed state, and the graph's zoom and pan state.
  - Bug Report:
    - Issue: Graph pan state cannot be exercised: drag-to-pan is broken, so pan is permanently (0,0)
    - Actual: Document selection (Design Tokens), section states (color-system:closed, spacing-scale:open) and graph zoom (140%) were all correctly preserved across Documents/Graph/Favorites switches. However the graph's pan can never be changed: a real Playwright drag on the canvas and synthetic mouse-drag sequences both left transform at "translate(0, 0) scale(1.4)". Root cause found in the svg onMouseDown handler: it starts dragging only when `e.target === containerRef.current?.querySelector('svg')`, but that selector returns the first svg in the container, which is the zoom-in button icon (class "lucide lucide-zoom-in"), never the graph canvas. So the pan portion of this requirement is non-functional and its preservation is untestable.

- [ ] FT-10: Favorites are retained after reload, their navigation count updates immediately, their markers survive document filtering and sorting, and the Favorites view continues to expose every favorited document regardless of a module filter used in Documents.
  - Bug Report:
    - Issue: Documents module filter leaks into the Favorites view and hides favorited documents, with no filter control there to clear it
    - Actual: Favoriting 3 docs (design-tokens, jwt-authentication, rest-api-design) updated the nav count to "Favorites 3" immediately, markers survived sorting (by Last Updated) and module filtering, and all 3 favorites plus the count persisted after a page reload. However, with the UI module filter active in Documents, the Favorites view listed only 1 of 3 favorites (design-tokens) while the badge still read 3; the Favorites view exposes only search and sort (favorites.search/favorites.sort) and no module filter, so the filter cannot be cleared from there. Returning to Documents and selecting "All" restored all 3 favorites.


## Constraint
- [X] CS-12: A document never lists itself as either a content-section reference or a related document.

- [X] CS-13: The knowledge graph contains one node for every available document, associates each node with the document's module, and draws connections for the available related-document relationships.


## Interaction
- [X] IX-15: Each content section has a clear collapsed or expanded indicator, and the indicator's state agrees with whether the section body and expanded links are visible.

- [ ] IX-16: Choosing a document node in the knowledge graph opens that document in the document view without requiring a separate tab change, while preserving the graph view for later return.
  - Bug Report:
    - Issue: Graph node click does not open the document; a manual tab change is required
    - Actual: Clicking node circles (rbac-system, then design-tokens) only set data-semtag-state="selected" on the node. After waiting 1.2s the active tab was still nav.graph and no document panel existed in the graph view (doc.title absent). The document only became visible after manually clicking the Documents tab (it then correctly showed the selected document). Nodes declare data-semtag-target="document.detail", so navigation was intended but never occurs.

- [X] IX-17: Choosing a reference link shown for a collapsed section leaves the current document open and displays a prompt to expand the section; after expansion, choosing the same link opens its target document.

- [X] IX-18: When search or module criteria match no documents, the document list displays a clear no-results message, and clearing those criteria restores matching documents.