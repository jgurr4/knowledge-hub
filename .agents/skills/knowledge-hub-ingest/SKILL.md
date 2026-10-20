---
name: knowledge-hub-ingest
description: Process Knowledge Hub daily journals into cited entity wiki pages (people, teams, projects, processes, concepts, resources) and priorities.
---

# Knowledge Hub Ingest

Process the Knowledge Hub repository's private `$HOME/.knowledge-hub/` data. Resolve the repository root from this skill's location, never from the conversation's current directory. Do not modify raw journals.

## Invocation

Process the newest unprocessed daily journal by default. Process journals in chronological order when several are unprocessed. A date argument selects one journal. Re-ingestion is explicit only:

```text
/knowledge-hub-ingest reingest YYYY-MM-DD
```

Before re-ingesting, state which derived pages will be rebuilt or changed and wait for confirmation.

## Files

- Raw journals: `$HOME/.knowledge-hub/dailynotes/YYYY-MM-DD.md`
- People: `$HOME/.knowledge-hub/wiki/people/`
- Teams: `$HOME/.knowledge-hub/wiki/teams/`
- Projects: `$HOME/.knowledge-hub/wiki/projects/`
- Processes: `$HOME/.knowledge-hub/wiki/processes/`
- Concepts: `$HOME/.knowledge-hub/wiki/concepts/`
- Resources and channels: `$HOME/.knowledge-hub/wiki/resources/`
- Ingest state: `$HOME/.knowledge-hub/ingest-state.md`
- Sources: `$HOME/.knowledge-hub/sources.md`
- Active priorities: `$HOME/.knowledge-hub/priorities.md`
- Archived priorities: `$HOME/.knowledge-hub/archive/priorities-YYYY.md`

Create directories and derived files lazily. Preserve the raw journal byte-for-byte.

## Wiki pages are entities, never dates

The wiki holds only entity pages: a person, team, project, process, concept, or resource. Never create a wiki page whose subject is a day, date, journal, or ingestion run. Never create `wiki/daily/` or any date-named wiki file (for example `YYYY-MM-DD.md`). The raw journal in `dailynotes/` is already the chronological record; do not duplicate it as a digest.

File each journal fact under the entity it is about. If a fact fits no existing entity, create a new typed entity page or ask the user where it belongs. If a fact is only a to-do with no lasting knowledge, put it in priorities instead of the wiki. Date context belongs inside entity pages as dated statements citing the raw journal, not as a separate page.

## Source setup

On every invocation, show the current sources and ask the user to confirm, edit, or skip them. If `$HOME/.knowledge-hub/sources.md` is absent, ask for documentation, communication, initiative, and ticket-system links and create it only after the user provides them.

Validate each source when it is needed. Record its name, URL, type, governed knowledge, and last successful access date. Do not store credentials, tokens, or cookies. Continue when a source is unavailable, but mark affected conclusions as incomplete.

Use available tools and the user's configured `okf` Bash command when useful. If `okf` rejects a derived document, preserve existing valid files, report the failure, and do not claim the update succeeded.

## Processing

1. Read the selected raw journal and its last-processed timestamp from `ingest-state.md`.
2. Process only entries after the recorded last-processed timestamp unless this is an explicit re-ingestion.
3. Ask only questions needed to resolve ambiguity, identity, urgency, ownership, purpose, follow-up, missing context, or conflicting sources.
4. Review relevant entity wiki pages and current-year raw journals, following older material only when relevant.
5. Resolve high-confidence contradictions using the stronger source and retain citations. Ask the user when confidence is insufficient.
6. Warn if content appears to contain secrets, credentials, customer data, personnel information, or other obvious sensitive material. Do not copy suspected sensitive values into derived pages unless the user explicitly confirms it is appropriate.
7. Complete clarification before changing derived files. If interrupted, leave existing derived files unchanged.
8. Update or create only the relevant entity pages (including people, teams, projects, processes/sops, concepts, resources, channels and priorities). Do not write any per-day summary page.
9. When the user provides new teammate details, update the matching `wiki/people/<slug>.md` page with the directly stated, work-relevant facts and clear nearby citations to the source journal/context. Resolve identity against the existing page/ADSID; ask if the match is ambiguous rather than guessing. Update `wiki/people/missing-details.md` to reflect which factual fields were confirmed, and preserve unknowns for fields not provided.
10. Update priorities from the notes, wiki context, and accessible external sources.
10. Record the latest processed note timestamp for the journal date in `ingest-state.md` (one line per journal date: `YYYY-MM-DD: HH:MM:SS`). Show the user a short summary of changes in chat instead of writing a digest file.

## People details and privacy

Treat teammate details as factual knowledge supplied by the user or a trusted source, not as a prompt to profile coworkers. Record names, role, self-described expertise, current work, and concrete work-relevant collaboration preferences only when directly supplied or appropriately sourced. Do not infer or store competence, dependability, friendliness, interpersonal compatibility, or numeric personnel ratings from tickets, journals, or other indirect evidence. If the user volunteers subjective evaluations, do not add them to people pages; explain that people pages should use factual, respectful, work-relevant information. Frequent collaborators may be documented only when explicitly identified by a reliable source; ticket co-assignment alone is not proof of frequent interaction. Keep unsupported fields as `unknown`, and update the missing-details log accordingly.

## Markdown contract

All derived pages must be readable by humans and agents and follow the user's OKF conventions. Run the user's `okf` command when its syntax is known; do not invent a replacement validator.

Every meaningful derived claim gets a nearby Markdown citation. Include a Sources section with links to the raw journal entries, related entity pages, and external URLs used. Cite raw journal entries by their timestamp headings (for example `dailynotes/YYYY-MM-DD.md#hhmmss`). Never cite or link a daily digest.

Use stable typed slugs. Ask before merging uncertain people, teams, projects, or resources. Preserve existing filenames across renames unless identity ambiguity requires separation.

## Priorities

Maintain `$HOME/.knowledge-hub/priorities.md` as Markdown with active `proposed`, `active`, and `waiting` items grouped by horizon. Each item includes status, urgency from 0–5, estimate, context, next action, and sources.

- Near: estimate `<= 3h`
- Mid: estimate `> 3h` and `<= 7d`
- Long: estimate `> 7d`

Treat `7d` as seven ordinary calendar days. Do not confuse elapsed duration with business days or focused-work effort. Archive completed and dropped items in `$HOME/.knowledge-hub/archive/priorities-YYYY.md`, retaining sources, outcome, and reason.

Do not silently change urgency, estimate, horizon, or scope merely to improve today's plan. Merge duplicate work only with high confidence; ask when a merge could erase a distinction.

## Version Control

Always commit any changes made to the local .knowledge-hub folder. Never push commit changes to a remote server.
