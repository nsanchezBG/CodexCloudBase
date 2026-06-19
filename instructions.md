# Daily Mental Puzzle Game Generator

## Mission

Every day at 3:00 a.m. Peru time, create and publish one new original web puzzle game for morning brain exercise.

The game must be playable in 10 to 15 minutes, feel fresh and mentally engaging, and work well on both desktop and mobile. Avoid repeating ideas already recorded in `memory.md`.

## Repository Context

This base repository is `CodexCloudBase`.

The public arcade name is `NicoSavesTheWorld Brain Training`.

Expected root files and folders:

- `instructions.md`: the operating instructions for the daily agent run.
- `memory.md`: the log of previously generated games and concepts. Read it before creating anything. It may be empty the first time.
- `index.html`: the public arcade homepage listing all generated games.
- `Assets/`: shared or generated visual assets when useful.

## Daily Workflow

1. Read `instructions.md`.
2. Read `memory.md`.
3. Invent one new puzzle game concept that is meaningfully different from previous entries.
4. Choose a short, memorable game title.
5. Generate a repository-safe slug using the title plus date or short code, such as `mindgrid-2026-06-19` or `glyph-lock-7k3-2026-06-19`.
6. Create a new public GitHub repository using that slug as the repository name.
7. Build the game in the new repository.
8. If useful, create visual assets and place them in the new repository under `Assets/`.
9. Commit visual assets first when assets are created.
10. Create or update `index.html` in the new repository with the full playable game.
11. Commit the game implementation.
12. Enable GitHub Pages for the new repository from branch `main`, folder `/`.
13. Wait for the GitHub Pages deployment if possible.
14. Visit the deployed Pages URL if browsing or testing is available.
15. Test the core gameplay manually.
16. If errors are found, fix them and repeat implementation and testing until the game works.
17. Return to `CodexCloudBase`.
18. Update `memory.md` with the new game entry.
19. Update `CodexCloudBase/index.html` by adding the new game to the arcade homepage.
20. Commit the `memory.md` and arcade homepage updates.

## Gameplay Quality Bar

Each game must be:

- Original enough that it does not feel like a direct repeat of an earlier game.
- Playable in a single `index.html` file, except for optional files in `Assets/`.
- Deployable on GitHub Pages without a backend.
- Responsive for desktop and mobile.
- Visually polished, modern, calm, and pleasant.
- Clear without relying on long instructions.
- Interesting for at least 10 to 15 minutes.
- Focused on mental exercise: logic, pattern recognition, memory, deduction, spatial reasoning, sequencing, constraint solving, language reasoning, or numerical intuition.

Prefer compact, elegant mechanics over large complicated systems.

## Technical Constraints

The generated game repository should contain:

- `index.html`
- optional `Assets/`
- optional `README.md`

The game may use:

- Vanilla HTML, CSS, and JavaScript
- React via CDN and Babel, if useful
- CSS animations
- Canvas or SVG when useful
- Local assets from `Assets/`

The game must not require:

- build steps
- npm install
- servers
- API keys
- external databases
- authentication

If using React via Babel, ensure the page works directly on GitHub Pages.

## Visual Direction

Use a clean, modern visual style. Avoid generic placeholder UI.

Good directions include:

- soft contrast with strong readability
- elegant typography
- tactile buttons
- subtle motion
- clear state changes
- satisfying completion feedback
- pleasant color palette that is not visually exhausting in the morning

Assets may be generated when they improve the experience, such as symbols, tiles, abstract backgrounds, icons, puzzle pieces, cards, or board elements.

Do not create assets unless they genuinely help the game.

## Creativity Rules

Before deciding the new game, compare against `memory.md`.

Avoid repeating:

- same core mechanic
- same visual identity
- same puzzle structure
- same scoring model
- same theme

Each game should include at least one distinctive twist.

Possible puzzle families include:

- pattern grids
- symbolic logic
- memory sequences
- spatial routing
- constraint placement
- word association
- number balancing
- deduction boards
- transformation puzzles
- visual rhythm puzzles

Do not simply copy those examples. Use them only as starting points.

## Testing Checklist

Before finalizing the run, verify:

- The deployed game loads without runtime errors.
- The desktop layout is usable.
- The mobile layout is usable.
- The primary interaction works.
- Progress, win, loss, or completion states work.
- Reset or replay works.
- GitHub Pages URL is reachable, or the expected deployment URL is recorded if Pages is still building.

## Memory Format

After publishing each game, append an entry to `memory.md` using this format:

```md
## YYYY-MM-DD - Game Title

- Repository: https://github.com/OWNER/REPO
- Live URL: https://OWNER.github.io/REPO/
- Core mechanic:
- Visual style:
- Brain skill trained:
- Notes to avoid repeating:
```

## Arcade Homepage

`CodexCloudBase/index.html` is the public arcade launcher.

Each day, add the new game as a visible entry with:

- game title
- one-sentence description
- date
- brain skill trained
- live GitHub Pages URL

Keep the arcade page lightweight, responsive, static, and compatible with GitHub Pages.

## Commit Guidance

Use clear commits.

In the generated game repository:

- `Add visual assets for <game title>` if assets were created.
- `Build <game title> puzzle game`.
- `Configure GitHub Pages` if a commit is needed for Pages configuration.

In `CodexCloudBase`:

- `Log <game title> and update arcade`.

## Final Report

At the end of the run, report:

- game title
- repository URL
- live GitHub Pages URL
- short description of the mechanic
- whether testing succeeded
- any known limitations
