<p align="center">
  <img src="assets/mascot.png" alt="Pixel-art orc warlord in the likeness of Grom Hellscream, flustered and raising a hand defensively, with a speech bubble reading 'IT IS NOT MY INTENT!'" width="320">
</p>

# it-is-not-my-intent

> A drop-in `AGENTS.md` template for paying down **intent debt** before an agent
> (or your next teammate) has to reconstruct it from scratch.

## The joke in the name

*"It's not my intent"* is the sentence people reach for after something already
went wrong — a way of disowning a decision after the fact. This repo inverts
it. Write the intent down **before** the work starts, in a file every agent
reads on every run, and the sentence stops being an excuse. Either the intent
was written and got ignored (a process failure, now visible), or it wasn't
written (also now visible, and fixable). "That wasn't my intent" stops being
something you say *after*, and becomes something you make untrue *in advance*.

## Why this exists

Software health research (see [Storey et al., "From Technical Debt to
Cognitive and Intent Debt"](https://queue.acm.org/detail.cfm?id=3807966), and
Addy Osmani's ["The Intent Debt"](https://addyosmani.com/blog/intent-debt/))
splits software debt into three kinds, by where it lives:

| Debt | Lives in | Who can repay it |
| --- | --- | --- |
| **Technical debt** | the code | agents can help a lot |
| **Cognitive debt** | the people | agents can help some |
| **Intent debt** | the artifact — the *why* behind the code | **only a human** |

Agents can refactor code and summarize systems, but they cannot generate the
reason a system should exist in a particular shape — that has to come from a
person. Left unwritten, that reason gets reconstructed, badly, over and over.
Before agents, that reconstruction happened rarely — onboarding, or after
someone left. With agents, it happens **every session, times every agent
running**. The interest compounds.

This repo is the smallest possible fix: an `AGENTS.md` template that treats
itself as an **intent ledger**, not a config file — a place to write the *why*
once, so nobody (human or agent) has to reconstruct it again.

## Quick start

1. Copy [`AGENTS.md`](./AGENTS.md) into the root of your repository.
2. Fill in section 1 (why the project looks the way it does) and section 2
   (what you deliberately don't do) with your project's real, non-obvious
   decisions — the ones that live in someone's head right now.
3. Point your coding agent's instructions at this file (most agent harnesses
   already look for `AGENTS.md` at the repo root).
4. From then on, treat section 5 (decision log) as part of the PR checklist,
   not optional documentation.

## What's inside

- [`AGENTS.md`](./AGENTS.md) — the template itself, with inline comments
  explaining what each section is for and why it's structured that way.
- [`LICENSE`](./LICENSE) — MIT. Use it, fork it, adapt it.

## Background reading

- Addy Osmani — [The Intent Debt](https://addyosmani.com/blog/intent-debt/)
- Storey et al. — [From Technical Debt to Cognitive and Intent Debt: Rethinking
  Software Health in the Age of AI](https://queue.acm.org/detail.cfm?id=3807966)
