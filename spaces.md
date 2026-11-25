# Spaces and objects

Everything in CRAFT is an object, and a space is an object that holds others. This page is about what is the same for all of them.

## What an object is

An object has four parts:

| Part | What it is |
|---|---|
| Content | The thing itself: the paragraphs, the rows, the cells, the slides |
| What people say about it | Its title, owner, who it is shared with, tags, a date and links to other objects |
| What it says about itself | A few facts read from the content, such as the number of rows or pages. Nobody writes them. |
| Its meaning | Numbers made from its words, for [search by meaning](search.md#by-meaning) |

An object also has a version number, a history and an id. The id is used once and never changes. Two objects may have the same title.

## Kinds

Each object has a kind, which is fixed when it is made.

| Kind | What it holds | Made by |
|---|---|---|
| space | Other objects, spaces too | **New space** |
| text | Paragraphs | **New text**, or a file |
| table | Columns and rows | **New table**, or a file |
| notebook | Cells of code and of words | **New notebook**, or a file |
| docx, xlsx, pptx | A Word, Excel or PowerPoint file | A file |
| pdf | A PDF and its text | A file |
| conversation | Messages, and [Sprout's](sprout.md) answers | **New conversation** |
| skill | What something is for, and its steps in order. Nothing runs them. | **New skill** |

The library also has kinds for settings, emails, schedules and machines. They have no screen: opening one shows its details and history. No email is sent or received, and nothing starts a schedule.

## Spaces

- **Files** shows the spaces you can read, as a tree. Open a space to see what it holds, as a list or as a map.
- An object can be in several spaces, or in none. One in none, such as a conversation started from Sprout's panel, is reached with Search or from your favourites. A space can be inside another.
- Putting an object in a space gives nobody access to it. Each object is shared by itself.
- A space you can read may hold things you cannot. The list leaves them out, and the map shows that something is there, with the note at its place if one was written.
- A space holds at most 500 objects.
- In the list you can sort, filter by name, kind or tag, pick several rows, and drag them to another space. Right-click for **Rename**, **Move to**, **Copy to**, **Duplicate**, **Download**, **My copy** and the bin.
- Drop files on a space, or use **Upload**, to add them. See [documents](documents.md).
- **Add to favourites** lists an object above the tree, for you alone.

The map draws the relations between a space's objects where there are some. The screens show relations and do not draw new ones.

## Sharing

What you make is yours alone until you share it. **Details** shows who an object is shared with and, if you own it, lets you change that.

| Level | What it allows |
|---|---|
| read | Open it, find it, see its history, download it, copy it, be told of its changes |
| organise | Also its title, description, tags and links. The screens offer renaming and tags. They do not show descriptions or links. |
| edit | Also its content and its date, restoring a version, the bin |
| owner | Also who it is shared with, and removing it for good |

- Share with a person, with a role, or with **everyone**, which is everyone who can sign in. The library also takes a space's id there, for all who can read that space; the screens do not show the id.
- A name that does not exist is refused.
- An admin has every level on every object.
- The library checks your rights on every call. To someone who may not read an object, the answer is the same as if it did not exist.

Accounts and roles are on [accounts and access](access.md).

## Drafts and commits

When you edit, you write a draft. It is kept in your own browser, for you and that object. Nobody else sees it, and it does not follow you to another browser.

- **Commit** asks what changed, and makes all of the draft one new version.
- **Discard** drops the draft.
- A draft is made of pieces: a paragraph, a cell, a row, a step. A commit writes only what you changed in them.
- Others' commits show on your screen within a few seconds, around your draft.
- If someone else changed a piece that is in your draft, the screen says who and shows theirs. Choose **Take theirs**, or commit: the later commit wins on that piece. The other one stays in the history.

Some changes are not drafted and are made at once, each as a version: titles, tags, sharing, dates, moves, the bin, an uploaded file (when it is sent, and again when it is read), a restore, a merge, changes in SQL, columns added, retyped or taken away, equations added, removed or run, a message in a conversation, a proposal of Sprout's that you approve, and **Notify me**. A renamed column is in the draft.

Asking to be told is itself a version of the object, so others who asked are told of it.

## Versions and restore

- Every change is a version, with who made it, when, and a note that says why. **Details** lists them.
- **Restore** puts back the content of an earlier version. It is itself a new version, so nothing is lost. Title, tags and sharing stay as they are now.
- What is fixed once written, such as a message in a conversation, is not taken back by a restore. The screen then offers to make a copy of the old version.
- **Its date** in Details is a date and time that people set, to say when the thing is true. The time of each change is the library's own and nobody can change it.
- Two things take history away. Removing an object for good takes all of it. An owner or admin can also join an object's early changes into one; the screens do not offer it.

## Copies

**My copy**, **Copy to** and **Duplicate** all make a copy: an object of its own, owned by you, shared with nobody, that remembers what it was made from. You need only be able to read the original. My copy is put in your favourites.

A copy of a copy belongs to the copy it was made from. What you do in a copy changes nothing in the original.

A copy of a space holds the same objects, not copies of them. A space has no My copy and no merge in the screens.

## Merges

A merge brings what one side changed since the two last agreed into the other, as one version with a note. The buttons are in the copy's Details:

| Button | What it does | You need |
|---|---|---|
| **Merge into the original** | Brings the copy's changes to the original | edit on the original, read on the copy |
| **Take others' changes** | Brings the original's changes to the copy | edit on the copy, read on the original |

- Pieces that only one side changed all land. Where both changed one piece, the side that is brought wins. The other stays in the history.
- A notebook's outputs are not merged: each side keeps its own.
- To let someone else merge your copy, share it with them first.

## The bin

- **Move to the bin** needs edit. The object leaves lists and searches, and nothing else about it changes. Its owner is told.
- Putting a space in the bin, or removing it, leaves what it holds. Find those objects with Search or in another space.
- **Bin** in Files lists what you put there. **Take out of the bin** brings it back.
- **Remove for good** is for the owner or an admin, and only for what is in the bin. The object and its whole history go, and nothing in CRAFT brings them back. Copies made from it stay. Those it was shared with are told.

In the code: the rules every object follows (`app/design/envelope.json`), the library (`app/store.py`), copies and merges (`app/copies.py`).
