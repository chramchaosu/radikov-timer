# Radikov Timer

A two-speaker timer that works like a chess clock.

## How to use

0. Enter the two **speakers' names** (the first one speaks first). The names
   from your last session are already typed in; check them and tap
   **Confirm and start**. Also choose the **minimum** and **maximum** speaking
   time for one turn. While a speaker talks, their time is **green** inside this
   range and **red** outside it (too short or too long).
1. Tap anywhere on the timer area: **Speaker A**'s clock starts from 0.
2. Tap again: Speaker A's time freezes on screen and **Speaker B**'s clock starts from 0.
3. Tap again: the round is over. Both speaking times are saved and shown in the
   middle of the screen, and added to the **total** for each speaker (with the
   number of rounds played). The next round starts right away: both clocks go
   back to 0 and Speaker A's clock runs.

The bar in the middle shows each speaker's share of the total speaking time
(blue = Speaker A, amber = Speaker B).

**Stop** (top right, tap twice to confirm) ends the session and shows a summary:
totals, the share bar, how many turns were in range, and every round's times.
If Speaker B was speaking, that round is counted; if Speaker A was speaking, the
unfinished round is left out.

Sessions are saved with their date on the phone (in the browser's storage) after
every finished round. Open **Past sessions** to see or delete them. Clearing the
browser's data deletes them too.

Taps closer together than 0.4 seconds are ignored, so an accidental double tap
doesn't skip a speaker.

## Putting it on your phone

The app lives at **https://chramchaosu.github.io/radikov-timer/** (GitHub Pages,
updated automatically after every push to `main`).

**iPhone:** open the address in **Safari**, tap **Share** (square with an arrow),
then **Add to Home Screen**. It opens full-screen and works without internet
once it has been opened.

**Android:** open the address in **Chrome**, tap the **⋮** menu, then
**Add to Home screen** (or **Install app**).

The app itself is still one file, `index.html`, which also works when opened on
its own. The other files only add the home-screen icon and offline use.

## For developers

- Plain HTML, CSS and JavaScript in a single file. No build step, no dependencies.
- `manifest.webmanifest` and `icon-*.png` make it installable on the home screen;
  `sw.js` is a service worker that keeps a copy for offline use (network first,
  so new versions arrive whenever the phone is online).
- The logic is a small state machine (`ready` -> `a` -> `b` -> `a` ...), described
  in the comment at the top of the `<script>` block. The session being timed is in
  `session` (`rounds` holds `{ a, b }` times in milliseconds); live timer state is
  in `timer`.
- Four screens (setup, timer, summary, history) live in one page; `showScreen()`
  switches between them.
- Sessions are stored in `localStorage` under `radikov-timer.sessions.v1`, the
  last-used range under `radikov-timer.limits.v1`, the last-used names under
  `radikov-timer.names.v1`. Sessions saved before names existed show
  "Speaker A" / "Speaker B".
- Times are measured with `performance.now()`, so they stay accurate even if the
  screen redraw pauses.
- The Screen Wake Lock API keeps the screen on while a speaker is talking, where
  the browser allows it.
