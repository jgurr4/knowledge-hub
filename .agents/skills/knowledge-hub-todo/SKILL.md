---
name: knowledge-hub-todo
description: Build a practical daily work plan from Knowledge Hub priorities, context, sources, and useful AI delegation opportunities.
---

# Knowledge Hub Todo

Resolve the Knowledge Hub repository root from this skill's location and read private data from `$HOME/.knowledge-hub/`. Do not invent work and do not dispatch agents yourself.

## Inputs

Read:

- `$HOME/.knowledge-hub/priorities.md`
- Relevant daily digests and topical wiki pages linked by selected priorities
- `$HOME/.knowledge-hub/sources.md` when links or people are useful
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

Provide user the items you've chosen for them to focus on today. Ask them if they approve or if they'd like to adjust the priorities. Adjust based on their feedback. Then proceed to the next step only after they've replied.

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

Use the user's `okf` Bash command when its syntax is known for changed derived Markdown. If validation fails, preserve existing valid files, report the failure, and do not claim success.

## AI assistance

Select up to two tasks from the priorities page which were not already chosen in previous steps.

For each task, consider whether a bounded AI action could reduce user effort. Include an `AI help` section only when useful. It may contain:

- A research, summarization, drafting, or review prompt.
- A complete Firstmate prompt for clearly scoped implementation or investigation work.
- A grouping of genuinely independent prompts that could run in parallel.

A Firstmate prompt should include the project, desired outcome, context, source links, boundaries, deliverable, validation criteria, and genuine unresolved questions. The user chooses whether to send it; this skill never dispatches work, chooses workers or models, or manages orchestration.

Do not suggest autonomous delegation for sensitive communications, personnel matters, ambiguous priorities, irreversible actions, or decisions requiring undocumented organizational judgment.

## Networking and Work relations

Add the `Networking Suggestions` section. It may contain:

- A entity such as team or person from the wiki pages
- A useful fact to remember about them such as when they are good to reach out to, or what the user notes has been good experiences to build rapport in the past
- A actionable step to reach out and engage with them based on the days priorities and a text snippet of what to say
- An optional work-related or programming joke

If no person or team entity wiki pages exist, then encourage the user to reach out to someone today and add any details using `note` so that you can begin to offer suggestions for building rapport
