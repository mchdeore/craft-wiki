# Sprout

Sprout is CRAFT's assistant. You write to it in a conversation. It looks in the library, answers with its sources, and can propose a change for you to make.

Sprout needs a hosted model. Where none is set up, it says so and does not answer.

## Conversations

- Open Sprout from the strip on the left, and pick a conversation or start one. A conversation is an object like any other: it can be shared, found, put in a space and opened as a tab.
- A message cannot be changed once it is written. A correction is a new message.
- You need edit on a conversation to write in it or to ask for an answer. Each message you send asks for an answer. An answer is a [job](notices.md#jobs), so you can cancel it.
- Sprout answers the person who asks. It sees what others wrote as theirs, and does not take it as a question to answer.

## What it may read

- Sprout reads as you. Each thing it looks at is checked against your rights, and what you may not read it is refused.
- It can open an object, also as it was at an earlier version, search, read a history, and ask tables questions in [SQL](tables.md#sql). A large object it reads a part at a time. It has nothing else: no web, no files on your computer, no way to run code.
- Your rights, and what it read, are checked again just before its answer is kept.
- A shared conversation is read by all who can open it. Sprout answers with what its asker may read, so its readers may be shown what they could not open themselves. The screen says so.

## Citations

Sprout is asked to put a source after each claim: the object, the version it read, and the place in it. Press one to open that object. A source that names something Sprout did not read while working on that answer is taken out before you see it.

The object may have changed since. The list under **Cited** says when it is at a newer version. A source shows where Sprout looked. CRAFT does not check that the source says what Sprout claims, so open the ones that matter. What has no source, Sprout worked out itself: check it.

## Proposals

Sprout changes nothing in the library but its own messages. Asked for a change, it shows a proposal: the object, what is there now, what it would be, and why. There is at most one in an answer.

- **Approve** makes the change as you, with your rights, as a version in your name. If you may not change that object, you cannot approve.
- If someone changed the same piece after Sprout read it, the change is refused and nothing is overwritten. **Ask again** writes the question for you. Send it, and Sprout looks anew.
- **Decline** changes nothing.
- Anyone who can write in the conversation can decide. The decision is kept in it.

## Limits

| Limit | Value |
|---|---|
| Steps in one answer | 8 requests to the model |
| Time for one answer | About 2 minutes |
| Messages it starts from | The last 20 |
| What it sees of one thing it looks up | 20,000 characters; it can then open a part |

## The monthly limit

A month's spending on hosted models, for Sprout and for [search by meaning](search.md#by-meaning) together, has a limit for the whole of CRAFT. Whoever runs CRAFT sets it. Before each request, the most it could cost is set aside, so the limit is not passed, and requests may stop a little early.

When the limit is reached, Sprout says so and does not answer until the next month, in UTC. Search goes by words. Everything else works as before.

## What leaves CRAFT

To answer, the conversation's last messages with the names of those who wrote them, and what Sprout looks up for you with its owner, sharing and history, are sent to the hosted model. Every message you send asks for an answer, also one meant for another person. The request asks that nothing be kept there. CRAFT writes no question, answer or document text to its logs.

In the code: Sprout (`app/agent.py`), the hosted models and the limit (`app/model.py`), what a conversation is (`app/design/content.conversation.json`).
