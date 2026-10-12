# Mode: design — screens, forms and flows

Specs for screens, forms, flows. No production code until the user picks **Implement** in the Next step question.

## Step 0 — Resolve inputs (before routing and roles, no handoff)

Work out three things, in this priority order, and record the source of each in one line at the top of the artifact.

**A. Requirements (what to design)**
1. Backlog the user gave or pointed to, or `03-backlog.md` → Grep the epic heading and read only that epic's stories.
2. Otherwise → the user's feature description (plus `validate-<feature>.md` if it exists: use its scope, pattern research and fit findings, don't redo them, and start from its `Source map` instead of searching again) + the project source: find the related existing screens, models, API/services and routes (Grep by feature keywords). Derive 3–8 working stories with AC, tagged `[A]` where inferred. These become the AC that Red Team checks against. If the description leaves the core behaviour open, that is a blocking question.
3. `review` → locate the screen in the source: Grep for distinctive visible text from the screenshot (title, labels, button text) in code and localization files, then follow to the screen file and its direct child widgets. Current behaviour = what the code does (fields, validation, states, API calls, navigation) + what the screenshot shows. Several candidate files → ask which (Recommended: the one whose strings match most). Not found → work from the screenshot alone and say so in one line.

**B. Project rules (constraints the design must obey)**
If `04-design-foundation.md` exists, take them from its `Project conventions` section and skip this step. Otherwise read, if present (`CLAUDE.md` is already in context): `AGENTS.md`, `.cursor/rules` or similar, `README` sections on UI or conventions, `docs/` design or style guides, lint config (`analysis_options.yaml`, `.eslintrc`…). These rules override this skill's defaults where they conflict.

**C. Foundation (visual system)**
1. `04-design-foundation.md` exists → use it.
2. Otherwise, if the project has UI code → EXTRACT the foundation from source; never invent a new one. Look for: theme definition (`ThemeData`, `ColorScheme`, `TextTheme`, CSS variables, Tailwind config…), color/spacing/typography constants, shared widgets or components folder, localization and RTL setup, navigation/routing pattern, form and validation pattern, how loading/error/empty states are shown today. Write the result to `04-design-foundation.md` (marked `Source: extracted from code`) so later runs reuse it. If the source shows two conflicting patterns, ask which to follow (Recommended: the more recent or more widely used one).
3. No UI code at all → run `design foundation` first (say so in one line).

Source-reading budget: locate with Glob/Grep first; open at most ~12 files, read only the relevant parts; use the 1–2 existing screens most similar to the requested feature as the reference pattern. Platform and stack come from the source (or `02-definition.md` if no source).

## design foundation (new products, or on explicit request)

If the project already has UI code, extract (Step 0-C2) instead — no roles run — unless the user explicitly asks for a redesign.

1. **Design Director** — CORE. Visual direction, 3–5 design principles, tone, what to avoid. Typical blocking question: choice between 2–3 visual directions (each described in one line).
2. **Design System, Platform & A11y** — CORE. Tokens (color, type scale, spacing, radius, elevation), base components with variants and states, platform rules (navigation patterns, iOS/Android differences, RTL), accessibility rules (contrast, touch targets, font scaling).
3. **Design Red Team** — CORE. Critique the system before screens are built on it. One revision loop to role 2.

`04-design-foundation.md`: Source · Direction & principles · Tokens · Components · Platform rules · A11y rules · Project conventions · Suggestions · Decisions from user · Assumed decisions · Role log.

## Design quality bar (design and review)

The goal is a creative design that a leading modern app would ship, never creativity at the cost of UX.

- **UX first.** Start from the user's job on the screen: why they come, the 2–3 questions they need answered, the one primary action. 5-second test: can they get the main answer without scrolling or reading body text? Find *opportunities* (`O-xx`, value the screen does not give yet), not only defects.
- **Current practice.** Check how 3–5 leading apps in the domain solve this screen, and current platform guidance (Material 3 / Apple HIG / WCAG 2.2). Cite each idea `[F]` with a URL.
- **Make it attractive.** Visual hierarchy, layout ideas (hero number or insight, cards vs. list, progressive disclosure, data viz with meaning), micro-copy, and motion where it explains change or gives feedback (state change, chart reveal, success). Motion is purposeful, short, interruptible and respects reduced-motion. Visual issues are valid when they name the user impact.
- **Consistency guard.** Every color, size, radius, font and motion value comes from the foundation tokens and existing components; the screen must not look like another app. A new token or component only when a direction really needs it: flag `NEW-TOKEN` / `NEW-COMPONENT`, derive its value from the existing scale, and say where else in the app it should be used. Max 2 per direction without asking.
- **Helper skills.** If the session lists design skills (e.g. `ui-ux-pro-max`, `impeccable`, `taste-skill`/`design-taste-frontend`, `emil-design-eng`, `animate`), roles may invoke at most 2 per run, for ideas or critique only. Their output never overrides project tokens or rules. None with `--fast`.
- **Ask with a recommendation.** Any real choice (direction, removing something, a new pattern) is asked with the role's own recommendation first, per Questions in SKILL.md.

