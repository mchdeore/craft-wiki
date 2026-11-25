# Accounts and access

Who can sign in, what each person may do, and where that is decided.

## Accounts

A person has an account or they cannot get in. There is no sign-up, no invitation and no password reset by email. Someone at the keyboard of the computer that runs CRAFT makes each account and gives it a password ([running CRAFT](running.md#people)). To change a password, ask them.

- A password is at least eight characters. CRAFT keeps what it needs to check your password, and never the password itself.
- A wrong name and a wrong password get the same answer.
- After too many wrong tries, the answer is to wait a minute.
- A person taken off the list of people is out at once, even in the middle of a session.

## Sessions

Signing in starts a session in that browser. It ends:

- after 8 hours without CRAFT open in front of you (an object open on screen keeps it going),
- after 12 hours in all, whatever you do,
- when you sign out,
- when your password is changed.

When a session ends, the screens ask you to sign in over what you had open. Text you typed and have not committed stays in that browser.

The browser keeps the session in a cookie that scripts and other sites cannot use, and that travels only over https, except on that computer itself. CRAFT keeps only a fingerprint of it, so nothing kept on the server signs anyone in. The rules are in `app/signin.py`.

## Roles

A role is a name that some people hold, such as `team`. It is given with the account. Two roles are built in:

| Role | Who holds it | What it means |
|---|---|---|
| `everyone` | every person who can sign in | share with it to share with all of them |
| `admin` | the people it was given to | every right on every object, as if they owned it |

A person and a role never share a name, and no new object takes the name of either.

## Levels

Each object has one owner, and a list of who else may do what. Whoever makes an object owns it, and at first nobody else but an admin can see it. Anyone signed in can make one.

Each level holds the one before it:

| Level | What it lets you do |
|---|---|
| read | open it, find it, read its history, download it, watch it, take a copy |
| organise | also name it, describe it, tag it, assign it, link it |
| edit | also change what it holds and its own date, restore an older version, put it in the bin and take it out, merge a copy into it |
| owner | also share it, hand it to another owner, squash its history, remove it for good |

Of what organise allows, the screens offer renaming and tags.

Share an object from **Details**, under **Shared with**: a person, a role or `everyone`, and a level. Only the owner or an admin sees that form. **Details** also says what you can do with the object in front of you. More is on [spaces and objects](spaces.md#sharing).

A space can be named in a sharing list too. It stands for everyone who can read that space. Putting an object in a space gives nobody access to it.

Where search by meaning is set up, the title, the description and the first part of the text of every object, private ones too, are sent to a hosted model ([search](search.md#by-meaning)). What Sprout reads for you is sent to one too ([Sprout](sprout.md#what-leaves-craft)).

Access is always as it is now. Restoring an older version never brings an older sharing list back.

## What you cannot see

Opening something you may not read gives the same answer as opening something that does not exist. It is left out of searches and of SQL. A space you share still says that it holds something: its readers are given the object's id and any note put beside it there, and the screens' list leaves it out. Anyone who can read an object can also read its history, with the older versions and who it was shared with.

## Things that run for you

A job, such as an equation, a notebook run, reading a file or an answer from Sprout, belongs to the person who asked for it and has their rights and no more. The rights are looked at again just before anything is saved, so a right lost meanwhile stops it.

Code someone else wrote never brings its author's rights. Before it runs for you, CRAFT says which version runs, who else wrote in it, what it is handed, where it writes, and who can read that place without being able to read all it was made from. It runs only after you say yes. You are asked again when others change the code, when what it is handed or where it writes changes, or when someone new can read that place ([machines](machines.md)).

[Sprout](sprout.md) reads only what its asker may read. It writes its answer in the conversation and changes nothing else: it proposes a change, and a person with the right to make it saves it.

## Where rights are checked

In one place: the library itself, in `app/store.py`, on every call that reads or changes an object. The screens, Sprout and the jobs people ask for use the same calls as everyone. Screens hide what would be refused, but the library decides, and a refusal is shown in its own words.

| Question | Where it is answered |
|---|---|
| Who is asking | `app/signin.py` and `app/server.py` |
| May they read it | `_can` and `_row` in `app/store.py` |
| May they write each part of a change | `_allow` in the same file: it looks at every path the change touches |
| May they ask for this job | `_may_run` in the same file |
| What a job will do, and whether its asker agreed | `app/runs.py` |

The rules are under `access` in `app/design/envelope.json` (what a role may do by kind is designed and not built), and most of their cases are in `app/tests/test_access.py`.
