# Dawg Plan: project info

Shared project context for AI coding agents. Codex reads this file directly; Claude Code reads it through the `@AGENTS.md` import in `CLAUDE.md`. Edit this file, not `CLAUDE.md`, when project facts change.

## What this is

A Chrome extension (Manifest V3) for University of Washington students. It runs on UW's class registration pages. When a student selects or hovers a class/section, the extension shows an inline panel with:

1. **Average GPA** for the course (and, where data allows, for the specific instructor teaching it).
2. **RateMyProfessor rating** for the instructor (overall rating, difficulty, would-take-again %, number of ratings, link to the RMP page).
3. **Instructor bio and personal website**, linking to their own site (or UW faculty profile as fallback) with a short bio.

Goal: help students pick sections without opening three other tabs.

## Status

Greenfield. The repo has no code yet. Everything below under "Planned architecture" is a proposal, not a decision. Confirm with the user before locking in a stack.

## Target pages (verify before relying on them)

- UW registration: `https://register.uw.edu/` (unsure of the exact current URL structure; inspect the live page before writing selectors)
- MyPlan: `https://myplan.uw.edu/` (possible secondary target)
- UW Time Schedule: `https://www.washington.edu/students/timeschd/`

These pages require UW NetID login. Agents cannot log in on the user's behalf. Content-script selectors must be developed from DOM the user supplies (saved HTML, screenshots, or devtools output) or from the user's own browser session.

## Planned architecture

- **Manifest V3**, content script injected on the registration URL patterns only (narrow `matches`, no `<all_urls>`).
- **Content script**: detects selected/hovered course rows (MutationObserver, since registration UIs are often SPA-like), extracts course code (e.g. `CSE 143`) and instructor name, requests data from the service worker, renders a panel.
- **Service worker (background)**: owns all cross-origin fetches, caching (`chrome.storage.local` with TTL), and rate limiting. Content scripts should not fetch third-party origins directly.
- **Optional popup/options page**: toggle data sources, clear cache.
- Plain JS or TypeScript with a small bundler (Vite or esbuild) is likely enough. No framework needed unless the panel UI grows.

## Data sources (all need verification)

| Data | Candidate source | Notes |
|---|---|---|
| Average GPA | UW grade distribution data (UW Registrar publishes grade data; community datasets/tools such as DawgPath exist) | Need to confirm what is public, its license, and whether it is queryable per instructor. Unsure which is best. |
| RateMyProfessor | RMP's unofficial GraphQL endpoint (`ratemyprofessors.com/graphql`) | No official public API. It is undocumented and may change or block requests. Must be called from the service worker, needs host permission. Check ToS risk with the user. |
| Instructor bio / website | UW directory or department faculty pages; instructor personal sites | No single source. Likely needs per-department scraping or a manually curated JSON seed. Treat as the hardest part. |

Name matching is a core problem: registration shows "LAST,FIRST M" style names, while RMP and department pages differ. Build a normalizer and a manual override map (`data/instructor-overrides.json`) rather than trusting fuzzy matches silently. Show "no match" instead of a wrong professor.

## Conventions

- Match the style of surrounding code. Keep diffs small and focused.
- Least-privilege permissions in `manifest.json`: list exact host permissions, add only what is used.
- Never collect, store, or transmit NetID credentials or any student record data. The extension only reads course code and instructor name from the page.
- Cache third-party responses; do not hammer RMP or grade sources on every hover.
- Show loading, empty, and error states in the panel. A failed lookup must not break the registration page.
- No telemetry.

## Testing and loading

- Load unpacked: `chrome://extensions` > Developer mode > Load unpacked > select the extension directory (or build output dir once a build step exists).
- Because the real pages sit behind login, keep saved DOM fixtures under `test/fixtures/` and unit test the parsing and name-matching logic against them.
- Test commands will be added here once tooling exists.

## Open questions for the user

- Which exact registration page(s): `register.uw.edu`, MyPlan, or both?
- Preferred stack: plain JS, TypeScript, or a framework?
- GPA source preference, and is per-instructor GPA required or is per-course enough?
- Bio source: curated list, UW directory scrape, or link-out only for v1?
- Intended distribution: personal unpacked use, or Chrome Web Store?
