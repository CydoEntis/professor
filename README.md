# Professor

A tutor that remembers what you actually proved you know.

Professor runs learning tracks that survive across sessions. It keeps a roadmap, gives you
the smallest useful exercise, waits while you attempt it, and only marks a concept learned
when *you* produced the evidence — a working solution, a correct explanation, a diagnosis,
a recalled answer. Explaining something to you does not count. Nor does copying the answer.

It runs in both Claude Code (`/professor`) and Codex (`$professor`) off one copy of the
skill.

## Install

```bash
git clone https://github.com/CydoEntis/professor.git
cd professor
./install
```

The installer links `skills/professor/` into whichever of Claude Code and Codex you have.
It links rather than copies, so `git pull` updates what the agent reads. Run it again any
time; it changes nothing that is already right.

## Use

Start where you are, in the project you want to learn in:

| You say | Professor does |
| --- | --- |
| `/professor start rust ownership` | Sets the goal, places you, sketches a roadmap, gives you a first objective |
| `/professor continue` | Picks up the active track at the next small objective |
| `/professor status` | Current milestone, what is solid, what needs review, what is next |
| `/professor quiz` | Active recall over your weakest and oldest material |
| `/professor check` | Grades the work you just wrote against the current objective |
| `/professor review` | Spaced retrieval, preferring your real work over flashcards |
| `/professor explain <topic>` | A direct lesson, then a question back to you |

Natural language works the same: "quiz me on what I learned last week."

## State

Everything lives in `.learning/` at the root of whatever project you are learning in —
plain Markdown you can read, edit, and commit (or gitignore) yourself.

```text
.learning/
|-- PROFILE.md          how you like to be taught
|-- TRACKS.md           your tracks, and which is active
`-- tracks/<slug>/
    |-- GOAL.md         what you want to be able to do
    |-- ROADMAP.md      Now / Next / Later
    |-- KNOWLEDGE.md    every concept, its state, and the evidence
    |-- PROGRESS.md     dated sessions
    `-- ...             resources, questions, lessons, assessments, reviews
```

Concepts move `UNKNOWN → INTRODUCED → PRACTICING → DEMONSTRATED → RETAINED`, and any of
them can fall back to `NEEDS_REVIEW`. Every transition carries the evidence that caused it
and the level of help you had, so the record stays honest about what you can do alone.

## Why the help is rationed

Professor uses a hint ladder — a nudge before a pointer, a pointer before pseudocode,
pseudocode before code — and records when it went high, because heavily assisted work is
not evidence of independence. It will hand you the whole answer if you ask for it, or if
you are genuinely stuck after real attempts. It just will not pretend that taught you
something.

The same logic governs question format. Answering in your own words is *recall*; picking
from a list is *recognition*. Professor prefers open questions, uses multiple choice only
where the options are the real task, and logs which you did.
