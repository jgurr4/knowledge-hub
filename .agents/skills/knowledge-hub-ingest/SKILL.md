---
name: knowledge-hub-ingest
description: Process Knowledge Hub daily journals into cited daily digests, topical wiki pages, and priorities.
---

# Knowledge Hub Ingest

Process the Knowledge Hub repository's private `.local/` data. Resolve the repository root from this skill's location, never from the conversation's current directory. Do not modify raw journals.

## Invocation

Process the newest unprocessed daily journal by default. Process journals in chronological order when several are unprocessed. A date argument selects one journal. Re-ingestion is explicit only:

```text
/knowledge-hub-ingest reingest YYYY-MM-DD
```

Before re-ingesting, state which derived pages will be rebuilt or changed and wait for confirmation.

## Files

- Raw journals: `.local/dailynotes/YYYY-MM-DD.md`
- Daily digests: `.local/wiki/daily/YYYY-MM-DD.md`
- People: `.local/wiki/people/`
- Projects: `.local/wiki/projects/`
- Processes: `.local/wiki/processes/`
- Resources and channels: `.local/wiki/resources/`
- Sources: `.local/sources.md`
- Active priorities: `.local/priorities.md`
- Archived priorities: `.local/archive/priorities-YYYY.md`

Create directories and derived files lazily. Preserve the raw journal byte-for-byte.

## Source setup

On every invocation, show the current sources and ask the user to confirm, edit, or skip them. If `.local/sources.md` is absent, ask for documentation, communication, initiative, and ticket-system links and create it only after the user provides them.

Validate each source when it is needed. Record its name, URL, type, governed knowledge, and last successful access date. Do not store credentials, tokens, or cookies. Continue when a source is unavailable, but mark affected conclusions as incomplete.

Use available tools and the user's configured `okf` Bash command when useful. If `okf` rejects a derived document, preserve existing valid files, report the failure, and do not claim the update succeeded.

## Processing

1. Read the selected raw journal and the existing daily digest marker.
2. Process only entries after the recorded last-processed timestamp unless this is an explicit re-ingestion.
3. Ask only questions needed to resolve ambiguity, identity, urgency, ownership, purpose, follow-up, missing context, or conflicting sources.
4. Review current-year daily and topical wiki context, following older material only when relevant.
5. Resolve high-confidence contradictions using the stronger source and retain citations. Ask the user when confidence is insufficient.
6. Warn if content appears to contain secrets, credentials, customer data, personnel information, or other obvious sensitive material. Do not copy suspected sensitive values into derived pages unless the user explicitly confirms it is appropriate.
7. Complete clarification before changing derived files. If interrupted, leave existing derived files unchanged.
8. Write one daily digest for the date and update only relevant topical pages.
9. Update priorities from the notes, wiki context, and accessible external sources.
10. Record the latest processed note timestamp in the daily digest.

## Markdown contract

All derived pages must be readable by humans and agents and follow the user's OKF conventions. Run the user's `okf` command when its syntax is known; do not invent a replacement validator.

Every meaningful derived claim gets a nearby Markdown citation. Include a Sources section with links to the raw journal, daily digest, topical pages, and external URLs used. Use timestamp headings from the raw journal for local citations.

Use stable typed slugs. Ask before merging uncertain people, projects, or resources. Preserve existing filenames across renames unless identity ambiguity requires separation.

## Priorities

Maintain `.local/priorities.md` as Markdown with active `proposed`, `active`, and `waiting` items grouped by horizon. Each item includes status, urgency from 0–5, estimate, context, next action, and sources.

- Near: estimate `<= 3h`
- Mid: estimate `> 3h` and `<= 7d`
- Long: estimate `> 7d`

Treat `7d` as seven ordinary calendar days. Do not confuse elapsed duration with business days or focused-work effort. Archive completed and dropped items in `.local/archive/priorities-YYYY.md`, retaining sources, outcome, and reason.

Do not silently change urgency, estimate, horizon, or scope merely to improve today's plan. Merge duplicate work only with high confidence; ask when a merge could erase a distinction.
