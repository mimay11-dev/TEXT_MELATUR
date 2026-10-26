# MiMachine documentation

MiMachine is a small desktop text editor written in C++ and Slint. Its source is built
with CMake, and its documents are saved as files in a folder selected by the user.

## Guides

* [User guide](USER_GUIDE.md) — choosing a folder, creating and saving files, and using
  undo and redo.

## At a glance

* **UI:** Slint markup in `ui/app-window.slint`.
* **Application logic:** C++ sources in `src/`.
* **Build system:** CMake.
* **Default documents folder:** `documents` beneath the process's current working
  directory; it is created on startup if needed.
* **Persistence:** the active document is autosaved as a recoverable draft; **Save**
  writes it to the chosen filename.
* **Undo/redo:** temporary in-memory stacks, with edits grouped after a 700 ms pause.
* **Draft handling:** the file list exposes a **[Draft]** entry for restoring the latest
  draft. Confirming **New** replaces that draft.

Start at the [project README](../README.md) for a quick build.
