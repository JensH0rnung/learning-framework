# Phase 3 — Teach

Walk the DAG, **one node per message**. This is the whole phase. The discipline of not
rushing is more important than any individual explanation.

## The step contract

Every node, in this order. Each part has a fixed callout in the log — the table in
[FORMATS.md](./FORMATS.md#callout-vocabulary) is authoritative; the reminders below repeat it
inline so the mapping is never guessed.

### 1. Tension — `> [!failure] Tension`

Name what the current frontier **cannot do**. This is what makes the next object necessary
rather than arbitrary (doctrine rule 2).

> We have something that eats one vector and returns a number. But a surface has two
> directions, and nothing we have can take two vectors at once.

No tension, no step. If you cannot state one, the node is misplaced in the DAG — say so and
fix the plan rather than teaching an unmotivated object.

### 2. Motivated move — prose, **not** boxed

Why would anyone try *this particular* thing? Make the move look like the obvious thing to
reach for, not a rabbit produced from a hat.

> So try the cheapest possible extension: a machine that eats two vectors and is linear in
> each. That is forced — anything less is not an extension, anything more is a new idea we
> have not earned yet.

This part stays unboxed in the log. It is the argument, and it must read as continuous
reasoning rather than another labelled box.

### 3. The object — `> [!abstract] Definition`

State it as a **real definition** — something that determines the object. Never a list of
properties dressed up as a definition (doctrine rule 1). If the honest form is "the unique
thing satisfying X", say that.

Notation gets introduced here, explicitly flagged as notation.

### 4. Anchor — `> [!important] Anchor`

One line, safe to accept at face value, universal in form where possible. This is what the
user will still hold in a month.

> A `k`-form is the kind of thing a `k`-dimensional surface can eat.
> All integration over a 1-dimensional thing is integration of a 1-form.

Exactly one anchor per node. Two anchors means two nodes.

### 5. Reframe — `> [!note] Reframe`

If this node recasts something they already hold, **say so explicitly**:

> So `dx` was never an infinitesimal. It is a covector — the machine that reads off the
> `x`-component of whatever vector you feed it. Everything you already know about line
> integrals still stands; it just now says something different.

Reframes are the cheapest learning available — existing structure relabelled, not new
structure built. Never let one pass silently.

### 6. Visual — `![[…]]`, or `> [!warning] visual pending`

When the content is genuinely **geometric** — something with shape, orientation, direction,
area, or a picture that carries information a sentence cannot — spawn the `svg-illustrator`
subagent.

**Non-blocking.** Never wait on it to continue teaching.

- Give it: the concept, the anchor, what must be visible, and the exact output path
  `<domain>/assets/<nn>-<slug>.svg`.
- Embed it in the log as `![[<nn>-<slug>.svg]]` when it returns.
- **If it fails or dies:** write `> [!warning] visual pending — <concept>` into the log,
  continue text-only, retry once at the next checkpoint. Teaching never stalls on a subagent.

Do not illustrate the non-geometric. A diagram of an abstract definition is decoration and
costs attention.

### 7. Quiz gate — `> [!question] Quiz`, then `> [!success]` / `> [!failure]`

**Free-text only. Never `AskUserQuestion` in this phase.** Ask one or two open questions in
chat, on **this step only**, and let the user answer in their own words.

Multiple choice is banned here, and the reason is doctrine rule 3. Options let a learner
recognise an answer they could not have generated — Brain A passes, and the gate measures
nothing. An accidental click reads as a pass too. Prose forces the structure to be produced,
and a shaky explanation of a *correct* answer is the most useful signal available: it names
the missing edge exactly.

Make the question require the step to be *used*, not recited. Compute something. Predict what
breaks. Apply it to a case not shown. Push one step past what was taught — a transfer question
("what if each wire had three states instead of two?") catches a memorised rule that re-running
the taught example never will.

Grade the **reasoning**, not the answer. A right answer with absent or wrong reasoning is a
fail: say so plainly and re-teach the reason. When the user reports that the answer is right
but the why has not clicked, that is a missing edge, and finding it is the highest-value work
available in the session.

- **Correct** → confirm in one line, advance.
- **Wrong** → do **not** advance. The explanation route failed, not the learner. Re-teach by a
  *different* route: different motivation, different concrete case, add a visual, drop half a
  level. Record the failed route in the log so it is not repeated.
- **Wrong twice** → the node is too big or the edge is lower than the probe found. Split the
  node or back up the DAG. Say which you are doing.
- **"I don't know"** → treat as wrong, but say the step was under-taught, not that they
  failed.

Log the grade in a `> [!success] Quiz — correct` or `> [!failure] Quiz — missed` callout
carrying the user's answer *and* the reasoning verdict. The verdict is the part worth
rereading; the answer alone says nothing.

Never advance past an unverified step. This is the gate that makes the whole system
trustworthy — it is very easy to feel that something was understood when it was not,
especially when learning with an AI.

## How a rung reads

A rung that is correct but unreadable teaches nothing. Two failure modes have been observed
in real use, both of them defects in the step, not in the learner:

**Fließtext.** The rung arrives as one continuous block of prose. The learner cannot see
where the tension ends and the definition begins, so they read it as narration and nothing
seats. Budgets, per rung:

| Part | Budget |
|---|---|
| Tension | 2 sentences |
| Motivated move | ~60 words, 4 short sentences |
| Definition | 1 sentence |
| Anchor | 1 line |
| Reframe | 2 sentences |
| Whole rung, before the quiz | ~150 words |

**These are diagnostics, not gags.** Over budget does not mean *write less* — it means the
rung is carrying two reasoning steps, so **split the node**. Cutting a motivated derivation
down to fit a word count drops the motivation, which trades a doctrine rule 2 violation for a
style win. That is the wrong trade, and it is the exact failure this skill exists to prevent.

So when a rung runs long, in order: split it into two nodes; or move a part into its callout
where the structure carries the length; or, if it genuinely is one indivisible step that needs
the words, **exceed the budget and say why in the log**. Never compress the reason away.

No paragraph longer than four lines, in chat or in the log. If a paragraph outgrows that, it
is doing two jobs — split it, or move half of it into its callout.

**Jargon-first language.** Rule: **plain words before names.** Say what the thing does in
everyday language, *then* attach its name.

> A machine that eats a vector and hands back a single number. That is what "covector"
> means.

Never the reverse — a name introduced before its meaning forces the learner to hold an empty
label, and an empty label is the most expensive thing they can carry (doctrine rule 1).

- **One new term per rung.** Two names means two rungs.
- **Analogy before formalism** when the field is new to this learner. The formal statement
  still follows in the same rung; it just does not go first.
- **Every symbol said out loud** the first time it appears: "$\partial_\mu$ — read: the rate
  of change along direction $\mu$."
- **Short sentences.** A sentence that needs a chain of commas to stand up should be two
  sentences.
- **Plain over idiomatic.** The learner may be reading in a second language; a literal phrase
  beats a colourful one.
- **Vocabulary check** before sending: any word that would not survive a conversation with a
  sharp person from outside the field gets replaced, or defined in the same breath.

Reread the rung before sending it. If it looks like a wall, it is one — fix it rather than
hoping.

## Pacing rules

- **One node per message.** No exceptions, including when the next node feels trivial.
- **Never look ahead.** No "we will see later that…" teasers that spend attention on
  something not yet earned.
- **Never batch quizzes.** Gate each step.
- **Answer interruptions fully, then resume the same node.** A question does not signal
  readiness to move on.
- Write each step to the live log as it is produced, in the callouts above — the user reads
  Obsidian, not the terminal. Cap it at ~3 callouts per rung: a note where every block is
  boxed reads as a wall of boxes, and then the tension, the anchor and the quiz stop standing
  out.

## Compression checkpoint

At each strand's end, or roughly every four nodes, stop walking and harvest (doctrine rule 4).

Ask the user, in chat:

> Before we go on — what does all of that collapse into? Which of these do you actually have
> to remember, and which now follow from the others?

Let them answer first. Their answer *is* the measurement of whether structure formed. Then
give your own compression and reconcile the two — where they differ is exactly where an edge
is missing.

Then:

1. **Write/update the reference note** (`reference/<concept>.md`, schema in
   [FORMATS.md](./FORMATS.md)): generators, motivation-labelled edges, anchors, sources. It
   **must be shorter than the log**. If it is not, no compression happened — find the
   generators again.
2. **Update `_map.md`**: move nodes to locked, extend the `## Structure` graph with the edges
   actually taught.
3. **Route conventions.** Every `convention` node goes to `<VAULT_ROOT>/Anki Drafts/` in the format
   the `anki-flashcards` skill expects. Also nudge the user to type `[card]` on anything that
   clicked hard — that skill only harvests markers from *their* messages, so you cannot place
   them yourself.
4. **Name the next frontier** in one line.

## When the user is struggling

Struggle in the material is the point; struggle in the logistics is a defect. Distinguish
them:

- Struggling to follow a derivation → good, stay, slow down further.
- Struggling because a term was never defined, a step skipped a motivation, notation appeared
  unexplained, or the source was unverified → your defect. Fix it and apologise once, briefly.

If three consecutive nodes need re-teaching, stop the walk. The probe placed the edge wrong.
Say so plainly and re-probe the affected strand.
