# Phase 2 — Plan

**Purpose.** Reason out the entire path from the user's measured edge to the goal, *before*
teaching anything. The DAG you emit is not decoration — it is the commitment that stops you
winging the arc mid-lesson, and it is literally the edge structure that doctrine rule 3 calls
understanding.

## Step 1 — Fire verification (non-blocking)

Classify the domain, then spawn the `learning-verifier` subagent. **Do not block on it.**
Continue building the plan; merge its findings before Phase 3 begins.

**Formal domains** — mathematics, logic, type theory, pure algorithms. Nothing external to
check against, so ask for an **internal-consistency pass**: are the claims mutually
consistent, are the definitions actually definitions rather than property lists, does each
stated derivation follow. No web search.

**Empirical or fast-moving domains** — libraries, APIs, language semantics, hardware,
physics constants, standards, history. Parametric memory is not acceptable here. Ask for
**real source verification with citations**, via Context7 when connected, WebFetch/WebSearch
otherwise.

Mixed topics get both, scoped per claim.

Pass the verifier the *claim set* you intend to teach — one line per claim — plus the domain
classification. Not the whole plan.

## Step 2 — Choose the roots

**Roots are the unconditional truths the user already accepts** (doctrine rule 1), taken from
the probe and the map. Not generic axioms of the field, and not where a textbook would start.

A root must be something they can accept with no caveats: a universal statement, or a real
definition. If a candidate root still feels conditional to them, it is not a root — descend
until you find one that is.

## Step 3 — Build the DAG

**Nodes.** One reasoning step each. Sized to working memory — if a node needs two new objects
to make sense, it is two nodes. A node that cannot be quizzed in one question is too big.

**Edges carry motivations.** This is the part that matters. Each edge is labelled with the
*problem at its tail that sends you to its head*: what the previous step cannot do. Not
"prerequisite of", not topic order.

Good: `covector --"one per point, so it can act along a whole path"--> 1-form`
Bad: `covector --> 1-form`

If you cannot state an edge's motivation, you do not yet understand the path and must not
start teaching it. Find the motivation or restructure.

**Tag every node.**

- `derive` — the object is the minimal answer to a real problem. Almost everything.
- `convention` — genuinely arbitrary: notation, naming, ordering conventions, historical
  accident. **Never fake-derive a convention** (doctrine rule 1). Tag it, teach it in one
  line as "this is a choice, not a consequence", and queue it for Anki (rule 6).

**Prefer reframes.** When a node recasts something the user already holds — `dx` becoming a
covector, a line integral becoming the integration of a 1-form — mark it as a reframe. These
are the cheapest nodes in the plan: relabelling existing structure rather than building new.
Order the DAG to front-load them where possible.

**Depth.** Plan the whole path to the goal, but expect not to walk all of it in one session.
Mark a realistic session boundary. Do not shorten the plan to fit — shorten the walk.

## Step 4 — Emit and present

Write the mermaid graph into the live log, with edge labels intact:

```mermaid
graph LR
  R1["line integral = work along a path<br/>(you already hold this)"]:::root
  R1 -->|"what object is it actually integrating?"| N1[covector]
  N1 -->|"one per point, to act along a whole path"| N2[1-form]
  N2 -->|"what can a 2D surface eat?"| N3[2-form]
  N3 -->|"needs bilinear AND antisymmetric — how?"| N4[wedge product]
  classDef root fill:#2d4,stroke:#191
```

Node labels use the plain-language phrase, not only the term: `covector — eats a vector,
returns a number`. The graph is the first thing the user sees in the note, and a graph of bare
technical names is a graph of empty labels (doctrine rule 7).

Mermaid goes into the log **unboxed** — it does not render reliably inside a callout. The
verifier outcome goes directly under it as `> [!info] Verification`.

Then in chat, briefly:

- the roots, named as things they already hold
- the arc in one sentence
- where the session boundary is
- anything the verifier flagged, if it has returned

Ask whether to prune. The user may already own a branch, or want a different order. Honour it
and update both the graph and the map.

## Step 5 — Merge verification, then start

Before the first teaching step:

- **Verifier confirms** → proceed.
- **Verifier contradicts a claim** → stop. Surface the contradiction in chat and in the log.
  Re-plan the affected node. **Never teach a contradicted claim, and never quietly drop it.**
- **Verifier still running** → start teaching only nodes it has already cleared. If it has
  cleared nothing, wait and say you are waiting.
- **Verifier died** → write a `> [!warning] verification failed` callout into the log naming
  the unverified claims, retry once, and if it fails again tell the user which claims are
  unverified so they know what to hold loosely.
