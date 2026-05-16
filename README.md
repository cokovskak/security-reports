# security-reports

GitHub Actions pipeline that generates dummy penetration test reports and sends them to the [Pentest Pipeline Dashboard](https://github.com/cokovskak/advance-web-app) PWA backend.

---

## How it works

1. Workflow picks a report JSON from `reports/`
2. Injects the current run number, commit SHA, branch, and timestamp
3. POSTs the report to the PWA backend (`/api/reports`)
4. Backend stores the report and fires a Web Push notification to all subscribed browsers
5. Report is saved as a GitHub Actions artifact for download

---

## Report types

| File | Pipeline status | Severity summary |
|---|---|---|
| `scan-critical.json` | FAIL | 2 critical, 3 high, 3 medium, 4 low |
| `scan-medium.json` | PASS | 0 critical, 1 high, 4 medium, 2 low |
| `scan-clean.json` | PASS | 0 critical, 0 high, 0 medium, 2 low |

---

## Triggers

| Trigger | Report sent |
|---|---|
| Manual (`workflow_dispatch`) | Your choice from dropdown |
| Daily schedule (08:00 UTC) | `critical` |
| Push to `main` | `critical` |

---

## Required secrets

Add these in **Settings → Secrets and variables → Actions**:

| Secret | Description |
|---|---|
| `BACKEND_URL` | Public URL of the PWA backend (e.g. `https://your-domain.ngrok-free.app`) |
| `INGEST_TOKEN` | Bearer token from the backend `.env` file |

---

## Running manually

1. Go to **Actions → Send Pentest Report → Run workflow**
2. Pick a report type from the dropdown
3. Click **Run workflow**

The PWA dashboard updates automatically and a push notification fires in any subscribed browser.

---

## Adding real scan results

Replace the contents of the JSON files in `reports/` with output from actual tools, following the schema in the [PWA README](https://github.com/cokovskak/advance-web-app#report-json-schema). The workflow injects `id`, `timestamp`, `runNumber`, `commit`, and `branch` automatically — everything else comes from the file.
