# Test Checklist

## Functionality
- [ ] FT-1: The document list identifies each document's module; choosing a module shows only documents from that module, and choosing all modules restores the complete list.

- [ ] FT-2: Selecting a document displays its title, description, version and update date, content sections, and related documents.

- [ ] FT-3: From an expanded content section or the related-documents list, choosing a valid document reference makes the referenced document the visible document.

- [ ] FT-4: Collapsing an expanded content section hides that section's body and expanded reference links while leaving the section available to reopen.

- [ ] FT-5: Expanding a collapsed content section restores that section's body and any reference links it contains.

- [ ] FT-6: Users can select a knowledge-graph node and receive a visible selection indication, and can change the graph zoom and reset the graph view with visible zoom feedback.

- [ ] FT-7: The knowledge graph represents every available document across all modules and their related-document connections, and its view controls allow the complete graph to be brought into view.

- [ ] FT-8: The application provides Documents, Knowledge Graph, and Favorites views, and users can switch among all three.

- [ ] FT-9: Switching among Documents, Knowledge Graph, and Favorites preserves the selected document, each section's expanded or collapsed state, and the graph's zoom and pan state.

- [ ] FT-10: Favorites are retained after reload, their navigation count updates immediately, their markers survive document filtering and sorting, and the Favorites view continues to expose every favorited document regardless of a module filter used in Documents.

## Constraint
- [ ] CS-12: A document never lists itself as either a content-section reference or a related document.

- [ ] CS-13: The knowledge graph contains one node for every available document, associates each node with the document's module, and draws connections for the available related-document relationships.

## Interaction
- [ ] IX-15: Each content section has a clear collapsed or expanded indicator, and the indicator's state agrees with whether the section body and expanded links are visible.

- [ ] IX-16: Choosing a document node in the knowledge graph opens that document in the document view without requiring a separate tab change, while preserving the graph view for later return.

- [ ] IX-17: Choosing a reference link shown for a collapsed section leaves the current document open and displays a prompt to expand the section; after expansion, choosing the same link opens its target document.

- [ ] IX-18: When search or module criteria match no documents, the document list displays a clear no-results message, and clearing those criteria restores matching documents.