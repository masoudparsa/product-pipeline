---
name: product-pipeline
description: Run a product idea through one of three sequential role pipelines: validate (worth building?), define (features, phases, backlog), design (screens, forms, flows). Use on /product-pipeline or when asked for one of these.
argument-hint: validate "<idea>" | define | design foundation | design <epic-id|"feature"> | design review <screenshot|screen> | design mock|implement <id> [--fast] [--lang <code>] [--no-ask] [--full] [--roles +X,-Y]
---

# Product Pipeline

Three independent pipelines. Run ONLY the requested mode and read ONLY its reference file.

| Mode | Question | Read | Input | Writes (in `docs/product/<slug>/`) |
|---|---|---|---|---|
| `validate <idea>` | Is it worth building? | `references/validate.md` | idea text | `01-validation.md` |
| `define` | What exactly do we build? | `references/define.md` | `01-validation.md`, else user's description + project source | `02-definition.md`, `03-backlog.md` |
| `design foundation` | Visual direction + design system | `references/design.md` | project source, else `02-definition.md` | `04-design-foundation.md` |
| `design <epic-id \| "feature description">` | Screens, forms, flows of one epic or feature | `references/design.md` | backlog if available, else project source | `design/<id>.md` (+ `04-design-foundation.md` if extracted) |
| `design review <screenshot \| screen>` | Audit and improve an existing screen | `references/design.md` | screenshot + project source | `design/review-<screen>.md` (+ `04-design-foundation.md` if extracted) |
| `design mock <id>` / `design implement <id>` | Mock or code for an already approved design (normally offered automatically at the end of a design run) | `references/design.md` | `design/<id>.md` + foundation | `design/<id>.mock.html` / project code |

No mode given → infer it from the request. Still ambiguous → ask (see Questions), then proceed.

