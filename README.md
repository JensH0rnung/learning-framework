# `/understand`

A Claude Code skill for building genuine structural understanding of a hard topic, instead of
being handed facts about it.

It runs three phases:

1. **Probe** — one question at a time, to locate the exact edge of what you already know and
   which unconditional truths you already accept.
2. **Plan** — build a motivated derivation path as a labelled DAG, rooted in those truths,
   verified by a subagent before anything is taught. Emitted as mermaid for your go-ahead.
3. **Teach** — walk the DAG *one reasoning step per message*, each step gated by a quiz, with
   periodic compression checkpoints where you state the compressed version and it gets
   reconciled against the graph.

State persists in your notes vault as a per-domain model of what you understand, so the second
session in a domain starts where the first one stopped.

The design rationale — and why each mechanism exists — is in
[`skills/understand/DESIGN.md`](skills/understand/DESIGN.md). The seven rules everything serves
are in [`DOCTRINE.md`](skills/understand/DOCTRINE.md).

## Requirements

- **Claude Code**.
- **An Opus-class model at high reasoning effort.** Teaching quality tracks model intelligence
  directly; a weaker model produces plausible teaching that quietly skips the motivation step.
  The skill checks and warns.
- **A local markdown notes directory.** Obsidian is assumed but not required — the skill writes
  plain markdown. Obsidian is what renders the LaTeX, mermaid, callouts and embedded SVG, and
  the session log is meant to be read there rather than in the terminal. Each part of a teaching
  step gets its own callout type (tension, definition, anchor, reframe, quiz), so the structure
  of a step is visible in the note without reading it. Other markdown editors show the callouts
  as plain blockquotes — readable, just not colour-coded.

## Install

Copy the skill and its two subagents into your Claude Code config:

```sh
git clone https://github.com/ddMarkov/understand-skill.git
cd understand-skill
mkdir -p ~/.claude/skills ~/.claude/agents
cp -R skills/understand ~/.claude/skills/
cp agents/*.md ~/.claude/agents/
```

Then **set your notes root**. Replace the literal string `<VAULT_ROOT>` with an absolute path
in both files that declare it:

```sh
VAULT="$HOME/Documents/my-vault"        # no trailing slash
cd ~/.claude/skills/understand
sed -i '' "s|<VAULT_ROOT>|$VAULT|g" SKILL.md FORMATS.md
```

(Drop the `''` after `-i` on GNU/Linux.)

The skill refuses to run while the placeholder is unset — it will not guess a path or write
outside the configured root.

It creates this layout on first use:

```
<VAULT_ROOT>/Learning/<domain>/
  _map.md                     what you understand + the accumulated structure graph
  logs/YYYY-MM-DD-<topic>.md  live session transcript (your reading surface)
  reference/<concept>.md      compressed generators + edges + anchors
  assets/*.svg                visuals
```

`<domain>` is a stable field name (`differential-geometry`, `rust-ownership`, `ldap`), not the
session topic, so sessions accumulate.

## Use

```
/understand differential forms
```

Then open the log path it prints, in Obsidian, and read there while you answer in the terminal.

Interrupting mid-step with a question is the expected mode, not a derailment.

## Optional: Anki

Items the skill classifies as genuinely arbitrary **conventions** — things that can never be
derived, only memorized — are queued as draft cards to `<VAULT_ROOT>/Anki Drafts/`. Importing
them needs a separate `anki-flashcards` skill, which is **not** part of this repo. Without it
the drafts are simply markdown files you can read or import yourself; nothing breaks.

## Subagents

| Agent | Role |
|---|---|
| `learning-verifier` | Checks the planned claim set before it is taught. Formal domains get an internal-consistency pass; empirical or fast-moving domains get real source verification with citations. |
| `svg-illustrator` | Authors one explanatory SVG per geometric step, views the render, and self-corrects until it is legible. |

Both are declared `model: opus`. Failures are surfaced visibly rather than swallowed.

## License

MIT — see [LICENSE](LICENSE).
