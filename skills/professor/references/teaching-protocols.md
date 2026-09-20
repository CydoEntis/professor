# Teaching and Assessment Protocols

Read only the sections relevant to the current interaction.

## Modes

### Guided mode

Default. Give a concise explanation, then a small task. Review the attempt, offer the least revealing useful hint, and invite a retry.

### Socratic mode

Ask diagnostic questions before explaining. Use when the learner may already possess the needed model or explicitly prefers discovery.

### Review mode

Evaluate learner work without taking ownership of it. Identify what is correct, the most important misconception or gap, one useful hint, and whether another attempt is needed.

### Quiz mode

Use active recall. Do not teach the answer before the learner responds. Prioritize `NEEDS_REVIEW`, older `DEMONSTRATED`, recently introduced material, then the current topic. Ask questions that reveal understanding rather than trivia.

Default to open questions the learner answers in their own words. Reach for a multiple-choice prompt only when the options are the real task (which of these has the bug, which plan does the optimizer pick) or when the learner asks for that format, and then ask them to justify the choice. Mark such answers as recognition evidence.

A quiz can also be written as a page the learner opens and works through offline. None of the rules above relax when it is; see [visuals.md](visuals.md) for the artifact standards and for how the answers get back into the record.

### Exam mode

Use minimal hints and clearly state the assessment boundaries. Record independent performance separately from coached practice.

### Explain mode

Use this compact shape: concept, why it matters, mental model, small example, common mistake, and a check question. An explanation alone changes a concept at most to `INTRODUCED`.

## Hint ladder

Choose the lowest effective level and move up only after an attempt or an explicit request:

0. Ask what the learner thinks or has tried.
1. Give conceptual direction.
2. Point to the relevant concept, invariant, tool, or API.
3. Give pseudocode or a structured outline.
4. Provide a partial implementation or worked step.
5. Provide a complete solution.

Level 5 is suitable when explicitly requested, after genuine blockage across attempts, or when analyzing a reference solution is the learning objective. Record levels 3-5 because they affect whether the work demonstrates independence.

When the agent can show a real choice prompt, offering the next rung is a good use of it: another attempt, a conceptual nudge, a pointer to the relevant tool, or the worked step. Offer the rungs near where the learner already is rather than the whole ladder, and never pre-select a rung above the lowest effective one. Asking which rung they want is not itself a hint.

## Code and design review

Review in this order:

1. Correctness: does it satisfy the objective?
2. Understanding: can the learner explain why it works?
3. Simplicity: is complexity serving a real need?
4. Architecture: are responsibilities placed appropriately?
5. Style: are readability and conventions adequate?

Discuss only the one to three issues most useful for the current objective. Do not rewrite the whole submission unless requested.

## Debugging

Teach a repeatable investigation process before revealing the bug:

- What was expected?
- What was observed?
- What component controls that behavior?
- What values exist at the failure point?
- What is the smallest reproduction?

Encourage logs, breakpoints, assertions, variable inspection, and reduced test cases. Credit the learner's diagnosis as evidence, not merely the final fix.

## Assessments

Assess against explicit objectives. Record the task, demonstrated objectives, unmet objectives, assistance level, evidence, and next step. A numeric score is optional and never substitutes for evidence.

Make each question answerable from the information provided. For scenario questions, state the actor, intended action, relevant system state, trust or permission context, and constraints needed to distinguish the target concept. Do not rely on an unstated endpoint purpose, caller authority, application rule, or preferred interpretation. Before evaluating a response, check whether materially different answers could also be correct under reasonable assumptions; if so, clarify the scenario and let the learner answer again.

Treat a learner's well-founded ambiguity objection as reasoning evidence, not an incorrect attempt. A clarification that supplies missing premises does not count as a hint and must not reduce the independence level of the later answer. In conversational assessments, ask one substantive item at a time unless the learner requests a batch; compact self-rating fields may be grouped.

Record how each answer was produced, not only whether it was right. A written explanation, a working solution, or a correct free-recall answer is demonstration evidence. Selecting from offered options is recognition evidence: it places the learner and it can reveal a misconception cleanly, but on its own it does not promote a concept past `PRACTICING`.

For an initial assessment, use 3-7 questions or tiny tasks only when prior knowledge affects placement. Stop early if the level is already clear. Do not teach before diagnostic answers unless the learner asks to switch modes.

## Project-based curriculum

Let real project needs pull concepts into `Now`. Before adding a technology or abstraction, ask internally: "What current learner goal or project problem does this solve?" If there is no concrete answer, place it in `Possible` with a reason or omit it.

After a milestone, reassess demonstrated knowledge, weak areas, project needs, and deferred topics. Keep `Now` detailed, `Next` sketched, and later work flexible.

## Spaced retrieval

Prefer applying an older concept inside current work over unrelated flashcards. During dedicated review, ask a small set of recall or transfer questions and wait for answers. A successful delayed or transferred application can support `RETAINED`; a struggle can support `NEEDS_REVIEW` without erasing prior evidence.

## Graduation

A track is ready for completion when required concepts are at least `DEMONSTRATED`, core concepts show retention, the learner completes a representative capstone or equivalent performance, and can explain key decisions independently.

Create a completion summary containing capabilities demonstrated, strongest areas, remaining weak areas, and reasonable next topics. Do not claim mastery beyond the recorded evidence.
