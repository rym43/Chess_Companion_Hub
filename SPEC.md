# Chess Companion Hub — Spec

Build a single-page web application using HTML, Tailwind CSS (via CDN), vanilla JavaScript, and the chess.js and chessboard.js libraries (via CDN). The app is an all-in-one local Chess Companion Hub with a sleek, dark-themed UI.

Please include the following features across 3 main navigation tabs:

## 1. TAB 1: DUAL CHESS CLOCK (Over-The-Board Play)
- Full-screen dual timer optimized for two players sitting across from each other.
- Time Control Controls: Presets for Bullet (1 min), Blitz (3 min, 5 min), Rapid (10 min), plus custom inputs for initial minutes/seconds and increment (seconds added per move).
- Interactive Mechanics: Clicking/tapping anywhere on the active player's side pauses their clock, applies increment, and starts the opponent's clock.
- Sound effects on tap/move and a clear visual banner when time expires ("Flagged!").
- Controls: Pause, Resume, Reset, and Swap Sides buttons.

## 2. TAB 2: LOCAL MULTIPLAYER (Pass-and-Play)
- An interactive chessboard using chessboard.js and chess.js for board state and move validation.
- Allows two players on the same screen/device to play a complete game locally.
- Integrated mini chess clock running above/below the board matching the current player's turn.
- Displays move history in standard algebraic notation (SAN), captured pieces, move validation, and game end detection (Checkmate, Stalemate, Draw by repetition).

## 3. TAB 3: FACTS ENGINE
- Chess Facts & Trivia: A curated card gallery or generator featuring 15+ fascinating historic facts, grandmaster records, and chess trivia, with a "Random Fact" button.

## Design Guidelines
- Clean, responsive dark UI using Tailwind CSS (slate-900 / emerald-500 accent palette).
- Return the entire app as a single self-contained index.html file with zero required build steps or local npm servers so it can run directly in a browser.
