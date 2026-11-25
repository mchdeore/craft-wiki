# Notices and jobs

CRAFT tells you about changes others make to what you care about. Notices are shown in CRAFT only: no email is sent.

## Asking to be told

Open an object's **Details** and tick **Notify me when this changes**. You need only be able to read it. Untick it to stop.

## What you are told

| What happens | Who is told |
|---|---|
| An object changes: its content, title, tags, sharing or anything else | Those who asked to be told |
| A job on an object is asked for, such as a run or an equation, and when it ends | Those who asked to be told |
| A file you sent waits, and a later one is sent to the same object by someone else | You |
| An object is put in the bin | Its owner, and those who asked to be told |
| An object's early history is joined into one change | Those whose changes are in it, and those who asked to be told |
| An object is removed for good | Those it was shared with, and those who asked to be told |

- You are not told of what you did yourself, but for one case: removing something shared with everyone or with a role you hold.
- The owner is not told when an admin removes their object for good, unless they asked to be told or it was shared with them as well.
- A notice of a removal shows the object's id, since the title went with the object.
- If you lose the right to read an object, you are no longer told of its changes.
- An object that is removed for good takes the earlier notices about it with it.

## Reading notices

**Notices** lists them, the newest first, with the object, what happened, who did it and when. Press the title to open the object.

A notice is read when you press **Mark read** or **Mark all read**, and not because you opened the object. Whether you read it is kept in the library, so it is the same in any browser. Where a role or everyone was told, your mark counts for you alone.

## Jobs

Some work takes a while and is done in the background: a notebook's run, an equation, adding or retyping a column, reading an uploaded file, an answer from Sprout, and reading an object's meaning for search.

- **Jobs** lists the newest 20 that you asked for in this browser, with how far each is and why one failed. One that you asked for elsewhere appears when you open its object while it is still open.
- **Cancel** stops one that is waiting or running.
- Jobs of several people take turns: the person with the fewest running goes next.
- A job has the rights of the person who asked for it, checked again when it saves. If you lose the right meanwhile, nothing is saved.
- A job that needs a [machine](machines.md) waits until there is one.
- Only you can read a job you asked for. Others see that something is being done to an object they can read, and by whom.
- The end of your own job is not a notice. It shows in Jobs and beside the object.

In the code: what a job is (`app/design/run.job.json`), the worker (`app/worker.py`).
