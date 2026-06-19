# Daily Mental Puzzle Game Generator

## Mission

Every day at 3:00 a.m. Peru time, create and publish one new original web puzzle game for morning brain exercise inside this repository.

The game must be playable in 10 to 15 minutes, feel fresh and mentally engaging, work well on desktop and mobile, and avoid repeating ideas already recorded in `memory.md`.

The daily output should feel like a small premium mobile puzzle: calm, restrained, tactile, modern, and easy to understand without long explanation.

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

## Product Direction

Each game should feel like a compact, art-directed puzzle object rather than a webpage about a puzzle.

Target qualities:

- sober
- fluid
- minimal
- tactile
- premium
- quiet
- modern
- immediately legible

The strongest references are small-screen abstract puzzle games with generous negative space, simple geometric pieces, restrained color, minimal copy, and clear interaction feedback.

Prefer:

- geometric symbols, boards, paths, tiles, rings, constellations, folds, masks, shadows, and spatial diagrams
- a small number of strong shapes instead of many decorative elements
- a limited palette with one calm ground color, one dark ink color, one muted support color, and one or two accents
- soft motion that confirms cause and effect
- concise labels that support the player instead of explaining the whole system at once

Avoid:

- busy dashboards
- large text blocks
- generic gradient backgrounds
- decorative blobs, orbs, bokeh, or ornamental particles
- nested cards
- oversized hero sections
- emoji as primary game art
- many competing accent colors
- fantasy UI chrome unless the mechanic truly needs it
- long "How to play" sections visible during play

## Game Screen Architecture

The game may be a single screen, but it does not have to be. Choose the structure that gives the player the clearest first minute.

Use one of these patterns:

1. Single-screen puzzle
   - Best for very simple rules.
   - Put the title, one-line objective, board, compact HUD, and primary controls in one balanced composition.

2. Three-step flow
   - Best for more novel mechanics.
   - Splash: title, one-sentence premise, one primary action.
   - Brief: no more than three short rules, each under 12 words.
   - Play: board first, HUD second, optional details behind a small control.

3. Progressive reveal
   - Best for deduction games with layered rules.
   - Start with the board and one goal.
   - Reveal new constraints as compact chips, short clues, or collapsible notes.

Do not make an onboarding flow if it only adds friction. Use it when it reduces visible clutter during play.

## Visual System Requirements

Before coding the game, define a small design system inside the file:

- background color
- surface color if needed
- ink color
- muted text color
- line color
- one primary accent
- one secondary accent
- success color
- warning or error color
- radius scale
- spacing scale
- motion duration

Use this system consistently. Do not invent new colors or radii mid-file unless the mechanic requires it.

Recommended palettes:

- Mist: `#eef2f0`, `#28302f`, `#8aa39b`, `#ffffff`, `#d9e1dd`
- Clay: `#dfa24a`, `#6f4a49`, `#f3d08c`, `#f7eee2`, `#2f342f`
- Ink: `#f5f4ed`, `#293231`, `#93aaa5`, `#cf6f78`, `#d8ddd8`
- Night: `#5b2e63`, `#f16f93`, `#7db7d0`, `#f5d6df`, `#342044`

Choose one palette family per game, then add at most one surprise accent for player focus.

## Typography

Use typography as interface, not decoration.

- Use system fonts unless a web-safe style clearly improves the concept.
- Keep letter spacing at `0`.
- Use clear hierarchy, but avoid enormous display type inside the playable area.
- HUD labels should be small, direct, and stable.
- Buttons must have deliberate font size, weight, and line height.
- Do not let text resize controls or shift the board.

## Layout Rules

Build for mobile first, then expand to desktop.

- The play board must be the visual center.
- Keep the main puzzle inside the first viewport on mobile when possible.
- The game should remain playable at 360px wide.
- Desktop may center the game like a premium tablet surface or split board and controls into two calm columns.
- Cards are allowed only for repeated game entries, modal-like overlays, or a single purposeful play surface.
- Border radius should usually be 8px or less for UI panels. Game pieces may use custom geometry.
- Use stable dimensions for boards, tiles, HUD counters, icon buttons, and controls.
- Avoid vertical walls of text beside the board.

## Copy Rules

The player should learn by acting.

Visible copy limits:

- Splash: title plus one sentence.
- Brief screen: three rules maximum.
- In-game objective: one sentence maximum.
- In-game feedback: one short sentence.
- Buttons: one or two words.
- Clue text: short fragments, not paragraphs.

Every sentence must earn its space. Remove copy that repeats what the interaction already shows.

## Interaction And Motion

Make the game feel fluid without becoming flashy.

- Add hover and focus states for all clickable pieces.
- Add pressed, selected, valid, invalid, solved, and disabled states where relevant.
- Use transitions around 140ms to 260ms.
- Prefer transforms, opacity, outline, and color changes.
- Use subtle movement to show state changes, not decoration.
- Respect `prefers-reduced-motion`.
- Controls must be reachable and understandable with keyboard focus.

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

Prefer vanilla HTML/CSS/JavaScript unless a game truly benefits from React. A single-file puzzle should not feel like a framework demo.

## Creativity Rules

Before deciding the new game, compare against `memory.md`, existing game folders, and the arcade homepage.

Avoid repeating the same core mechanic, visual identity, puzzle structure, scoring model, or theme.

Each game should include at least one distinctive twist.

Possible puzzle families include pattern grids, symbolic logic, memory sequences, spatial routing, constraint placement, word association, number balancing, deduction boards, transformation puzzles, and visual rhythm puzzles. Do not simply copy those examples.

When choosing a theme, make it quiet and useful. The theme should clarify the mechanic, not cover it with lore.

## Arcade Homepage

`CodexCloudBase/index.html` is the public arcade launcher for `NicoSavesTheWorld Brain Training`.

The root arcade page is static. It does not automatically discover folders in the repository. Each daily run must update the `const games = [...]` array in `index.html` and add the new game object as the first entry.

Each day, add the new game as a visible entry with game title, one-sentence description, date, brain skill trained, and live GitHub Pages URL.

The arcade homepage should feel like a refined catalog, not a marketing page:

- restrained title treatment
- compact metadata
- minimal filters
- cards with consistent height
- no oversized hero copy
- no visual noise behind the game list

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
- The first viewport is visually calm and uncluttered.
- Text does not overflow or crowd controls at 360px wide.
- The play board remains the clearest focal point.
- The game is understandable with no more than three visible rules.

If browser automation, screenshots, Chromium, or Playwright are unavailable, state that limitation clearly and still perform static and HTTP checks.

## Visual Review Checklist

Before committing, inspect the page like a designer:

- Is there one clear focal point?
- Are there too many panels, labels, badges, pills, or counters?
- Can the player start in under five seconds?
- Would removing any sentence make the interface cleaner without hurting play?
- Are shapes, spacing, and colors consistent?
- Does the game feel modern without looking generic?
- Does mobile look intentionally designed, not merely compressed?
- Is the game art code-native and crisp, not broken emoji or placeholder symbols?
- Does motion make the mechanic clearer?

If the answer to any item is weak, edit before publishing.

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

For `Visual style`, describe the design system, not only the theme. Example: "Mist-gray background, charcoal geometric tiles, one coral accent, compact HUD, progressive rules."

## Final Report

At the end of the run, report:

- game title
- folder path
- live GitHub Pages URL
- short description of the mechanic
- whether testing succeeded
- confirmation that changes were pushed to GitHub `main`
- any known limitations

