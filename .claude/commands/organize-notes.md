# Organize Software Engineering Notes

Organize the user's Software Engineering knowledge base according to
the rules in `CLAUDE.md` and `Software Engineering/README.md`.

## Scope

Only operate on:

Software Engineering/

Do not modify other areas of the vault.

## Workflow

### Step 1 — Inspect

Read:

- `CLAUDE.md`
- `Software Engineering/README.md`

Then inspect the contents of:

`Software Engineering/Inbox/`

Search the existing:

- `Engineering/`
- `Patterns/`
- `Decisions/`

for related knowledge before proposing changes.

### Step 2 — Analyze each Inbox note

For every Inbox note, determine:

- What is the note about?
- Is its purpose sufficiently clear?
- Does related knowledge already exist?
- Does it answer:
  - "How does this work?" → Engineering
  - "This is a solution I can reuse." → Patterns
  - "I chose X instead of Y because..." → Decisions
- Should it remain in Inbox?

Do not force a classification.

### Step 3 — Prepare a proposal

Before making any changes, show a proposal.

For each note include:

- Current path
- Proposed path
- Classification
- Whether content will be cleaned up
- Existing related notes
- Wikilinks to add
- Any possible duplicate or merge concern

Example:

    Programming Stack.md
    → Engineering/Programming Stack.md

    Reason:
    The note documents the technologies currently used and serves
    as durable technical reference.

    Cleanup:
    Yes

    Links:
    [[Flutter]], [[Dart]], [[Hono]]

### Step 4 — Wait for approval

Do not modify files until the user explicitly approves the proposal.

If the user asks for changes to the proposal, revise it.

### Step 5 — Apply approved changes

After approval:

- rewrite notes where appropriate
- move notes to their approved destination
- add meaningful wikilinks
- preserve useful information
- do not create new folders
- do not delete knowledge
- do not modify unrelated notes

### Step 6 — Report

After completing the operation, report:

- notes moved
- notes rewritten
- links added
- notes left in Inbox
- anything that requires manual review

Do not claim changes were made if they were not actually made.