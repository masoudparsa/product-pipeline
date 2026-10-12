# Mode: define — what exactly do we build?

Input: `01-validation.md`. Requires a Go (or user override). If the decision was not Go, ask whether to continue anyway (Recommended: stop and run the next experiment).
No `01-validation.md` (validate was never run) → use the user's description plus the project source if any; tag what is inferred `[A]`, say in one line that validation was skipped, and continue.

## Roles (in order)

1. **PM + Data Analyst** — CORE. Product brief: vision, goals, in/out of scope, 3–5 KPIs, key funnel events.
2. **User Researcher** — 1–3 personas, main journeys, needs vs. stated wants.
   Skip when: a single, well-known user type and journeys are already clear from validate or the source.
3. **Business Analyst + Security & Compliance** — CORE (BA part). Functional requirements (FR-xx), business rules, entity states, roles/permissions. Security & Compliance part: privacy and consent requirements that shape features.
   Skip the Security & Compliance part when: no personal, financial, health or children's data and no regulated domain.
4. **PM** — feature catalog (F-xx): goal, user, linked FRs. Cut duplicates and low-value items.
   Skip when: Small size — fold the catalog into role 7.
5. **UX Architect** — information architecture, main flows, screen list (S-xx). NO per-screen detail (that is design mode).
   Skip when: no user interface (API, backend job, integration).
6. **Tech Lead** — architecture outline, external services, data, non-functional requirements, complexity per feature (S/M/L), technical dependencies and enablers.
   Light (dependencies + complexity only) when: the work lives inside an existing project with a fixed architecture.
7. **PM** — CORE. MVP boundary and phases (0 Foundation, 1 MVP, 2 Enhancement, 3 Growth), ordered by value, risk, cost, dependency. Typical blocking question: features on the MVP line.
8. **Red Team** — isolated, on MVP scope + phases. One revision loop back to role 7 if needed.
   Skip when: Small size and a single release — role 11 does a short inline challenge instead.
9. **Business Analyst** — CORE. Backlog: Epic (E-xx) › Story (US-xx) / Enabler. Each story: title, `As a…I want…so that`, AC (incl. edge and negative cases), analytics events, phase, deps, priority, open questions.
10. **Tech Lead** — Definition of Ready check; list failing story IDs with the reason. Fix inline once.
    Skip when: ≤5 stories — BA self-checks DoR in role 9.
11. **Product Chairman** — CORE. Approved | Approved with conditions | Sent back; list blockers and open decisions; review suggestions.

## Artifacts

`02-definition.md`: Route · Brief & KPIs · Personas & journeys · Requirements & rules · Feature catalog · IA, flows & screen list · Technical outline & NFRs · Roadmap & phases · Risks & open questions · Decision · Suggestions · Decisions from user · Assumed decisions · Role log. Omit sections whose role was skipped.

`03-backlog.md`: grouped by phase › epic; one compact block per story; no prose between stories.

## Chat summary (max 12 lines)
Decision, MVP in one line, epic list with story counts, top 3 open questions, top suggestions, paths.
