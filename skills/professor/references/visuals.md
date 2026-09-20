# Visuals and Artifacts

Read before drawing a diagram or writing any file for the learner to open.

## What a visual is for

A diagram earns its place when it shows a **mechanism**: what points at what, what happens
in what order, what sits where in memory, which states a thing can be in, where a value
crosses a boundary. It does not earn its place by restating a list, decorating a
definition, or making an explanation look thorough.

Before drawing one, ask whether the learner should draw it instead. "Sketch the ownership
graph as you understand it, then I will show you mine" turns a picture they would have
skimmed into recall evidence, and surfaces the misconception in one move. Their sketch is
evidence. Yours is not.

## Three tiers

Use the cheapest tier that does the job.

### Inline text diagrams

The default. Box-drawing characters and arrows render in every terminal, need nothing
opened, and cost no context switch. Use them for structure, sequence, state, memory layout,
ownership, and small trees.

Keep them under about 60 columns so they survive a narrow pane, label the parts with the
names the learner is actually using in their code, and put the point of the diagram where
the eye lands first.

```text
     stack                 heap
  ┌──────────┐        ┌──────────────┐
  │ s1  ─────┼───────▶│ "hello"      │
  ├──────────┤        └──────────────┘
  │ s2  ─────┼───────▶ (moved from s1)
  └──────────┘          s1 is now invalid
```

Two or three boxes beat a perfect rendering. Do not spend the learner's attention on
drawing craft.

### Mermaid

A fenced `mermaid` block **does not render in a terminal** — the learner sees source code.
Emit mermaid only into a file something will actually draw: an HTML artifact, or a Markdown
file they will open in an IDE preview or on a forge. Tell them where to look at it.

Use it when the shape is genuinely a graph — many nodes, real edges, branching flow, a
state machine with more than a few transitions — and the picture is worth keeping. For
three boxes and an arrow, text is faster and needs no viewer.

### HTML artifacts

Write a file when the thing must be **interactive** (a quiz, a drill), **steppable** (an
algorithm run, a protocol exchange), or **kept** (a reference sheet the learner will return
to). Never for an ordinary explanation — that is what the conversation is for.

Ask before writing one. A file lands in the learner's project, and an unrequested file is
clutter. An inline diagram needs no permission.

## Where artifacts go

Inside the active track, in the directory that matches what the file is:

```text
.learning/tracks/<track-slug>/
├── lessons/       0001-borrowing-and-moves.html
├── exercises/     0002-fix-the-lifetime.html
├── assessments/   0003-borrow-checker-quiz.html
└── reviews/       0004-week-two-recall.html
```

Number them `0001-<dash-case-name>.html`, incrementing across the track so the order is the
order they were made. Never write outside `.learning/`.

Link every artifact from the file it belongs to — a lesson from `ROADMAP.md` or
`PROGRESS.md`, an assessment from the knowledge entry it produced evidence for — so it can
be found again without listing directories.

## Artifact standards

- **One self-contained file.** No CDN, no network request, no build step. Professor works
  offline and its output must too.
- **Inline CSS and JS**, kept small. If it wants a framework, the lesson is too big.
- **Legible in light and dark** via `prefers-color-scheme`, and it prints cleanly — a
  reference sheet that cannot be printed is half a reference sheet.
- **Real form controls**, labelled, reachable by keyboard.
- **A header stating the track, the objective it serves, and the date**, so the file still
  makes sense a month later.
- **Plain, quiet typography.** The learner came to learn, not to admire a page.

## Interactive quizzes

The rules from `teaching-protocols.md` do not relax because the questions are in a file.

- **Written answers by default.** Offer choices only where the options genuinely are the
  task — which of these four has the race, which index is chosen, safe or unsafe.
- **The page never grades a written answer.** It captures the text verbatim and returns it.
  A JavaScript string comparison is not comprehension, and a page that marks prose right or
  wrong will be confidently wrong.
- **Do not reveal the answer before capture.** For choice questions the page may explain
  after submission; it records the first answer regardless.
- **Ask for reasoning on every choice question**, in a free-text box next to it. That
  sentence is the part worth grading.

### The result block

A page cannot write to `.learning/`. Evidence gets back by the learner pasting a block the
page renders when they finish, with a copy button:

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

When one is pasted back:

1. Grade the written answers and the reasoning yourself. That is the real assessment.
2. Record `[written]` as `(recall)` and `[choice]` as `(recognition)`.
3. Apply the usual promotion rules — recognition alone still stops at `PRACTICING`.
4. Treat anything under `looked-up:` as assisted, and say so in the evidence line.
5. Update the knowledge entries and `PROGRESS.md`, then say what moved.

If the learner reports a result in their own words instead of pasting the block, take it —
the block is a convenience, not a requirement.

## Artifacts are not evidence

Producing one is teaching. A cheat sheet they read, a diagram they looked at, an animation
they watched, a lesson page they opened — all of it moves a concept to `INTRODUCED` at
most, exactly like an explanation, no matter how much work the artifact took to make.

Evidence is what comes back from the learner: a sketch they drew, an answer they wrote, a
change that works, a result block from a quiz they actually sat.
