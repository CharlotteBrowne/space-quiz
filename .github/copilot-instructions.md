# Copilot instructions

Single-file space quiz app: `index.html` (inline CSS + JS). No build, no dependencies, no tests, no linter, no server. Open the file directly (`file://`) to run it.

## Architecture

- `QUESTIONS` array: each entry is `[question, [4 options], correctIndex, funFact]`. Adding or removing entries changes the quiz length automatically (counts, progress bar and results all use `QUESTIONS.length`), but the static "10 questions" subtitle and `aria-valuemax="10"` in the HTML are hardcoded and must be updated by hand.
- DOM is static (`#quiz` and `#result` sections toggled via `hidden`); `render()` rebuilds only the option buttons, and `answer()` / `finish()` / `start()` mutate state through the `$("id")` helper. State is three globals: `i`, `score`, `locked`.
- The progress bar fill is `#bar` (width %) inside `#barWrap`, which carries the ARIA progressbar attributes. Update both together.

## Conventions

- Theming is via CSS custom properties on `:root` with a `prefers-color-scheme: dark` override. Use the variables (`--good`, `--bad`, `--accent`, etc.) rather than hardcoded colors.
- The "mission-control" look is a second CSS block layered over the base styles (marked `/* Mission-control panel */`) that overrides earlier rules for `main`, `h1`, `.bar`, `button.opt`, etc. Edit the later override, not the base rule, when changing those elements.
- Feedback animations (`shake`, `pop`, `bump`) are CSS keyframes toggled by classes (`.correct`, `.wrong`, `.bump`). Re-triggering `.bump` requires remove class → force reflow (`void el.offsetWidth`) → add class. Animations are disabled under `prefers-reduced-motion`.
- The Next button is hidden with `visibility` (not `display`) via the `.show` class, and focus is moved to it after answering, and to "Play again" on the results screen, to keep keyboard flow working.
- Body font is system sans-serif at 15px; the layout is a single centred column (`max-width: 440px`). Keep it within the 14–16px body range.

## Verifying changes

There is no test suite. Verify in a browser: reload `index.html`, answer questions (including a wrong one), and confirm the score, progress bar, correct/wrong feedback, results emoji and "Play again" reset still work.
