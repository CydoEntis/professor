# Working on Professor

Professor is a tutor that runs in both Claude Code and Codex. It is a skill — Markdown
instructions and nothing else. There is no program here, and there should not be one.

## The shape

- **`skills/professor/SKILL.md` is the product.** It is loaded every time the skill runs, so
  it stays short and stays about behavior. Routing, state rules in outline, the teaching
  loop, the assistance boundary.
- **`references/` holds what is read on demand.** `state-management.md` for anything that
  touches `.learning/`; `teaching-protocols.md` for the modes, the hint ladder, reviews and
  assessments. Detail belongs here, not in `SKILL.md`.
- **One copy, two agents.** The same files serve Claude Code and Codex. Never write an
  instruction that only one of them can follow without saying what the other does instead.
- **`install` links, never copies.** A checkout is the live source for both agents.

## The rules that matter

**Evidence decides state.** A concept is promoted because the learner produced something,
not because we explained it, and not because they said they understood. Any change that
would let a concept advance on the strength of our own output is wrong.

**Recall outranks recognition.** Writing the answer is evidence; picking it off a list is
weaker, and gets recorded as such. When adding an interactive prompt anywhere, check which
one it is creating.

**The learner attempts before we help.** The hint ladder starts at the bottom. Full
solutions are legitimate — when asked for, when truly stuck, when the reference *is* the
lesson — and are always logged as assisted.

**We stop and wait.** If the learner has been handed an exercise, the turn ends. Do not run
through the lesson on their behalf.

**Their files are theirs.** Write only inside `.learning/`, preserve anything a human wrote
there, prefer focused edits to regenerating a file, and never claim state was written when
it was not. Resetting a topic resets state; it does not delete history unless asked.

## Changing it

Keep `SKILL.md` roughly its current length. If a section wants to grow, it probably belongs
in a reference file. Before adding a verb, a file, or a state, ask what learner situation is
badly served without it — if there is no concrete answer, leave it out.

Test by teaching something real in a throwaway directory, in both agents, and read the
`.learning/` it produces. A track that looks right in conversation and wrong on disk is
wrong.
