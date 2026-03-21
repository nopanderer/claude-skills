---
name: obsidian-markdown
description: Create and edit Obsidian notes using the Obsidian CLI and Obsidian Flavored Markdown. Use when the user wants to create, edit, append, or manage notes in their Obsidian vault, or when working with wikilinks, callouts, frontmatter, tags, embeds, daily notes, or any Obsidian-related task. Also trigger when the user mentions their vault, note-taking, or wants to add content to Obsidian.
---

# Obsidian Markdown Skill

This skill creates and edits notes in the user's Obsidian vault using the Obsidian CLI (`obsidian` command) combined with Obsidian Flavored Markdown syntax.

**Vault path**: `~/Documents/Obsidian Vault`

## Obsidian CLI

The Obsidian CLI controls the running Obsidian app from the terminal. It launches Obsidian automatically if not already running.

### Creating Notes

Create a new note with content using `file:create`:

```bash
obsidian file:create path="Notes/My New Note.md" content="$(cat <<'CONTENT'
---
title: My New Note
date: 2024-01-15
tags:
  - project
---

# My New Note

Content goes here with [[wikilinks]] and other Obsidian syntax.
CONTENT
)"
```

The `path` is relative to the vault root. Subdirectories are created automatically if they don't exist.

### Editing Notes

For full content replacement, use `file:edit`:

```bash
obsidian file:edit path="Notes/Existing Note.md" content="$(cat <<'CONTENT'
Updated full content here.
CONTENT
)"
```

### Appending and Prepending

Add content to existing notes without overwriting:

```bash
# Append to the end of a note
obsidian file:append path="Notes/Log.md" content="
## New Entry

Added at the bottom."

# Prepend after frontmatter
obsidian file:prepend path="Notes/Log.md" content="This goes at the top (after frontmatter)."
```

These are particularly useful for log-style notes, journals, and accumulating information.

### Reading Notes

Read existing note content to understand context before editing:

```bash
obsidian file:read path="Notes/My Note.md"
```

### Moving and Deleting

```bash
obsidian file:move from="Old/Path.md" to="New/Path.md"
obsidian file:delete path="Notes/Obsolete.md"
```

### Daily Notes

```bash
# Open today's daily note (creates if needed)
obsidian daily

# Append content to today's daily note
obsidian daily:append content="- Met with team about [[Project Alpha]]"

# Prepend content
obsidian daily:prepend content="## Morning Goals
- [ ] Review PRs
- [ ] Write documentation"
```

### Searching the Vault

Search helps find existing notes to link to or reference:

```bash
# Full-text search
obsidian search query="meeting notes"

# JSON output for programmatic use
obsidian search query="project alpha" --json
```

### Managing Properties

```bash
# Get a property value
obsidian property:get path="Notes/My Note.md" key="status"

# Set a property
obsidian property:set path="Notes/My Note.md" key="status" value="complete"
```

### Task Management

```bash
# List tasks in a note
obsidian tasks path="Notes/Project.md" todo

# Toggle a task's completion
obsidian task path="Notes/Project.md" line=5 toggle
```

### Creating from Templates

If the user has templates configured:

```bash
obsidian template path="Notes/New Meeting.md" template="Meeting Template"
```

## Workflow Guidelines

### Creating a New Note

1. Determine the appropriate path within the vault (ask the user if unclear about folder structure)
2. Write the content with proper Obsidian Markdown — use wikilinks `[[Other Note]]` to connect to existing notes, add frontmatter with relevant properties, and use callouts for important information
3. Use `obsidian file:create` to create the note

### Editing an Existing Note

1. Read the current content with `obsidian file:read` to understand the structure
2. Decide whether to replace the full content (`file:edit`), append (`file:append`), or prepend (`file:prepend`)
3. Preserve existing wikilinks, tags, and frontmatter unless the user explicitly wants changes

### Building Connected Notes

Obsidian's power comes from linking notes together. When creating or editing notes:

- Link to related concepts with `[[wikilinks]]`
- Use `[[Note Name|display text]]` when the link text should differ from the note name
- Embed content from other notes with `![[Note Name]]` or `![[Note Name#Section]]`
- Consider what existing notes might be relevant — use `obsidian search` to find them

## Quick Syntax Reference

The most commonly needed Obsidian-specific syntax:

```markdown
[[Wikilink]]                    Internal link
[[Note|Display Text]]           Link with custom text
![[Embedded Note]]              Embed another note
![[image.png|300]]              Embed image with width

> [!note] Title                 Callout block
> Content here.

> [!warning]- Foldable          Collapsed callout
> Hidden until clicked.

- [ ] Task item                 Unchecked task
- [x] Done item                 Checked task

==highlighted text==            Highlight
%%hidden comment%%              Comment (invisible in reading view)

#tag #nested/tag                Tags
```

For the complete syntax reference including tables, math, Mermaid diagrams, footnotes, and more, see `references/syntax.md`.
