# Weekly Analytics Briefs — OLJ & OT

One notebook (`weekly_report.ipynb`) builds **both** weekly briefs: one Word file and one email per brand.
It runs every Monday on GitHub Actions and can also be run by hand in Colab.

Each brief:
- **Page 1:** scorecard, top 10 articles (with a Discover note when Discover is 20%+ of an article's views),
  top countries, sources (GA4 + CMS + the acquisitions sheet)
- **Page 2:** appendices, iOS vs Android
- **Page 3:** Matomo top countries (Web / App / Total) and Matomo vs GA4

Files are named `OLJ_Brief_Oct5-11.docx` / `OT_Brief_Oct5-11.docx` (`Sep28-Oct4` when the week spans two months).
The notebook never makes up numbers: if a source can't be reached, the run stops and says why. Search Console is the
one exception: the brief still goes out, and its footnote says why the Discover notes are missing.

## Setup

**Repo secrets** (Settings → Secrets and variables → Actions → Secrets):

| Secret | What |
|---|---|
| `GSHEET_SA_KEY` | the service account's JSON key (weekly-report-bot@…): GA4, the acquisitions sheet, Search Console |
| `CMS_API_KEY` | CMS API key (new accounts, subscriptions) |
| `MATOMO_TOKEN` | Matomo API token (page 3) |
| `GMAIL_ADDRESS` / `GMAIL_APP_PASSWORD` | sending account + its 16-character App Password |

The workflow must pass `CMS_API_KEY` and `MATOMO_TOKEN` to the notebook in its `env:` section
(`MATOMO_TOKEN: ${{ secrets.MATOMO_TOKEN }}`).

**Recipients** (optional, Variables tab): `BRIEF_RECIPIENTS_OLJ` / `BRIEF_RECIPIENTS_OT`, comma-separated. When set,
they replace the lists in the notebook's ⚙️ Settings.

**Access the service account needs:** Viewer on the GA4 property, Viewer on the acquisitions Google Sheet, and
Restricted user on Search Console (`sc-domain:lorientlejour.com`), with the Search Console API enabled in its
Google Cloud project.

## Running

- **Scheduled:** Mondays, emails sent at 12:00 Beirut time (summer and winter handled by the workflow).
- **By hand:** Actions → the weekly brief workflow → **Run workflow**. Test runs only email `TEST_RECIPIENTS`.
  The Word files are in the run's Artifacts.
- **Colab:** turn on Notebook access for the `CMS_API_KEY` and `MATOMO_TOKEN` 🔑 secrets, upload
  `service_account.json` (again after every reconnect), Run all.

## Files

- `weekly_report.ipynb`: the notebook
- `requirements.txt`: Python packages
- `.github/workflows/weekly_brief.yml`: schedule + secrets
- `tests/matomo_test.ipynb`, `tests/gsc_test.ipynb`: read-only connection checks, run them in Colab when Matomo
  or Search Console misbehaves
- `.gitignore`: keeps keys and generated files out of the repo
