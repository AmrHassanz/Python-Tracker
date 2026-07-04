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
- The app reads that file on load. When you log a day, it updates your local
  copy and gives you an **Export** button — download the updated
  `progress.json`, commit it, push. GitHub Pages rebuilds automatically, and
  the next time either of you opens the site (or hits "Sync"), you'll see the
  latest state.
- This means **git commits are the sync mechanism** — no API keys, no
  databases, no monthly limits. It fits "if git is free, no issue."

Trade-off: it's not real-time. Updates show up after a commit + push, not
instantly. For a daily coding-streak tracker that's a completely fine cadence
(you're not logging every minute). If you outgrow this later, the natural
upgrade path is swapping the JSON file for a free Firebase/Supabase project —
same UI, different storage layer.

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

## 4. Features (v1 — built now)

- Segmented "logging as" switch (You / Sister) to avoid mixing up entries
- 10×10 day grid (or N-day grid based on `totalDays`) — click a day to
  toggle it done for whoever's selected, with an optional note
- Two horizontal "ascent tracks" with a rocket marker showing % complete
- Per-person stats: days completed, % complete, current streak, longest streak
- Head-to-head banner: who's leading and by how many days
- Milestone markers every 20 days (gold tick on the track + grid)
- Export progress.json (for committing) / Sync button (re-fetch from repo)
- Editable names, editable total-day count
- Local draft autosave (localStorage) so nothing's lost between edits/commits

## 5. Not in v1 (possible later upgrades)

- Real-time sync without manual commit/push (needs Firebase/Supabase — free
  tier available, adds an API key to manage)
- Daily reminder notifications
- Per-day linking to the actual course project/lesson name
- Public shareable read-only leaderboard link

## 6. Build order

1. ✅ Build the static app (`index.html`) with local state + localStorage
2. ✅ Seed `data/progress.json`
3. ⬜ Deploy to free hosting (GitHub Pages recommended) — next step, on request
4. ⬜ Walk through the commit/export/sync loop together once live

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
