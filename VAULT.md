# Vault

This vault is managed by [Cortex](https://github.com/frontal-cortex/cortex).
Your data is plain Markdown — readable anywhere, version-controlled with git.

A vault starts with **New vault** in the app or `cortex init DIR` — offline,
from the starter built into the app — or from any template repository or
folder: `cortex init DIR --template frontal-cortex/vault-template` (an
`owner/repo`, a git URL, or a path). The template's files are copied; its
history is not. This file and `AGENTS.md` are written by the app, so a vault
always carries the version that matches it.

---

## Directory structure

```
my-vault/
├── notes/                  ← All your notes. Organise into subdirectories freely.
│   ├── journal/            ← Example: a folder for daily notes (optional convention).
│   ├── work/               ← Create any folders you like via the + button in the app.
│   └── my-note-2024.md
├── collections/            ← Databases: one folder per collection, one note per row.
│   └── tasks/              ← _index.md is the collection's page (its views); rows are notes.
├── templates/              ← Note templates. See Templates section below.
├── assets/                 ← Images & files, referenced from notes by relative path.
├── .trash/                 ← Soft-deleted notes (committed) so you can restore them.
├── .cortex/                ← Your config (committed, portable, human-readable YAML).
│   ├── settings.yaml      ← App settings.
│   ├── schemas/           ← Typed properties per collection (tasks.yaml, …).
│   ├── packs.yaml         ← Which template packs are installed, and which files they own.
│   └── favorites.yaml     ← Favorited notes.
├── .brain/                 ← Cache (gitignored). Safe to delete — rebuilt on open.
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
| `tags`    | List of tags. `#tag` in the body counts too; `a/b` nests. |
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

Supported variables: `{{date}}`, `{{time}}`, `{{title}}`, `{{uuid}}`.

Quote placeholders in frontmatter so the file stays valid YAML
(`title: "{{date}}"`, not `title: {{date}}`).

Example `templates/daily.md`:
```markdown
---
title: "{{date}}"
type: journal
tags: [journal]
---

## What happened today


## What I learned


## Tomorrow
```

## Collections and template packs

A folder under `collections/` is a database: every note in it is a row, its
frontmatter the row's properties, typed by `.cortex/schemas/<name>.yaml`. The
folder's `_index.md` is the collection's own page — its `views:` (table,
board, calendar, gallery, chart, timeline, tracker) plus any prose you write
around them. Rollups, formulas and streaks are computed when read and never
written into files.

Template packs from the marketplace (**Browse templates** in the app,
`cortex packs list` / `install` from a terminal) add collections, schemas and
note templates. A pack is Markdown and YAML only — no code — and every page
and row it installs carries `pack: <id>` in its frontmatter, so you can
always see what came from where. Installing never overwrites a file you
edited.

## AI agent integration

Any AI agent that can read/write files and run git commands can propose changes:

1. Agent creates a branch named `agent/<description>`.
2. Agent commits note changes to that branch.
3. The app shows the branch as a pending **proposal**.
4. You review the diff and **Apply** (merge) or **Discard** (delete branch).

The agent never needs to know about the app — just git and Markdown.

## Publishing

Notes stay private unless you say otherwise. Add `publish: true` to a note's
frontmatter (or tag it `public`) to mark it for the site; that alone changes
nothing on the internet. Publishing is always your own act: **Publish site…**
in the command palette, or `cortex publish --out DIR` from a terminal. The
result is a plain folder of HTML you can put on any static host, or push to
GitHub Pages with `cortex publish --gh-pages`. Wiki links to notes you did
not publish become plain text on the site, so nothing private is revealed.

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

All app settings live in one file, `.cortex/settings.yaml`. It is written with
every key on first open (so you — or an agent — always see the full schema),
and the app reloads it live whenever it changes on disk. `.cortex/` is
committed to git so your config travels with the vault; `.brain/` is a
gitignored cache you can delete at any time.

Every key, with its allowed values and default:

    auto_commit           Commit after every note save, debounced so a burst of edits is one commit. true | false (default true).
    default_note_type     Frontmatter `type` pre-filled on new notes (default note).
    journal_template      What Today opens: a template under templates/ (default daily.md), or collections/<name> to make Today that collection's row for the day.
    theme                 Colour scheme: light | dark | system (default system).
    trash_retention_days  Days before trashed notes are pruned; 0 = never (default 30).
    auto_sync_minutes     Minutes between automatic git syncs, plus on launch/focus; 0 = off (default 0).
    collab_url            Yjs websocket relay for presence and co-editing, e.g. ws://host:1234; empty = off (default empty).
    theme_file            Palette file to follow (Omarchy colors.toml shape, `~` expands); empty = use `theme` (default empty).
    accent                Action colour: empty = the theme's accent; a palette colour name (blue green yellow orange red magenta cyan brown) or a `#hex` value (default empty).
    prose_font            Page typeface: ysabeau | quattro | duo | recursive | alegreya | fraunces | crimson | serif | system | mono | any font-family; empty = ysabeau (default empty).
    prose_slant           Page tilt: empty (upright) | degrees such as 4 or 8 | italic (default empty).
    keybindings           Shortcut overrides, id -> keys (e.g. toggle-sidebar: mod+shift+b); set one with keybindings.<id>=<keys>, empty value removes it (default {}).
    terminal_command      Command run when the terminal pane opens — an agent CLI such as claude, hermes, openclaw; empty = plain shell (default empty). See `cortex agents`.
    site_title            Title of the published site (`cortex publish`); empty = the vault folder's name (default empty).
    site_home             Published note shown on the site's front page above the list, e.g. notes/about.md; empty = list only (default empty).
    marketplace_url       Template marketplace index URL; empty = the official one. Point it at a company registry to use your own packs (default empty).
    marketplace_extra     Additional marketplace index URLs, comma-separated, merged with the first (default empty).
    marketplace_tiers     Trust tiers shown in the marketplace: official, verified, community (comma-separated); empty = all (default empty).
    explorer_sort         Order of notes in the sidebar tree: name | modified | created | type, with -asc or -desc (default name-asc). Folders stay alphabetical.

From the terminal: `cortex settings` prints the file, `cortex settings describe`
explains every key, and `cortex settings set key=value…` edits it with the
right types (`cortex settings set terminal_command=claude
keybindings.toggle-sidebar=mod+shift+b`). `cortex agents` lists which agent
CLIs are installed.
