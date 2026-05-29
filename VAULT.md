# Vault

This vault is managed by [Second Brain](https://github.com/frontal-cortex/second-brain).
Your data is plain Markdown — readable anywhere, version-controlled with git.

---

## Directory structure

```
my-vault/
├── notes/                  ← All your notes. Organise into subdirectories freely.
│   ├── journal/            ← Example: a folder for daily notes (optional convention).
│   ├── work/               ← Create any folders you like via the + button in the app.
│   └── my-note-2024.md
├── templates/              ← Note templates. See Templates section below.
├── .brain/                 ← App metadata (gitignored). Safe to delete — rebuilt on open.
│   └── index.db           ← Full-text search index (SQLite).
└── VAULT.md                ← This file.
```

## Note format

Every note is a Markdown file with optional YAML frontmatter:

```markdown
---
title: My Note
type: note
tags: [ideas, project-x]
created: 2024-01-15
---

Note body here. Use [[Note Title]] to link to other notes.
```

### Frontmatter fields

| Field     | Description                                        |
|-----------|----------------------------------------------------|
| `title`   | Display name — used in search, links, and the UI.  |
| `type`    | Note type: `note`, `task`, `meeting`, or anything. |
| `tags`    | List of tags for filtering.                        |
| `created` | ISO date the note was created (YYYY-MM-DD).        |

Custom fields are fully supported — add any key/value pair you need.
Keys are always sorted alphabetically for clean git diffs.

## Wiki links

Type `[[` inside any note to link to another note by title:

```markdown
See my notes on [[Project Alpha]] and [[Meeting 2024-01-15]].
```

Links are resolved by `title` frontmatter, falling back to filename stem.
Backlinks (notes that link *to* the current note) are shown at the bottom of the editor.

## Templates

Place `.md` files in `templates/` to use as note templates.

**Special templates:**
- `templates/daily.md` — used when creating a journal entry via the Today button.

Supported variables: `{{date}}`, `{{title}}`

Example `templates/daily.md`:
```markdown
---
title: {{date}}
type: journal
tags: [journal]
---

## What happened today


## What I learned


## Tomorrow
```

## AI agent integration

Any AI agent that can read/write files and run git commands can propose changes:

1. Agent creates a branch named `agent/<description>`.
2. Agent commits note changes to that branch.
3. The app shows the branch as a pending **proposal**.
4. You review the diff and **Apply** (merge) or **Discard** (delete branch).

The agent never needs to know about the app — just git and Markdown.

## Sync

The vault syncs to any standard git remote:

```bash
# Set up a remote once
git remote add origin git@github.com:you/my-vault.git
git push -u origin main
```

After that, use the sync button (↑↓) in the app to push/pull.
The sync button turns orange when you have unpushed or unpulled commits.

## Settings

App settings live in `.brain/settings.yaml` (created automatically if absent):

```yaml
auto_commit: false          # commit after every note save
default_note_type: note     # pre-filled type for new notes
journal_template: daily.md  # template used for Today notes
```
