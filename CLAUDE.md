# Chess Companion Hub — Project Rules

See [SPEC.md](SPEC.md) for the full feature spec. This file holds the rules for how it is built.

## Structure
- The entire app lives in **one file: `index.html`**. No other source files (no separate .js/.css, no local images).
- It must open directly from disk (`file://`) in a modern browser. **No build steps, no npm, no bundlers, no local dev server required.**

## Dependencies (CDN only, pinned exact versions)
Load everything from a CDN with an exact version — never `@latest` or a bare major:
These URLs were verified to resolve (HTTP 200) — use exactly these:
- **Tailwind CSS** — Play CDN, pinned: `https://cdn.tailwindcss.com/3.4.17`.
- **jQuery** — required by chessboard.js; must load **before** chessboard.js: `https://code.jquery.com/jquery-3.7.1.min.js`.
- **chessboard.js** — `https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.css` and `.../chessboard-1.0.0.min.js`.
- **chess.js** — exposes the browser global `Chess`: `https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js`. Use the 0.10.x API consistently (`game_over()`, `in_checkmate()`, `in_threefold_repetition()`, etc.).
- **Piece images** — `pieceTheme`: `https://cdn.jsdelivr.net/gh/oakmac/chessboardjs@v1.0.0/website/img/chesspieces/wikipedia/{piece}.png` (the npm package does not ship images).

## Workflow
- Make a git commit with a clear message after each build-plan step.
- Pawn promotion uses a piece picker (Q/R/B/N), not auto-queen.

## Code style
- **Vanilla JavaScript only** (jQuery is present solely because chessboard.js needs it; don't use it for app logic).
- **Well-commented**: a header comment per section/module (Tabs, Dual Clock, Pass-and-Play, Facts), and comments explaining non-obvious logic (timing, increment, game-end checks).
- Keep JS organized into clearly separated sections inside a single `<script>` block; avoid globals beyond one namespace object per feature.
- Use `const`/`let`, no `var`. Prefer small named functions over long inline handlers.

## Design
- Dark UI: `slate-900` base, `slate-800`/`slate-700` surfaces, `emerald-500` accent.
- Responsive: must work on phones (portrait) through desktops; tap targets large on the clock.
- Sounds are generated with the Web Audio API (no audio files).

## Timing
- Clocks must be accurate: compute remaining time from `performance.now()` deltas, not by counting `setInterval` ticks.
