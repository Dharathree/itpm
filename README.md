# Singlish → Sinhala Playwright Tests

This repository contains Playwright end-to-end tests for a Singlish-to-Sinhala converter (a web app). The primary test file is in the `singlish-test-automation` subfolder and validates real-time conversion output.

## What this repo includes

- `singlish-test-automation/tests/example.spec.ts` — Playwright test suite with multiple test cases (positive and negative) that interact with the converter at https://www.swifttranslator.com/.
- `playwright-report/` — previously generated HTML reports (if present).
- `test-results/` — example test artifacts.

## Quick contract

- Input: Singlish text typed into the app's input textarea
- Output: Sinhala (or mixed) text displayed by the web app
- Success: For tests marked `Pass`, actual output must exactly match expected output in the test data

## Prerequisites

- Node.js (LTS recommended). Node 14+ should work; Node 16+ is recommended for best compatibility.
- npm (bundled with Node.js).
- Internet access is required for tests that navigate to the live site (https://www.swifttranslator.com/).
# Singlish → Sinhala Playwright Tests

This repository contains Playwright end-to-end tests for a Singlish-to-Sinhala converter web app. The primary test file is in the `singlish-test-automation` subfolder and validates the app's real-time conversion output.

## What this repo includes

- `singlish-test-automation/tests/example.spec.ts` — Playwright test suite with positive and negative test cases that exercise the converter at https://www.swifttranslator.com/.
- `playwright-report/` — generated HTML reports (if present).
- `test-results/` — example test artifacts.

## Quick contract

- Input: Singlish text typed into the app's input textarea
- Output: Sinhala (or mixed) text displayed by the web app
- Success: Tests marked `Pass` require exact match of the expected string; negative tests assert inequality.

## Prerequisites

- Node.js (LTS recommended). Node 16+ is recommended.
- npm (bundled with Node.js).
- Internet access for tests that target the live site (https://www.swifttranslator.com/).

Verify Node/npm versions in PowerShell:

```powershell
node -v
npm -v
```

## Install dependencies (PowerShell)

From the repository root:

```powershell
# Install dependencies declared in package.json at root (if any)
npm install

# Ensure Playwright browsers are installed
npx playwright install
```

If you prefer to run the tests from the `singlish-test-automation` subproject (it has its own `package.json`):

```powershell
cd singlish-test-automation
npm install
npx playwright install
```

Notes:
- `npx playwright install` downloads browser binaries Playwright needs (Chromium, Firefox, WebKit by default).
- If you are behind a corporate proxy, configure npm and the environment accordingly before running `npm install`.

## Add optional npm scripts (suggested)

You can add these to `package.json` to simplify commands (example contents shown for guidance):

```json
"scripts": {
  "test": "npx playwright test",
  "test:headed": "npx playwright test --headed",
  "test:report": "npx playwright show-report"
}
```

Then run:

```powershell
npm run test
npm run test:headed
```

## Run tests

Run all tests (from repo root):

```powershell
npx playwright test
```

Run tests in the subproject:

```powershell
cd singlish-test-automation
npx playwright test
```

Run a single spec file:

```powershell
npx playwright test singlish-test-automation/tests/example.spec.ts
```

Run a specific test by name or id (grep):

```powershell
npx playwright test -g "Pos_Fun_001"
```

Run in headed mode to observe the browser:

```powershell
npx playwright test --headed
```

Run a specific browser project (if defined in `playwright.config.ts`):

```powershell
npx playwright test --project=chromium
```

## Where to look in the test code

- Input selector used by the tests: `textarea[placeholder="Input Your Singlish Text Here."]`
- Output selector used by the tests: `div.bg-slate-50.whitespace-pre-wrap`
- Primary test file: `singlish-test-automation/tests/example.spec.ts` — contains test case array with fields `id`, `name`, `input`, `expected`, `lengthType`, and `status`.

The tests navigate to `https://www.swifttranslator.com/`, type the `input` text, wait for conversion, then compare the app output with `expected`.

## Troubleshooting

- Timeout errors: increase test timeout by editing `test.setTimeout(...)` in the spec or adjust `playwright.config.ts`.
- Selector changes: if the app's DOM changes, update the selectors in `example.spec.ts`.
- Intermittent failures: re-run the test in headed mode to observe the UI and timing; consider adding small waits or improving retry logic in the spec.

## Reports

- Generate/show HTML test report (after a run):

```powershell
npx playwright show-report
```

- Report artifacts are stored in `playwright-report/` by default.

## CI suggestions

- Add a CI job that installs Node, runs `npm ci`, runs `npx playwright install --with-deps` and then `npx playwright test`.
- Persist `playwright-report` or `test-results` as build artifacts.

## Disclaimer

These tests target the live site at `https://www.swifttranslator.com/`. To test a different environment, change `APP_URL` inside `singlish-test-automation/tests/example.spec.ts`.

---

If you want, I can also:

- Add the suggested npm scripts to the root or subproject `package.json`.
- Create a minimal GitHub Actions workflow to run tests on push.

Created/updated to make installing and running Playwright tests on Windows PowerShell straightforward.
