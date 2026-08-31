# AGENTS.md

<!--
  This file is not a config file. It is an intent ledger.
  Config tells an agent HOW to run a command.
  This file tells an agent (and every future human) WHY things are the way they are —
  and, just as importantly, what was tried and rejected.

  Background: "The Intent Debt" (Addy Osmani, 2026) and the Triple Debt model
  (Storey et al., "From Technical Debt to Cognitive and Intent Debt: Rethinking
  Software Health in the Age of AI"). Of the three debts — technical, cognitive,
  intent — intent debt is the only one an agent cannot repay for you, because
  intent has to come from a human. This file exists so nobody on this project
  ever again has to say "that wasn't my intent" — because the intent was written
  down before the work started.

  Delete a section only if you understand why it's here. If you don't, that's a
  sign the debt is still live — ask, don't guess.
-->

## 0. Before you touch anything

- Read section 1 and 2 in full before writing code. They are short on purpose.
- If your task conflicts with something in section 2 ("what we deliberately don't
  do"), stop and raise it with a human instead of working around it silently.
- Before editing any class, function, or field, check whether it carries an
  `Intent:` / `Do not:` tag (section 3) — this file cannot list every symbol in
  the codebase, and the symbol's own docstring is often the only place that
  intent lives.
- If you make a non-trivial decision, you MUST add an entry under section 5
  (Decision log) before opening a PR. This is not optional documentation — it's
  the only mechanism this project has for repaying intent debt in real time.

## 1. Why this project looks the way it does

<!--
  Format: "We do X, not Y — because Z."
  Z must be a reason, not a restatement of X. Link evidence (issue, incident,
  benchmark, ADR) whenever one exists. Keep entries dated so staleness is visible.
-->

- We do `<practice>`, not `<obvious alternative>` — because `<the actual reason,
  with a link if one exists>`. (`YYYY-MM-DD`)
- _(add one bullet per non-obvious convention: architecture choices, library
  picks, naming rules, folder structure, why the build looks the way it does)_

## 2. What we deliberately do NOT do

<!--
  Format: "We don't do X. Someone tried. See <link>. It broke <thing>."
  This section exists specifically to stop agents (and new hires) from
  re-discovering the same dead end at agent speed, every session.
-->

- We don't `<thing that looks like an obvious improvement>`. It was tried in
  `<PR/issue link>` and it `<what broke>`.
- _(add one bullet per rejected-but-tempting path)_

## 3. In-code intent annotations

<!--
  Sections 1 and 2 hold project-wide rationale. Most intent isn't project-wide —
  it belongs to one class, one field, one function, and would bloat this file
  into something nobody reads if it lived here instead. Put it where the next
  editor of that exact symbol cannot miss it: in the symbol's own docstring or
  the comment directly above it.

  Use two fixed, grep-able tags so intent can be found mechanically, not just
  by reading:
    Intent:  what this symbol exists for / who is allowed to depend on it
    Do not:  the specific tempting-but-wrong change, and what breaks if made

  Example (Django model):

    class LegacyInvoice(models.Model):
        """
        Intent: read-only audit snapshot of pre-2024 billing records.
        Auditors must see the invoice as it looked at purchase time, even
        after Invoice's schema changes.

        Do not: merge this into `Invoice` or backfill missing fields from
        it. Tried in PR #482 — broke the FY2024 audit export. See ADR-0011.
        """

  Example (field-level, as an inline comment since Python has no field
  docstrings):

    external_id = models.CharField(max_length=64, unique=True)
    # Intent: mirrors the payment provider's ID 1:1.
    # Do not: generate or backfill this locally — reconciliation with the
    # provider breaks silently if this value is ever synthetic.

  Escalation rule: if the same `Do not:` shows up on more than one symbol,
  it has stopped being that symbol's private business and become a project
  convention — move it (or a pointer to it) into section 2.
-->

- Find every existing annotation with: `grep -rn "Intent:\|Do not:" <path>`
- When you add a new symbol whose obvious-looking alternative is wrong, tag it
  at the point of authorship — don't wait for someone else to break it first.

## 4. Constraints that don't live in any single file

<!--
  Legal, compliance, infra, or organizational constraints that no linter and
  no test suite will ever catch, because they aren't expressed in code.
-->

- `<constraint>` — required by `<law/policy/contract/incident>`. Violating it
  is not a code review nit, it's a `<consequence>`.

## 5. Decision log protocol

<!--
  This is the cheapest form of intent-debt repayment there is: capture the
  "why" at the moment of the decision, not after the fact. Agents: append an
  entry for every PR that makes a non-trivial choice. Do not skip this because
  the change "seemed small" — intent debt is invisible until someone needs it.
-->

Append one entry per non-trivial PR/commit, newest first:

```
### YYYY-MM-DD — <short title>
- Intent: what were you actually trying to achieve?
- Alternatives excluded: what did you consider and rule out, and why?
- Human verified: [ ] yes  [ ] no   <!-- an agent must never check this box itself -->
```

> An agent-written decision log entry is a draft of intent, not intent itself.
> It records what the agent reasoned, not what the team actually wanted. A
> human must read and confirm it before it counts as paid-down debt.

## 6. Tribal knowledge (write once, read every run)

<!--
  The "we don't do X because of the incident on <date>" knowledge that used to
  live only in the most senior engineer's head. Write it here once; every
  agent session reads it from now on instead of re-learning it the hard way.
-->

- `<incident/lesson>` (`YYYY-MM-DD`) — `<what happened, what we changed
  because of it, what NOT to revert>`.

## 7. Spec pointer

<!--
  If this project uses spec-driven development, point here instead of
  duplicating specs into this file. Specs carry the WHY behind a feature;
  this file carries the WHY behind the project's shape. Keep them separate.
-->

- Specs / ADRs live in: `<path, e.g. specs/ or docs/adr/>`
- Before implementing, an agent should check the relevant spec's "why" before
  trusting the existing code's "how" — code can drift from intent silently;
  specs are supposed to be updated deliberately.

## Appendix — keeping this file honest

- **Owner**: `<who is accountable for this file staying current>`
- **Review cadence**: `<e.g. every release, every quarter>`
- **Staleness signal**: if section 5 hasn't gained an entry in `<N>` PRs, either
  nothing non-trivial happened, or this file is being skipped. Check which.
- **This file is a ledger, not a monument.** Entries that are no longer true
  should be corrected or removed with the same rigor as dead code — but never
  silently. Explain why an entry no longer applies before deleting it.
