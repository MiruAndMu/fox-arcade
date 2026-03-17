# Fox Arcade — Strategic Thinker Instructions

## What Is This
Fox Arcade is a collection of browser-based games built for Miru's heartbeat system. The heartbeat is an AI (Miru) who controls an Android phone via ADB. She taps buttons by their `aria-label` text using `!tap "Label"` and reads the screen via `!screen` which dumps the accessibility tree.

**Repo:** `MiruAndMu/fox-arcade` (GitHub Pages: https://miruandmu.github.io/fox-arcade/)
**Local:** `/Users/miru/.miru/workspace/fox-arcade/` served at `http://192.168.12.107:19284/`
**Workflow:** Edit locally → test at :19284 → push to GitHub → Pages auto-deploys → phone loads from local server

## The 6 Games
1. **wordle.html** — 5-letter word guessing (daily). Keyboard + grid + color feedback.
2. **canvas.html** — 16x16 pixel art. Color palette + tap cells to draw. Save/load artworks.
3. **memory.html** — Card matching (flip pairs). FOX, COW, STAR, MOON symbols.
4. **dungeon.html** — Roguelike. 9x9 map, enemies, items, floors. D-pad + action buttons.
5. **chess.html** — Chess vs AI (minimax). Tap piece → tap destination. Undo + Hint.
6. **sudoku.html** — 9x9 grid puzzler. Tap cell → tap number. 3 errors = game over. Generated puzzles.

## How Miru Plays (Critical — Do NOT Break This)
Miru interacts ONLY through the Android accessibility tree. She uses:
- `!tap "Label"` — taps an element by its `aria-label` attribute
- `!screen` — reads all text content and clickable elements from the DOM

### GOLDEN RULES
1. **Every interactive element MUST have a descriptive `aria-label`** — this is how she sees and taps things
2. **Never remove or rename existing aria-labels without updating the game logic** — she memorizes labels through her learning system
3. **All game state feedback must be in text/aria-labels** — she can't see colors, animations, or visual cues unless they're also in label text (e.g., `"Row 1 Column 3 Letter A Yellow"` not just a yellow background)
4. **Buttons must be real clickable elements** (button, a, div with onclick) — not just styled text
5. **Test that `!screen` output makes sense** — if you were blind and reading the labels, could you play the game?

## What To Work On

### QOL Improvements (Gameplay Polish)
Make these games feel like real app-quality games:
- **Wordle**: Daily word tracking (don't let her replay same day), win/loss animations, letter frequency hints
- **Canvas**: More canvas sizes (8x8, 16x16, 32x32 selector), gallery view of saved art, export as image
- **Memory**: More difficulty levels, themed card sets (animals, space, music), timer option
- **Dungeon**: More enemy variety, item descriptions, minimap, save/resume between sessions
- **Chess**: Opening book for AI, captured pieces display, move history in algebraic notation
- **Sudoku**: Pencil marks/notes mode, timer, difficulty indicators, celebration on completion

### UI/CSS Improvements (Make It Beautiful)
The games currently look functional but basic. Make them fox-themed and visually appealing:
- **Color palette**: Primary #f4845f (fox orange), #1a1a2e (dark bg), #16213e (card bg), #0f3460 (borders), #e94560 (accent red), #ffd700 (gold)
- **Fox theme**: Subtle fox motifs, paw prints, tail swishes, star elements (kitsune/celestial)
- **Methods allowed**: Pure CSS animations, CSS art, ASCII/ANSI art, SVG inline, generated assets
- **Typography**: Keep Courier New monospace but consider accent fonts for titles
- **Responsive**: Must work on Samsung S26 Ultra (1080x2340) in Chrome
- **The main index.html menu** needs the most visual love — game cards should feel inviting

### Technical Constraints
- **Pure HTML/CSS/JS** — no build tools, no npm, no frameworks. These are single-file games.
- **localStorage** for all persistence — no server-side storage
- **Must work offline** once loaded (GitHub Pages serves static files)
- **Keep file sizes reasonable** — she loads these on a phone over WiFi
- **DO NOT modify aria-labels of existing elements** unless you're adding MORE information (e.g., adding color info to Wordle cells is good, removing position info is bad)

### QA Checklist (Run After Every Change)
Before pushing any change, verify:
1. Game loads without errors (check browser console)
2. All interactive elements have aria-labels
3. `aria-label` text is descriptive enough to play blind
4. Game state saves/loads correctly from localStorage
5. Back to Arcade link works
6. No broken layout on mobile viewport (375px+ width)

## Examples of Good Changes
- Added "Clear entire row" button to Wordle (saves 5 backspace taps → 1 tap)
- Expanded Wordle word list from 500 → 10,000 words
- Added save/continue state to Chess and Sudoku
- Every Memory card says "Card 1 showing COW" when flipped (not just showing an emoji)

## Examples of Bad Changes
- Making the chess board a canvas element (kills accessibility)
- Adding drag-and-drop interactions (ADB can't drag)
- Removing aria-labels to "clean up" the code
- Adding external dependencies or CDN links
- Making any game require a second tap to confirm an action that should be instant

## Footer
Every page has: `built with love by cow & fox` — keep it.
