# How this app was built with Claude Code

Chess Companion Hub was built in a series of short, reviewable steps with [Claude Code](https://claude.com/claude-code). I wrote the spec and made the decisions; Claude Code wrote and tested the code. After each step it stopped so I could try the result myself before going on.

## 1. Set up before coding

Before any code was written, I asked Claude Code to:

1. Save my feature spec as [SPEC.md](SPEC.md).
2. Create [CLAUDE.md](CLAUDE.md) with the project rules: one `index.html`, Tailwind/chess.js/chessboard.js from a CDN only, plain JavaScript, well-commented code, no build steps. Claude Code reads this file automatically, so every later step followed the same rules.
3. Propose a step-by-step build plan and wait for my approval.

I approved the 7-step plan with two changes:
- a **pawn promotion picker** instead of always promoting to a queen
- **a git commit after every step**

Both rules were added to CLAUDE.md.

## 2. Building it step by step

| Step | What was built | Checked in the browser |
| --- | --- | --- |
| 1. Page setup | One HTML page, CDN libraries with exact versions, dark layout, 3 tabs | All libraries load, no console errors |
| 2. Shared helpers | One countdown timer for both clocks (time measured with `performance.now()` so it stays accurate), time display, generated sounds, mute button | Timer, increment and running-out-of-time tested with scripts |
| 3. Tab 1: Clock | Two-player clock, presets and custom times, Pause/Reset/Swap, FLAGGED! banner, keyboard shortcuts, full screen, screen kept on | Increment, ignored taps, pausing, swapping and running out of time, on desktop and phone layouts |
| *(my test)* | I tested Tab 1 myself before going on | |
| 4. Tab 2: Board | chessboard.js board with chess.js move checking, drag *or* tap to move, promotion picker, highlights, flip | Tap moves, a real mouse drag, promotion with cancel, castling, en passant, checkmate |
| 5. Tab 2: Game panel | Mini clocks, move list, captured pieces and material lead, every game ending, New game/Undo/Resign | Every type of game ending, Undo after checkmate, flipped board, phone and desktop layouts |
| *(my test)* | I tested Tab 2 myself before going on | |
| 6. Tab 3: Facts | 21 facts, category filters, featured card, Random fact | 40 random picks with no repeats, filtering |
| 7. Review | A full read-through for bugs plus tests at 320px, phone portrait and landscape, tablet and desktop | See the bug list below |

Claude Code tested each step in its built-in browser. It ran scripts that played moves and pressed the clocks, took screenshots at different screen sizes and read the browser console. Then it committed the step.

## 3. Bugs found and fixed

### While building

- **The piece images weren't in the npm package.** Claude Code checked each CDN link before using it. The chessboard.js npm package returned 404 for its piece images, so the board loads them from jsDelivr's copy of the project's GitHub repository instead.
- **Invalid Tailwind `@apply`.** A custom CSS class name was passed to Tailwind's `@apply`, which only accepts Tailwind's own classes. Claude Code noticed this before it broke the page and replaced it with the equivalent built-in classes.
- **The page crashed on load.** The clock's timer called the drawing code while it was still being set up, before the rest of the app could see it, which stopped the whole script with an error. Claude Code saw the blank page in a screenshot, read the error in the console and added a check so drawing waits until the timer is ready.
- **The clock buttons wrapped on phones.** At 375px wide, the button row spilled onto a second line. Claude Code tightened the spacing and made the Fullscreen button icon-only on narrow screens.
- **The board was too wide.** At some window sizes it caused a sideways scrollbar. The fix was to size the board by the available height as well as width, reserve space for the scrollbar, and move the board's border outside its layout.
- **The board was 8px narrower than its frame.** chessboard.js calculates square size from (width − 1), so one pixel is added back.
- **Clicking a fact card lost keyboard focus.** The whole card grid was rebuilt each time, so the focused card disappeared. Now only the highlight outline is updated.

### Found in the final review (step 7)

1. **The Play tab didn't fit on a sideways phone.** The bottom player's bar ended below the screen. Now the side panel moves beside the board on any sideways screen at least 640px wide, and the board can shrink further to fit.
2. **Keyboard users couldn't use buttons on the Clock tab.** The Space/Enter clock shortcut caught those keys even when another button had focus, so pressing Enter on "Play" or "Reset" pressed the clock instead. The shortcut now leaves keys alone when another control has focus.
3. **Right-clicking counted as a tap.** It could end a move on the clock or select a piece on the board. Only normal left-clicks and taps count now.
4. **Resigning before the first move left the clock in the wrong state.** It became "paused" instead of staying not-started.
5. **Small accessibility cleanups.** Tab buttons now carry the attribute that links each one to its panel, and an unused CSS rule was removed.

## 4. Getting ready to publish

- One commit message wrongly said it included a change from an earlier commit. Claude Code noticed and reworded it before anything was pushed.
- Before publishing, all commits were rewritten to use my private GitHub noreply email, and every file and commit was searched to confirm no personal information was left.

## What worked well

- **Writing the spec and rules down first.** SPEC.md and CLAUDE.md kept every step consistent without repeating instructions.
- **Approving a plan before any code.** I could change the plan (adding the promotion picker) before anything was built.
- **Small steps, each tested and committed.** Each one could be reviewed, tested and undone on its own.
- **Testing in a real browser.** Screenshots, scripted moves and console checks caught layout and runtime bugs that reading the code alone would have missed.

**Still to test by hand:** dragging pieces with a finger, sound and vibration on a real phone.
