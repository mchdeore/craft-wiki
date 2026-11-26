# Running CRAFT

How to start CRAFT on your own computer or in a cluster, keep it healthy, back it up and undo.

## On your own computer

CRAFT is not open source: running it takes its owner's written permission. You need a clone of the repository, which only people given access can see, [uv](https://docs.astral.sh/uv/) and Node 22. Docker is for the throwaway database.

```
scripts/dev setup
scripts/dev person
scripts/dev run
```

The first fetches what is needed and says what is missing. The second makes a person and shows their password once, so it needs a terminal: over ssh, `ssh -t`. The third serves the screens on `http://localhost:5130` until Ctrl-C, or until its process is killed. `PORT` and `SCREENS` name other ports for the server and the screens.

| Command | What it gives |
|---|---|
| `scripts/dev run` | a throwaway library in a folder, with no worker |
| `scripts/dev run worker` | a throwaway library in a throwaway database, with a worker |
| `scripts/dev run <folder>` | the library kept in that folder |
| `scripts/dev reset` | takes the throwaway library and database away, once you say yes; the screens' packages and what was built stay |

`scripts/dev` alone lists all it does. Jobs that need a [machine](machines.md), reading an uploaded file among them, wait where there is none.

## People

Someone at the keyboard makes each account. The password is asked for twice and shown nowhere.

```
python3 app/server.py <folder> person <name> [role ...]
```

Give the first person the role `admin`. The same command, run again, sets a new password and ends that person's sessions. In a container it is `python server.py /data person <name> [role ...]`, run inside the server's container.

Roles named in the command replace the ones the person had. With none named they keep them. To take a person off, take their name out of `users.json` in the server's folder: they are out at their next call. What they own stays theirs until an admin hands it to someone.

## Settings

Every setting is an environment variable, listed once in `.env.example` with what it does. None is needed to start the server; a worker needs the database's address. Without a database the library is kept in the server's folder. Without the models' settings Sprout says it is not set up and search goes by words.

Keys and passwords belong in the cluster's secrets, never in git. The database's address has no password in it: that is in `PGPASSWORD` or `~/.pgpass`, as PostgreSQL's own programs take it.

## The images

| Image | Holds | Built by |
|---|---|---|
| `craft` | the server, the workers and the built screens; one image, a different command for each | `Dockerfile` |
| `craft-machine` | the machine that runs one job | `machine/build` |
| PostgreSQL with pgvector | the database, as its makers give it | not ours |

To try the server alone, with the library in a volume:

```
docker build -t craft .
docker run --name craft -p 127.0.0.1:5120:5120 -v craft-data:/data craft
```

Make the first person in another terminal with `docker exec -it craft python server.py /data person <name> admin`, then open `http://localhost:5120`. Over plain http, signing in works only there. There is no worker in it, so no job is done and no uploaded file is read.

## A cluster

`deploy/craft.yaml` is the whole of CRAFT for Kubernetes: the server, a worker, PostgreSQL, a nightly backup, who may reach whom, and the public address. The commands to apply it by hand are at its top. What is kept is in the database's volume and the server's folder, so moving is a copy. Before you apply it, put your own image name in, and your public name where it says `CRAFT_HOST`, and make the backup's folder on the host. It is written for k3s: on another cluster, leave out or replace its Traefik parts and give the backup a volume.

- Each part runs as an ordinary user, with no added rights and a read-only disk where it can.
- The app's database login owns its own database and is no superuser.
- Network rules let only the server, the workers and the backup reach the database.
- `deploy/traefik.yaml` gives https, for the ingress that k3s ships. `deploy/traefik-front.yaml` takes its place where another machine in front holds the certificate.

## Machines

Jobs that need a machine need a Linux host with an x86-64 processor, `/dev/kvm`, systemd, cgroups v2, glibc 2.39 or newer (Ubuntu 24.04 has it) and Python 3.11 or newer. The launcher lets all machines together hold 2.2 GB, which is one large machine or three small ones; its unit (`machine/launcher.service`) says how much, and a host with more room may say more (`deploy/README.md`). As root there, `machine/install` puts the image and the launcher in place, as the fingerprint in `machine/print` says. It takes the image from a folder, or has k3s fetch it from the registry it is kept in; with no k3s, build it and name the folder. It checks the image, starts the launcher, and puts the one before back when the new one does not come up. Run it again at any time: where nothing changed it does nothing.

`deploy/machines.yaml` is the worker that uses them. It reaches the launcher's socket and the database, and holds no model's key. It has no rights on the host beyond that.

## Deploying

A merge to main runs the checks, builds the image and keeps it under its commit. On a machine set up by `deploy/vm.yaml`, `deploy/pull` is run by a timer, as root: it reaches out, finds that main has moved and its image is there, puts in that commit's machines, and applies its files with that image. To deploy, nothing outside reaches in. It takes the site's public name from `deploy/site.env`, one line `CRAFT_HOST=<site>` that git ignores, or from `CRAFT_HOST` in its environment; without it nothing is deployed. Where k3s runs on Docker a person runs it: it builds the image there from the commit, and says when the machines are to be put in as root. Until the commit's machines are in, uploaded files wait, and people's code runs in the machine before, where there is one. A commit that does not come up, its workers among it, is put back, and is not tried again until main moves. Nor is a machine that was fetched later and did not come up. The steps done by hand once are in the launch checklist (`deploy/README.md`).

## Health

| Look at | What it says |
|---|---|
| `GET /health` on the server | 200 when the database answers, 503 when it does not |
| a worker's `alive` file | touched while the database answers and no job has hung; the cluster restarts a worker whose file is old |
| **Jobs** on screen | your own jobs only, and why one failed; nothing lists the jobs of others |
| the launcher's log | for each machine: size, time, memory and why it ended |

Logs hold the kind of an error and never what a call carried. The server makes a lost database connection again by itself, and workers wait for the database to come back.

## Backup

`scripts/backup` makes one folder named by its time. It holds the database as `pg_dump` writes it, who may sign in, and a manifest with each file's fingerprint and how many rows each table has. A folder without a manifest is not a backup. The newest 14 are kept. In the cluster it runs each night.

A backup holds everything in the library, and the password hashes. Keep it as private as the library, and keep a copy on another computer: `deploy/pull-backups` brings them over, checks those of the last three days, keeps the newest 30, and fails when none is newer than two days.

A library kept in a folder is backed up by copying the folder while the server is stopped.

## Restore

```
CRAFT_DATABASE=... scripts/restore <a backup> <the server's folder>
```

`scripts/restore` checks the fingerprints, restores into an empty database only, all of it or none, and counts the rows of every table against the manifest. A list of people that is already in the folder is left as it is. Stop the server and the workers first. In a cluster the steps are in the launch checklist (`deploy/README.md`).

## Undo

| What went wrong | What to do |
|---|---|
| someone changed an object | **Restore** an older version in its history (needs edit): its content comes back as one more version. Title, tags and sharing are put back by hand. |
| someone put an object in the bin | **Take out of the bin** |
| someone removed an object for good, or squashed its history | only a backup brings it back |
| a deploy | `deploy/pull <commit>` puts that commit back until main moves; then revert the change on main |
| the database was damaged or lost | restore a backup into an empty database |

Going back a deploy does not go back in data.
