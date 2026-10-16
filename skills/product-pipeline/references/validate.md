# Mode: validate — is it worth building?

Input: the raw idea. Web research: only in role 3, max 4 WebSearch calls (none with `--fast`).

**Feature mode** — the idea is a feature of the project in the working directory (it has source code). Also read the project rules (`CLAUDE.md`, `AGENTS.md`, lint config) and the related source: Grep by feature keywords, open at most ~12 files, relevant parts only. Artifact: `validate-<feature-slug>.md` instead of `01-validation.md`. Role changes are marked *Feature mode* below.

## Roles (in order)

1. **Product Strategist** — CORE. Problem statement, target segment, 3–5 riskiest assumptions ranked. Typical blocking question: target user or market, if the idea leaves it open.
2. **Customer Discovery** — CORE. Evidence the problem exists and hurts: current workaround, frequency, cost of pain. End with `Problem strength: Strong | Weak | Unknown`.
   **GATE:** `Weak` → skip roles 3–6; Red Team and Chairman still run (CORE). `Unknown` (the usual case without user-supplied evidence) → continue, but the Chairman may decide at most `Validate Further`, and the Next experiment must test the problem itself.
   *Feature mode:* evidence may also come from the user's stated reason and the source (existing workarounds, TODOs, related issues). `Unknown` does not cap the decision when effort is S/M — the Chairman may still Go, listing the unproven need as a risk.
3. **Market & Competition** — reachable segment size (range), trend, competitors, substitutes (incl. "do nothing"), the gap.
   Skip when: internal tool for a known team, or a feature of an existing product with no separate market.
   *Feature mode:* not skipped; becomes **Pattern Research** — how 2–4 competitors or similar apps solve this feature, the established UX pattern, known pitfalls, and what users complain about. Each claim `[F]` with URL.
4. **Growth & GTM** — channels to first 100 / 1,000 users, CAC range, one-line positioning.
   Skip when: users are already captive (internal tool, existing user base the feature ships to).
5. **Business & Pricing** — who pays, for what, price range, unit economics using role 4's CAC (or a stated assumption if role 4 was skipped).
   Skip when: no revenue is intended (internal, personal or non-profit) — the Chairman then answers cost vs. value in one line.
6. **Feasibility & Compliance** — blockers only.
   Skip when: none of health, finance, children, sensitive personal data, regulated markets or hard technical risk apply.
   *Feature mode:* CORE, from the source — fit with the current architecture and project rules, what already exists and can be reused (`file` refs), areas likely touched, effort (S/M/L), technical risks and conflicts with existing behaviour.
7. **Red Team** — CORE. Isolated.
8. **Product Chairman** — CORE. Decides on evidence, not enthusiasm; accept or rebut every Red Team point; review suggestions.

**Helper skills.** If the session lists product-management skills, roles may invoke at most 2 per run, mainly to bring in real data: role 3 → `competitive-brief`; role 2 → `synthesize-research`, only when the user supplied interview notes or transcripts; *feature mode* role 2 → `metrics-review`, only when an analytics connector (Amplitude, Pendo…) is connected. Their claims are tagged `[F]`/`[A]` like any other; they never feed Red Team or the Chairman. None with `--fast`.

## Artifact: `01-validation.md`

```
# Validation — <idea>
## Route
## Decision
Go | Validate Further | Pivot | No-Go — Confidence: Low | Medium | High
<rationale, max 5 sentences>
## Target user & problem
## Advantage vs alternatives
## Revenue model & GTM path   (omit if both roles skipped)
## Pattern research   (feature mode)
## Fit with current project   (feature mode: reuse, areas touched, effort, technical risks)
## Source map   (feature mode: the related files read, one line each — design starts from these)
## Top 3 risks / unproven assumptions
## Next experiment
Test: | Success metric: | Kill criterion: | Time box:
## Suggestions
## Decisions from user
## Assumed decisions
## Role log
```

## Next step

- Go → `Run define now` · `Stop here`.
- Go in feature mode → `Design this feature now` (Recommended for effort S/M; starts `design "<feature>"`) · `Run define now` (Recommended for L) · `Stop here`.
- Validate Further / Pivot → `Plan the next experiment` · `Stop here`.
- No-Go → `Stop here` · `Pivot: re-run validate on a reframed idea`.

`Plan the next experiment` = expand `## Next experiment` into a runnable plan (who to reach and how, script or test asset, success metric, kill criterion, time box), Edit that section only, then stop; re-run `validate` with the results.

## Chat summary (max 15 lines, before the Next step question)
Decision + confidence, one-line rationale · **For it:** 2–3 strongest points · **Against it:** 2–3 biggest risks or gaps · feature mode: fit, reuse and effort in one line each · next experiment · top suggestions · path.
