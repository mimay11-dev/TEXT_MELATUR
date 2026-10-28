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

# MiMachine: Project Overview and Technical Architecture

## 1. Overview & Project Purpose

- **Dual Focus:** The sources cover both an architectural specification for a C++ data structures project (Sprint 1 - Planning and Design) and the technical/user guides for the **MiMachine** desktop application built with C++ and Slint.
- **Core Purpose:** The system provides a simple text editor for inserting and deleting characters while ensuring operational reversibility through undo and redo features.
- **Scope:** The initial architectural scope focuses on in-memory text manipulation without advanced formatting such as custom fonts, colors, images, or collaborative editing.

---

## 2. Technical Architecture & Core Data Structures

- **Doubly Linked List (Text Storage):** Character data is stored in a **Doubly Linked List**, where each node contains a character alongside pointers to previous and next nodes. This structure allows bi-directional text traversal and character insertion or deletion at designated positions.
- **Stack Data Structures (LIFO):** Reversible edits are managed by two distinct stacks following the **Last In, First Out (LIFO)** principle:
  - **Undo Stack:** Stores operational data for recent edits, executing the most recent edit first when Undo is selected.
  - **Redo Stack:** Holds operations that have been undone, allowing them to be reapplied when Redo is selected.
- **Desktop UI & Application Split:** In the MiMachine implementation, the user interface is defined using **Slint markup** (`ui/app-window.slint`), backend logic resides in C++ source files (`src/`), and compilation is managed via **CMake**.

---

## 3. File Management, Autosaved Drafts & Safety Safeguards

- **Folder Workspace:** Documents are stored in a user-selected folder (defaulting to a `documents` directory created automatically beneath the working directory). The interface lists regular files directly inside that directory without browsing subfolders.
- **Filename Constraints:** When creating or saving files, only direct filenames are permitted; path separators and relative directory paths (such as `../`) are rejected.
- **Autosaved Draft Backups:** Edits are automatically backed up to a hidden draft file in the active folder after a brief pause in typing. Unsaved documents are marked with **[Draft]** and an asterisk (`*`) in the window title bar.
- **Data Loss Prevention:**
  - Changing folders or opening another document is **blocked** while an unsaved draft is active until the file is saved.
  - Selecting **New** when a draft exists triggers a confirmation prompt allowing you to either keep the draft or replace it.
  - On exit, MiMachine flushes the active draft to disk; if the backup write fails, the window remains open so you can address the issue before closing.

---

## 4. Undo/Redo Engine & Smart Edit Grouping

- **In-Memory History:** Undo and redo states are maintained in temporary in-memory stacks, holding up to **100 undo states** per document.
- **Smart Edit Grouping:** Consecutive character inputs are grouped into single undo steps rather than recording every individual keypress. A **700 ms pause** in typing, saving the document, or executing an undo/redo command closes the active edit group.
- **Lifecycle:** Opening another file, creating a new file, or quitting the application permanently clears the undo/redo history stacks.

---

## 5. System Flow & Operational Testing

- **Execution Loop:** The program operates through a user menu loop:

  1. Display Main Menu
  2. Select Operation (Insert/Delete, Undo, Redo, Display)
  3. Process Edit & Record Operations
  4. Update & Display Text
  5. Return to the Menu or Exit

- **Testing Scenarios:** Planned test cases evaluate empty stack boundaries, invalid positions, and verify that **making a new edit after an Undo clears the Redo history stack**.
