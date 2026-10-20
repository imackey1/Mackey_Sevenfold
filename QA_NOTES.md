# QA Notes

Checks run on the finished HTML/CSS site. "Automated" means a scripted browser run (Playwright, headless Chromium) done during the build, with the results recorded here. Anything not listed under Still to do has not been tested.

## Viewport checks (automated, Chromium)

| Viewport | Pages checked | Horizontal scroll | One h1 and one main | Console errors |
| --- | --- | --- | --- | --- |
| Mobile 360 x 800 | all 5 | none after fix 1 | yes | none |
| Tablet 768 x 1024 | all 5 | none | yes | none |
| Desktop 1280 x 900 | all 5 | none | yes | none |

Screenshots of every page at each width were reviewed by eye for overlap, clipping and unreadable text.

## Navigation and links

- Every internal link and anchor target in the five pages resolves to a file or id (script check). No duplicate ids.
- Current page is marked with `aria-current="page"` and an underline in the nav on each page.
- Mobile menu works with the keyboard (checkbox focus ring on the Menu button) and is hidden from the tab order on desktop.
- Home to Lobby to Deck Table to sample match path clicked through: works. `deck.html#turn-1`, `#turn-2`, `#turn-3` each show exactly one turn.

## Functional checks (automated)

- Deck counter: shows 0 / 7 at the start; "Deck complete" appears at exactly 7 cards; the warning and red border appear at 8 or more. Deck slots list the chosen cards in order. Switching pools hides the other pool's slots and count.
- Keyboard: Tab order reaches skip link, menu, nav links, pool tabs, then each card checkbox; Space selects a card and the counter updates.
- Reduced motion (emulated `prefers-reduced-motion: reduce`): float, flip, shake, glow, dice tumble and count-up are off; the Home number shows its final value; procs show as a highlighted outline plus a text label.

## Accessibility

- Landmarks on every page: skip link, header, nav with aria-label, main, footer. Headings are in order with one h1.
- Decorative art and glyphs are `aria-hidden`; demo values have text labels ("Demo value", "Simulated").
- No images are used, so there are no alt text gaps; card art is CSS.
- Contrast (computed from the CSS tokens, WCAG ratio): body text 14.7, muted text 7.4 to 8.5, brass on dark 8.2, button text 8.1, card text on paper 13.6, suit text on paper 5.0 to 6.1, accents on felt 4.9 to 6.5, white on suit badges 6.2 to 7.6. All meet AA (4.5:1) for text. The "Spent" card state is deliberately dimmed with reduced opacity and is labelled in text.

## Defects found and fixed during QA

1. **Rules page overflowed at 360px (page 498px wide).** Cause: the dice table has a minimum width and the grid column could grow to fit it. Fix: `grid-template-columns: minmax(0, 1fr)` and `min-width: 0` on `.rules-layout` children so the table scrolls inside its wrapper.
2. **Dice finished upside down.** Cause: the tumble ended at 540deg, which is 180deg. Fix: end at 720deg.
3. **Sticky header covered anchor targets.** Fix: `scroll-margin-top: 6rem` on every `[id]`.
4. **Hidden menu checkbox was a tab stop on desktop.** Fix: `display: none` for `.nav-toggle` at 768px and up.
5. **Low contrast on the "Graveyard" state label (3.6:1).** Fix: use the dark paper text colour (9.9:1).
6. **Zone labels touched the cards beneath them.** Fix: bottom margin on `.zone__title`.
7. **Deck counter stayed at "0 / 7" when cards were added.** Cause: `.deck-count__num` had its own `counter-reset: picked`, which started a fresh counter at zero for the text and hid the real count. The seven-card and eight-card messages still worked because they use a different selector, which is why it was missed. Fix: remove that `counter-reset`. Retested at 360 and 1280px by adding nine cards one at a time and removing them again, reading the number each step: 0, 1, 2 up to 9 and back down to 0, with "Deck complete" at 7 and the warning at 8 and 9.

## Bonus feature checks (model comparison cards)

1. Layout: three columns at 768px and wider, one column at 360px, no clipping (screenshots).
2. Accessibility: each bar has a text label and a number (for example "Speed 60 / 100"), the radio is keyboard focusable and the focus ring shows, and contrast is AA.
3. Responsive and motion: tested at 360, 768 and 1280px; with reduced motion the flip becomes a fade and the bars do not animate.

## Play page checks (solo score runs, Chromium)

1. Walked every possible path on all three run pages by clicking the card links: the start view plus 120 results (121 views, 81 endings per champion), at 360px. Each click showed exactly the right view and no other, the browser Back button returned to the previous round every time, and there was no horizontal overflow and no console error.
2. All nine pages checked again at 360, 768, 1024 and 1280px after the game change: no overflow, one `h1` and one `main` each, no duplicate ids, no inline styles, no scripts, and every internal link and anchor resolves.
3. The card choices are real links, so they work with Tab and Enter and show a visible focus ring.
4. Defect found and fixed while adding the Lobby score table: the Lobby overflowed by 188px at 360px because a table's minimum width let a grid column grow. Fix: `grid-template-columns: minmax(0, 1fr)` on the base `.grid-2`, `.grid-3` and `.grid-4` classes, and a `table--narrow` class for the small 3-column score tables.
5. Calibration check: for each starter deck the final scores over all 81 paths range from about 15 to 59. The targets were set so that roughly a third to a half of paths reach the target, so the order you play cards in matters.

## Cross-browser check

Tested for real: Chromium (the engine behind Chrome and Edge). Not tested in Firefox or Safari, because neither could be installed in the test environment, so their results below are from published support data, not from running the site.

| Feature used | Chrome / Edge | Firefox | Safari |
| --- | --- | --- | --- |
| `:has()` (deck counter, tabs, lobby choices) | 105 | 121 | 15.4 |
| `@property` (dice numbers, players-online count) | 85 | 128 | 16.4 |
| `backface-visibility` (card flip) | yes | yes | prefixed `-webkit-` version added for older Safari |

Source: caniuse.com for `:has()` and `@property`. If a browser is older than the versions above, the deck counter, tab switching and animated numbers will not work there. The page text, navigation and layout still do.

## Still to do (not yet tested)

- Firefox and Safari, run in the real browsers. Open each page in them and compare with Chrome before submitting.
- A real phone.
- W3C HTML and CSS validators (they need network access that was not available during the build).
- Screen reader pass.
- Peer usability rounds (see the development journal).
- Link and path check on the CODD server after upload.
