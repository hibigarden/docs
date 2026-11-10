# History

**History** lists the text a note had at earlier points, and returns the note to one of them. Run **History** from the command palette; searching for "version" finds it too.

## Return to an earlier point

1. Run **History** from the command palette.
2. Under **Points**, choose an entry to preview what returning to it changes. Entries are the versions your saves kept and points while you edited: before an undo, where edits branched, a pause, before a large edit such as a paste, and every few minutes of steady typing.
3. Choose **Restore**.

Restoring is an edit, so undo takes it back. With autosave on, Hibi saves the restored text after the usual wait. With autosave off, the file on disk stays unchanged until you save. A saved version too large to preview or to undo replaces the text instead: History says so before you restore it, and Hibi asks about unsaved changes first.

Older entries thin out: past a day, the list keeps one an hour, and past a week, one a day. Within the last day, autosaves list about 3 minutes apart. Every save you ask for and the newest version stay listed.

## All steps

**All steps** lists every undo step of the note, oldest first. Steps that an undo replaced sit one level in, under the step they left from. Choose a step and **Go to step** to undo and redo your way there; undo and redo go on from that step.

## What is kept

Hibi keeps a local copy of the text after each save. This works without Git. History keeps recent saves on this computer, outside your workspace and exported documentation. It has storage limits, so it is not a substitute for a backup. When a note's history is full, autosaves make room before saves you asked for. A new note has no saved versions until you save it.

Saved versions belong to a file's path. **Save as** uses the destination's versions. If the file saves but its version cannot be stored, Hibi tells you.

A note also keeps its undo history after you close it, up to its last save, so a reopened note can still undo those changes. Hibi keeps these steps, including text you deleted, on this device. Turn this off, or clear the steps kept so far, under **Settings → Editor → Undo history**. Each note keeps a limited number of steps: past that, older steps join into larger ones, so undo still reaches far back, while recent typing stays in small steps.
