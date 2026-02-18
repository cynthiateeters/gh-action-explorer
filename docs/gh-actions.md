# GitHub Actions: `shots.yml` tutorial

This short guide explains the `./github/workflows/shots.yml` workflow in this repository. It's written for beginners and shows what the workflow does, why it exists, and how to change common settings.

## Purpose

- Automate screenshots of the live site using `shot-scraper`.
- Timestamp and store screenshots in `screenshots/`.
- Prune older screenshots to limit repository bloat.
- Commit resulting changes back to the repository (current configuration).

## Where it runs

- The workflow runs on a GitHub Actions runner (a temporary `ubuntu-latest` VM). Files created during the job live only on that runner unless the workflow either commits them to git or uploads them as artifacts.

## Triggers and protections

- Trigger: runs on `push` and can be started manually with `workflow_dispatch`.
- Loop protections included:
  - `paths-ignore: ['screenshots/**']` — pushes that only change files in `screenshots/` will not re-trigger the workflow.
  - `if: github.actor != 'github-actions[bot]'` — the job will skip if the actor is the Actions bot.

These two protections prevent the workflow from creating commits that immediately re-run it and cause an infinite loop.

## Main steps (what the workflow actually does)

1. Checkout the repo with `actions/checkout@v4`.
2. Set up Python (3.13) so `pip` is available.
3. Cache pip packages and Playwright browsers to speed up repeated runs.
4. Install dependencies: `pip install -r requirements.txt` (this installs `shot-scraper`).
5. Install Playwright browsers used by `shot-scraper` with `shot-scraper install`.
6. Run `shot-scraper multi shots.yml` — this reads `shots.yml` to take the screenshots.
7. Timestamp and move generated images into `screenshots/YYYY-MM-DD_HH-MM-SSZ-FILENAME` so filenames are unique and sortable.
8. Prune the `screenshots/` folder so only the 5 newest files remain (helps limit repository growth).
9. Commit and push any repository changes (the workflow config currently commits the screenshots).

## Important notes about `.gitignore` and runner files

- If `screenshots/` is in `.gitignore` and the screenshots are untracked, `git add -A` will skip them and they will remain only on the runner (and then disappear when the job finishes).
- If screenshots are already tracked (committed previously) `.gitignore` will not untrack them — they will continue to be updated and pushed until you explicitly untrack them with `git rm --cached`.

## Alternatives to committing images (recommended for many images)

- Upload artifacts instead of committing (recommended to avoid repo bloat):

```yaml
- name: Upload screenshots
  uses: actions/upload-artifact@v4
  with:
    name: screenshots-${{ github.run_id }}
    path: screenshots/
    retention-days: 30
```

- Push screenshots to a separate branch (keeps main history clean).
- Use Git LFS for large binaries.
- Store images externally (S3, GCS) and commit only a small metadata file to the repo.

## Keep only N latest screenshots

- The workflow currently keeps the 5 newest images. To change that number, update the prune step, for example to keep 10: change `tail -n +6` to `tail -n +11`.

## How to download artifacts (if you switch to upload-artifact)

- Use the GitHub CLI to download artifacts from the latest run:

```bash
gh run download --dir downloaded-artifacts
```

Or download a specific run's artifacts with its run id:

```bash
gh run download <run-id> --dir downloaded-artifacts
```

## Quick troubleshooting

- Action does not trigger: check the Actions tab and that workflows are enabled on the repository.
- Shot-scraper fails to capture: try adding `wait:` to `shots.yml` to give the page time to load, or add JavaScript to hide dynamic elements before capture.
- Repo grows too large: switch to artifacts or external storage, or use the prune strategy.

## Small changes you can make

- Change image size/quality: edit `shots.yml` entries (set `width`, `height`, `quality`).
- Change retention/count: modify the prune step's `tail -n` argument or switch to artifact retention.
- Avoid commits: remove the commit step and add `actions/upload-artifact` instead.

---

If you want, I can update the workflow to upload artifacts instead of committing, or push screenshots to a separate branch — tell me which option you prefer and I'll implement it.
