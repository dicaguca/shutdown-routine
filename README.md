# shutdown-routine

A guided evening shutdown routine: a single-page app that walks through 11 steps, one screen at a time.

**Live:** https://night.sadhanas.app (Cloudflare Pages, behind Cloudflare Access) and https://dicaguca.github.io/shutdown-routine/ (GitHub Pages). Both deploy from `main`.

## Steps

1. Activate SDR with your shiny Sunny
2. Listen to Waking Up Moment (opens Waking Up)
3. Planning for Tomorrow (with a reminder to keep it brief)
4. Daily Check-In (opens habits.sadhanas.app)
5. Mantras (built-in notes editor)
6. Lessons & Wisdom (built-in notes editor)
7. Core Commitments (opens a ChatGPT project chat)
8. D RAM 2025 Review
9. Write D Journal Entry
10. Close the Routine with Sunny
11. Get Ready for Your Rooting Ritual

Each screen has its own URL: `#/welcome`, `#/step1` … `#/step11`, `#/complete`.

## How it's built

- Everything is in `index.html`: React 18 and Tailwind from CDNs, with JSX compiled in the browser by Babel standalone.
- Babel is pinned to `7.23.9` and the script uses the classic JSX runtime pragma. Newer Babel versions broke the page, so don't unpin it.
- Mantras and Lessons notes save to the browser's `localStorage` (`shutdownRoutineMantrasContent`, `shutdownRoutineLessonsContent`) as you type, and about a second later to the sadhanas API (`https://api.sadhanas.app/night-mantras` and `/night-lessons`, Cloudflare Worker + KV) as `{ html, updatedAt }`. When a note opens, whichever copy was edited last wins; a browser copy replaced by a newer cloud copy is kept under `<key>PreviousLocal`. The API only accepts requests from the origins listed in its `ALLOWED_ORIGINS`, which includes `https://night.sadhanas.app` but not GitHub Pages, so cloud sync works on the sadhanas.app address only.

## Making changes

- Step titles, descriptions and links are in the `ritualSteps` list in `index.html`.
- Commit to `main` and push. Cloudflare Pages and GitHub Pages both republish automatically.
- Sister app: [morning-routine](https://github.com/dicaguca/morning-routine).
