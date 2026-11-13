# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

- **Valuora** (P0) landing page: a single static `index.html` with inline CSS/JS and no build, dependencies or tests. Open it in a browser to run it. All copy is in pt-BR.
- Theming: a `<head>` script sets `data-theme` before paint. Use the `:root` / `[data-theme="dark"]` color tokens, never hardcoded colors.
- Simulator (`#simulador`): `dcf()` (5 years + Gordon) → `run()` re-renders `#result` (per-share value, 3×3 sensitivity) and `#stats` on every input. Use `Intl` pt-BR formatting. Everything stays client-side.
- Valuation method, WACC/CRP formulas, the WEG worked example and the review checklist are in [docs/valuation.md](docs/valuation.md). Read it before changing any finance logic or copy, and challenge the assumptions it lists (CRP, g, capex/D&A, base date, currency).
- Keep the fictitious sample data and the "not investment advice" disclaimer. Example numbers are illustrative, not recommendations, and sources must be verified before they are cited.
