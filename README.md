# Dawg Plan

A Chrome extension for University of Washington students that adds course and instructor info directly to the class registration page.

When you select a class, Dawg Plan shows:

- **Average GPA** for the course
- **RateMyProfessor rating** for the instructor
- **Instructor bio and personal website**

> Status: early planning. No code yet. See [AGENTS.md](AGENTS.md) for the architecture proposal, data sources, and open questions.

## Planned features

- Inline panel on UW registration pages, no extra tabs
- Per-course (and, where available, per-instructor) average GPA
- RateMyProfessor overall rating, difficulty, would-take-again %, and link
- Instructor bio with a link to their personal site or UW profile
- Local caching so lookups are fast and third-party sites are not hammered

## Install (once built)

1. Clone this repo.
2. Open `chrome://extensions` and enable **Developer mode**.
3. Click **Load unpacked** and select the extension folder.
4. Sign in to UW registration and select a class.

## Privacy

The extension only reads the course code and instructor name shown on the page. It does not read, store, or send your NetID, credentials, or registration records, and it has no telemetry.

## Limitations

- RateMyProfessor has no official public API, so that integration may break without notice.
- Matching instructor names across UW, RMP, and department sites is imperfect. Unmatched instructors are shown as "no match" rather than guessed.
- Bios depend on what instructors and departments publish.

## Development

Tooling is not set up yet. See [CONTRIBUTING.md](CONTRIBUTING.md) for the branch and PR workflow. Project context for contributors and AI agents lives in [AGENTS.md](AGENTS.md).

## Disclaimer

Not affiliated with or endorsed by the University of Washington or RateMyProfessors.
