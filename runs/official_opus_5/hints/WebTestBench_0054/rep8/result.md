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
    - Issue: Graph pan state cannot be set or preserved — panning is non-functional
    - Actual: Document selection, per-section expand/collapse state, and graph zoom ARE preserved across all tab switches (Design Tokens with color-system=closed / spacing-scale=open survived Documents->Favorites->Graph->Documents; zoom stayed 120% and node selection design-tokens persisted). However the graph never pans: the transform stayed "translate(0, 0) scale(1.2)" after a real Playwright drag from the empty SVG canvas centre to another point, and after paced synthetic mousedown/5x mousemove/mouseup sequences. The SVG advertises panning via class "cursor-grab active:cursor-grabbing" and the transform has a translate() slot, but translate is always (0,0), so pan state can never be established or retained.

- [ ] FT-10: Favorites are retained after reload, their navigation count updates immediately, their markers survive document filtering and sorting, and the Favorites view continues to expose every favorited document regardless of a module filter used in Documents.
  - Bug Report:
    - Issue: Documents module filter leaks into the Favorites view, hiding favorited documents from other modules
    - Actual: Persistence, count and marker survival all work: favorites survived a full page reload via localStorage key "tech-docs-storage" (count "Favorites 3", markers on jwt-authentication/database-schema/design-tokens restored); the nav count updated immediately on add (0->1->3) and on remove (3->2); markers survived sorting by Last Updated and the UI module filter. BUT with the UI module filter selected in Documents, the Favorites view listed only favorites.list.item.design-tokens while the nav badge still read 3 — jwt-authentication (Authentication) and database-schema (Data Layer) were hidden. The Favorites view has no module filter of its own (only favorites.search and favorites.sort), yet is filtered by the Documents selection. Setting Documents back to All restored all 3 favorites, confirming the leak.


## Constraint
- [X] CS-12: A document never lists itself as either a content-section reference or a related document.

- [X] CS-13: The knowledge graph contains one node for every available document, associates each node with the document's module, and draws connections for the available related-document relationships.


## Interaction
- [X] IX-15: Each content section has a clear collapsed or expanded indicator, and the indicator's state agrees with whether the section body and expanded links are visible.

- [ ] IX-16: Choosing a document node in the knowledge graph opens that document in the document view without requiring a separate tab change, while preserving the graph view for later return.
  - Bug Report:
    - Issue: Graph node click does not open the document view; a manual tab change is required
    - Actual: Clicking node graph.nodes.item.design-tokens (and earlier redis-caching) only set data-semtag-state="selected" on the node. The active tab stayed nav.graph:active, and no doc.title element existed anywhere on the page. The document only appeared after separately clicking the Documents tab (then doc.title="Redis Caching Strategy"). Graph zoom 120% and node selection were preserved on return, so only the auto-open-in-document-view behavior is missing.

- [X] IX-17: Choosing a reference link shown for a collapsed section leaves the current document open and displays a prompt to expand the section; after expansion, choosing the same link opens its target document.

- [X] IX-18: When search or module criteria match no documents, the document list displays a clear no-results message, and clearing those criteria restores matching documents.