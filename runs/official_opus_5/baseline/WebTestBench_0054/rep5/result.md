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
    - Issue: Graph pan state cannot be exercised (pan drag is non-functional), so pan preservation across tabs is not delivered
    - Actual: Selected document (JWT Authentication), per-section expand/collapse states (Token Structure collapsed, Token Validation expanded) and graph zoom (140%) were correctly preserved across Documents→Knowledge Graph→Favorites→Documents. However the graph can never be panned: a real mouse drag on the canvas (and synthetic mousedown/mousemove) leaves transform at "translate(0, 0)". The svg onMouseDown guard is `e.target === containerRef.current?.querySelector('svg')`, which resolves to the Zoom-In icon svg (first svg in the container), so isDragging is never set and pan stays 0,0 in every view.

- [ ] FT-10: Favorites are retained after reload, their navigation count updates immediately, their markers survive document filtering and sorting, and the Favorites view continues to expose every favorited document regardless of a module filter used in Documents.
  - Bug Report:
    - Issue: Favorites view is filtered by the module filter chosen in Documents, hiding favorited documents from other modules
    - Actual: With 3 favorites (JWT Authentication/Auth, Design Tokens/UI, Rate Limiting/API) and the "API" module filter selected in Documents, the Favorites tab listed only "Rate Limiting" while the nav badge still read "Favorites 3". The Favorites view has no module filter controls (only a Sort select), so the filter cannot be cleared from there; returning to Documents and clicking "All" restored all 3 favorites. Other aspects passed: badge updated to 3 immediately on starring, markers survived sorting by Last Updated and API module filtering, and after reload the badge (3), card markers and Favorites list were all retained (localStorage key tech-docs-storage).


## Constraint
- [X] CS-12: A document never lists itself as either a content-section reference or a related document.

- [X] CS-13: The knowledge graph contains one node for every available document, associates each node with the document's module, and draws connections for the available related-document relationships.


## Interaction
- [X] IX-15: Each content section has a clear collapsed or expanded indicator, and the indicator's state agrees with whether the section body and expanded links are visible.

- [ ] IX-16: Choosing a document node in the knowledge graph opens that document in the document view without requiring a separate tab change, while preserving the graph view for later return.
  - Bug Report:
    - Issue: Graph node click does not open the document in a document view; a manual tab change is required
    - Actual: Single-clicking the "REST API Design Guidelines" node (and double-clicking "JWT Authentication") only added a selection glow ring. The Knowledge Graph tab stayed active and main content contained only the graph (legend, zoom label, node labels) — no document title, sections, or related-documents panel was rendered. The user must click the Documents tab to read the selected document.

- [X] IX-17: Choosing a reference link shown for a collapsed section leaves the current document open and displays a prompt to expand the section; after expansion, choosing the same link opens its target document.

- [X] IX-18: When search or module criteria match no documents, the document list displays a clear no-results message, and clearing those criteria restores matching documents.