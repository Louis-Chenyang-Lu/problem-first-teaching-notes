---
name: problem-first-teaching-notes
description: Write student-ready teaching or lecture notes that build a concept, definition, method or theorem from the problem that motivates it, instead of stating a definition and proving properties. Use when the user asks for teaching notes, lecture notes, a handout, or to teach, present or explain a term or topic to students or an audience.
---

# Problem-first teaching notes

Write a note that a student can read on their own and that teaches a concept the way someone could have *discovered* it: start from a real problem, look at what we concretely have, try the obvious idea, refine it, and only then let the formal definition appear as the natural answer.

The pattern to avoid is the textbook pattern: *"X is defined as … . Assume … . Then by integrating we get … ."* A definition stated before the reader knows why they need it is a fact to memorise. The goal is that a student finishes the note thinking *"I could have come up with this myself."*

The person asking is usually an instructor, presenter or tutor who will hand the note to learners. It can also be a learner who wants to be taught; the note is the same.

## Before writing

1. **Pin down the topic and audience.** Note the concept, the course or field, and the prerequisites the readers have. If the level would change the note substantially and cannot be inferred, ask once; otherwise assume readers who know the prerequisites but have never seen this concept, and state that assumption in the reply.
2. **Find the motivating problem.** Ask: what question was this concept invented to answer, or what does it let us do that we could not do before? Choose one concrete, recognisable setting to use as the **running example** for the whole note.
3. **Find the naive attempt.** What would a smart student try first, and exactly why does it fail or fall short? Prepare a concrete counterexample or a tiny numeric case that shows the failure.
4. **Verify the mathematics.** Derive the key steps yourself. If you can run code (for example a short Python script), compute every number in the worked example (eigenvalues, integrals, probabilities) instead of doing it mentally; do not leave the script in the user's workspace unless asked.

## The story arc

Every note follows this arc. Each section answers the question left open by the previous one. Headings are phrased as questions or as the step in the story, never as topic labels such as "Definition" or "Properties".

1. **Where does this problem live?** Two to four concrete, recognisable instances of the setting. Make the reader see what they have in common.
2. **What do we want to know?** State the goal as a plain-language question first, then sharpen it until it is precise enough to compute.
3. **What do we actually have?** The concrete ingredients: data, objects and their notation. Introduce each symbol here, at the moment it is needed, with what it means in the running example. Set up the running example with real numbers.
4. **What would we try first?** Write down the naive idea. Show concretely *why* it fails or is incomplete (a counterexample, a small numeric case, a unit mismatch). Extract the lesson that points to the fix.
5. **How do we build something better?** Make each modelling choice explicit as a *choice we make*, with its reason. Assumptions are decisions with justifications, never decrees.
6. **The formal object appears.** Show the algebra that turns the model into the standard form. *Only now* give the object its standard name. The name labels something the reader already understands.
7. **What can we do with it?** Derive the key result, simplest case first (one variable before many, two nodes before n). Show the one or two algebra steps that carry the idea; condense routine manipulation. Explain *why* the result holds, not only that it does.
8. **A worked example with real numbers.** Small enough to check by hand, using the running example. Compare the result with a hand guess or with intuition so the reader sees what the method buys.
9. **What does this buy us, and where does it stop?** What the concept lets us do; what it does *not* tell us; which assumption breaks first in real data; common misreadings.
10. **The chain in one breath.** A 4–6 line recap that replays the reasoning as a chain: problem → what we had → naive idea → better model → named object → result.
11. **Check your understanding** (optional). Two to four questions that test the reasoning, not recall, with brief answers at the end.

Not every topic needs all eleven sections; merge or skip as the topic requires, but keep the order: need → ingredients → naive idea → model → name → result → example → limits → recap.

## Writing rules

