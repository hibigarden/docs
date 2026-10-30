# Export a note as PDF

Open a note, then choose **Export as PDF…** from the command palette or the **File** menu. Choose where to save the PDF. Hibi includes your unsaved edits and renders the note the same way from Normal and Source view.

The PDF always uses Hibi’s light theme, including diagrams, even when the app uses a dark theme. Enabled syntax addons render supported content such as math and Mermaid diagrams. Local images referenced by the note are embedded, and images with an `https://` address are downloaded during the export. Other remote images show their description. Frontmatter properties are omitted.

The page size follows your system’s region: Letter in the United States, Canada, Mexico, the Philippines, Chile, Colombia, and Venezuela, and A4 elsewhere. Pages have 0.75-inch margins.

To export a whole workspace, see [Export a workspace to HTML](exporting.md). For Typst’s own PDF export, see [Typst documents and PDF export](../editing/typst.md).
