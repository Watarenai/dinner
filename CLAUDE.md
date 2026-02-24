# CLAUDE.md

## Project Overview

**食材スロット (Ingredient Slot Machine)** — A browser-based slot machine app that randomly selects cooking ingredients to help decide what to cook for dinner. Built as a single-page web application deployed via GitHub Pages.

- **Repository**: `Watarenai/dinner`
- **Language**: Japanese (UI text and commit messages)
- **Deployment**: GitHub Pages (served from `index.html` at the repository root)

## File Structure

```
dinner/
├── index.html                 # Main application (current version)
├── slot_app_prototype.html    # Earlier 3-reel prototype
├── slot-sound.mp3             # Audio file played during reel spin
└── CLAUDE.md                  # This file
```

### Key Files

- **`index.html`** — The live application. Contains 4 slot reels (3 vegetable reels + 1 meat reel), each spinning at randomized speeds and intervals. All HTML, CSS, and JavaScript are inlined in this single file.
- **`slot_app_prototype.html`** — The original prototype with 3 vegetable-only reels and fixed spin speed. Kept for reference.
- **`slot-sound.mp3`** — Looping sound effect that plays while reels are spinning.

## Technology Stack

- **Pure HTML/CSS/JavaScript** — No frameworks, no build tools, no package manager
- No `package.json`, no npm dependencies, no bundler
- All code is self-contained in single HTML files with inline `<style>` and `<script>` blocks

## Architecture & Code Patterns

### Application Structure (index.html)

The app follows a simple imperative DOM-manipulation pattern:

1. **Data**: Ingredient arrays defined as constants (`vegIngredients`, `meatIngredients`)
2. **DOM Setup**: Reels are populated by duplicating shuffled ingredient lists into `<ul>` elements (doubled for seamless looping)
3. **Animation**: `setInterval`-based scrolling with randomized `step` (5–15px) and `delay` (20–60ms) per reel
4. **Stop Logic**: Snaps to nearest item boundary (100px grid) when stopped
5. **Result Display**: Shows selected ingredients in `#result` div after all reels stop
6. **Audio**: `<audio>` element with `loop=true`, plays during spin, pauses on full stop

### Key Functions

| Function | Purpose |
|---|---|
| `shuffle(arr)` | Fisher-Yates shuffle for randomizing ingredient order |
| `spinReel(i)` | Starts interval-based animation for reel `i` |
| `stopReel(i)` | Stops reel `i`, snaps to nearest item, shows result if all stopped |
| `stopAll()` | Stops all 4 reels simultaneously |
| `resetReels()` | Clears all reels and restarts spinning |

### Reel Configuration

- Reels 1–3 (`reel1`–`reel3`): Vegetable ingredients (19 items)
- Reel 4 (`reel4`): Meat ingredients (3 items: 牛肉, 鶏肉, 豚肉)
- Each reel item is 100×100px

### Differences: Prototype vs Current

| Aspect | `slot_app_prototype.html` | `index.html` |
|---|---|---|
| Reels | 3 (all vegetables) | 4 (3 vegetables + 1 meat) |
| Spin speed | Fixed (10px / 30ms) | Randomized per reel |
| Button labels | ストップ1/2/3 | 野菜/野菜/野菜/肉 |
| Ingredient pool | Single shared array | Separate veg/meat arrays |

## Development Workflow

### No Build Step Required

Open `index.html` directly in a browser to test. No server, compilation, or installation needed.

```bash
# Quick local preview (if a simple server is needed)
python3 -m http.server 8000
# Then open http://localhost:8000
```

### Making Changes

1. Edit `index.html` directly — all code (HTML, CSS, JS) lives in this one file
2. Test by opening in a browser
3. Commit with a descriptive message (Japanese or English)

### No Testing / Linting / CI

There are no automated tests, linters, or CI/CD pipelines configured. Manual browser testing is the only verification method.

## Git Conventions

- **Default branch**: `main` (remote) / `master` (local)
- **Commit messages**: Mix of Japanese and English. Follow existing style:
  - Japanese: `初回コミット：プロトタイプ追加`
  - English: `Add index.html as the homepage`
  - Mixed: `Update index.html: ランダム速度化コードを反映`
- **History**: Linear (no merge commits)

## Deployment

The site is deployed via **GitHub Pages** serving from the repository root. Pushing to the default branch triggers a Pages rebuild. The `index.html` at the root is the entry point.

## Guidelines for AI Assistants

- This is a minimal, no-build-tool project. Do not introduce package managers, bundlers, or frameworks unless explicitly requested.
- All application code is inline within HTML files. Maintain this pattern unless asked to refactor.
- UI text is in Japanese. Preserve Japanese text in the UI and use Japanese for user-facing strings.
- Commit messages can be in Japanese or English, following the existing mixed style.
- The 100×100px reel item size is a fundamental layout constant — CSS and JS snap logic both depend on it.
- When adding ingredients, update the appropriate array (`vegIngredients` or `meatIngredients`) and ensure the corresponding reel count and button labels stay consistent.
- Audio playback depends on the `slot-sound.mp3` file being co-located with the HTML files.
