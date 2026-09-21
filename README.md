# OLJ Weekly Analytics Brief

Automated weekly report — Acquisitions/Churn, GA4, and account creation, all filtered to OLJ —
assembled into a Word doc and emailed automatically every Monday at 8:00 AM Beirut time.

## Setup

1. **Add three repo secrets** (Settings → Secrets and variables → Actions):
   - `GSHEET_SA_KEY` — the full contents of your Google service account's JSON key
   - `GMAIL_ADDRESS` — the Gmail/Workspace address sending the email
   - `GMAIL_APP_PASSWORD` — a 16-character App Password (not your normal password; requires
     2-Step Verification: myaccount.google.com/apppasswords)

2. **Test it immediately** rather than waiting for Monday: Actions tab → "Weekly Analytics Brief
   (OLJ)" → **Run workflow**. Check the run's logs, and download `output.ipynb` / the `.docx` from
   that run's Artifacts section to see exactly what it produced.

3. **Schedule**: runs Monday 08:00 Beirut time (`cron: "0 5 * * 1"`, since GitHub Actions cron is
   always UTC). This does **not** auto-adjust for Lebanon's DST changes — when Beirut shifts to
   UTC+2 in winter, change the `5` to a `6` in `.github/workflows/weekly_report.yml`.

## Files

- `weekly_report.ipynb` — the report itself, runs top to bottom
- `requirements.txt` — Python dependencies
- `.github/workflows/weekly_report.yml` — the schedule + secret wiring
- `.gitignore` — blocks `service_account.json`, `token.json`, and generated `.docx`/`output.ipynb`
  files from ever being committed
