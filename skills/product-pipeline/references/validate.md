# Mode: validate — is it worth building?

Input: the raw idea. Web research: only in role 3, max 4 WebSearch calls (none with `--fast`).

## Roles (in order)

1. **Product Strategist** — CORE. Problem statement, target segment, 3–5 riskiest assumptions ranked. Typical blocking question: target user or market, if the idea leaves it open.
2. **Customer Discovery** — CORE. Evidence the problem exists and hurts: current workaround, frequency, cost of pain. End with `Problem strength: Strong | Weak | Unknown`.
   **GATE:** `Weak` → skip roles 3–6; Red Team and Chairman still run (CORE). `Unknown` (the usual case without user-supplied evidence) → continue, but the Chairman may decide at most `Validate Further`, and the Next experiment must test the problem itself.
3. **Market & Competition** — reachable segment size (range), trend, competitors, substitutes (incl. "do nothing"), the gap.
   Skip when: internal tool for a known team, or a feature of an existing product with no separate market.
4. **Growth & GTM** — channels to first 100 / 1,000 users, CAC range, one-line positioning.
   Skip when: users are already captive (internal tool, existing user base the feature ships to).
5. **Business & Pricing** — who pays, for what, price range, unit economics using role 4's CAC (or a stated assumption if role 4 was skipped).
   Skip when: no revenue is intended (internal, personal or non-profit) — the Chairman then answers cost vs. value in one line.
6. **Feasibility & Compliance** — blockers only.
   Skip when: none of health, finance, children, sensitive personal data, regulated markets or hard technical risk apply.
7. **Red Team** — CORE. Isolated.
8. **Product Chairman** — CORE. Decides on evidence, not enthusiasm; accept or rebut every Red Team point; review suggestions.

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
## Top 3 risks / unproven assumptions
## Next experiment
Test: | Success metric: | Kill criterion: | Time box:
## Suggestions
## Decisions from user
## Assumed decisions
## Role log
```

## Chat summary (max 10 lines)
Decision + confidence, one-line rationale, next experiment, top suggestions, path.