## design <epic-id | "feature description"> (repeat per epic or feature)

1. **UX Architect** — CORE. Per screen: user's job and primary action, opportunities `O-xx`, purpose, entry/exit and where it plugs into the existing navigation, fields with type + validation + error text, states (empty, loading, error, success, offline), interactions, edge cases. Follow existing patterns from Step 0 unless they hurt the user (then propose the change with its reason; never silently change).
2. **Pattern Research** — how 3–5 leading apps solve the same job, plus relevant platform guidance; 3–6 ideas worth borrowing, each with URL and why it fits here. Max 4 WebSearch. Reuse `validate-<feature>.md` Pattern research if it exists.
   Skip when: `--fast`, or the change is flow, validation, logic or copy only.
3. **UI Maker** — two directions per screen, both built on the foundation (Consistency guard):
   - **A · Refine** — best version within current patterns and components.
   - **B · Elevate** — bolder: new layout idea, stronger hierarchy, purposeful motion, borrowed patterns from role 2; still the same app.
   Each: layout top-to-bottom naming actual components and tokens, motion spec (trigger, property, duration/curve from tokens), `NEW-TOKEN` / `NEW-COMPONENT` flags with one-line spec. Small size or no layout change → one direction only.
   Skip when: the change is flow, validation or logic only on existing screens with no layout change.
4. **Design Red Team** — checks both directions against the AC from Step 0-A, UX quality, consistency with the existing app and tokens, project rules, and **timidity**: what a leading app would do better that neither direction does. Output: issues with screen IDs, direction and severity. Isolated for Medium/Large; inline for Small.
   Skip when: a single field or copy change with no new state or flow.
5. **Implementer** — buildability of each direction in the project's actual stack: reuse of existing components/services, motion cost on the target platform, costly interactions with a cheaper alternative, conflicts with project rules; effort per direction; handoff spec (files likely touched, components reused, new tokens/components, assets, open dev questions).
   Light (handoff spec only) when: only existing components are used and there is no new animation or custom interaction.
6. **Design Director** — CORE. Approved | Changes required; resolve every Red Team issue (fixed / accepted / rejected + reason); recommend A or B with a one-line reason. A bold idea is dropped only with a UX reason, never for caution alone; review suggestions.

`design/<id>.md` (id = epic ID, or a kebab slug of the feature): Route · Inputs & sources · Working stories & AC (only if derived) · User job & opportunities · Pattern research · Screens (one section per S-xx: flow, fields, states, then direction A and B layout + motion) · New tokens & components · Handoff spec · Decision · Suggestions · Decisions from user · Assumed decisions · Role log.

Then ask the Next step question.

## design review <screenshot | screen name | file path> (improve an existing screen)

Input: a screenshot and/or a pointer to the screen. Step 0 applies (A3 for requirements, B, C). Hard rule: keep all existing functionality — removing or merging a field, action or state is a blocking question.

1. **UX Architect** — CORE. First the user's job on this screen and the 5-second test. Then audit: 2–4 strengths to keep (`K-xx`, with evidence); opportunities `O-xx` (value the screen should give but doesn't); issues `R-xx` with severity (High / Medium / Low), evidence (screenshot area or `file:line`) and the principle broken (task fit, hierarchy, clarity, form usability, feedback and states, visual quality, consistency with the rest of the app, accessibility, RTL, platform conventions). Then specify the improved screen: flow, fields, validation and error text, states, interactions — each change linked to an `O-xx` or `R-xx`.
2. **Pattern Research** — as in design mode, for this screen's job.
   Skip when: `--fast`, or all High/Medium items are logic or copy only.
3. **UI Maker** — directions A · Refine and B · Elevate, as in design mode, on the existing foundation; every change linked to `O-xx` / `R-xx`.
   Skip when: all High/Medium items are flow, logic or copy only.
4. **Design Red Team** — checks that every High/Medium `R-xx` and key `O-xx` is addressed, no existing behaviour from the code was lost, nothing conflicts with the app, tokens or project rules, and flags timidity. Isolated unless Small.
5. **Implementer** — change-oriented handoff per direction: files to touch, what changes in each, components reused, new tokens/components, motion cost, migration or API impact, effort, cheaper alternatives for costly changes.
6. **Design Director** — CORE. Approved | Changes required; resolve every Red Team issue; recommend A or B; review suggestions.

