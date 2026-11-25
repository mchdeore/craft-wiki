# Search

**Search** looks through everything you can read. It never shows an object you may not read, and nothing that is in the bin.

## By words

- Type one or more words. An object is found when it holds all of them, in any order, as whole words. Capitals do not matter.
- Of a long question, the ten longest words count.
- At most 50 objects are found by their words. By words alone they are in no order that means anything.
- A newly saved word is found at once.

| Found | Not found |
|---|---|
| Titles and descriptions | Tags, and the names of columns |
| Paragraphs, slides, sheets and the read text of PDFs | Pictures, and scanned pages |
| Rows of tables | What a notebook's code printed |
| Cells of notebooks | |
| Messages, and the steps of skills | A word of a Word or PowerPoint file that is formatted in parts, such as half of it bold |

To find by tag or kind, open a space and use its filter box.

## By meaning

Where CRAFT is set up with an embedding model, search also goes by meaning: it finds what is close to your question though it uses other words.

- What holds your words and is close in meaning comes first. What holds only some of the words is shown too, lower down, and up to 50 more may be found for their meaning alone.
- An object's meaning is made from its title, its description and the first part of its text. This is done a little after each change, so search by meaning runs slightly behind.
- If the model cannot be reached, or the month's [limit](sprout.md#the-monthly-limit) is used up, search goes by words alone.

To make meanings, the title, the description and the first part of the text of every object, private ones too, are sent to the embedding model, and so is what you search for. The model gives back numbers, and those are kept with the object. Search results still show each person only what they may read.

## More ways to look

- In a table, **Show rows that hold** narrows [the rows](tables.md#rows) you see.
- [SQL](tables.md#sql) asks questions of tables.
- [Sprout](sprout.md) can search for you, also by an exact phrase, by kind, tag or space, and by facts such as the number of rows.

In the code: the library's find call (`app/store.py`), how meanings are made (`app/index.py`).
