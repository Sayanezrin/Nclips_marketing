# NCLIPS.AI — Marketing Site + Interactive App Prototype

A single-file static site: marketing landing page for NCLIPS.AI, with a fully
click-through prototype of the real app embedded in the hero section (built to
match the actual product screens 1:1 — Home, Courses, Exam, Settings, and every
Settings sub-screen).

**This is front end only.** There is no backend, no server, no database, and no
real authentication. Every screen renders from data that's hardcoded directly in
the HTML/JS. Buttons, tabs, and toggles just switch which pre-built screen is
visible — nothing is fetched, saved, or sent anywhere.

## Running it locally

No build step, no dependencies. Either:

- Double-click `index.html` to open it directly in a browser, or
- Serve it (recommended, avoids any local-file quirks):
  ```bash
  python3 -m http.server 8000
  # then open http://localhost:8000
  ```

## File structure

```
.
├── index.html   ← everything: markup, CSS (in <style>), JS (in <script>)
└── README.md
```

Everything lives in one file on purpose (it started as a shareable prototype).
The first thing a backend dev will likely want to do is break this apart into a
real app structure (see "Suggested next steps" below) — that's expected, not a
constraint you need to preserve.

## What's real vs. what's fake

- **Real:** all copy, colors, fonts, layout, the exact screens/flows of the app
  (based on actual product screenshots), and the navigation logic between them.
- **Fake:** every piece of data on screen — the user's name/email, course
  progress percentages, chapter lists, watch history, bookmarks, notification
  toggle states, subscription renewal dates. All of it is placeholder content
  written directly into the HTML.

## Data model implied by the UI

This is the shape of data the backend will need to serve, inferred directly
from the screens already built. Useful as a first pass at your API/DB schema:

**User**
- name, email, auth method (e.g. "via email" / "via Google")

**Course**
- title, description, banner style, chapter count, total hours
- subscription tier (e.g. "Annual"), renewal date
- list of chapters

**Chapter / Lesson**
- number, title, category (e.g. "Pathophysiology")
- video reference, duration
- per-user watch progress (% watched, resume timestamp, completed flag)

**Bookmark**
- reference to a chapter/video, category label, saved timestamp

**Watch history entry**
- reference to a chapter/video, category, % watched or "completed", timestamp

**Notification preferences** (per user, toggle booleans)
- purchases, renewal reminders, newsletter, weekly digest

**Subscription / Billing**
- current plan, renewal date, cancellation state
- (purchases are handled by Apple/Google IAP per the real app's refund policy —
  see the Refund Policy screen in Settings for the exact terms already agreed)

## Suggested next steps for backend integration

1. Pull the inline `<style>` and `<script>` out into their own files once this
   moves into a real framework (Next.js, etc.) — trivial copy-paste, no logic
   changes needed.
2. The screen-switching logic lives in the `App prototype: screen navigation`
   section of the `<script>` block (search for `appFrame`). Right now it just
   toggles which hardcoded `.app-screen` div is visible — that's the natural
   seam to replace with real routing + data fetching per screen.
3. Anywhere you see literal text like "test account", "39% watched", "12-day
   streak", or the course/chapter data — that's a hardcoded stand-in for an API
   response.
4. Auth, payments, and video hosting/streaming aren't represented in the
   frontend at all yet (the video "player" is just a static thumbnail) — those
   are wide open for you to design.

## Pushing this to GitHub

From this folder:

```bash
git init
git add .
git commit -m "Initial commit: NCLIPS.AI marketing site + app prototype"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

(Create the empty repo on GitHub first — github.com → New repository — without
a README/gitignore, so there's nothing to conflict with the push above.)

## Handing it to the other developer

Pick whichever fits how you two work:

- **Private repo + collaborator (simplest):** GitHub repo → Settings →
  Collaborators → add their GitHub username. They then just `git clone` it.
- **Public repo:** share the URL directly, no invite needed.
- **Fork model:** they fork your repo, work in their fork, and open a PR back
  to yours when ready.

Once they have the repo, they point Codex at it (open the repo in their Codex
environment, or reference the GitHub URL, depending on how they normally start
a task) and can work directly against `index.html` — the README above should
give it enough context to know what to build without you needing to explain it
live.
