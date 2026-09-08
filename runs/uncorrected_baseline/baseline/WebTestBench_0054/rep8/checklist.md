# Test Checklist

## Functionality
- [ ] FT-1: Users can browse a list of technical documents organized by module.

- [ ] FT-2: Users can view the document's title, content section, and a list of related documents.

- [ ] FT-3: Users can navigate to the relevant documents by clicking the reference links.

- [ ] FT-4: Users can collapse sections of content to hide their details.

- [ ] FT-5: Users can expand the collapsed content section to display its details.

- [ ] FT-6: Users can interact with nodes and mapping modules.

- [ ] FT-7: Users can view the dynamic knowledge graph in its entirety.

- [ ] FT-8: The webpage is divided into several tabs: viewing documents, images, and bookmarks.

- [ ] FT-9: When cutting tags, the previous page state (such as whether paragraphs are collapsed or how large the map is) must be preserved.

- [ ] FT-10: The saved content is stored locally, and we need to count how many have been saved. After filtering and sorting, the saved items must not be lost.

- [ ] FT-11: A dynamic knowledge graph visualization of document relationships is provided, displaying connections through interactive nodes and links for intuitive navigation.

## Constraint
- [ ] CS-12: A document cannot reference itself as a related document.

- [ ] CS-13: Dynamic knowledge graphs must correspond to documents and modules.

## Interaction
- [ ] IX-14: When a user clicks a reference link that points to a non-existent document, the error must be handled gracefully without breaking the application.

- [ ] IX-15: Visual indicators must show whether the content section is currently collapsed or expanded.

- [ ] IX-16: Clicking on the relevant document node must navigate the user to the view of that document.