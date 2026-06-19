# Daily Mental Puzzle Game Generator

## Mission

Every day at 3:00 a.m. Peru time, create and publish one new original web puzzle game for morning brain exercise inside this repository.

The game must be playable in 10 to 15 minutes, feel fresh and mentally engaging, work well on desktop and mobile, and avoid repeating ideas already recorded in `memory.md`.

## Repository Context

This base repository is `CodexCloudBase`.

The public arcade name is `NicoSavesTheWorld Brain Training`.

This repository is the single GitHub Pages site. Do not create a new repository for each game. Each daily game must live in its own folder inside this repository.

Expected root files and folders:

- `instructions.md`: the operating instructions for the daily agent run.
- `memory.md`: the log of previously generated games and concepts.
- `index.html`: the public arcade homepage listing all generated games.
- `Assets/`: optional shared visual assets.
- `<game-slug>/index.html`: one folder per generated game, each with its own playable static page.

GitHub Pages should be enabled once for this repository from branch `main`, folder `/`.

## Daily Workflow

1. Read `instructions.md`.
2. Read `memory.md`.
3. Inspect existing game folders and the root `index.html` so the new game fits the arcade without duplicating earlier concepts.
4. Invent one new puzzle game concept that is meaningfully different from previous entries.
5. Choose a short, memorable game title.
6. Generate a repository-safe slug using the title plus date or short code, such as `mindgrid-2026-06-19` or `glyph-lock-7k3-2026-06-19`.
7. Create a new folder in this repository using that slug.
8. Build the full playable game at `<game-slug>/index.html`.
9. If useful, create visual assets and place them either in `<game-slug>/Assets/` or in the root `Assets/` folder when they are intended to be shared.
10. Update the root `index.html` arcade homepage by adding the new game, newest first.
11. Update `memory.md` with the new game entry.
12. Test the game locally in the cloud environment as much as available.
13. Commit all changes with a clear message.
14. Ensure `origin` exists without printing its URL:

    ```bash
    git remote | grep -qx origin || git remote add origin https://github.com/nsanchezBG/CodexCloudBase.git
    ```

15. Do not expect `GH_PUSH_TOKEN` to be visible during the agent phase. It is a Codex environment secret used by the environment setup script before the task starts. The setup script should already have configured Git credentials for `origin`.
16. Before pushing, clear any GitHub injected credential header so Git uses the configured `origin` credentials:

    ```bash
    git config --global --unset-all http.https://github.com/.extraheader >/dev/null 2>&1 || true
    git config --local --unset-all http.https://github.com/.extraheader >/dev/null 2>&1 || true
    ```

17. Do not run `git remote -v`, `git remote get-url origin`, or any other command that could print an authenticated remote URL.
18. Push the committed work directly to GitHub `main` with:

    ```bash
    git push origin HEAD:main
    ```

19. Verify that the new folder, root `index.html`, and `memory.md` changes are visible on GitHub `main`.
20. Report the live GitHub Pages URL for the new game.

A local commit is not complete. The run is only complete after the changes are pushed to GitHub `main`.

## URL Rules

Use this format for live game URLs:

`https://nsanchezBG.github.io/CodexCloudBase/<game-slug>/`

Use this format for the arcade URL:

`https://nsanchezBG.github.io/CodexCloudBase/`

If GitHub Pages is still building or cannot be reached during the run, record the expected URL and mention the deployment status in the final report.

## Gameplay Quality Bar

Each game must be:

- Original enough that it does not feel like a direct repeat of an earlier game.
- Playable in a single `index.html` file inside its own folder, except for optional files in that game's `Assets/` folder.
- Deployable on GitHub Pages without a backend.
- Responsive for desktop and mobile.
- Visually polished, modern, calm, and pleasant.
- Clear without relying on long instructions.
- Interesting for at least 10 to 15 minutes.
- Focused on mental exercise: logic, pattern recognition, memory, deduction, spatial reasoning, sequencing, constraint solving, language reasoning, or numerical intuition.

Prefer compact, elegant mechanics over large complicated systems.

## Technical Constraints

Each generated game folder should contain:

- `index.html`
- optional `Assets/`
- optional `README.md`

The game may use vanilla HTML/CSS/JavaScript, React via CDN and Babel, CSS animations, Canvas, SVG, and local assets.

The game must not require build steps, npm install, servers, API keys, databases, authentication, GitHub CLI, or creation of additional repositories.

If using React via Babel, ensure the page works directly on GitHub Pages.

## Creativity Rules

Before deciding the new game, compare against `memory.md`, existing game folders, and the arcade homepage.

Avoid repeating the same core mechanic, visual identity, puzzle structure, scoring model, or theme.

Each game should include at least one distinctive twist.

Possible puzzle families include pattern grids, symbolic logic, memory sequences, spatial routing, constraint placement, word association, number balancing, deduction boards, transformation puzzles, and visual rhythm puzzles. Do not simply copy those examples.

## Testing Checklist

Before finalizing the run, verify as much as the environment allows:

- The root arcade page still parses.
- The new game page parses.
- Inline JavaScript passes syntax checks when Node or another checker is available.
- The new game can be served from a static HTTP server when available.
- The primary interaction works.
- Progress, win, loss, or completion states work.
- Reset or replay works.
- Desktop layout is usable.
- Mobile layout is usable.

If browser automation, screenshots, Chromium, or Playwright are unavailable, state that limitation clearly and still perform static and HTTP checks.

## Memory Format

After publishing each game, append an entry to `memory.md` using this format:

```md
## YYYY-MM-DD - Game Title

- Folder: <game-slug>/
- Live URL: https://nsanchezBG.github.io/CodexCloudBase/<game-slug>/
- Core mechanic:
- Visual style:
- Brain skill trained:
- Notes to avoid repeating:
```

## Arcade Homepage

`CodexCloudBase/index.html` is the public arcade launcher for `NicoSavesTheWorld Brain Training`.

The root arcade page is static. It does not automatically discover folders in the repository. Each daily run must update the `const games = [...]` array in `index.html` and add the new game object as the first entry.

Each day, add the new game as a visible entry with game title, one-sentence description, date, brain skill trained, and live GitHub Pages URL.

## Final Report

At the end of the run, report:

- game title
- folder path
- live GitHub Pages URL
- short description of the mechanic
- whether testing succeeded
- confirmation that changes were pushed to GitHub `main`
- any known limitations
