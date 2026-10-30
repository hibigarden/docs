# Media and navigation

## Files and folders

Drop a supported file onto Hibi to open it, or drop a folder to open a workspace. See [workspace file actions](explorer.md) for moving and organizing files in the sidebar.

## Attachments

Use the image toolbar button to choose an image or video, or drop one into the editor. An untitled note asks you to save first; canceling leaves it unchanged.

Hibi copies attachments into an `assets` folder beside the note and keeps the originals. Move the note and its assets together to keep media working. In Markdown source, edit the description in `![description](assets/image.png)` to give screen readers useful text.

### Supported media

Images can be PNG, JPEG, GIF, WebP, AVIF, or SVG, up to 8 MiB each. Videos can be MP4, MOV, WebM, or OGG/Theora, up to 512 MiB each, though playback depends on the codec.

Images with an `https://` address load from their website, which can see when you open the note. Hibi does not load `http://` images or remote videos.

[Documentation exports](../guides/exporting.md) have a 20 MiB total limit, so share large videos separately.

### Local paths

Relative paths start from the saved note's folder. Absolute paths and local `file:` URLs also work. Use percent encoding or angle brackets for spaces in Markdown paths. Missing media shows its description instead.

Obsidian `![[image.png]]` and `![[drawing.svg]]` embeds also display in the formatted editor. Hibi looks beside the note, at the vault root, and in the attachment folder named by Obsidian's `attachmentFolderPath` setting. Hibi reads that setting without changing `.obsidian` files. `![[Other note]]` shows a link to the note rather than embedding its content.

## Edit a link

Place the caret in a link and choose **Edit link** in the toolbar; the **Link** action changes to **Edit link** there. The dialog opens prefilled with the link's destination. Choose **Save** to apply a change, **Remove link** to keep only the link's text, or **Cancel** to leave the link unchanged. Use `Cmd/Ctrl+Z` to undo a saved change or removal.

Editing a reference link changes that occurrence to an inline link and leaves its shared definition unchanged.

Normal view also shows a link's destination and its **Edit**, **Open**, **Copy**, and **Remove** actions when you hover over the link or place the caret in it or at either end of it. **Remove** keeps the link's text, including web and email addresses that Hibi detects as links. Escape hides the actions until you move the caret out of the link. When a note opens with the caret at a link, that link's actions appear once the caret leaves it, or when you point at or click the link.

Right-click a link, or press `Shift+F10` with the caret in it, to choose **Edit link…**, **Open link**, **Copy link**, or **Remove link**. When an addon adds editor actions, such as **Dictionary and thesaurus**, they appear in the same menu as a separate group below the link's actions, in table cells too. In the menu, use the arrow keys, Home, and End to move between actions and Enter to choose one; Escape or Tab closes the menu. When **Tab key indents** is off, Tab from a link also reaches the actions; Escape, or Tab past the last action, returns to the link.

While a note is read-only, **Edit** and **Remove** are unavailable, including in actions and menus that were already open; **Open** and **Copy** still work.

## Links and history

Shift-click a link to follow it. In Normal view, you can also hold Ctrl+Cmd while clicking a link. Web and email links open in your default app; local note links open in Hibi. A link to a heading, such as `guide.md#setup`, opens the note at that heading, including a link to the note you are reading. Regular clicks keep editing.

Obsidian `[[Note]]`, `[[Note|Alias]]`, and links to headings or blocks open their note in the current workspace. A heading-only link in the current note jumps to that heading. Links to headings or blocks in another note open the note; Hibi does not yet scroll to the target within that note. Ambiguous note names do not open a guessed target.

In Markdown source, this also follows reference links such as `[guide][help]` with a `[help]: guide.md` definition elsewhere in the note. Labels ignore case, and the first matching definition wins. Definitions inside enabled frontmatter are excluded. This supports the built-in Markdown, GitHub Markdown and Text extras readers; addons with custom parsers or projections keep their existing link behavior.

Use `Cmd/Ctrl+[` to go back and `Cmd/Ctrl+]` to go forward through opened notes. In Settings, these shortcuts navigate settings pages instead. History lasts until you close the app.

## Open remote Markdown

Choose **File → Open from URL…** and enter a direct HTTP or HTTPS link to a text file. Hibi opens it as an editable draft. Save it locally to keep it; Hibi never writes back to the server.

The download must be UTF-8 text under 2 MiB, rather than a webpage. Relative attachment paths need matching local assets after you save.

## Continue after a formatted block

To type after a final table, code block, or list, press `Cmd/Ctrl+Enter` or click the blank editor area below it. Hibi adds a normal paragraph.
