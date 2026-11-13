# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

- Theming: a `<head>` script sets `data-theme` before paint. Use the `:root` / `[data-theme="dark"]` color tokens, never hardcoded colors.
- Valuation method, WACC/CRP formulas, the WEG worked example and the review checklist are in [docs/valuation.md](docs/valuation.md). Read it before changing any finance logic or copy, and challenge the assumptions it lists (CRP, g, capex/D&A, base date, currency).
- Copy in pt-BR; the simulator stays client-side. Keep the fictitious sample data and the "not investment advice" disclaimer. Example numbers are illustrative, not recommendations, and sources must be verified before they are cited.
