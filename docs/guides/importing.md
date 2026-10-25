# Import documents

Open a Hibi workspace and choose **File → Import into workspace…**. If the folder has no manifest, create one in **Settings → Workspace** first.

Choose **Folder** to copy an ordinary folder. Enable **Import from Obsidian**, **Import from Notion**, or **Import from Bear** in Settings → Addons for those exports. The import dialog explains which export to choose.

Hibi copies the import into a new folder and keeps your originals. Existing documents are never replaced. Review any notices shown after the import, especially unresolved note links.

Folder imports have no file-count or total-size limit. Hibi streams attachments from disk rather than loading them all into memory, and folder attachments have no import size limit. Hidden files, application settings, `node_modules`, and symbolic links are excluded.

For Obsidian, Notion, and Bear conversion, each note or CSV file must be UTF-8 text no larger than 2 MiB. Converted files must fit within 32 MiB. CSV tables have no separate row or column limit.

Archives must be a single unencrypted ZIP file, without ZIP64 or split volumes. ZIP imports retain safety limits of 10,000 entries, 256 MiB for both the archive and its expanded files, and 32 MiB per extracted file. Extract a larger export yourself and import its folder instead; conversion limits still apply.

Choose **Cancel** while an import is busy, or dismiss the dialog, to stop it. Hibi removes temporary files before cancellation completes. An import already published into the workspace remains there.

## Obsidian

To use an Obsidian vault in place, [open the vault as a workspace](workspaces.md#open-an-obsidian-vault). No import is needed.

Use **Import from Obsidian** only when you want a separate copy. Choose the vault folder. Notes and attachments keep their folder structure. Resolvable wiki links become Markdown links; embedded notes become links.

## Notion

Export as **Markdown & CSV**, including subpages and files. Import the ZIP or extracted folder. Databases become Markdown tables, with the original CSV files alongside them. Notion views, permissions, and formulas are not recreated.

## Bear

Export as **TextBundle** to keep images, or **Markdown** for text. Import the export folder or ZIP. Bear app links keep their original destination.
