# Chess Companion Hub — Project Rules

See [SPEC.md](SPEC.md) for the full feature spec. This file holds the rules for how it is built.

## Structure
- The entire app lives in **one file: `index.html`**. No other source files (no separate .js/.css, no local images).
- It must open directly from disk (`file://`) in a modern browser. **No build steps, no npm, no bundlers, no local dev server required.**

## Dependencies (CDN only, pinned exact versions)
Load everything from a CDN with an exact version — never `@latest` or a bare major:
These URLs were verified to resolve (HTTP 200) — use exactly these:
- **Tailwind CSS** — Play CDN v3.4.17, **embedded inline** in `index.html` (not loaded from `https://cdn.tailwindcss.com/3.4.17`, which can't support SRI). Embedded copy: 407,279 bytes, `sha384-igm5BeiBt36UU4gqwWS7imYmelpTsZlQ45FZf+XBn9MuJbn4nQr7yx1yFydocC/K`. To upgrade, download the new version, check it contains no `</script`, and replace the inline script verbatim.
- **jQuery** — required by chessboard.js; must load **before** chessboard.js: `https://code.jquery.com/jquery-3.7.1.min.js`.
- **chessboard.js** — `https://unpkg.com/@chrisoakman/chessboardjs@1.0.0/dist/chessboard-1.0.0.min.css` and `.../chessboard-1.0.0.min.js`.
- **chess.js** — exposes the browser global `Chess`: `https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js`. Use the 0.10.x API consistently (`game_over()`, `in_checkmate()`, `in_threefold_repetition()`, etc.).
- **Piece images** — `pieceTheme`: `https://cdn.jsdelivr.net/gh/oakmac/chessboardjs@v1.0.0/website/img/chesspieces/wikipedia/{piece}.png` (the npm package does not ship images).

## Integrity checks (SRI) — required for every CDN script and stylesheet
- Every `<script>`/`<link rel="stylesheet">` from a CDN must have `integrity="sha384-…"`, `crossorigin="anonymous"` and `referrerpolicy="no-referrer"`. The browser then refuses to run a file whose bytes don't match.
- Compute a hash from the exact URL: `curl -sL <url> | openssl dgst -sha384 -binary | openssl base64 -A`. Cross-check it against a second source (the project's published hash, cdnjs's SRI, or the same file from another CDN) before using it.
- Changing a library version means recomputing its hash; a wrong hash blocks the file and breaks the app (fail-safe).
- The CDN must send `Access-Control-Allow-Origin` for SRI to work. A library whose CDN doesn't (like `cdn.tailwindcss.com`) is embedded inline instead, so no unverified code is ever loaded.
- Piece images (`<img>`) can't use SRI; images can't run code, so this is acceptable.

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
