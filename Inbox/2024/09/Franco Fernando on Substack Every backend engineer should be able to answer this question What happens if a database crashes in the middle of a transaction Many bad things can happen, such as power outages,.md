---
created: 2024-09-09T09:50:13 (UTC -03:00)
tags: []
source: https://substack.com/@francofernando/note/c-62323212?utm_source=feed-email-digest
author: Substack
---

# Franco Fernando on Substack: "Every backend engineer should be able to answer this question: What happens if a database crashes in the middle of a transaction? Many bad things can happen, such as power outages, hardware failures, etc. The main idea for not losing data is to store it in a non-volatile s…"

> ## Excerpt
> Every backend engineer should be able to answer this question: 

What happens if a database crashes in the middle of a transaction? 

Many bad things can happen, such as power outages, hardware failures, etc.

 The main idea for not losing data is to store it in a non-volatile storage like a disk. 

Whenever the user performs a transaction, the database does 2 things: 

- it writes the data on a separate log 

- it makes the update 

The log allows transaction reprocessing during reboot to restore a consistent state after failure. 

Writing to the log is fast because this is an append-only binary file. 

Data is only ever added to the end of the file, avoiding time-consuming seek operations. 

What if a database is distributed? 

This case is trickier since the database servers must coordinate using a Two-Phase Commit Protocol. 

There is a process where a server acts as a coordinator: 

- it communicates the commit to all the participants 

- it waits for all acknowledgments 

- it communicates the commit or rollback

---
Every backend engineer should be able to answer this question:

_What happens if a database crashes in the middle of a transaction?_

Many bad things can happen, such as power outages, hardware failures, etc.

The main idea for not losing data is to store it in a non-volatile storage like a disk.

Whenever the user performs a transaction, the database does 2 things:

\- it writes the data on a separate log

\- it makes the update

The log allows transaction reprocessing during reboot to restore a consistent state after failure.

Writing to the log is fast because this is an append-only binary file.

Data is only ever added to the end of the file, avoiding time-consuming seek operations.

_What if a database is distributed?_

This case is trickier since the database servers must coordinate using a Two-Phase Commit Protocol.

There is a process where a server acts as a coordinator:

\- it communicates the commit to all the participants

\- it waits for all acknowledgments

\- it communicates the commit or rollback
