---
name: knowledge-hub-todo
description: Build a practical daily work plan from Knowledge Hub priorities, context, sources, and useful AI delegation opportunities.
---

# Knowledge Hub Todo

Resolve the Knowledge Hub repository root from this skill's location and read private data from `$HOME/.knowledge-hub/`. Do not invent work and do not dispatch agents yourself.

## Inputs

Read:

- `$HOME/.knowledge-hub/priorities.md`
- Relevant entity wiki pages and raw journal entries linked by selected priorities
- `$HOME/.knowledge-hub/sources.md` when links or people are useful
- `$HOME/.knowledge-hub/wiki/people/missing-details.md` whenever person entities or networking suggestions may be relevant
- The current date and any user-provided constraints

If priorities are absent or empty, say so and stop. Do not create placeholder tasks.

## Planning

Start with an eight-hour maximum planning window. Ask about meetings, availability, or constraints only when they could materially change the plan. Accept an explicit time budget from the user when provided.

Select two to four executable items from near populated horizon, one to two from mid and one from long:

- Near: estimate `<= 3h`
- Mid: estimate `> 3h` and `<= 7d`
- Long: estimate `> 7d`

Treat `7d` as seven ordinary calendar days. Do not interpret it as business days or focused-work effort.

Rank by urgency first, then respect deadlines, dependencies, blockers, waiting status, available time, impact, and completion leverage. Do not recommend waiting or blocked items as executable work. Do not invent a task to fill an empty horizon.

Fit recommendations within the available time. A plan may allocate less than eight hours; the default is a ceiling, not a requirement.

## User Feedback

Do not output the list of tasks for user to focus on until you have asked these questions and gotten feedback.

1) Review any tasks found in the existing priorities pages which are not complete, whether blocked or not, and ask the user to provide the current status of the top 5 highest priority tasks.

2) After the user responds, use their answers to adjust the list of priorities. Figure out the top items you've chosen for them to focus on today. Ask them if they approve or if they'd like to adjust the priorities. Adjust based on their feedback. 

## Output

Present the plan in urgency order while retaining horizon headings. For every selected task include:

- Title and status
- Urgency, estimate, and recommended time today
- Why it matters now
- Known context and source links
- What is unknown or needs to be obtained
- The first concrete steps
- People or channels to contact, when relevant
- A concise professional draft message when communication is the next useful action

Clearly identify unavailable or stale source context. Never present an inference as a verified fact.

Keep the default result in chat. Save a plan only when the user explicitly asks for a file.

## Priority updates

When the user explicitly reports that a task is completed, dropped, or waiting, update its lifecycle status and archive completed or dropped items in `$HOME/.knowledge-hub/archive/priorities-YYYY.md` as appropriate. Do not silently alter urgency, estimate, horizon, scope, or priority merely to improve today's plan.

If an update would be ambiguous, ask before editing. Preserve citations and outcomes.

When `okf` syntax is known and wiki files changed, run `okf validate $HOME/.knowledge-hub/wiki` to validate only files under `wiki/`. Do not run OKF validation against the Knowledge Hub root or check priorities, archives, sources, journals, ingest state, or any other files outside `wiki/`. If wiki validation fails, preserve existing valid files, report the failure, and do not claim success.

## AI assistance

Select up to two tasks from the priorities page which were not already chosen in previous steps.

For each task, consider whether a bounded AI action could reduce user effort. Include an `AI help` section only when useful. It may contain:

- A research, summarization, drafting, or review prompt.
- A complete Firstmate prompt for clearly scoped implementation or investigation work.
- A grouping of genuinely independent prompts that could run in parallel.

A Firstmate prompt should include the project, desired outcome, context, source links, boundaries, deliverable, validation criteria, and genuine unresolved questions. The user chooses whether to send it; this skill never dispatches work, chooses workers or models, or manages orchestration.

Do not suggest autonomous delegation for sensitive communications, personnel matters, ambiguous priorities, irreversible actions, or decisions requiring undocumented organizational judgment.

## Networking and Work Relations

Always add a concise `Networking Suggestions` section when relevant people or teams appear in the selected priorities. Read the applicable person/team pages and `wiki/people/missing-details.md` before suggesting outreach.

- Suggest a teammate only when there is a concrete work connection to today's priorities; explain the evidence and keep ticket assignments as leads, not proof of frequent collaboration.
- Offer a low-pressure, work-relevant next step (for example, ask for context, offer a review, or pair on a specific blocker) and a short draft message.
- Use names and collaboration preferences only when confirmed by the user or an approved source. Otherwise use the existing handle and say what is unknown.
- Encourage getting to know relevant teammates naturally through useful collaboration. Ask Jared to share only directly known, work-relevant details if he wants them recorded; update the missing-details log only with that confirmation.
- Treat competence, dependability, friendliness, and interpersonal compatibility as subjective personnel judgments: do not score, infer, solicit ratings about, or store them. Prefer voluntary self-described expertise and concrete collaboration preferences/outcomes, with clear sourcing.
- If there are no evidenced people to contact, say so rather than inventing a networking task.
