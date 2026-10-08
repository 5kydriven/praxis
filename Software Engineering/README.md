# Software Engineering Notes

How a note moves through this folder, from a rough idea to a place where it can be found again.

## The folders

| Folder          | The question it answers                         |
| --------------- | ----------------------------------------------- |
| **Inbox**       | "I haven't figured out where this belongs yet." |
| **Engineering** | "How does this work?"                           |
| **Patterns**    | "This is a solution I can reuse."               |
| **Decisions**   | "I chose X instead of Y because..."             |

The distinction matters: the same topic can produce a note in any of the three destination folders, depending on which question the note actually answers.

## Work and personal

Notes are written as general engineering knowledge. Where the knowledge came from is recorded in an `origin` property on each note, not a folder.

| `origin`              | Where the knowledge came from |
| --------------------- | ----------------------------- |
| `work`                | Learned at work.              |
| `personal`            | Learned on my own projects.   |
| `work` and `personal` | Drawn from both.              |

`origin` is where the knowledge came from, not where it can be used. Something learned at work that is useful anywhere is still tagged `work` only.

Set it when the note is created, always as a list:

```yaml
---
origin:
  - work
---
```

A different origin alone does not split a note. My company has its own standards for how code is written, so only when the content itself differs between the two (a company standard versus my own approach) does the topic get two notes with a `(work)` / `(personal)` suffix that link to each other, for example `flutter architecture (work)` and `flutter architecture (personal)`. A work note and a personal note are never merged.

To see one origin only, search `[origin:work]` or `[origin:personal]`.

## Workflow

1. **Start in Inbox.** Every new note begins here, no matter what it is about. Do not decide where it belongs yet.
2. **Plan and construct the idea.** Use the Inbox note as a workspace: collect the pieces, work out what you are trying to say, and let the idea take shape.
3. **Choose where it belongs.** As the note becomes clear, ask which question it answers and move it to that folder.

```
Inbox  ──►  Engineering   "How does this work?"
       ──►  Patterns      "This is a solution I can reuse."
       ──►  Decisions     "I chose X instead of Y because..."
```

A note can stay in Inbox for as long as it takes. Moving it is gradual, not a deadline.

## Graduation rules

Not defined yet.

The rules for when a note graduates from Inbox to Engineering, Patterns, or Decisions will be written here after a few notes have accumulated, so they come from real notes instead of guesses.
