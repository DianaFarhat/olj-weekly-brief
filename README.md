# Weekly Analytics Briefs — OLJ & OT

Two fully independent, separately-scheduled reports — each pulls Acquisitions/Churn, GA4, and account
creation numbers filtered to its own brand, assembles a Word doc, and emails it automatically.

- `weekly_report_OLJ.ipynb` + `.github/workflows/weekly_report_olj.yml` — Mondays 08:00 Beirut time
- `weekly_report_OT.ipynb` + `.github/workflows/weekly_report_ot.yml` — Mondays 08:10 Beirut time

They share nothing at runtime — each notebook has its own copy of every function it needs, so one can
fail, be edited, or be disabled without touching the other.

## Setup

1. **Add three repo secrets** (Settings → Secrets and variables → Actions) — shared by both workflows:
   - `GSHEET_SA_KEY` — the full contents of your Google service account's JSON key
   - `GMAIL_ADDRESS` — the Gmail/Workspace address sending the email
   - `GMAIL_APP_PASSWORD` — a 16-character App Password (not your normal password; requires
     2-Step Verification: myaccount.google.com/apppasswords)

2. **Test each one immediately** rather than waiting for Monday: Actions tab → pick either
   "Weekly Analytics Brief (OLJ)" or "(OT)" → **Run workflow**. Check the run's logs, and download
   the output notebook / `.docx` from that run's Artifacts section to see exactly what it produced.

3. **Schedule**: both run Monday morning Beirut time (`cron: "0 5 * * 1"` / `"10 5 * * 1"`, since
   GitHub Actions cron is always UTC). This does **not** auto-adjust for Lebanon's DST changes —
   when Beirut shifts to UTC+2 in winter, change the `5` to a `6` in both workflow files.

## Files

- `weekly_report_OLJ.ipynb` / `weekly_report_OT.ipynb` — the two reports, each runs top to bottom independently
- `requirements.txt` — Python dependencies (shared by both)
- `.github/workflows/weekly_report_olj.yml` / `weekly_report_ot.yml` — schedule + secret wiring, one per report
- `.gitignore` — blocks `service_account.json`, `token.json`, and generated `.docx`/output notebook
  files from ever being committed
