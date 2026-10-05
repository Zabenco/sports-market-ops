# sports-market-ops

Source for my sports market operations site: a settlement casebook, live calls, market proposals, runbooks, and tools.

Live site: https://markets.zaben.co/

## One-time setup

1. Create a public GitHub repo named `sports-market-ops` and push these files to the `main` branch.
2. In the repo, go to **Settings > Pages > Build and deployment** and set **Source** to **GitHub Actions**.
3. Push any change (or run the workflow by hand from the **Actions** tab). The site publishes in about a minute.
4. Put your resume at `docs/resume.pdf`. The home page links to it.
5. Read through `docs/index.md` and rewrite the intro in your own words.

If you name the repo something else, update `site_url`, `repo_url`, and `repo_name` in `mkdocs.yml`, plus the commit-history link in `docs/live-calls/index.md`.

## Preview locally

```
python -m venv .venv
.venv\Scripts\activate          # Windows. On Mac/Linux: source .venv/bin/activate
pip install -r requirements.txt
zensical serve --config-file mkdocs.yml
```

Then open http://localhost:8000. The page reloads as you edit.

## Adding a casebook entry

1. Copy `templates/casebook-entry.md` into `docs/casebook/`.
2. Name it `league-short-topic.md`, for example `tennis-retirement-mid-match.md`. The sidebar sorts by file name, so the league prefix groups entries by sport.
3. Fill it in. Replace every placeholder, and link a real source for every rule you cite.
4. Add a row to the table in `docs/casebook/index.md`.
5. Add it to **Recent entries** on `docs/index.md` (keep the five newest).
6. Commit and push.

## Making a live call

The value of a live call is proof that it was written before settlement, so the order matters.

1. During the event, copy `templates/live-call.md` into `docs/live-calls/` as `YYYY-MM-DD-league-teams.md`.
2. Fill in **The call** section only. Leave **Outcome** as-is.
3. Commit and push right away:
   ```
   git add docs/live-calls/
   git commit -m "Live call: NFL Team A at Team B"
   git push
   ```
4. After the market settles, fill in **Outcome** in a new commit. Do not change anything in **The call**. Anyone can check this in the commit history.
5. Update the row in `docs/live-calls/index.md` and the counts on the home page.

## Proposals and runbooks

Same pattern: copy the template from `templates/`, save it in `docs/proposals/` or `docs/runbooks/`, add a row to that section's table, push.

## Custom domain

This site is meant to live at a subdomain of zaben.co (`markets.zaben.co`).

1. At your DNS provider, add a **CNAME** record: name `markets`, value `zabenco.github.io` (no repo name).
2. In the repo, go to **Settings > Pages > Custom domain**, enter `markets.zaben.co`, and save. No CNAME file is needed, because the site deploys through GitHub Actions.
3. Wait for the DNS check to pass, then tick **Enforce HTTPS**. The certificate can take up to an hour.
4. `site_url` in `mkdocs.yml` is already set to `https://markets.zaben.co/`.
5. Recommended: verify `zaben.co` under your GitHub account's **Settings > Pages** (adds a TXT record). This stops anyone else from claiming a subdomain of it on GitHub Pages.

## About the build

The site is built with [Zensical](https://zensical.org), from the team behind Material for MkDocs. Material for MkDocs reaches end of life on November 5, 2026, and Zensical reads the same `mkdocs.yml`. If Zensical ever causes trouble, change `requirements.txt` to `mkdocs-material`, replace the build step in `.github/workflows/publish.yml` with `mkdocs build`, and preview locally with `mkdocs serve`.

## Design notes

- Typeface: Public Sans, the open-source typeface made for US government sites. Plain and official, which fits a rulebook.
- Color: dark ink on white, with one green used for links and the proposed-clarification box. Dark mode follows the reader's system setting.
- No hero section, cards, icons, or animation. A reader who clicks the link is already interested; the job of the site is to get them to the content.
- The one styled element is the clarification box in each casebook entry, because that's the part a reader is looking for.