`design/review-<screen-slug>.md`: Route · Current screen & sources · User job · Strengths (`K-xx`) · Opportunities (`O-xx`) · Issues (`R-xx` table: severity, evidence, principle) · Pattern research · Changes (`O/R-xx` → change → why) · Direction A · Direction B · New tokens & components · Handoff spec · Decision · Suggestions · Decisions from user · Assumed decisions · Role log.

Then ask the Next step question (the mock gets a Current / Improved toggle).

## Next step (after every approved design or review)

When the Design Director approves, first print the chat summary (below) so the user can decide from the chat alone, THEN ask with ONE AskUserQuestion call — never decide alone. When two directions exist, ask two questions in that call: **Direction** (`B · Elevate` / `A · Refine` / `Both in the mock`, the Director's pick first as Recommended) and **Next step**:

- **Build an HTML mock first** — recommended when the run adds or visibly changes screens (Medium/Large, or a review with layout changes).
- **Implement now** — recommended when the change is Small or flow/logic/copy only.
- **Stop here** — keep the spec only.

Put the recommended one first with `(Recommended)` and a one-line reason. If the Director chose `Changes required` and the revision loop is still unused, apply them once and let the Director decide again. If the loop was already used or changes are still required, do not ask this question: write the spec with the open changes under `Decision`, list them in the chat summary, and stop. With no answer or no AskUserQuestion tool: stop after the spec and list the two options in the chat summary — never write code without the user's choice.

After the mock, ask again: `Implement now (Recommended)` · `Request changes` (user lists them; apply them to spec and mock with targeted Edits, never a full rewrite; one round, then ask again) · `Stop here`.

`design mock <id>` and `design implement <id>` run these steps later from an existing `design/<id>.md` without re-running any role.

## HTML mock

Built by UI Maker from the APPROVED spec, for the chosen direction (`Both` → an `A / B` toggle). Output: `design/<id>.mock.html`. Reads only the spec and `04-design-foundation.md`. Motion from the spec is built in CSS, honouring `prefers-reduced-motion`.

Rules:
- One self-contained HTML file: inline CSS and JS, no external scripts or images (inline SVG for icons). Web fonts only from Google Fonts, and only if the foundation uses that font; otherwise system fonts. The font stack always ends in system fonts, so the mock still renders offline.
- Foundation tokens become CSS variables (`--color-*`, `--space-*`, `--radius-*`, type scale) — exact values from `04-design-foundation.md`, no new colors or sizes.
- Real copy in the output language, with realistic sample data. RTL (`dir="rtl"`) when the app is RTL.
- Mobile app → each screen inside a ~390×844 phone frame; web → responsive container. Screens laid out side by side, wrapping.
- Per screen, a state switcher for the states in the spec (default, empty, loading, error, success…).
- Clickable flow: buttons and links navigate between mock screens as in the spec. Forms run the spec's validation and show its error text.
- `Annotations` toggle overlays screen IDs, `NEW-COMPONENT` markers, and (review) `R-xx` labels on the changed areas.
- Review mode: a `Current / Improved` toggle; Current is a faithful wireframe of the existing screen.
- Budget: reuse one CSS class set across screens; keep JS minimal (state switching, navigation, validation). Write it in a single Write.

After writing, give the path and ask the post-mock question above.

## Implement

Only after the user picks **Implement now**. Implementer writes the code in the project, from the approved spec (and mock, if built):
- Follow the project rules from Step 0-B and the patterns of the reference screens; reuse existing components, services and localization. Build `NEW-COMPONENT`s in the shared components location the project already uses.
- Touch the files listed in the handoff spec; any extra file is reported with a reason.
- Review mode: change only what the `R-xx` changes require; keep all existing behaviour.
- Verify once with the project's own check if available (e.g. `flutter analyze`, `dotnet build`, the lint/test script); fix what it reports.
- Self-check every AC / High-Medium `R-xx` against the code and list any not met.
- Do not commit. Report: files changed, AC status, check result, anything left for the user.

## Chat summary (before the Next step question)

Plain language, in the output language; IDs stay in English.

**review** (max 30 lines):
1. **User job:** one line — why the user opens this screen.
2. **Strengths — kept:** each `K-xx` in one line.
3. **Opportunities:** each `O-xx` — what value is missing → idea (with the app it comes from, if any).
4. **Issues:** top 6 by severity — `R-xx · High|Medium|Low — what is wrong → why it hurts the user`.
5. **Direction A · Refine / B · Elevate:** 2–3 lines each — what changes visually and in UX, motion, new tokens/components, effort. Which one the Director recommends and why.
6. **Unchanged:** existing fields, actions and states that stay as they are.
7. Decision, unresolved issues, top suggestions, artifact path.

**design** (max 22 lines): user job and opportunities · screens covered with one line of purpose each · directions A and B (2–3 lines each, recommendation and why) · new tokens/components · effort and files touched · decision, unresolved issues, top suggestions, artifact path.

Then the Next step question. After a mock: its path plus one line per screen on what to try in it.
