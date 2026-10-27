# Radikov Timer

A two-speaker timer that works like a chess clock.

## How to use

1. Tap anywhere on the timer area: **Speaker A**'s clock starts from 0.
2. Tap again: Speaker A's time freezes on screen and **Speaker B**'s clock starts from 0.
3. Tap again: the round is over. Both speaking times are saved and shown in the
   middle of the screen, and added to the **total** for each speaker (with the
   number of rounds played). The next round starts right away: both clocks go
   back to 0 and Speaker A's clock runs.

The **Reset** button (top right) clears the totals and goes back to round 1. Tap it
twice to confirm.

Taps closer together than 0.4 seconds are ignored, so an accidental double tap
doesn't skip a speaker.

## Putting it on your phone

The whole app is one file: `index.html`. It needs no internet connection.

1. Download `index.html` to your phone (for example from the GitHub page: open the
   file, then use **Download raw file**).
2. Open it in your phone's browser (Chrome, Safari, Firefox...).

## For developers

- Plain HTML, CSS and JavaScript in a single file. No build step, no dependencies.
- The logic is a small state machine (`ready` -> `a` -> `b` -> `a` ...), described
  in the comment at the top of the `<script>` block. Completed rounds are kept in
  `state.rounds`, and the totals in `state.total`.
- Times are measured with `performance.now()`, so they stay accurate even if the
  screen redraw pauses.
- The Screen Wake Lock API keeps the screen on while a speaker is talking, where
  the browser allows it.
