# Documents

You bring a file in by dropping it on a [space](spaces.md#spaces) or with **Upload**. You need edit on the space. It becomes an object, named after the file, which you edit in the browser and download again as a file.

The object is there at once, and the file is read into it by a [job](notices.md#jobs) of yours, in a [machine](machines.md). Until then it says that the file waits or is being read, and you can cancel that, share it, or download the file as it came. You can make a copy of it once it is read, and of a PDF at once. Where there is no machine, it waits.

## What can be uploaded

| Ending | Becomes | Edited in CRAFT |
|---|---|---|
| .md .txt .rst | text | Its paragraphs |
| .docx | Word document | The words of its paragraphs and tables |
| .xlsx | Excel workbook | Its cells, except formulas |
| .pptx | PowerPoint deck | The words on its slides |
| .pdf | PDF | No |
| .csv .tsv | [table](tables.md) | Rows, columns, SQL and equations |
| .ipynb .py .r .sql .js .sh | [notebook](notebooks.md) | Its cells |

The screens refuse any other ending, the older .doc, .xls and .ppt among them.

## Limits

- A file is at most 50 MB.
- A file can also hold too much for its size. About 100,000 rows by 12 columns of a table file, or 50,000 rows by 8 columns of a workbook, can be read. A table file has at most 4,000 columns. A larger one is not read: make it into several.
- A new version of a very large workbook can be read when about half of it at most is different. Upload one that differs more as a file of its own.
- A file that is damaged, locked with a password, or not what its ending says is not read. A text, table or code file must be plain text in UTF-8.

A file that is not read is kept as it came. The object says so, and to the one who sent it, in the browser it was sent from, why. Whoever may edit the object can have the file read again or upload another in its place. If the file was its first, it can also be downloaded as it came, and the object put in the bin. An object that had a version before keeps it, and **Keep it as it is** takes the new file off it again, whoever sent it, also while it still waits to be read.

When several files are uploaded, each is read by a job of its own, and **Jobs** lists them. One that is too large or of another kind is refused before it is sent: the screen says why, and the ones after it are not sent.

## Text

- A blank line starts a new paragraph. Each paragraph is a piece of its own, so two people can write in one text at once.
- Text is shown as it is typed. Markdown signs are not turned into formatting.
- A download has one blank line between paragraphs, whatever the file had.

## Word

- You see the words of the document's body, its tables among them, with its headings. Pictures, headers, footers and layout are not shown. They stay in the file.
- Enter splits a paragraph in two. The new one looks like the first.
- A commit changes only the words you changed. The rest keeps its style.
- Words that Word's own track changes marks as deleted are not shown. They stay in the file: a download has them, and so do search and Sprout. Accept the changes in Word first if nobody should read them.
- Other words that are not shown, such as headers, are still found by search too.
- An edit made here is not marked as a tracked change. CRAFT's own [history](spaces.md#versions-and-restore) is the record.

## Excel

- Each sheet is a grid. You can change cells and add rows and columns at the end.
- A formula is shown as it is written, or, where Excel filled it down from another cell, as its last result. It cannot be changed here.
- CRAFT works out no formula. After a change made here, the results in the file are the old ones. Excel may show them until it is told to work everything out again (Ctrl+Alt+F9).
- A date is shown as the number Excel keeps for it.
- What you type is kept as a number when it is digits, with or without a point. Anything else is kept as text: a formula, a percentage, a date or TRUE typed here too.
- A sheet laid out in an unusual way is not shown. It stays in the file, so download it to read it.

For columns with types, SQL and equations, use a [table](tables.md) instead: save the sheet as .csv and upload that.

## PowerPoint

You see each slide's words, in order, and can change them. Pictures, layout and speaker notes are not shown. They stay in the file, and the words of speaker notes are still found by search.

## PDF

- **Pages** shows the PDF as your browser does. **Text** shows its words, page by page.
- The pages are there at once. The words are found by [search](search.md) once they have been read. Where none were, **Read its words** tries again.
- A scanned page has no words: CRAFT does not read text from pictures.
- A PDF is not edited. Upload a new version in its place.

## Downloading

**Download** gives the file as it was last committed. Your draft is not in it. Until the first file of an object is read, it gives that file as it came.

- A Word, Excel or PowerPoint file comes back with everything it came with, and your changes in it.
- A PDF comes back exactly as it was uploaded.
- A table comes back as .csv or .tsv, a text with the ending it came with. For notebooks see [downloading a notebook](notebooks.md#files).
- Spaces, conversations and skills have no Download.

## Uploading a new version

**Upload a new version** takes a file of the same kind, edited elsewhere, and makes it the next version. What did not change stays the same piece, with its history. In a table, a column is matched by its name and a row by its values. A file that lacks a column an equation uses is not read in: remove the equation first.

The object then says what the file says. If someone committed a change after you downloaded the file and before you uploaded it, the upload replaces it. Their version is in the history, and **Restore** brings it back. What someone commits while the file waits to be read stays; where the file would change the same place it is not read in, and says so, and **Read it again** puts the file over their change.

When a second file is uploaded while the first waits, only the later one is read, and the job of the first says so. The same file uploaded again under the same ending, with no change committed since, adds nothing to the history.

In the code: files in and out (`app/files.py`), what reads a file (`app/readers.py`), how it lands (`app/importing.py`), writing Office files (`app/office.py`), what each kind declares (`app/design`).
