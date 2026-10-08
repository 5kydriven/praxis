# Obsidian Engineering Knowledge Vault

This repository is my Software Engineering knowledge vault.

It stores knowledge from both my work and my personal projects. Each note
records which one it comes from in its `origin` property (see
"Work and personal origin").

## Purpose

The purpose of this vault is to capture durable engineering knowledge,
including:

- Technical concepts
- Tools and libraries
- Programming languages and frameworks
- Engineering patterns
- Development workflows
- AI skills, agents, and workflows
- Engineering decisions
- Lessons learned
- Problems and solutions

The goal is useful knowledge, not maximum note count.

## Core principle

Capture first, organize later.

## Vault structure

Knowledge lives under:

`Software Engineering/`

Read `Software Engineering/README.md` before modifying notes in that area.

Current categories:

- `Inbox/`
- `Engineering/`
- `Patterns/`
- `Decisions/`

Do not create additional top-level knowledge folders automatically. Only create
one under `Software Engineering/` when the user explicitly asks for it.

## Classification

### Inbox

Unprocessed, incomplete, or developing knowledge.

- Notes can be rough, incomplete, duplicated, or exploratory.
- A note may remain here indefinitely.

### Engineering

How something works.

- Contains durable technical knowledge and explanations.

### Patterns

Reusable solutions, techniques, or workflows.

- A pattern should be applicable to more than one specific situation.

### Decisions

Important choices and the reasoning behind them.

- A decision should explain the context, alternatives, and rationale.

### Choosing a category

Use these questions:

Engineering:
"How does this work?"

Patterns:
"This is a solution I can reuse."

Decisions:
"I chose X instead of Y because..."

If none of these clearly applies, keep the note in `Inbox/`.

Do not force a classification.

## Important distinction

Claude Code configuration and Obsidian knowledge are different things.

Claude Code configuration lives under:

`.claude/`

This includes:

- Agents
- Skills
- Commands
- Hooks

These files define how Claude operates.

`Software Engineering/` contains the knowledge itself.

Do not treat `.claude/` configuration files as knowledge notes unless they are
being explicitly documented as engineering knowledge.

## Note creation

New or immature knowledge should normally begin in:

`Software Engineering/Inbox/`

Before creating a note:

1. Search the existing knowledge base for related notes.
2. Determine whether the information already exists.
3. Prefer updating or linking to an existing note over creating a duplicate.
4. Do not create multiple notes merely because they have slightly different titles.

Avoid duplicate notes.

## Links

Use meaningful Obsidian wikilinks:

`[[Note Name]]`

Only create links when the relationship is useful.

Do not add links merely for the sake of linking notes.

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

## Work and personal origin

Notes are written as general engineering knowledge. Where the knowledge came
from is recorded as metadata, not used to scope or split the note.

Every knowledge note records where it came from in an `origin` list property:

    ---
    origin:
      - work
    ---

- `work` — learned at work.
- `personal` — learned on my own projects.
- both values — drawn from both.

`origin` records where the knowledge came from, not where it can be used.
Knowledge learned at work that is useful anywhere is still tagged `work` only.

When creating or organizing notes:

- Set `origin` on every new note. Always use the list form, even for one value.
- If the origin is not clear from the request, ask instead of guessing.
- A different origin alone is not a reason to create a separate note.
- My company has its own standards for how code is written. Only when the
  content itself differs between work and personal (a company standard versus
  my own approach), keep two separate notes named with a `(work)` /
  `(personal)` suffix, and link each to its counterpart.
- Do not merge a work note with a personal note, and do not copy company
  standards into a personal note.
- Separation is by property, not by folder. Do not create `Work/` or `Personal/`
  folders.

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

## Mutations

Do not silently perform substantial changes.

Do not silently move, delete, merge, or substantially rewrite notes.

Before:

- Deleting notes
- Merging notes
- Renaming many notes
- Moving many notes
- Creating new top-level folders
- Rewriting unrelated notes
- Graduating notes

show the proposed changes and wait for approval:

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

Rewriting and cleaning up a note is allowed when it is part of an approved
knowledge improvement or graduation.

## Deletion

Never delete knowledge notes as part of normal organization.

If two notes contain overlapping information:

- prefer merging only with explicit approval
- preserve useful information
- update links that would otherwise become broken

## Scope

These rules apply to:

`Software Engineering/`

Do not modify unrelated areas of the Obsidian vault unless explicitly instructed.

Do not modify files outside this vault unless explicitly instructed.

## Quality

Prefer:

- Clear concepts
- Durable knowledge
- Meaningful connections
- Explicit reasoning
- Reusable patterns
- Small, understandable notes

Avoid:

- Duplicate information
- Artificial categorization
- Excessive linking
- Premature organization
- Overly complex note structures
