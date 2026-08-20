# Extracted Portal Configuration & Structured Content

Pulled from `WBG_C4E_Enablement_Library-9.html` (the single-artifact prototype)
into the JSON structure defined in Appendix A of the Architecture Guide.

## Done in this pass

| File | Source in the HTML | Notes |
| --- | --- | --- |
| `config.json` | `<title>`, sidebar status box, `REPLACE-owner@ahead.com` | This is the instance boundary — swapping this file is what would let the same framework serve Client B or Client C. |
| `navigation.json` | `.nav-btn[data-page]` + `.section-link[data-section]` (6 pages, 25 sections) | Includes the `QJ_CURATED` quick-jump shortlist. |
| `milestones.json` | `LANES` and `MILESTONES` JS objects (~line 4434) | Verbatim structure — this file already separates cleanly; it's a direct copy, just relocated out of inline `<script>`. |
| `glossary.json` | `GLOSSARY` object (~line 5187) | Programmatically parsed — all 23 terms extracted exactly, no manual transcription risk. |
| `role-journeys.json` | The three "Find your path" role cards on Home (~line 2219) | General / Daily / Power user tiers, with their routing targets. |

## Not yet extracted (next pass)

Per Appendix A these still need to come out of the artifact:

- **`key-steps.json`** — the Milestone browser / Key Steps cards in the Onboarding Guide (implementation-section-3). Larger extraction — each milestone has detailed step cards, code refs (I3, R4, D5...), and content.
- **`learning-paths.json`** — the module lists under Learning Hub (learning-section-2/3), which build on the role-journeys tiers.
- **`directory.json`** — Team Directory. Per the guide (Section 9), this is fine as JSON initially; move to SharePoint only once operational owners need to self-maintain it.
- Fundamentals content (core concepts, reliability frameworks, references) — largely prose; per Section 9's placement test this may be better split between portal experience copy (stays in the app) vs. anything that's really a governed reference doc (→ SharePoint).

## What does NOT move to JSON

Per the placement test (Guide, Section 9), anything that's a governed document —
templates, standards, runbooks, training materials, validation/handoff packages —
should go to SharePoint libraries instead, with the portal linking to the current
published version. That content isn't in this HTML file today (it's referenced,
not embedded), so there's nothing to extract for that bucket yet — it just needs
the libraries created (Section 10, Decision D7).
