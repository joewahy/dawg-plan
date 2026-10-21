# Contributing

How we work on Dawg Plan. Project context (architecture, data sources, open questions) is in [AGENTS.md](AGENTS.md).

## Setup

```bash
git clone https://github.com/joewahy/dawg-plan.git
cd dawg-plan
```

Load the extension in Chrome: `chrome://extensions` > Developer mode > Load unpacked.

## Workflow

1. Update `main` before starting: `git checkout main && git pull`.
2. Create a branch per task: `feat/gpa-panel`, `fix/name-matching`, `docs/readme-update`.
3. Make small, focused commits and keep PRs small.
4. Open a pull request into `main` using the PR template.
5. The other person reviews and approves. Do not merge your own PR without a review.
6. Squash-merge, then delete the branch.

Never push directly to `main`.

## Avoiding conflicts

- Agree on who owns which area before starting. Suggested split: one person on the content script and DOM selectors, the other on data sources and instructor name matching.
- Rebase or merge `main` into your branch often, and tell the other person before touching files in their area.
- Do not reformat code you are not changing.

## Code style

- Follow `.editorconfig` and match the surrounding code.
- Keep `manifest.json` permissions minimal. List exact host permissions and justify any new one in the PR.
- Do not add comments that only restate the code.

## Privacy and secrets

- Never commit NetIDs, credentials, API keys, or real student/registration data.
- Saved page fixtures under `test/fixtures/` must have personal info stripped before committing.
- The extension must only read the course code and instructor name from the page.

## Using AI coding agents

- `AGENTS.md` is committed and read directly by Codex.
- Claude Code reads `CLAUDE.md`, which is gitignored, so create your own local copy containing one line:

  ```
  @AGENTS.md
  ```

- Put shared project facts in `AGENTS.md`, not in your local `CLAUDE.md`, so both of us (and both tools) see the same context.
- Review agent-written code like any other code before opening a PR.
