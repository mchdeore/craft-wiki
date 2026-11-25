# Tables

A table is columns and rows in one object. Each column and each row has an id of its own that never changes, so renaming a column or adding a row moves nothing else.

## Making one

- Upload a .csv or .tsv file. It is read by a [job](notices.md#jobs), and so are the lines typed for a new table. Its first line names the columns, and each later line is a row. A file with a line of more fields than there are columns is not read. Empty lines are left out.
- Or use **New table** and type the columns' names with commas between them. Rows may follow.

The limits on size are on [documents](documents.md#limits).

## Columns and types

A column's type is read from its values: whole numbers become int, other numbers decimal, dates written year-month-day become date, and everything else, true and false too, text. Whole numbers with a leading zero, a plus sign or over 15 digits make the column decimal, so no digit is lost.

| Type | Holds |
|---|---|
| text | Any text |
| int | A whole number |
| number | Any number |
| decimal | A number kept digit for digit, so nothing is rounded |
| date | A date, as 2026-01-31 |
| bool | true or false |
| user | The name of a person who can sign in |

- A value that does not fit its column's type is refused.
- Press a column's name to sort by it, rename it, change its type or remove it.
- **Add column** and **Change its type** are [jobs](notices.md#jobs), one at a time on a table. When a type changes, a value that cannot be read as the new type without losing something is left out. The job says how many, and **Restore** brings them back.
- A column that an equation is handed or writes to cannot be removed. Remove the equation first.

## Rows

- Edit cells in the grid. They go into your [draft](spaces.md#drafts-and-commits) until you commit.
- **Add row** adds one at the end. Press row numbers to pick rows, then **Remove**.
- Sorting and **Show rows that hold** change what you see, and nothing in the table.

## SQL

Open **SQL** on a table. **Ask** runs one question and shows what it gives. It only reads, and needs only read on the tables it names. The SQL is SQLite's.

```sql
select part, sum(cost) as total
from "Costs"
group by part
order by total desc
```

- A table is named by its title or its id, in double quotes. A column is named by its title, or by its id.
- A title that several tables you can read share is refused: use the id. A copy with the same title as its original is named by its id.
- A question may join several tables. One you cannot read is not found.
- A decimal and a date are text in SQL. To compare a decimal as a number, write `cast(cost as real) > 5`.
- Each row's id is the column `id`.

**Change rows** runs one insert, update or delete on the table that is open. It may read other tables. It needs edit, lands as one version, and follows the same rules as any other change. It cannot change columns. It may use dates, with `now()` for the time of the change in UTC, and no random values.

| Limit | Most |
|---|---|
| Time | 5 seconds |
| Rows given or put in | 10,000 |
| Text given or put in | About 10 million characters |
| One value | 1 million characters |

A question past a limit is refused and says which. A few run at once, and the rest wait a moment.

## Equations

An equation fills a column by running a function over other columns. The function is Python, in a notebook or a text in the library.

```python
def total(price, quantity):
    return float(price) * quantity
```

The numbers of a decimal column arrive as text, and so do dates. An empty cell arrives as None. Turn a decimal into a number in the function, as here, or give the column the type number first. A decimal column takes text back and refuses numbers, so write results that are numbers into a column of type number.

An equation names:

| Part | Meaning |
|---|---|
| Name | What it is called in this table |
| Script | The notebook or text that holds the function, at one version. A later change to the script does not change the equation. |
| Function | The function's name. Left out, it is the equation's own name. |
| Inputs | The columns it is handed, in order or by the function's own names for them |
| Into | The column it writes to |

- Adding and running an equation need edit on the table, and read on the script.
- **Run** is a [job](notices.md#jobs). The script runs in a [machine](machines.md) used once, with no network, and gets its columns and nothing else. Where there is no machine, the job waits.
- The function is called once with whole columns, as NumPy arrays. If that fails, or gives the wrong number of results, it is called once for each row.
- The results land as one version. If one does not fit its column's type, the run fails and nothing lands. The job says how many rows and how long.
- A run ends as failed when it passes 60 seconds or fills 512 MB, when the function fails, or when an input column changes while it runs.
- An equation runs only when someone asks. Changing an input runs nothing.
- It runs with the rights of the person who asks. Before you run a script others wrote in, CRAFT shows you who wrote it, what it is handed and where it writes, and asks you to say yes. It asks again when they change the script, when what it is handed or where it writes changes, or when someone new can read the result but not the script.

In the code: what a table is (`app/design/content.table.json`), SQL (`app/sql.py`), equations (`app/equations.py`), column types (`app/retype.py`).
