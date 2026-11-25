# Notebooks

A notebook is cells of code and cells of words, in order. Code in Python can be run. A code file becomes a notebook when it is uploaded.

## Cells

- **+ Code** and **+ Words** add a cell. Words are shown as they are typed.
- What you change is a [draft](spaces.md#drafts-and-commits) until you commit it. Commit before you run: a run takes the cells as they are committed.

## Running

**Run all** runs every code cell from the top. **Run to here** runs the code cells down to that one.

- Each run starts fresh, in a [machine](machines.md) of its own that is destroyed after it. No variable stays from one run to the next.
- A run goes as far as the first cell that fails. That cell shows the error.
- The outputs land together, as one version. A cell that changed while the run was under way gets no output.
- An output stays beside its cell until the next run of it, even when the cell has been edited since.
- A cell shows what it prints, the value of its last line, tables and pictures. Charts drawn with matplotlib are shown by themselves.
- One run of a notebook is under way at a time. A run is a [job](notices.md#jobs): it can be cancelled, and it waits when there is no machine.
- Only notebooks in Python are run. Others can be read and edited.

## Who a run belongs to

A run has the rights of the person who asks for it, never those of the person who wrote the code.

- Running needs edit. If you can only read the notebook, **Run my copy** makes your own copy and runs that. If you have a copy already, it opens.
- Before you run code that others wrote in, CRAFT shows you who wrote it, which version runs, what it is handed and where it writes, and asks you to say yes. It asks again when they change the code, or when the objects it is handed are others. A copy still counts as the work of those who wrote it.
- It also says when someone can read the notebook but not all that the run is handed, and asks again when someone new can.

## Inputs

The code reaches nothing in the library but the inputs the notebook names. Add one under **Inputs**: a name to use in the code, an object, and a version if it should not be the newest.

| An input that is | Arrives in the code as |
|---|---|
| a table | A pandas data frame, with a column for each of the table's. Decimals and dates are text: turn them into numbers or dates in the code. |
| a text | Its text |
| a notebook | The text of the file it would be downloaded as; that of an .ipynb is JSON |
| a Word, Excel, PowerPoint or PDF file | The bytes of the file |

Without a version, the input is as it was when the run was asked for. You must be able to read it when the run starts and when it ends.

Inputs arrive as copies. A run changes nothing in the library but the notebook's outputs.

## The machine

| Limit | Value |
|---|---|
| Time | 10 minutes |
| Memory | 2 GB |
| Processors | 2 |
| What it is handed | About 50 MB in all |
| What it keeps of what cells give | 5 MB; the rest is left out, and the output says so |

There is no network, so nothing can be downloaded or installed. Its Python has the standard library and these libraries (`machine/pyproject.toml`): numpy, pandas, scipy, matplotlib, scikit-learn, statsmodels, sympy, pint, duckdb, pyarrow, openpyxl, python-docx, docxtpl, python-pptx, pypdf and vl-convert-python. Files that the code writes are gone with the machine.

## Files

| Uploaded | Read as | Downloaded as |
|---|---|---|
| .ipynb | Its cells, with their outputs | .ipynb, with the outputs |
| .py .r .sql .js .sh | Cells, cut where a definition starts at the edge of a line, or at `# %%` | The same kind of file, with cells of words as comments |

A notebook made in CRAFT is downloaded as .py. Outputs are shown as text, tables and pictures only, never as a web page, and are not [searched](search.md).

In the code: what a notebook is (`app/design/content.notebook.json`), a run (`app/notebook.py`), what happens inside the machine (`machine/runner.py`).
