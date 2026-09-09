# Vault template

A starter [Cortex](https://github.com/frontal-cortex/cortex) vault: plain
Markdown notes in a git repository. The app is a lens on the files; the files
are the truth, readable and grep-able anywhere.

## Start a vault

Three ways, same result:

- **In the app**: *New vault*. Offline; this starter is built into the app.
- **From a terminal**: `cortex init ~/my-vault`. Same starter, same layout.
- **From this repository**, or any template repository or folder:

  ```bash
  cortex init ~/my-vault --template frontal-cortex/vault-template
  cortex init ~/my-vault --template you/your-template   # an owner/repo, a git URL, or a folder
  ```

  The template's files are copied and its history is not, so the vault is
  yours from the first commit. Or click **Use this template** on GitHub,
  clone the result, and open it in the app.

The app writes `VAULT.md`, `AGENTS.md` and `.cortex/settings.yaml` into a
vault on first open (and `cortex init` writes them right away), so the copies
in this repository are for reading here — the vault always carries the
version that matches the app.

## What's inside

A short tour, one note per feature, each doing the thing it describes, and a
three-row collection for the tour to show. Delete `notes/tour/` and
`collections/books/` when you know your way round.

```
my-vault/
├── notes/
│   ├── welcome.md            ← Start here: the tour as a checklist.
│   ├── tour/                 ← Writing, linking, daily notes, collections,
│   │                            templates and packs, agents and the CLI, sync and looks.
│   ├── ideas/second-brain.md ← A note to link to.
│   ├── journal/              ← Daily notes (Today uses templates/daily.md).
│   └── work/
├── collections/books/        ← A database: _index.md holds the views, rows are notes.
├── templates/                ← Note templates: {{date}} {{time}} {{title}} {{uuid}}.
│   ├── daily.md  meeting.md  note.md
├── .cortex/schemas/books.yaml ← Typed properties for the collection.
├── .gitignore                ← Ignores .brain/, the app's rebuildable index.
├── VAULT.md                  ← The conventions, for people.
└── AGENTS.md                 ← The conventions, for agents (CLI, MCP, rules).
```

More collections, schemas and templates arrive as you install packs from
the marketplace (`cortex packs install <id>`).

## Make your own template

Any vault is a template. Put whatever notes, folders, templates, collections
and schemas you want every new vault to start with in a repository, and point
`cortex init --template` at it. `{{today}}` in a note's `created:` becomes the
day the vault is made.

## Sync

```bash
git remote add origin git@github.com:you/my-vault.git
git push -u origin main
```

Then the sync button in the app pushes and pulls.