Flags: `--fast` (no web search, Red Team inline) · `--lang <code>` (output language; default = user's language) · `--no-ask` (never stop to ask; take the recommended option and log it) · `--full` (run every role, no routing) · `--roles +<Role>,-<Role>` (force a role in or out).

## Routing — run only the roles the problem needs

Before the first role (after the design mode's Step 0), plan the route:

1. Classify the request in one line: type (commercial product, internal tool, feature in an existing app, backend-only, small change…) and size (Small: ≤1 screen or ≤3 stories · Medium · Large).
2. For each role, apply its `Skip when` rule from the reference file. **CORE** roles are never skipped. Skip an optional role only when its rule clearly holds; if in doubt, keep it.
3. Print the route as one chat line before starting: `Route: ✓ Role · ✓ Role · ⊘ Role (reason) · …`
4. A skipped role writes no handoff; the next role covers only the outputs later roles strictly need, one line each.
5. Re-entry: if a later role finds that a skipped role's question now matters (sensitive data appears, a new screen is needed…), add that role back once, at the point it is needed, and note it in the route.
6. Record the final route with skip reasons under `## Route` at the top of the artifact.
7. `--full` disables routing; `--roles` overrides it for the named roles only.

## Shared rules (token and time budget)

1. Artifacts live in `docs/product/<slug>/` (slug = short kebab name of the product). Reuse the folder if it exists.
2. Each mode reads only its listed inputs — never re-derive from chat history. Read large files by section (Grep the heading, then Read with offset/limit). Input missing → use the mode's fallback; if none, proceed from the user's description and say in one line which step was skipped.
3. One agent plays every role, in order, inline. No subagents except the isolated Red Team.
4. Each role writes one handoff (format below), max 120 words. Deciding roles and artifact sections are exempt. No preambles, no restating earlier roles.
5. Anti-anchoring: each role first forms its own view from the raw input, THEN reads the previous handoff and records disagreement or `None`. Never rewrite earlier findings; add to them.
6. Tag claims `[F]` fact (with source: URL or file path) or `[A]` assumption. Never invent precise numbers — give a range marked `[A]`. No real interviews or data exist unless the user supplied them.
7. Gates are hard: a failed gate skips all remaining optional roles; CORE roles still run.
8. At most ONE revision loop per run, shared by Red Team and the decider. If it is already used, or the work still fails after it, the decider does not loop again: it records the remaining issues as blockers in its decision and the run ends.
9. Write each artifact with a single Write at the end, handoffs in a `## Role log` section. Do not print handoffs in chat. (Exception: design Step 0 may write `04-design-foundation.md` once when extracting it from code.)
10. Chat = the route line, one progress line per role (`✓ <Role> — <one-line take>`), any questions, the mode's final summary, and the file path.
11. Artifacts and chat in the output language; role names, IDs, KPI names and technical terms stay in English.

Handoff format:
```
### <n>. <Role>
Take: <one-line verdict>
Findings:
- [F|A] ...  (2–5 bullets)
Disagreement: <with previous role> | None
Open: <unresolved questions> | None
Suggest: <concrete improvement beyond the role's scope, with one-line why> | None
```

## Next step

After the deciding role, ask ONE AskUserQuestion with the sensible next step first as `(Recommended)`:
- validate: Go → `Run define now` · `Stop here`. Validate Further / Pivot → `Plan the next experiment` · `Stop here`. No-Go → `Stop here` · `Pivot: re-run validate on a reframed idea`.
  `Plan the next experiment` = expand `## Next experiment` in `01-validation.md` into a runnable plan (who to reach and how, script or test asset, success metric, kill criterion, time box), Edit that section only, then stop; re-run `validate` with the results.
- define: Approved / Approved with conditions → `Design the first epic (<E-xx>)` · `Design foundation first` (if no UI exists) · `Stop here`. Sent back → `Fix the blockers and re-run define` (Recommended; user answers the blockers first) · `Stop here`.
- design: see Next step in the design reference (mock / implement / stop).
On a yes, start that mode in the same session, reading its artifact inputs as usual.

## Questions

Any role may ask the user when an answer would change the outcome and cannot be found in the inputs or the project source. Never ask what the source, artifacts or a quick search can answer.

- **Blocking** (a wrong guess would derail later roles: target user, platform, scope boundary, a business rule, a conflicting requirement) → pause and ask right away.
- **Non-blocking** (a wrong guess is cheap to fix later) → do not pause. Continue with the recommended option, mark it `[A] assumed: <choice>`, and add it to the end-of-run batch.
- Ask with AskUserQuestion: 1–4 questions per call, 2–4 options each, each option with a one-line consequence. The role's own recommendation is ALWAYS the first option, labelled `(Recommended)`, and its description says why. The user can always type another answer.
- At most one pause per role; ask its questions together.
- End of run: before writing the artifact, ask the non-blocking batch (max 4, most impactful first). If an answer differs from the assumption, apply it to the affected sections in the same single Write (no re-run).
- With `--no-ask`, no AskUserQuestion tool, or no answer: take the recommended option, keep going, and list it under `## Assumed decisions`.
- Record every answered question under `## Decisions from user` (question → answer).

## Suggestions

Every role may add one `Suggest:` line when it sees an improvement outside its own task (a better feature, a cheaper approach, a risk to avoid, a reusable pattern). Suggestions never change the output on their own; the deciding role (Chairman / Design Director) marks each `Adopted | Deferred | Rejected` with a one-line reason. Adopted ones are applied; all go to `## Suggestions`. The chat summary lists the top 3 non-rejected suggestions.

## Isolated Red Team

Run as ONE subagent (Agent tool). Pass only the raw input under review (idea / MVP scope / design) and a draft decision of max 300 words — never the role log. Ask for: strongest failure case, weakest assumption, evidence that would kill it, and one suggestion; max 150 words. With `--fast`, Small size, or no Agent tool, run inline but argue against the raw input before reading the draft.
