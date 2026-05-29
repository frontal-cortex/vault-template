# Vault Template

A starter [Second Brain](https://github.com/frontal-cortex/second-brain) vault.

A vault is a plain git repository of Markdown notes. The app stays out of the
way — your data is readable, grep-able, and version-controlled anywhere.

## Get started

1. Click **Use this template → Create a new repository** at the top of this
   page (or `gh repo create your-name/my-vault --template frontal-cortex/vault-template`).
2. Clone your new repo locally.
3. Open it in the Second Brain app (**Open Vault → select the folder**).

That's it. The app creates a `.brain/` index on first open and writes nothing
else you haven't authored.

## What's inside

```
my-vault/
├── notes/                 ← All your notes. Organise into folders freely.
│   ├── welcome.md         ← Start here.
│   ├── ideas/
│   │   └── second-brain.md
│   ├── journal/           ← Daily notes (Today button uses templates/daily.md).
│   └── work/
├── templates/             ← Note templates.
│   ├── daily.md           ← Special: used by the Today button.
│   └── note.md
├── .gitignore             ← Ignores .brain/ (the app index).
└── VAULT.md               ← Full conventions reference.
```

See [VAULT.md](VAULT.md) for the full format and conventions.

## Sync

```bash
git remote add origin git@github.com:you/my-vault.git
git push -u origin main
```

Then use the sync button (↑↓) in the app to push/pull.
