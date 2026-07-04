# Antigravity — 100 Days of Code Tracker & Sibling Leaderboard

A head-to-head progress tracker for Angela Yu's *100 Days of Code: The Complete
Python Pro Bootcamp* (Udemy), built for two competitors to log daily progress
and see who's pulling ahead.

> Note: the course is 100 days, not 90 — I built the tracker with a
> configurable "total days" setting (default 100) so you can dial it to 90 if
> that's the plan you're actually following. Change it any time in ⚙️ Settings.

## 1. Goals

- [x] Track daily progress for two people independently
- [x] Show a live head-to-head: who's ahead, by how much, streaks
- [x] Work as a pure static site (no server, no paid backend)
- [x] Persist progress for free, using Git/GitHub as the "database"
- [x] Feel like *your* project, not a generic tracker

## 2. Why this architecture

The two hard requirements were "free hosting" and "shared progress between two
people." A normal backend (database + server) would cost money or need a paid
tier eventually. Instead:

- The site is a **static HTML/CSS/JS app** — deployable free forever on
  GitHub Pages (or Netlify/Vercel free tier, or Cloudflare Pages).
- Progress lives in **`data/progress.json`**, a plain file in the repo.
- The app talks to GitHub's REST API directly to read and write that file —
  no database, no third-party service, no cost. GitHub *is* the backend.

### v1 → v2: from manual export to direct auto-save

**v1 (first pass):** the app only wrote to `localStorage`. To sync, you'd
click Export, manually replace `data/progress.json`, and commit/push
yourself. Simple, but easy to forget and not truly "saved" until you
remembered that manual step.

**v2 (current):** each person adds a personal access token (scoped only to
this repo, Contents: read/write) in Settings. The app now:

- Fetches the live file straight from GitHub on load and every 60s in the
  background, so the other person's updates show up on their own
- Auto-commits your changes to GitHub ~1.5s after you stop editing, via
  GitHub's Contents API (`GET` for the current file + sha, `PUT` to update
  it) — no export, no manual git commands
- Merges intelligently before writing: it always keeps your own edits, and
  trusts whatever's already on GitHub for the other person's data, so one
  person saving doesn't overwrite the other's progress
- Retries once automatically if both people happen to save at nearly the
  same moment (a 409 conflict from GitHub, meaning the file changed
  underneath the request)
- Still ships an **Export** button as a manual backup/escape hatch

Trade-off versus a "real" backend: writes go through GitHub's API rate limits
(generous — 5,000 authenticated requests/hour, nowhere near what two people
logging daily entries would use) and there's a small delay (~1.5s debounce +
however long the API takes), not literal real-time push. For this use case
that's imperceptible in practice.

Security trade-off, stated plainly: the token lives in `localStorage` in
each person's own browser, on a site that's publicly reachable (GitHub Pages
is a public URL). That's why the setup instructions insist on a
**fine-grained** token scoped to just this one repo with only
Contents:read/write — worst case if it ever leaked, someone could edit files
in this tracker repo, nothing else.

## 3. Data model

```json
{
  "totalDays": 100,
  "competitors": [
    { "id": "p1", "name": "You", "accent": "a" },
    { "id": "p2", "name": "Sister", "accent": "b" }
  ],
  "progress": {
    "p1": { "1": { "done": true, "date": "2026-07-01", "note": "loops, basic syntax" } },
    "p2": {}
  }
}
```

## 4. Features (v2 — current)

- Segmented "logging as" switch (You / Sister) to avoid mixing up entries
- 10×10 day grid (or N-day grid based on `totalDays`) — click a day to
  toggle it done for whoever's selected, with an optional note
- Two horizontal "ascent tracks" with a rocket marker showing % complete
- Per-person stats: days completed, % complete, current streak, longest streak
- Head-to-head banner: who's leading and by how many days
- Milestone markers every 20 days (gold tick on the track + grid)
- **Direct GitHub auto-save**: connect once with a repo + fine-grained token,
  then every edit debounce-commits straight to `data/progress.json`
- **Pull latest** button + a quiet 60-second background poll, so the other
  person's saves show up without anyone touching git
- Live connection status indicator (connected / saving / error, with the
  reason shown inline)
- Export progress.json as a manual backup/fallback
- Editable names, editable total-day count
- Local draft autosave (localStorage) so nothing's lost even offline

## 5. Not in scope (possible later upgrades)

- True real-time push (would need a websocket-based backend like
  Firebase/Supabase — free tier available, adds a different kind of key to
  manage instead of a GitHub token)
- Daily reminder notifications
- Per-day linking to the actual course project/lesson name
- Public shareable read-only leaderboard link

## 6. Build order

1. ✅ Build the static app (`index.html`) with local state + localStorage
2. ✅ Seed `data/progress.json`
3. ✅ Deploy to GitHub Pages
4. ✅ Upgrade to direct GitHub API auto-save (v2)
5. ⬜ Each of you generates a token and connects, per the README

## 7. Deployment plan (for the next step)

Recommended: **GitHub Pages**, because it's what you already mentioned, it's
free with no account limits for a public repo, and it pairs naturally with
the git-as-database sync approach above.

Rough steps (to do together next):
1. Create a GitHub repo (e.g. `antigravity`)
2. Push these files to it
3. Repo → Settings → Pages → deploy from `main` branch, root folder
4. Site goes live at `https://<username>.github.io/antigravity/`
5. Both of you bookmark it; whoever logs progress exports + commits
   `data/progress.json` afterward

Alternatives if you'd rather not use GitHub Pages: Netlify or Vercel free
tier (drag-and-drop deploy, or connect the same GitHub repo for
auto-deploys on every push — arguably even smoother than Pages).
