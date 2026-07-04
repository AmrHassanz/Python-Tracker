# Antigravity

A head-to-head tracker for Angela Yu's *100 Days of Code* Python course —
built for two people to log daily progress and compete. Progress auto-saves
straight to this GitHub repo, so it stays in sync between two laptops.

## Run it locally

```bash
python3 -m http.server 8000
```
then visit `http://localhost:8000`.

## One-time setup: connect the app to GitHub

Each person does this once, in their own browser:

1. Open the site → **⚙ Settings**
2. Fill in:
   - **Repo owner**: your GitHub username (e.g. `amrhassanz`)
   - **Repo name**: e.g. `antigravity` or `Python-Tracker` — whatever you named it
   - **Branch**: `main`
   - **File path**: `data/progress.json`
3. Create a personal access token (see below), paste it into **Your personal access token**
4. Click **Save connection** — it'll test itself and show a green ✓ if it worked

### Creating a token (fine-grained, scoped to just this repo)

1. GitHub → click your avatar (top right) → **Settings**
2. Left sidebar, scroll down → **Developer settings**
3. **Personal access tokens → Fine-grained tokens → Generate new token**
4. Resource owner: your account
5. Repository access: **Only select repositories** → choose this repo
6. Under **Permissions → Repository permissions**, set **Contents: Read and write**
7. Set an expiration (90 or 100 days lines up nicely with the challenge — you can always generate a new one later)
8. **Generate token**, copy it immediately (GitHub only shows it once)
9. Paste it into the app's Settings panel

**If your sister isn't the repo owner:** add her as a collaborator first —
repo → **Settings → Collaborators → Add people**. She needs at least write
access to the repo before her own token can push to it.

The token is stored only in `localStorage` in that one browser — it's never
written into `progress.json`, never committed, and never leaves that device.

## Using it day to day

- Pick who's logging with the switch at the top
- Click a day in the Mission Log grid → mark it done → add a note
- It autosaves to your browser instantly, and pushes to GitHub ~1.5s after
  your last edit (once connected) — no manual export needed
- Click **Pull latest** any time to grab the other person's updates on demand
  (it also quietly checks every 60 seconds in the background)
- **Export (backup)** still works if you ever want a manual local copy

## Deployment

See `strategy.md` for the full plan — short version: push this repo to
GitHub and turn on GitHub Pages (Settings → Pages → deploy from `main`).
