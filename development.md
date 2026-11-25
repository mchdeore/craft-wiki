# Development

How the code is laid out, how to check it, and how to make the usual changes.

## Set up

CRAFT is not open source: the code may not be used or changed without its owner's written permission. You need a clone of the repository, which only people given access can see.

`scripts/dev setup` fetches Python, what the checks need and the screens' packages, and says what is missing and where it is had. It installs nothing system-wide. [Running CRAFT](running.md#on-your-own-computer) says how to start it.

The map of the repository is in its `README.md`, and the map of the screens in `web/README.md`. Start with [architecture](architecture.md), then `app/store.py` and `app/design`.

## Checks

| Command | What it checks |
|---|---|
| `scripts/check` | every case, with the library in memory |
| `scripts/dev test db` | the same cases and those of the database, in a throwaway PostgreSQL, which stays up until `scripts/dev reset` |
| `scripts/dev screens` | builds the screens, then draws each panel and view once |
| `python3 app/design/check.py` | the design files: every example replays, and what they declare holds |
| `machine/prove` | the launcher on this host, against hostile code; it needs `/dev/kvm` and the built image |
| `scripts/speed` | how fast search and history are in a large library, in the test database whose address is in `CRAFT_TEST_DATABASE`; `scripts/dev database` starts one and says it |

A word after the first two runs only the cases whose name holds it, or those of the file it names: `scripts/check equation`, `scripts/check app/tests/test_sql.py`. `scripts/check --list` lists them. They fail when a case fails, when none ran, and when a file of cases could not be read, and name at the end the cases that failed.

Cases are in `app/tests`, by topic. A case is a function under the sentence it asserts. It makes every object it needs, so it runs alone and in any order. It goes above the last line of its file, which runs the cases that are there by then. No case asks a hosted model. The program inside a machine is run in the test's own process; one case does the same in real machines, says so, and is skipped where the host has none.

## Make a change

1. Make a branch from main, for one change.
2. Change the code. Keep it short, in plain words, like the code around it. Remove before adding.
3. Add a case when there is behaviour to hold. Give it a name that says what is true.
4. Run `scripts/check` and `scripts/dev test db`, `scripts/dev screens` when the screens changed, and `python3 app/design/check.py` when a design file did.
5. Open a pull request. The maintainer merges it. After a change to `machine/`, put the fingerprint the machine workflow says in `machine/print`, or the host goes on with the machine it has.

Some rules hold everywhere:

- A new feature is a new kind, a field of a kind, or a use of the calls. It is not a new table, service or side store.
- The library decides. A screen asks, and shows a refusal in its own words.
- Python a person wrote runs in a [machine](machines.md).
- Never log a key, password, token, prompt, answer or document text.
- The design files and what the store allows change only when the maintainer agrees first.

## Add a kind

Ask the maintainer first: a kind is a design file.

1. Write `app/design/content.<name>.json`. Start from the shortest, `app/design/content.skill.json`. `declares` says what the content may hold, what is read into `about`, which maps are in an order, what text it exposes, which file endings it takes (`accepts`), its named edits (`recipes`) and what is checked. `app/design/envelope.json` says what each of those means.
2. Give it an example: a row, the changes that make it and their jobs. The store and the checker find the file by its name.
3. Run `python3 app/design/check.py` and the cases. Each kind's example is replayed through the store.
4. If it is a file people upload, its reader runs in a machine, as `app/pdf.py` does, and its writer goes in `app/files.py`. Add its endings (`ENDS`) and what its ids start with (`PREFIX`) to `web/src/library/files.js`, and the endings to `accepts`.
5. Give it a view. If people make one from the screens, add it to `create` in `web/src/library/objects.js` and to the list of New buttons in `web/src/views/Space.jsx`. If a notebook may be handed one, add it to `KINDS` in `web/src/views/Notebook.jsx`.

## Add a view or a panel

A view shows one kind. Write `web/src/views/<Name>.jsx`, name it under its kind in `web/src/views/index.js`, and add an object of that kind, with words the view shows, to `web/check.mjs`. The check fails without it. Start from `web/src/views/Skill.jsx`.

A panel is a file in `web/src/panels/`, named in `PANELS` in `web/src/App.jsx` and in `web/check.mjs`. How a screen reads and changes objects is in `web/README.md`.

## Add a job

Ask the maintainer first: a job's name is in the design files.

1. Write its step, a function in a file of its own, such as `app/retype.py`, which needs no machine, or `app/pdf.py`, which does. It starts the job, makes its asker's own calls, and ends the job.
2. Put its name in the `purpose` of `app/design/run.job.json`. The names the store takes are read out of the brackets in that sentence. Name it too where `app/design/tables.json` and `app/design/envelope.json` list them. Anyone who may read the object may ask for it, unless a check of its kind names the job or `_may_run` in `app/store.py` says it takes edit.
3. Name it in `OPS` in `app/worker.py`, in `WAITS` if it waits for a machine or a model, and in `REPEAT` if it is safe to repeat.
4. Add it to a worker's command: in `deploy/craft.yaml`, or in `deploy/machines.yaml` if it needs a machine, and in `scripts/dev`. A job no worker is told to take stays queued.
5. If it runs a person's code, say what it is handed in `handed` in `app/runs.py`, what the machine does in `KINDS` in `machine/runner.py`, and what a failure is called in `CALL` in `app/machine.py`. The machine is then built again.
6. Give it a label in `JOB` in `web/src/panels/Jobs.jsx`. How a button asks for a job and follows it is in `web/README.md`.
7. Add a case, and a row to the [table of jobs](architecture.md#workers-and-the-queue).

## Workflows

| Workflow | When | What it does |
|---|---|---|
| `image` | a merge to main | runs the checks in memory and on PostgreSQL, builds `craft` and keeps it under its commit |
| `machine` | a merge that changes the machine or the workflow, each month, and by hand | builds `craft-machine` and keeps it under its fingerprint |

No workflow deploys, and this site needs none ([how it is published](EDITING.md#how-it-is-published)). The host that runs CRAFT pulls what was built ([deploying](running.md#deploying)).
