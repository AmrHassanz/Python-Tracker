# Antigravity

A head-to-head tracker for Angela Yu's *100 Days of Code* Python course —
built for two people to log daily progress and compete.

## Run it locally

Just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

then visit `http://localhost:8000`.

## How it works

- Pick who's logging with the switch at the top.
- Click a day in the Mission Log grid to mark it done and add a note.
- Progress autosaves to your browser (`localStorage`).
- Hit **Export progress.json**, replace `data/progress.json` in this repo,
  commit, and push — that's how you and your sister sync progress across
  devices for free (no backend needed). Hit **Sync** to pull the latest.

## Deployment

See `strategy.md` for the full plan — short version: push this repo to
GitHub and turn on GitHub Pages (Settings → Pages → deploy from `main`).
Free, and pairs naturally with the git-based sync above.
