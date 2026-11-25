# Editing the site

The site is the Markdown, the images and the videos in this repository, on `main`. It holds the wiki of CRAFT and its demo videos, and nothing else: the code is in a private repository. GitHub Pages publishes `main` within a few minutes of each change, with no workflow. The site is open to everyone on the internet, and so is the repository with all its history. Pages are updated by hand, when asked.

## How to update

1. Change the page in a branch. Keep one topic per page. Add a new page only for a new topic, and list it on the [home page](README.md).
2. Open the code or the design file behind each thing you state. Never write from memory or from an older page.
3. Write what CRAFT does now. Future work goes in the [roadmap](roadmap.md) under Deferred or Vision, not on topic pages.
4. If a decision explains current behaviour, add at most one sentence saying why, on the page it affects.
5. Open a pull request. After it merges, check the page on the site. If it did not change, see the repository's **Actions**, **pages build and deployment**.

A change to the code that makes a page wrong goes with a change here.

## How it is published

Set once, in the repository's **Settings**, **Pages**: **Source** says **Deploy from a branch**, with the branch `main` and the folder `/ (root)`. There must be no file named `.nojekyll`, which would turn the generator off, and no `index.html`, which would take the place of the home page, `README.md`. The settings of the site are in `_config.yml`.

## How to add a video

1. Make it an MP4 (H.264 and AAC), with its index at the front so that it plays before it is all there: `ffmpeg -i in.mp4 -c copy -movflags +faststart out.mp4`.
2. GitHub takes no file over 100 MB. Shrink a larger one: `ffmpeg -i in.mp4 -c:v libx264 -crf 28 -preset slow -vf scale=1920:-2 -c:a aac -movflags +faststart out.mp4`, with a higher `-crf` for a smaller file. The whole site may hold 1 GB, and every version of a video stays in the repository's history: replace one seldom.
3. Put it beside the pages with its poster, a JPEG of one picture of it. Name both in lower case with hyphens.
4. Add a section to [videos](videos.md): its title, what it is, and the video tag as the first section has it, with other names. Keep `preload="none"`, so that nobody fetches a video they do not play.
5. Look through the whole video, its poster and what is said in it for [what never goes in](#what-never-goes-in).

## Formatting

- Plain GitHub Markdown only: headings, lists, tables, code blocks, links and images. No HTML but the video tag on [videos](videos.md), and never two opening curly braces in a row or a curly brace followed by a percent sign: the site generator (Jekyll) reads those as template tags and the page fails to publish.
- A vertical bar only in tables: the site generator reads a line of text with one as a table.
- One `#` title per page, then `##` sections and `###` subsections. No numbers in headings.
- Link to another page with its file name, such as `[roadmap](roadmap.md)` or `[levels](access.md#levels)`. Anchors are the heading in lower case, punctuation removed, spaces turned into hyphens. Never link with this site's or this repository's full address: the links then hold if it is renamed.
- Name code by its path in the code repository, such as `app/store.py`, and never link to it: a reader cannot open it, so the page must make sense without it.
- Never copy what the code already states. Name the file instead: what a kind holds (`app/design/`), versions (`machine/uv.lock`, `web/package-lock.json`), settings (`.env.example`).
- Images go in `images/` as SVG with no script, with the Mermaid source beside them (`.mmd`).
- Short sentences, present tense. On pages for people who do not program, plain words.

## What never goes in

A private path, anything from `.local`, a host name or an address, where the site of CRAFT itself runs, an account, the name of a person or of an organisation (but for the makers of what CRAFT is built with), a key or a password, a document's text, or a weakness that is not yet fixed. The same goes for commit messages and for the repository's description.