- **Never open with a definition.** If the first paragraph contains "is defined as", "Let X be" or "Assume", rewrite it.
- **Say why before each step.** Before each move, one sentence says what we are trying to achieve with it ("we want a quantity that measures change over time; that is exactly what a derivative does").
- **Concrete before abstract.** Tie each symbol to the running example the first time it appears. Keep one running example through the whole note.
- **Answer the natural objection.** Raise the question a student would ask ("why not just use the simpler version?") and answer it with algebra or numbers.
- **Use analogies only when they carry the logic,** and say where they break.
- **Keep the mathematics visible but light.** Display the key equations in LaTeX; condense routine steps. A reader with the stated prerequisites should be able to follow every line.
- **Be accurate.** The story is pedagogical, but the mathematics must be correct and the terminology and notation must match standard textbooks. If the request contains an inaccuracy, correct it in the note and mention the correction in the reply.
- **Be honest about history.** Present the reasoning as "how one could arrive at this", not as invented history, unless the historical route is actually known.
- **Use the reader's language.** Write in the language the user asks for; default to the language of the request.

## Slide-ready structure

Open each section with a **one-sentence takeaway in bold**, written as an assertion ("**Heat flows along an edge in proportion to the difference across it.**"), not a topic label. Keep each section to one idea. This makes the note easy to present, and each section can later become one slide whose title is the takeaway.

## Deliverable

A standalone, polished note that can be handed to learners directly:

- Title = the concept; a one-line subtitle stating the problem it solves.
- No commentary addressed to the instructor inside the note (no "Here is your note", no explanation of the teaching method).
- Save it as a Markdown file with LaTeX math (`$…$` inline, `$$…$$` display) in the current workspace, for example `notes/<concept-slug>.md`, unless the user names another path. Use a specific format (.tex, .docx, .pdf) when the user names one and the tools for it are available.
- In the reply, give one or two lines: what the note covers, the assumed audience level, and any correction made.

## Self-check before delivering

- [ ] Does each heading answer the question left open by the previous section?
- [ ] Is the formal name introduced only after the reader has seen why it is needed?
- [ ] Is every modelling assumption stated as a choice with a reason?
- [ ] Does the naive attempt fail concretely, with numbers or a counterexample?
- [ ] Is there one running example and a worked numeric case that can be checked by hand?
- [ ] Are all equations and numbers correct (re-derived, and computed with code where possible)?
- [ ] Does the recap replay the reasoning chain rather than list facts?

## Example: bad and good openings

**Bad:** "The heat kernel on a graph is defined by dx/dt = −Lx, where L is the graph Laplacian. We assume heat travels according to −L. Integrating gives x(t) = e^{−tL} x(0)."

**Good (outline):**

1. *Networks are everywhere:* the internet, road networks between cities, social networks, brain regions linked by nerve fibres. All are nodes plus weighted connections.
2. *What we want:* probe the network by watching how something placed on it evolves. If we put heat on some nodes, where is it at time t?
3. *What we have:* a weight matrix W (w_ij = strength of the connection between i and j) and a value x_i(t) on each node. We know x(0) and want x(t).
4. *How do we describe change?* Change over time is what the derivative dx_i/dt measures, so we need a rule for what drives it.
5. *Choosing the rule:* heat should flow from hot to cold, driven by the difference x_j − x_i, and more along stronger connections, scaled by w_ij. Summing over neighbours: dx_i/dt = Σ_j w_ij (x_j − x_i).
6. *The matrix appears:* with degree d_i = Σ_j w_ij this is dx/dt = −Lx, where L = D − W. Only now: "L is called the graph Laplacian." Contrast with dx/dt = Wx: a node would gain heat from neighbours without losing any, so total heat would not be conserved.
7. *Solving it:* one variable first, y′ = −ay gives y(t) = e^{−at} y(0); the same logic gives x(t) = e^{−tL} x(0). The operator e^{−tL} is the **heat kernel**: it maps the starting state to the state at time t.
8. *Two-node check:* w_12 = 1, x(0) = (1, 0) gives x(t) = ½(1 + e^{−2t}, 1 − e^{−2t}) → (½, ½). Heat evens out and the total stays 1.
9. *Limits:* this models linear, symmetric, conservative diffusion; real spreading processes need not behave this way.
10. *Chain:* network → values on nodes → model change with a derivative → flow ∝ difference × weight → the Laplacian falls out → the heat kernel solves it.

A complete note written with this skill is in [references/example-graph-laplacian.md](references/example-graph-laplacian.md). Read it when you need a model of tone, length and section structure.
