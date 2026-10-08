# Knowledge Hub

Knowledge Hub is a local-first toolkit for helping an engineer capture daily observations, turn them into durable organizational knowledge, and plan useful work.

It has three parts:

- `note`: a fast, AI-free Bash command for capturing thoughts.
- `knowledge-hub-ingest`: an interactive skill that clarifies notes and updates the wiki and priorities.
- `knowledge-hub-todo`: a planning skill that turns priorities into a practical daily work plan.

Personal data is kept in `.local/`, which is ignored by Git.

## Installation

Clone or place this repository wherever you want, then make the note command executable:

```bash
chmod +x bin/note
```

To use `note` from any directory, add a symlink to a directory on your `PATH`:

```bash
mkdir -p ~/.local/bin
ln -s "$HOME/knowledge-hub/bin/note" ~/.local/bin/note
```

Ensure `~/.local/bin` is on your `PATH`.

The command uses `$VISUAL`, then `$EDITOR`, and finally `vim`.

## Capture a note

Run:

```bash
note
```

The command opens an empty temporary file. After saving and exiting the editor, the note is appended to:

```text
.local/dailynotes/YYYY-MM-DD.md
```

Each note receives a timestamp heading. Empty or whitespace-only entries are ignored. The raw daily journal is never rewritten by the capture command.

You can also run the script directly from the repository:

```bash
./bin/note
```

## Ingest notes

Use the project-local `knowledge-hub-ingest` skill from your Pi environment.

The ingest skill:

- Reviews new daily journal entries.
- Shows saved source links and asks whether to confirm, edit, or skip them.
- Asks focused questions when a note is ambiguous.
- Creates a cited daily digest.
- Updates relevant people, project, process, and resource pages.
- Updates priorities using notes, wiki context, and available external sources.
- Warns when content appears to contain obvious sensitive information.
- Uses the repository's `okf` command when appropriate for validating derived Markdown.

To explicitly rebuild material for a date, request:

```text
/knowledge-hub-ingest reingest YYYY-MM-DD
```

Re-ingestion is never implied by an ordinary ingest request.

## Plan the day

Use the project-local `knowledge-hub-todo` skill from your Pi environment.

The todo skill reads `.local/priorities.md` and proposes up to two executable tasks from each populated horizon:

- Near: estimated duration up to 3 hours.
- Mid: more than 3 hours and up to 7 calendar days.
- Long: more than 7 calendar days.

It uses an eight-hour maximum planning window by default. The plan includes urgency, recommended time, context, next steps, missing knowledge, relevant people and channels, and draft messages when communication is the next useful action.

When useful, it can also provide prompts for bounded research, drafting, review, or implementation work that Firstmate could perform. It does not dispatch work automatically.

## Repository layout

```text
knowledge-hub/
├── bin/
│   └── note                         # AI-free note capture command
├── .agents/
│   └── skills/
│       ├── knowledge-hub-ingest/
│       │   └── SKILL.md             # Daily journal ingestion instructions
│       └── knowledge-hub-todo/
│           └── SKILL.md             # Daily planning instructions
├── .local/                          # Private, Git-ignored engineer data
│   ├── dailynotes/                  # Immutable raw daily journals
│   │   └── YYYY-MM-DD.md
│   ├── wiki/
│   │   ├── daily/                   # Derived daily digests
│   │   ├── people/                  # Coworker and contact knowledge
│   │   ├── projects/                # Project knowledge
│   │   ├── processes/               # Process and workflow knowledge
│   │   └── resources/               # Documentation, channels, and resources
│   ├── archive/                     # Completed and dropped priority history
│   │   └── priorities-YYYY.md
│   ├── sources.md                   # Saved initiative and reference sources
│   └── priorities.md                # Active proposed, active, and waiting work
├── .gitignore                       # Keeps `.local/` out of Git
└── README.md                        # This guide
```

The `.local/` directory is specific to one engineer and should not be committed or shared accidentally. Derived Markdown is intended to be readable by both people and agents, while raw journals remain simple source material.

## Priority fields

Active priority items should record:

- Status: proposed, active, or waiting.
- Urgency: `0` through `5`.
- Estimate: for example `45m`, `3h`, or `4d`.
- Horizon: near, mid, or long.
- Context and next action.
- Links to supporting daily digests, wiki pages, or external sources.

Completed and dropped items are moved to the yearly archive by the ingest or todo workflow.

## Privacy

Knowledge Hub is local-first, but the ingest skill may read sources and use the AI tools configured in the user's environment. Do not place credentials or secrets in notes. The skills are instructed to warn about obvious sensitive material and avoid copying suspected sensitive values into derived pages unless the user confirms it is appropriate.
