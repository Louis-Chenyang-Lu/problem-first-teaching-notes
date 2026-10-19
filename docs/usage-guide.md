# Usage guide

## When to use it

Use this skill whenever you need to *teach* a concept rather than merely state it:

- lecture notes or handouts for a course;
- a reading for a reading group, lab onboarding or a tutorial;
- an explanation you want students to be able to read without you in the room;
- rewriting an existing definition-first note into a motivation-first one.

It works best for concepts that were invented to solve a problem: definitions, estimators, transforms, algorithms, theorems, models. It is less useful for pure reference material (lists of formulas, API documentation) where readers already know why they need it.

## How to prompt

Give three things; the first is required, the other two improve the note a lot:

1. **The concept** ("the graph Laplacian", "maximum likelihood estimation", "the p-value").
2. **The audience and prerequisites** ("second-year statistics students who know calculus and basic probability"). Without this, the skill assumes readers who know the usual prerequisites but have never seen the concept, and says so.
3. **A running example you like** (optional). Otherwise the skill chooses one.

See [the prompt templates](../examples/prompts.md).

## What the note looks like

Each note follows the same arc, with headings written as questions:

| Stage | What the reader gets |
|---|---|
| Where does this problem live? | Two to four concrete settings that share a structure |
| What do we want to know? | The goal in plain words, then made precise |
| What do we actually have? | Data, notation and the running example, introduced only when needed |
| What would we try first? | The naive idea, and a concrete reason it fails |
| How do we build something better? | Each modelling choice with its justification |
| The formal object appears | The standard definition and name, as the answer |
| What can we do with it? | The key result, simplest case first, with the reason it holds |
| Worked example | Numbers small enough to check by hand |
| Where does it stop? | What the concept does not tell you, common misreadings |
| The chain in one breath | A short recap of the reasoning |
| Check your understanding | Reasoning questions with answers (optional) |

Every section opens with a bold one-sentence takeaway, so the note is easy to present from and easy to skim.

A full example is in [`skills/claude/problem-first-teaching-notes/references/example-graph-laplacian.md`](../skills/claude/problem-first-teaching-notes/references/example-graph-laplacian.md).

## Output format

- **Claude:** a document you can edit and share when your Claude app offers one, otherwise a Markdown file with LaTeX math.
- **Codex:** a Markdown file with LaTeX math in your workspace (for example `notes/<concept>.md`).
- Ask for LaTeX, Word or PDF explicitly if you need it.

## Follow-ups that work well

- "Section 5 is too fast for my students; add an intermediate step."
- "Use <another setting> as the running example instead."
- "Shorten it to a two-page handout, keeping the naive-idea section."
- "Add three more check-your-understanding questions on <topic>."

## FAQ

**Is the "story" historically accurate?** Not necessarily. The skill presents a route by which one *could* arrive at the concept and does not invent history. Ask for the actual history if you want it.

**Will the mathematics be correct?** The skill derives every step and, when the agent can run code, computes the worked-example numbers. You should still review the note before handing it out, as with any teaching material.

**Can it make slides?** Not yet. The note is structured so that each section can become one slide (its bold takeaway is the slide title); a slides mode is planned.
