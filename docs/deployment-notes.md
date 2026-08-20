# Deployment Notes — Internal Test (Release 1, pre-MVP)

## What this repo currently does

- `index.html` — the existing C4E experience, **deployed unchanged** (Workstream 7:
  "Experience preservation"). No visual or functional regression risk.
- `content/*.json` — the extracted structured content (Workstream 5), sitting
  alongside the app but **not yet wired into `index.html`**. The HTML still reads
  its own inline `GLOSSARY`, `MILESTONES`, `LANES`, nav buttons, etc. — it hasn't
  been pointed at the JSON files yet.
- `staticwebapp.config.json` — enforces `authenticated` on every route, using
  Azure Static Web Apps' preconfigured Entra provider. No custom app registration
  needed for this stage; anyone in the pre-configured provider's directory can
  authenticate. Tenant-restriction (Section 11 of the guide, Decision D3) is a
  later step once you're beyond internal testing.
- Two CI/CD options are included — use whichever matches where the repo lives:
  - `.github/workflows/azure-static-web-apps.yml` — if hosted on GitHub
  - `azure-pipelines.yml` — if hosted on Azure DevOps Repos
  Delete whichever one you don't use; having both present but unconfigured is fine,
  Azure only runs the one connected to the SWA resource.

## Why the JSON isn't wired in yet

Per the guide's own MVP sequencing (Section 16), "extract content" and "preserve
experience unchanged" are separate workstreams. Shipping the untouched HTML first
de-risks this test to *pure infrastructure*: auth, CI/CD, hosting, routing. Once
that's confirmed working, the next PR swaps the inline `const GLOSSARY = {...}`
etc. for `fetch('/content/glossary.json')` calls — a contained, reviewable change
against a known-good deployment, rather than debugging infra and content wiring
at the same time.

## One-time setup before first push

1. Create the Static Web App resource (Free tier) in the Azure Portal, pointing
   it at this repo/branch. Azure auto-populates the deployment token as a
   **GitHub secret** (`AZURE_STATIC_WEB_APPS_API_TOKEN`) if using GitHub —
   nothing to copy manually.
2. **Azure DevOps only:** the token isn't auto-injected the way it is for
   GitHub. Copy it from the SWA resource's "Manage deployment token" blade in
   the Azure Portal, and add it as a secret pipeline variable named
   `AZURE_STATIC_WEB_APPS_API_TOKEN` in the Azure DevOps pipeline settings.
3. Push to `main` (or open a PR to test the preview-environment flow first —
   recommended for the very first run).

## What "internally test" means here

- You're testing: does auth gate work, does the existing experience render
  correctly from Azure, does the PR → preview → merge → production flow work.
- You're **not** yet testing: JSON-driven content (that's next), SharePoint
  links (Workstream 6, not started), custom domain (Decision D4, deferred).
