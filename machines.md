# Machines

Python that a person writes runs, and every uploaded file is read, in a small virtual machine of its own. The machine does one job and is destroyed.

## Why

CRAFT runs Python people write, and reads files that come from anywhere. A file can be made to hurt what reads it. Done on the server, either would share an operating system with the database and with everyone else's work. A machine has an operating system of its own, kept apart by the processor, so the job sees that and nothing else. Clean-up is simple too: everything goes with the machine.

No machine, no run of Python, and no file read. Where there is none, or no room for one, the job waits in the queue. A file also waits while the machine on the host is of another image than the one this CRAFT goes with.

## What runs in one

| Job | Size |
|---|---|
| an equation that fills a table's column | small |
| a notebook run | large |
| reading an uploaded file | small for the first version of a small file that is no PDF, else large |
| recording which image jobs run in | small |

Opening, editing, searching, sharing and merging need no machine. Neither do SQL, which runs in [a limited process of its own](architecture.md#the-server), Sprout, the text index or the vectors.

## How a job runs

1. You ask. The library writes down exactly what will run: the script, its version, what it is handed and where it writes, or the file that is read. A later change to the script or the equation does not change this job.
2. If someone else wrote in the script, or someone who may not read all it is made from can read the result, you are shown that and say yes first, unless you said yes to the same before.
3. A worker claims a machine, and only then is the job running.
4. The worker reads the inputs as you, with your rights, and passes them in.
5. The code runs, or the file is read. What a notebook prints is taken out as it runs, so a run that is cut short by its limits keeps what it gave. You see how far it is, and the outputs land together at the end.
6. The machine is killed, whatever the code is doing.
7. The worker checks the answer, looks again at what you may read and write, and saves the result as your change. Nothing is saved if you have lost a right, if an equation's inputs changed while it ran, or if a later file was uploaded. What a file gives is held to a size before it is taken apart, and to what a file of its kind may hold.

A job can be cancelled. Its machine ends at once.

Machines start fast because each is a clone. When the launcher starts it boots one machine of each size, loads the libraries most of its jobs use, and freezes it before it has met any job. Each job gets a fresh clone, with random numbers of its own. Nothing carries over from one job to the next.

## Limits

| | Small | Large |
|---|---|---|
| Memory | 512 MB | 2 GB |
| Processor cores | 1 | 2 |
| Time | 60 seconds | 10 minutes |
| Handed in, at most | 32 MB | 72 MB |

All machines together have a limit too. A job that finds no room waits. What a machine may say is capped, and a notebook's output past its limit is cut and says so. Of the files that go to a large machine, a worker reads one at a time. The exact values are in `machine/launcher.py`, `app/machine.py` and `machine/runner.py`.

Each notebook run starts fresh. The cells run from the top to the one you chose, and variables do not stay alive between runs.

## What it cannot reach

- The network. The machine has no network device.
- The database, or any login, key or password. None is ever put in.
- The library. The code cannot make a call. It gets the inputs named when the job was asked for, and gives back its results.
- The host's files, and other machines.
- Its own disk. It is read-only. Temporary files are in memory and go with the machine.

The machine also cannot call out: the host calls it. Code inside runs as a user with no rights, and cannot refuse to stop.

## What it holds

Python 3.12 and a fixed list of libraries for tables, numbers, statistics, charts and Office files. The list is in `machine/pyproject.toml`. Beside them are CRAFT's own readers of files (`app/readers.py`). Nobody installs their own: a new library is a line in that list and a new image. A change to a reader is a new image too.

The image is the disk, the kernel and the hypervisor, named together by one fingerprint. It is built from fixed versions (`machine/build`, `machine/Dockerfile`). The launcher checks every file against its fingerprint and does not start with one that differs.

## The parts

| Part | Where | What it does |
|---|---|---|
| `machine/runner.py` | inside the machine | reads one job, works it out, answers once |
| `app/readers.py` | inside the machine | reads an uploaded file |
| `machine/launcher.py` | on the host, as a service | makes a machine, holds it to its limits, kills it |
| `app/machine.py` | in the worker | claims a machine, passes the job in, checks what comes back |
| `app/importing.py` | in the worker | saves what a file gave as one change |
| `machine/prove` | run by hand | tries a list of ways out with hostile code, each beside the same try where it should work |

The launcher takes calls from the worker's user alone, over a socket on the host. A machine goes with the worker's connection, so a worker that is lost leaves none behind. The machine's log says times, memory and why it ended. It holds no data.

Putting the machines on a host is on [running CRAFT](running.md#machines).
