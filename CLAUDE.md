# Obsidian Software Engineering Knowledge Base

This repository is an Obsidian knowledge base.

The primary knowledge area is:

Software Engineering/

Read `Software Engineering/README.md` before modifying notes in that area.

## Core principle

Capture first, organize later.

New knowledge starts in:

Software Engineering/Inbox/

Do not bypass the Inbox when creating new knowledge notes.

## Folder meanings

The Software Engineering folders have distinct purposes:

- `Inbox/`
  - Unprocessed or developing ideas.
  - Notes can be rough, incomplete, duplicated, or exploratory.
  - A note may remain here indefinitely.

- `Engineering/`
  - Explains how something works.
  - Contains durable technical knowledge and explanations.

- `Patterns/`
  - Documents reusable solutions, practices, or approaches.
  - A pattern should be applicable to more than one specific situation.

- `Decisions/`
  - Records a choice that was made and the reasoning behind it.
  - A decision should explain the context, alternatives, and rationale.

Do not create additional top-level folders under `Software Engineering/`
unless the user explicitly asks for them.

## Existing notes first

Before creating a note:

1. Search the existing knowledge base for related notes.
2. Determine whether the information already exists.
3. Prefer updating or linking to an existing note over creating a duplicate.
4. Do not create multiple notes merely because they have slightly different titles.

Use Obsidian wikilinks when a meaningful relationship exists:

[[Note Name]]

Do not add links merely to increase the number of links.

## Note writing

Notes should be:

- concise
- technically accurate
- understandable without requiring the conversation that created them
- structured according to the purpose of the note
- written in the user's own knowledge-base context rather than as generic documentation

Do not add unnecessary sections.

Do not rewrite a note merely for stylistic preference.

Preserve useful technical details when cleaning up a note.

## Graduation

A note graduates from `Inbox/` only when its purpose is sufficiently clear.

When graduating an Inbox note:

1. Read the complete note.
2. Search for related existing notes.
3. Determine whether it belongs in:
   - `Engineering/`
   - `Patterns/`
   - `Decisions/`
   - or should remain in `Inbox/`
4. Clean up and rewrite the note when necessary.
5. Preserve the actual knowledge and reasoning contained in the original.
6. Add meaningful wikilinks to related notes.
7. Avoid creating duplicate knowledge.
8. Do not create new folders.

A note does not need to be perfect before graduation.

## Classification

Use these questions:

Engineering:
"How does this work?"

Patterns:
"This is a solution I can reuse."

Decisions:
"I chose X instead of Y because..."

If none of these clearly applies, keep the note in `Inbox/`.

Do not force a classification.

## Mutations and approval

Before modifying the knowledge base as part of an organization operation:

1. Inspect the relevant notes.
2. Produce a proposed change list.
3. Show:
   - note being changed
   - proposed destination
   - whether content will be rewritten
   - links that will be added
   - existing notes that were considered
4. Wait for explicit user approval.
5. Only then perform the changes.

Do not silently move, delete, merge, or substantially rewrite notes.

## Deletion

Never delete knowledge notes as part of normal organization.

If two notes contain overlapping information:

- prefer merging only with explicit approval
- preserve useful information
- update links that would otherwise become broken

## Scope

These rules apply to:

Software Engineering/

Do not modify unrelated areas of the Obsidian vault unless explicitly instructed.