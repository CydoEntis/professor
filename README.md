# Professor

<p align="center">
  <img src="assets/professor-logo.png" alt="Professor logo" width="300">
</p>

A tutor that remembers what you actually proved you know.

Ask an AI to teach you something and it will explain beautifully, you will nod, and a week
later none of it is there. The explanation felt like learning. It wasn't. Worse, when you
get stuck it hands you the finished answer, you paste it in, it works, and the record of
that session — if there is one at all — says you learned the topic.

Professor is built to refuse that. It runs learning tracks that survive across sessions,
gives you the smallest useful exercise and then *stops talking*, rations help up a ladder
instead of dumping the solution, and marks a concept learned only when **you** produced the
evidence. Explaining something to you does not promote it. Nor does copying the answer. Nor
does saying "makes sense."

It runs in both Claude Code (`/professor`) and Codex (`$professor`) from one copy of the
skill.

---

## Contents

- [What you need](#what-you-need)
- [Install](#install)
- [Your first session](#your-first-session)
- [Commands](#commands)
- [How it teaches](#how-it-teaches)
- [Diagrams, quizzes, and other artifacts](#diagrams-quizzes-and-other-artifacts)
- [Knowledge states](#knowledge-states)
- [Where your progress lives](#where-your-progress-lives)
- [What it will not do](#what-it-will-not-do)
- [Working on more than one thing](#working-on-more-than-one-thing)
- [Finishing a track](#finishing-a-track)
- [Questions](#questions)
- [Repository layout](#repository-layout)

---

## What you need

- **Claude Code or Codex**, or both. Professor runs inside either.
- **Nothing else.** It is Markdown instructions and a link script. No runtime, no
  dependencies, no account, no network calls.

## Install

```bash
git clone https://github.com/CydoEntis/professor.git
cd professor
./install
```

The installer links `skills/professor/` into whichever of Claude Code and Codex it finds:

```text
linked   /home/you/.claude/skills/professor
linked   /home/you/.codex/skills/professor

Ready. Start a track with /professor in Claude Code or $professor in Codex.
```

It **links rather than copies**, so `git pull` updates what the agent reads — there is one
source of truth and both agents share it. Run `./install` again whenever you like; it
changes nothing that is already correct, and it will never overwrite a real folder someone
put there by hand. On Windows it makes a junction, so you do not need developer mode.

No restart needed. The skill is available in your next message.

## Your first session

Open your agent **in the project you want to learn in** — the directory matters, because
that is where your progress is stored.

```
/professor start rust ownership
```

Professor will do four things and then hand the floor back to you:

1. **Ask what you actually want to be able to do.** Not "learn Rust" — *ship a CLI without
   fighting the borrow checker*. The goal grounds every later decision about what to teach
   and what to skip.
2. **Place you.** Usually three to seven quick questions or tiny tasks, and it stops early
   once your level is obvious. If placement clearly does not matter, it skips this.
3. **Sketch a roadmap** — `Now` in detail, `Next` in outline, `Later` as a list. It will not
   design forty lessons up front, because the third one will be wrong by the time you get
   there.
4. **Give you one small objective**, and wait.

That last part is the whole design. When Professor gives you something to attempt, the turn
ends. It will not keep lecturing, and it will not do the exercise for you while you watch.

Come back whenever — an hour or a month later:

```
/professor continue
```

## Commands

Every command works as a slash command, a `$` command in Codex, or plain English. "Quiz me
on what I did last week" and `/professor quiz` reach the same place.

| Command | What it does |
| --- | --- |
| `start <topic>` | Begin a track. Sets the goal, places you, sketches a roadmap, gives you a first objective. |
| `continue` | Resume the active track at the smallest useful next objective, picking up any unfinished attempt. The default way back in. |
| `checkpoint` | Stop cleanly: record the attempt in flight, what blocked it, and the next action. Or just say "I'm done for today". |
| `status` | Current milestone, what is solid, what is weak, what is due for review, what is next. |
| `roadmap` | `Now`, `Next`, and a short `Later`. Adapts as evidence comes in. |
| `check` | Assess work you just wrote against the current objective, and ask for another attempt if it is close. |
| `quiz` | Active recall over your weakest and oldest material. It will not teach you the answer first. |
| `review` | Spaced retrieval, preferring your real work over flashcards. |
| `explain <topic>` | A direct lesson — concept, why it matters, mental model, example, common mistake — then a question back to you. |
| `resources` | Curated high-quality sources, and why each one is worth your time. |
| `reset-topic <topic>` | Reset one topic's state after confirming scope. Keeps your history unless you explicitly ask to delete it. |

### When to reach for which

- **Stuck on something you are building?** `check` — paste what you wrote.
- **Been away a while?** `continue`, or `review` if it has been weeks.
- **Not sure it stuck?** `quiz`. Being wrong here is useful; it is how `NEEDS_REVIEW` gets
  set and how the roadmap reorders itself.
- **Want the lecture, not the exercise?** `explain`. Note that an explanation alone only
  moves a concept to `INTRODUCED` — see below.
- **Stopping for the day?** Nothing required — but see below for why it is worth a sentence.

### Saving and resuming

You do not have to save. Professor writes to `.learning/` as evidence arrives, so progress
persists whether or not you say anything.

The catch is that evidence is not the only thing worth keeping. Stop halfway through an
exercise — stuck, interrupted, out of time — and you have produced nothing to promote, so a
naive tutor records nothing and greets you tomorrow from a stale next step, as though the
attempt never happened. And there is no way to detect that a session ended; you just stop
replying.

So Professor writes the **open loop** the moment it hands you something to do: what you are
attempting, how far you got, what blocked you, and the next action.

```markdown
### Open loop
Attempting: writing the parser's lifetime annotations on `Token<'a>`
Reached: compiles until the third method; borrow outlives the struct
Blocked by: not sure whether the lifetime belongs on the struct or the method
Assistance so far: hint level 2
Next action: try the annotation on the struct, then explain why the third method fails
```

`continue` resumes from that, not from the last thing that happened to go well.

`checkpoint` writes the same record on demand, for when you want to stop deliberately —
"I'm done for today" reaches it too. It is a convenience, not the mechanism. Closing your
terminal mid-exercise loses nothing.

## How it teaches

### It waits

If you have been handed an exercise, a question, or something to go observe, the turn is
over. This is the single most important behavior in the skill, and the easiest one to
undervalue until you have used a tutor that does not do it.

### The hint ladder

When you are stuck, Professor climbs from the bottom, one rung at a time, and only after
you have actually attempted something:

| Rung | What you get |
| --- | --- |
| 0 | "What have you tried? What do you think is happening?" |
| 1 | Conceptual direction |
| 2 | A pointer to the relevant concept, invariant, tool, or API |
| 3 | Pseudocode or a structured outline |
| 4 | A partial implementation or one worked step |
| 5 | The complete solution |

Rung 5 is entirely legitimate — when you ask for it outright, when you are genuinely stuck
after real attempts, or when studying a reference implementation *is* the lesson. It is not
withheld to be precious about it. It is simply **logged**, because rungs 3 through 5 mean
the work no longer demonstrates that you could do it alone.

In Claude Code, Professor can offer you the next rung as a real choice prompt. Asking which
rung you want is not itself a hint and costs you nothing.

### Modes

**Guided** is the default: short explanation, small task, review your attempt, smallest
useful hint, try again.

| Mode | When |
| --- | --- |
| Guided | Default |
| Socratic | You may already have the mental model, or you prefer to discover it |
| Review | You have written something and want it critiqued, not rewritten |
| Quiz | Active recall, no teaching before you answer |
| Exam | Minimal hints, stated boundaries, recorded separately from coached practice |
| Explain | You want the lesson directly |

Just ask for one by name, any time.

### Recall beats recognition

Answering in your own words is **recall**. Picking the right answer off a list is
**recognition** — much easier, and much weaker evidence that you could produce it unaided.

Professor asks open questions by default. It uses multiple choice only where the options
genuinely *are* the task — which of these four has the race condition, which index will the
planner choose, is this safe or unsafe — and then it asks you to justify the pick. Those
answers get logged as `(recognition)` and **cannot promote a concept past `PRACTICING`.**

This is why Professor does not lean on Claude Code's interactive pickers for the actual
knowledge checks, even though it has them. They are used for decisions *about* the session
— which track to resume, which mode, which hint rung, confirming a reset — where a fixed
list of options really is the whole answer space.

### Spaced review

Roughly 1, 3, 7, 14, and 30 days, treated as guidance rather than a schedule to obey.
Professor prefers to slip an old concept into whatever you are working on now over quizzing
you on something disconnected. Passing a delayed or transferred application is what moves
`DEMONSTRATED` to `RETAINED`.

## Diagrams, quizzes, and other artifacts

Professor draws when a picture shows a **mechanism** — what points at what, what happens in
what order, what sits where in memory — and not to decorate a list. Often the better move
is to make you draw it first: "sketch the ownership graph as you understand it, then I'll
show you mine" produces recall evidence and exposes the misconception in one step. A
picture you looked at produces neither.

It works in three tiers, cheapest first:

| Tier | When | Permission |
| --- | --- | --- |
| **Inline text diagrams** | Default. Structure, sequence, state, memory layout, small trees | None needed |
| **Mermaid** | Genuinely graph-shaped and worth keeping | Written to a file, never raw in chat |
| **HTML file** | Must be interactive, steppable, or kept as reference | Asks first |

Mermaid never appears inline, for a boring practical reason: a ` ```mermaid ` fence does not
render in a terminal. You would be reading source code. Box-drawing characters render
everywhere, so that is the default.

HTML artifacts land in the track's own directories — `lessons/`, `exercises/`,
`assessments/`, `reviews/` — numbered in the order they were made, and linked from whatever
file they belong to. Each one is a single self-contained page: no CDN, no network request,
no build step, legible in light and dark, and printable.

### How a quiz page gets its answers back

A page in your browser cannot write to `.learning/`, so an interactive quiz that just scores
you is a dead end — Professor never sees it. Instead the page ends with a copyable block:

```text
professor-result v1
track: rust-ownership
artifact: assessments/0003-borrow-checker-quiz.html
date: 2026-09-19

Q1 borrow-vs-move [choice] answer=B expected=B
   because "s1 was moved, so the later read has nothing to read"
Q2 lifetime-scope [written] "the borrow ends at its last use, not at the end of the block"
Q3 elision-rules  [choice] answer=C expected=A
   because "I guessed"
looked-up: Q3
```

Paste it back and Professor grades the written answers itself — the page never does, because
a JavaScript string comparison is not comprehension and will be confidently wrong about
prose. `[written]` is logged as recall, `[choice]` as recognition, and anything you looked up
is logged as assisted. Reporting your results in your own words works just as well; the block
is a convenience, not a requirement.

**Artifacts are not evidence.** A cheat sheet you read, an animation you watched, a lesson
page you opened — all `INTRODUCED` at most, exactly like an explanation, no matter how much
work went into making it.

## Knowledge states

Every concept in a track sits in exactly one state:

| State | Meaning |
| --- | --- |
| `UNKNOWN` | Not meaningfully encountered yet |
| `INTRODUCED` | Explained to you or observed — but not yet used by you |
| `PRACTICING` | You are attempting or applying it, with support |
| `DEMONSTRATED` | You used or explained it correctly without substantial help |
| `RETAINED` | You did it again later, or in a meaningfully different context |
| `NEEDS_REVIEW` | Previously solid, but recent recall or application was weak |

The usual path is `UNKNOWN → INTRODUCED → PRACTICING → DEMONSTRATED → RETAINED`, but
**evidence drives the transition, not sequence**. Anything can fall back to `NEEDS_REVIEW`,
and recovering returns it to `DEMONSTRATED` or `RETAINED` depending on what you showed.

### What counts as evidence

**Counts:** code you wrote that works, an accurate explanation in your own words, a bug you
diagnosed, an exercise you solved, a real quiz answer, applying it somewhere new.

**Does not count:** "I understand." A solution Professor wrote. Code you pasted from
somewhere. Having had it explained to you, however clearly.

Each entry records enough to audit the judgment later:

```markdown
### Transactions
State: DEMONSTRATED
Last evidence: 2026-09-19
Assistance: Hint level 1
Evidence:
- Explained atomicity and fixed a transaction boundary independently. (recall)
Review after: 2026-09-26
```

Recurring misconceptions are tracked separately — the wrong model, the corrective one, the
evidence, and whether it is still `Active`.

## Where your progress lives

Everything lives in `.learning/` at the root of whatever project you are learning in. Plain
Markdown. Read it, edit it, commit it, or gitignore it — it is yours, and Professor
preserves anything you write there by hand.

```text
.learning/
├── PROFILE.md              how you like to be taught — pace, examples, constraints
├── TRACKS.md               your tracks, their status, and which one is active
└── tracks/<track-slug>/
    ├── GOAL.md             what you want to be able to do, and what "done" looks like
    ├── ROADMAP.md          Now / Next / Later / Possible
    ├── KNOWLEDGE.md        every concept, its state, its evidence, its review date
    ├── PROGRESS.md         dated session log — work, evidence, struggles, next action
    ├── RESOURCES.md        vetted sources and when each is useful
    ├── QUESTIONS.md        your open questions, which become future lessons
    ├── PROJECT.md          the thing you are building, if there is one
    ├── lessons/
    ├── exercises/
    ├── assessments/
    └── reviews/
```

Subdirectories appear only when there is something to put in them, and trivial exchanges do
not get their own file.

Two rules Professor holds itself to: it writes **only** inside `.learning/`, never your
project files; and it never claims state was saved unless the files were really written.

**Should you commit it?** If you are learning in a real repo and want the history, yes —
`KNOWLEDGE.md` diffs are a genuinely nice record of getting better at something. If it is a
scratch directory, or the repo is shared with people who do not need your learning log, add
`.learning/` to `.gitignore`. This repo ignores its own.

## What it will not do

Unless you explicitly ask, Professor will not generate whole features for you, rewrite your
solution, quietly fix what you submitted, or scaffold a large project. It prefers a pointed
question, a hint, a diagram, pseudocode, a tiny isolated example, a documentation pointer,
or a critique.

It also holds to one to three issues at a time, because a twelve-point review of your first
attempt teaches nothing, and it teaches a technology when it solves a problem you actually
have rather than because it is fashionable.

**It is not a refusal machine.** Ask for the answer and you get the answer. The only thing
Professor insists on is being honest in the record about how you got it.

If you just want the code, don't use Professor — ask your agent normally. It is for the
times you want to end up able to do it yourself.

## Working on more than one thing

Tracks are independent. `TRACKS.md` indexes them and marks one active, so `continue` knows
where to go. Name a track directly to switch to it, and in Claude Code you will get a picker
when it is ambiguous.

Knowledge and progress stay separate per track, but relevant prior evidence gets reused
explicitly — if you demonstrated recursion in one track, Professor knows that rather than
teaching it at you again from scratch.

## Finishing a track

A track is ready to close when the required concepts are at least `DEMONSTRATED`, the core
ones show real retention, you have completed a capstone or equivalent, and you can explain
your key decisions unprompted.

You get a completion summary: what you demonstrated, your strongest areas, what is still
weak, and reasonable next topics. It will not claim mastery the evidence does not support —
that honesty is the entire point of keeping the record.

## Questions

**Do I need to use the commands?** No. Plain English routes the same way.

**What if it misjudges my level?** Tell it. Explicit preferences go into `PROFILE.md`
immediately. You can also edit `KNOWLEDGE.md` yourself.

**Can I reset one topic without losing everything?** `reset-topic <topic>`. It confirms
scope first, resets that topic's state, and records the reset in your history. History is
deleted only if you explicitly ask.

**Does it work offline?** Yes, apart from whatever your agent itself needs. Professor adds
no network calls. `resources` will suggest sources to go read, which you will want a
connection for.

**Can I move my progress between machines?** Copy the `.learning/` directory, or commit it.
There is no hidden state anywhere else.

**Why is it not teaching me the thing I asked about?** Probably because it does not serve
your stated goal yet. It is in `Later` or `Possible` in `ROADMAP.md`, with a reason. Say you
want it now and it moves.

**Claude Code or Codex — any difference?** Same teaching, same files, same standards. Claude
Code can show real choice prompts for session decisions; Codex asks the same questions in
prose. Nothing about what gets taught or how evidence is judged changes.

## Repository layout

```text
professor/
├── skills/professor/
│   ├── SKILL.md                        loaded every run — routing, the loop, the boundaries
│   ├── references/
│   │   ├── state-management.md         read before anything touches .learning/
│   │   ├── teaching-protocols.md       modes, hint ladder, reviews, assessments
│   │   └── visuals.md                  when to draw, and the artifact standards
│   └── agents/openai.yaml              Codex display metadata
├── assets/
├── install                             links the skill into Claude Code and Codex
├── AGENTS.md                           standards for working on this repo
└── CLAUDE.md                           pointer to AGENTS.md, so the two cannot drift
```

If you are changing Professor, read [AGENTS.md](./AGENTS.md) first. The short version:
`SKILL.md` stays short and behavioral, detail goes in `references/`, one copy serves both
agents, and evidence decides state.
