# Persistent Learning State

Read this reference before creating, reading, or updating `.learning/`.

## Location and discovery

Use `.learning/` at the active project or workspace root. Do not search outside the active workspace. If the user selects another location, use it consistently and record that choice in the session response.

Use this structure, creating subdirectories only when they are needed:

```text
.learning/
|-- PROFILE.md
|-- TRACKS.md
`-- tracks/
    `-- <track-slug>/
        |-- GOAL.md
        |-- ROADMAP.md
        |-- KNOWLEDGE.md
        |-- PROGRESS.md
        |-- RESOURCES.md
        |-- QUESTIONS.md
        |-- PROJECT.md
        |-- lessons/
        |-- exercises/
        |-- assessments/
        `-- reviews/
```

Use a stable lowercase hyphenated track slug. Never silently rename an existing track.

## Shared files

`PROFILE.md` contains preferences that apply across tracks: desired teaching style, preferred examples, accessibility needs, pace, and explicit constraints. Record explicit preferences immediately; infer durable preferences only after repeated evidence and label them as inferred.

`TRACKS.md` is the index and source for the active track. For each track, record its display name, slug, status (`Active`, `Paused`, `Completed`, or `Archived`), primary topics, and last activity date. Mark at most one track as the default active track unless the user explicitly wants otherwise.

## Track files

- `GOAL.md`: desired capability, practical outcome, boundaries, and completion evidence.
- `ROADMAP.md`: adaptive curriculum organized as `Now`, `Next`, `Later`, and optional `Possible`. Fully detail only the near-term work.
- `KNOWLEDGE.md`: concept states, evidence, assistance level, last demonstrated date, review timing, and misconceptions.
- `PROGRESS.md`: dated session summaries containing completed work, evidence, struggles, assistance given, and next action.
- `RESOURCES.md`: vetted resources, why each matters, and when to use it. Prefer primary or official sources.
- `QUESTIONS.md`: open and resolved learner questions that may become future lessons.
- `PROJECT.md`: the learning project's current scope, decisions, milestones, and links to learner-owned artifacts. If no project exists, say so briefly instead of inventing one.

Place substantial lesson notes, exercises, assessments, and spaced-review records in their matching directories. Do not create a separate file for trivial interactions.

Generated artifacts — quiz pages, reference sheets, diagrams the learner opens — live in those same directories, named `0001-<dash-case-name>.html` and numbered across the track. Link each one from the file it belongs to so it can be found without listing directories. See [visuals.md](visuals.md) before writing any of them, and note that producing one is teaching: an artifact the learner merely read supports `INTRODUCED`, nothing more.

## Knowledge states

Use exactly these states:

- `UNKNOWN`: no meaningful encounter yet.
- `INTRODUCED`: explained or observed, but not independently used.
- `PRACTICING`: the learner is attempting or applying it with support.
- `DEMONSTRATED`: used or explained correctly without substantial assistance.
- `RETAINED`: demonstrated again after time has passed or in a meaningfully different context.
- `NEEDS_REVIEW`: previously demonstrated but later recall or application was weak.

Typical transitions are `UNKNOWN -> INTRODUCED -> PRACTICING -> DEMONSTRATED -> RETAINED`, but evidence controls the transition. Any learned state can move to `NEEDS_REVIEW`; recovery returns it to `DEMONSTRATED` or `RETAINED` depending on evidence.

Do not promote from statements such as "I understand," from assistant-written solutions, or from copied code. Valid evidence includes learner-written working code, an accurate explanation, successful debugging, a solved exercise, a meaningful quiz response, or application in a new context.

Note how the evidence was produced, since the form bounds what it can support. Mark an answer the learner wrote or built as `(recall)` and one they picked from options offered to them as `(recognition)`. Recognition can place a learner or expose a misconception, but `DEMONSTRATED` and `RETAINED` require recall, application, or explanation in their own words.

Record enough detail to audit the judgment:

```markdown
### Transactions
State: DEMONSTRATED
Last evidence: 2026-09-19
Assistance: Hint level 1
Evidence:
- Explained atomicity and fixed a transaction boundary independently. (recall)
Review after: 2026-09-26
```

Track recurring misconceptions separately with the misconception, corrective model, evidence, and status (`Active` or `Resolved`).

## Reviews and retention

Use review intervals near 1, 3, 7, 14, and 30 days as guidance, not rigid scheduling. Prefer retrieval embedded naturally in current work. Promote `DEMONSTRATED` to `RETAINED` only after successful later recall or transfer.

## Update protocol

Before writing, read the files relevant to the request. After an interaction produces evidence:

1. Update the affected knowledge entries and misconception records.
2. Append or amend the current dated progress entry without duplicating it.
3. Adjust `ROADMAP.md` only when priorities or evidence changed.
4. Update `TRACKS.md` activity/status when appropriate.
5. Tell the learner concisely what changed and what comes next.

Never overwrite unexplained user content. Prefer focused edits over regenerating whole files. Resetting a topic means returning its current state to `UNKNOWN` or `INTRODUCED` as requested and recording the reset in progress history; delete historical evidence only on an explicit deletion request.

## Initialization minimum

For a new track, establish only what is needed to begin:

- learner goal and desired outcome;
- current experience or a 3-7 item lightweight assessment when placement matters;
- a rough milestone map with a detailed `Now` section;
- the first small objective.

Do not design hundreds of lessons in advance.
