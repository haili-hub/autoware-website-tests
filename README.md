# auto-website-tests

[English](README.md) | [日本語](README.ja.md)

![CI](https://github.com/haili-hub/autoware-website-tests/actions/workflows/ci-e2e-tests.yml/badge.svg)

Playwright end-to-end tests for the [Autoware](https://autoware.org/) website navigation workflow, covering the journey from the homepage through to the GitHub repository README.

## Test coverage

| File | Test | Target |
| ---- | ---- | ------ |
| `tests/website-github-navigation/TC001_initial-access.ts` | Initial Website Access | autoware.org |
| `tests/website-github-navigation/TC002_find-github-link.ts` | Find and Access GitHub Link | autoware.org → github.com |
| `tests/website-github-navigation/TC003_repository-access.ts` | GitHub Repository Access | github.com/autowarefoundation |
| `tests/website-github-navigation/TC004_readme-translation.ts` | README Access | github.com/autowarefoundation/autoware |

## Requirements

- Node.js (LTS)
- Chromium (installed via Playwright)
- Internet access to `autoware.org` and `github.com`

## Setup

```bash
npm install
npx playwright install chromium
```

## Running tests

```bash
# Run all tests (HTML reporter)
npm test

# Run with UI mode
npm run test:ui

# Run with debug mode
npm run test:debug

# Open last HTML report
npm run test:report
```

## Allure reporting

```bash
# Run tests and collect Allure results
npm run test:allure

# Generate and open the Allure report
npm run allure:report
```

Results are written to `allure-results/` and the generated report to `allure-report/`.

## Configuration

`playwright.config.ts` — Chromium only, with the following timeouts to accommodate the `autoware.org` site load:

| Setting | Value |
| ------- | ----- |
| Test timeout | 60s |
| Navigation timeout | 60s |
| Action timeout | 30s |
| Retries (CI) | 2 |
| Workers (CI) | 1 |

The HTML reporter is used by default. Set `ALLURE=true` (via `npm run test:allure`) to switch to the Allure reporter. Traces are collected on first retry.

## Test plan

The source test plan lives at `specs/autoware-website-navigation.plan.md`.

## Notes

- **Browser translation** (right-click › Translate to Japanese) is a browser-native feature and cannot be automated via Playwright. Translation quality requires manual verification.
- `autoware.org` uses background polling that prevents `networkidle` from ever resolving; tests use the default `load` event instead.
