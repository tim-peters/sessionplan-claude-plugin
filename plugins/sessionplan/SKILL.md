---
name: sessionplan
description: Create structured agendas for workshops, trainings, meetings, and events. Turn the user's goals, timing, and constraints into a practical session plan and produce a shareable Sessionplan link. Use proactively for concrete timed-session requests, including German requests for an Agenda, Workshop-Ablauf, Trainingsplan, Meeting-Ablauf, or Event-Zeitplan. Also use when the user provides an existing Sessionplan link to inspect, summarize, review, validate, adapt, or change. Do not use for generic facilitation advice, calendar scheduling, or ordinary meeting-note summaries without a request for a concrete timed agenda.
---

# Sessionplan

Use Sessionplan when the user wants a concrete, timed plan for a workshop, training, meeting, facilitated session, seminar, retrospective, offsite, lesson, or event agenda.

Do not activate this workflow for generic advice about facilitation or meetings when the user does not want a concrete agenda or Sessionplan.

Do not activate this workflow for calendar scheduling, ordinary meeting-note summaries, or general engagement advice without a request for a concrete timed agenda.

## Choose the workflow

### Create a new session

1. Infer the session goal, format, duration, audience, and constraints from the conversation.
2. Use the user's language for titles, descriptions, notes, and materials unless they ask for another language.
3. If some details are missing, make reasonable low-risk assumptions when a useful plan is still possible. State important assumptions briefly. Ask a clarifying question only when the missing information would materially change the plan.
4. Design a coherent timed agenda. Prefer a practical rhythm of input, activity, discussion, reflection, breaks, and transitions as appropriate to the user's goal.
5. Build a complete Sessionplan JSON object that matches the live `create_session_link` tool schema. Use `https://sessionplan.de/import-schema.md` as the shared JSON reference; the live MCP schema remains authoritative.
6. Call `create_session_link` exactly once with the completed session.
7. Return the URL produced by the tool as the Sessionplan link. Never invent, reconstruct, or manually modify a Sessionplan URL.
8. Briefly summarize the resulting agenda and mention any important assumption you made.

### Read or summarize an existing Sessionplan

1. When the user provides a Sessionplan URL, hash, or snapshot code, call `decode_session_link` first.
2. Use the decoded session as the source of truth.
3. If the user only wants a summary, review, or check, do not create a new link unless they ask for changes.

### Modify an existing Sessionplan

1. Call `decode_session_link` first.
2. Preserve the existing structure, IDs, people, block types, and untouched fields where practical.
3. Apply only the requested changes plus any minimal consistency fixes required by the schema.
4. Call `create_session_link` with the complete updated session.
5. Return the new link from the tool. Never claim that the old link was modified in place.

## Planning rules

- Make block durations realistic and internally consistent.
- When the user gives a total duration, make the agenda fit it unless they explicitly allow overflow.
- Use breaks for longer sessions when appropriate, but do not force a break into very short sessions.
- Use groups and breakouts only when they improve the requested format.
- Add participant/person records only for people or roles actually supplied by the user. Do not invent names or personal details.
- Put facilitation instructions and method details in `notes`.
- Put physical or digital requirements in `material`; use an empty string when no material is needed.
- Keep titles concise enough to scan in a timeline.
- Prefer a small set of meaningful block types over many near-duplicates.
- If a date or start time is unknown, use `null` where the tool schema allows it rather than inventing one.

## Responsible use

- Treat the generated agenda as a planning draft that the user can review and adapt.
- Do not invent participant characteristics, diagnoses, performance assessments, sensitive attributes, or confidential context.
- Avoid adding personal or sensitive information that is not necessary for the requested agenda.
- Do not present generated facilitation choices as objectively correct; adapt them to the user's stated goals and constraints.
- For high-stakes contexts, keep the plan organizational and facilitative rather than making unsupported professional, legal, medical, or psychological judgments.

Sessionplan is designed for shareable workshop and session planning. Its public product information describes the core web app as usable without registration and "Private by Design", with data stored locally in the browser by default. Do not make stronger privacy claims about a specific link, integration, or external AI system than the available tools or product information support.
