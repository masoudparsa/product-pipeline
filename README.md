# product-pipeline

A Claude Code skill that runs product work through **sequential expert roles**. Each role reviews the previous role's handoff, records disagreements, and passes its own findings on. A deciding role closes every run.

Three independent pipelines — call whichever you need, whenever you need it:

| Command | Question | Output |
|---|---|---|
| `/product-pipeline validate "<idea>"` | Is it worth building? | `01-validation.md` — Go / Validate Further / Pivot / No-Go |
| `/product-pipeline validate "<feature>"` (inside a project) | Is this feature worth adding here? Reads your code, researches how others do it | `validate-<feature>.md` → on Go: design → mock / implement |
| `/product-pipeline define` | What exactly do we build? | `02-definition.md`, `03-backlog.md` |
| `/product-pipeline design foundation` | Visual direction and design system | `04-design-foundation.md` |
| `/product-pipeline design E-03` or `design "feature description"` | Screens, forms and flows | `design/<id>.md` |
| `/product-pipeline design review` + screenshot | Audit and improve an existing screen | `design/review-<screen>.md` |
| `/product-pipeline design mock <id>` · `design implement <id>` | Mock or code for an already approved design | `design/<id>.mock.html` / project code |

All artifacts are written to `docs/product/<slug>/` in your project.

## Pipelines and roles

**1. Validate** — Product Strategist → Customer Discovery *(gate)* → Market & Competition → Growth & GTM → Business & Pricing → Feasibility & Compliance → Red Team → Product Chairman

**2. Define** — PM + Data Analyst → User Researcher → Business Analyst + Security & Compliance → PM (feature catalog) → UX Architect → Tech Lead → PM (MVP & phases) → Red Team → Business Analyst (backlog) → Tech Lead (Definition of Ready) → Product Chairman

**3. Design** (specs only, no production code)
- Foundation, once: Design Director → Design System, Platform & A11y → Design Red Team
- Per epic or feature: UX Architect (user job + opportunities) → Pattern Research (how leading apps solve it) → UI Maker (two directions: A · Refine, B · Elevate, both on your design tokens) → Design Red Team (risks *and* timidity) → Implementer → Design Director (recommends a direction; you choose)
- Review an existing screen: finds the screen in your source from the screenshot's text, audits it (strengths, opportunities, issues by severity with evidence), then runs the same chain — keeping all existing functionality
- Creative but consistent: layout ideas, hierarchy and purposeful motion, built only from your existing tokens and components; any new token or component is flagged and justified. Uses installed design skills (ui-ux-pro-max, impeccable, taste…) for ideas when available.

**Product-management skills**, when installed: validate and define may call at most 2 per run (`competitive-brief`, `synthesize-research`, `metrics-review`, `senior-product-owner`), mainly to bring in real data from connected analytics or research tools. Define can end by exporting the backlog to your tracker via `sprint-planning`.

## How it stays cheap and fast

- **Smart routing** — each run classifies the request and skips roles it doesn't need (e.g. no market analysis for a feature inside an existing app, no UI Maker for a validation-only change). The route is shown before the run starts. Core roles (Red Team, deciders) are never skipped.
- **One agent, short handoffs** — roles run inline with a fixed 120-word handoff format. Only Red Team runs as an isolated subagent, seeing the raw input instead of the chain, to break anchoring.
- **Artifacts, not chat history** — each pipeline reads only the previous artifact; handoffs go to the file, not the chat.
- **Mode-scoped loading** — only the reference file of the requested mode is read.
- **Works on existing projects** — `design` reads your backlog if you have one; otherwise it reads the project source and its rules (`CLAUDE.md`, lint config, theme, shared widgets) and extracts the design foundation from the code instead of inventing one.

## What happens at the end

Each run closes by asking what to do next, with its recommendation first:
- **validate** → run define now / plan the next experiment / stop
- **define** → design the first epic / design foundation first / stop
- **design** → build an interactive HTML mock first / implement now / stop. The mock uses your project's real tokens, copy, states and clickable flow; after it you can request changes, implement, or stop. Code is never written without your choice.

## Questions and suggestions

- Roles ask only when the answer changes the outcome and can't be found in the inputs or source. Blocking questions are asked immediately; the rest are batched at the end while the run continues on the recommended option.
- Every question offers options, with the role's own recommendation first, marked **(Recommended)**.
- Roles may add a `Suggest:` line; the decider adopts, defers or rejects each one.
- Every artifact logs `Decisions from user` and `Assumed decisions`.

## Flags

| Flag | Effect |
|---|---|
| `--fast` | No web search; Red Team inline |
| `--lang <code>` | Output language (default: your language) |
| `--no-ask` | Never pause; take recommended options and log them |
| `--full` | Run every role, no routing |
| `--roles +Growth,-UI Maker` | Force roles in or out |

## Install

As a plugin:

```
/plugin marketplace add masoudparsa/product-pipeline
/plugin install product-pipeline@product-pipeline
```

Or manually: copy `skills/product-pipeline/` into `~/.claude/skills/` (personal) or `.claude/skills/` (per project).

## Notes

- This is a structured simulation of expert roles. It does not run real interviews or access market data you didn't provide; claims are tagged `[F]` (fact, with source) or `[A]` (assumption).

## License

MIT — see [LICENSE](LICENSE).
