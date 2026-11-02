# ♞ Chess Companion Hub

An all-in-one chess companion for playing in person: a two-player chess clock, a pass-and-play board, and a gallery of chess facts. It's a single HTML file with a dark design, and it runs straight in your browser with no install, build step or server.

**Live demo:** https://rym43.github.io/Chess_Companion_Hub/

## Features

### 1. Clock: two-player chess clock
- Full-screen clock with one half per player. On a phone lying flat between two players, the top half is turned upside down so the opponent can read it.
- Presets: Bullet (1 min), Blitz (3 and 5 min), Rapid (10 min), or your own minutes, seconds and increment.
- Tap your half to end your move. Your increment is added and your opponent's clock starts. Taps on the waiting player's half are ignored, so a stray touch can't pass the turn.
- Pause/Resume, Reset and Swap Sides, plus a full-screen mode. The screen stays on while a clock is running, on browsers that support it.
- A sound on every tap and a clear **FLAGGED!** banner when time runs out.
- Keyboard: <kbd>Space</kbd>/<kbd>Enter</kbd> press the clock, <kbd>P</kbd> pauses.

### 2. Play: two players on one device
- Complete games on one board. Drag a piece, or tap it and then tap where it should go.
- Only legal moves are accepted, and pawn promotion opens a Queen/Rook/Bishop/Knight picker.
- A small clock above and below the board runs for whoever's turn it is. Choose no clock, 1/3/5/10 minutes, or the Clock tab's setting.
- A move list in standard notation (e.g. `Nf3`, `exd5`), captured pieces with the material lead, and highlights for the last move and a king in check.
- Detects checkmate, stalemate, threefold repetition, insufficient material, the 50-move rule, running out of time and resignation.
- New game, Undo, Resign and Flip board buttons.

### 3. Facts: chess facts and trivia
- 21 facts about chess history, record holders and trivia, shown as cards you can filter by category.
- A **Random fact** button that never shows the same fact twice in a row.

## Tech stack

| What | Library (pinned versions) |
| --- | --- |
| Styling | [Tailwind CSS](https://tailwindcss.com/) 3.4.17 (Play CDN script, embedded in the page) |
| Move rules and game state | [chess.js](https://github.com/jhlywa/chess.js) 0.10.3 |
| Board | [chessboard.js](https://chessboardjs.com/) 1.0.0, which needs [jQuery](https://jquery.com/) 3.7.1 |
| Piece images | chessboard.js's Wikipedia piece set, via jsDelivr |
| Everything else | Plain JavaScript; sounds are generated in the browser with the Web Audio API |

The whole app is one file: [`index.html`](index.html).

**Security:** every library loaded from a CDN carries a [Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) hash, so the browser refuses any file that has been altered. Tailwind's CDN can't support these checks, so a verified copy of Tailwind is embedded in the page instead. A strict [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) lets the browser run only those known scripts and blocks network requests, so injected code can't run or send data anywhere.

## How to run

1. Download or clone this repository.
2. Open `index.html` in any modern browser (double-click it).

That's it. An internet connection is needed the first time so the libraries can load from their CDNs.

## Built with Claude Code

This project was built step by step with [Claude Code](https://claude.com/claude-code), Anthropic's AI coding assistant, following a written spec ([SPEC.md](SPEC.md)) and project rules ([CLAUDE.md](CLAUDE.md)). Each step was planned, approved, built, tested in a browser and committed separately. [AI_WORKFLOW.md](AI_WORKFLOW.md) describes the process, including the bugs found and fixed along the way.
