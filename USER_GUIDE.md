# User guide

## Choosing where documents are stored

When MiMachine starts, it uses a folder named `documents` under its current working
directory and creates the folder if it does not exist. To use another location, enter
its path in the Folder field and select **Load Folder**. The file list shows regular
files directly inside that folder; it does not browse subfolders.

If the current document has unsaved changes, MiMachine refuses to switch folders. Save
the document first, then load the other folder.

## Creating a document

1. Select **New**.
2. Enter a filename in the filename field. For example, `notes.txt`.
3. Type in the editor.
4. Select **Save** to write the document in the selected folder.

Use a filename, not a path. Folder separators and parent-directory paths are rejected.
Saving to a name that already exists replaces that file's contents.

If a current or recoverable draft exists, **New** asks you to confirm before replacing
it. Choose **Cancel** to keep the draft or **Create New** to replace it with a blank
draft.

## Opening a document

Select a file in the list. Its contents are loaded into the editor. If there are
unsaved changes in the current document, save them before opening another file.

The file list is refreshed when the folder is loaded and after a successful save. It
does not automatically track changes made to the folder by other applications; select
**Load Folder** to refresh it.

## Saving and unsaved changes

The window title marks an active document as **[Draft]** and shows an asterisk while
its contents differ from the saved document. Changes are backed up automatically to a
draft in the selected folder after a short pause in editing. Use **Save** to write the
document under its filename and remove the draft backup. If the app closes before you
save, use the **Draft** entry in the file list to restore it. Opening another file or
changing folders is blocked while a draft is active.

MiMachine also writes the current draft when you close the window. If that backup write
fails, the window stays open so you can address the problem before exiting.

## Undo and redo

Use **Undo** and **Redo** to move through recent editing states. Consecutive edits are
grouped together while typing continues; a pause of 700 ms starts a new undo step.
Saving or using undo/redo also ends the current group. The in-memory history retains up
to 100 undo states for the current document and is cleared when a different document is
opened or created. History is not restored after closing the application, but the last
autosaved draft can be restored.

## Status messages

The message below the editor reports whether the folder loaded, a file was opened or
saved, changes are unsaved, or an operation failed. If a file operation fails, confirm
that the folder exists or can be created and that the application has permission to
read or write there.
