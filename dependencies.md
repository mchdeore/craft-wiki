# Dependencies

What CRAFT is made with, and why each is there. Versions that are fixed are in the files named here. Licences are in `THIRD-PARTY.md`.

## The server and the workers

| What | Why | Named in |
|---|---|---|
| Python 3.12 | the language of the app; its own library is nearly all it needs | `.python-version` |
| psycopg | reaches PostgreSQL; the app's one library | `Dockerfile` |
| SQLite | runs SQL over tables, in memory; it comes with Python | `app/sql.py` |
| PostgreSQL 18 with pgvector | keeps the library; compares vectors | `deploy/craft.yaml` |
| Alpine Linux | the base of the image | `Dockerfile` |

Without a database the app needs Python alone.

## The screens

Versions are locked in `web/package-lock.json`.

| What | Why |
|---|---|
| SolidJS | draws the screens; the one package that reaches the browser |
| Vite, with Solid's plugin | builds the screens, and serves them while developing |

## The machine

Python is as uv fetches it. Its libraries are chosen in `machine/pyproject.toml` and locked, with fingerprints, in `machine/uv.lock`. People's code uses them; CRAFT itself uses only the ones marked.

| What | Why |
|---|---|
| numpy (also CRAFT's), scipy, statsmodels, scikit-learn | numbers, statistics and models |
| pandas (also CRAFT's), pyarrow, duckdb | tables and files of data; a notebook is handed a table as a pandas data frame |
| matplotlib (also CRAFT's), vl-convert-python | charts |
| sympy, pint | algebra and units |
| pypdf (also CRAFT's) | the text of a PDF |
| openpyxl, python-docx, python-pptx, docxtpl | Office files |
| Debian | the machine's disk, from a dated copy of its archive |
| Cloud Hypervisor and its Linux kernel | run the machine; fetched by version and checked by fingerprint in `machine/build` |

The launcher uses Python's own library and the host's systemd.

## A cluster

| What | Why |
|---|---|
| Kubernetes, such as k3s | runs the parts, with the files in `deploy`; elsewhere than k3s, leave out or replace the Traefik parts and give the backup a volume; fetching and deploying by itself is written for k3s |
| Traefik, which k3s ships | https, page headers and limits on how often one address may call |

## Work on CRAFT

| What | Why |
|---|---|
| uv | fetches Python and psycopg for the checks; locks the machine's libraries |
| Node 22 and npm | build the screens |
| Docker | the throwaway database; builds the images |
| PostgreSQL's own programs | backup and restore |
| GitHub Actions | the checks and the images; each is in `.github/workflows` |
| GitHub Pages | this site |

## Hosted models

Optional: one model answers for Sprout and one makes vectors. They are named in the settings (`.env.example`) and asked over HTTPS with no library of their makers.
