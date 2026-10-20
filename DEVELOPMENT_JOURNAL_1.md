# Development Journal

Honest note: I did not write entries during the first week. I wrote the entries for 2026-10-12 to 2026-10-17 on 2026-10-19 from my project notes and chat history, so those dates are approximate and match the dates on the Kanban board. Entries from 2026-10-18 on were written from the build log and QA notes.

## 2026-10-12 (approx.): choosing the project and first tests

- **Kanban cards worked:** Pick track, extension and bonus (started and done); Test what HTML and CSS can do versus JavaScript (started).
- **What I did:** Read the Project 1 requirements and chose Track D (Card Gaming Hub), the Strategy Table Pack extension, and bonus 6 (model comparison cards). Started small tests of what plain HTML and CSS could do for a card game, compared with JavaScript.
- **Problem and how I solved it:** I was not sure how much of a card game could run with no scripts. I decided to test the risky parts first (selecting cards, switching views, animation) before designing anything.
- **Decision and trade-off:** Track D fit my interest in card games, and a deck builder, a dice table and a lobby gave enough pages without needing a back end.
- **Next:** Try card selection and view switching in both approaches.

## 2026-10-13 (approx.): CSS-only versus JavaScript tests

- **Kanban cards worked:** Test what HTML and CSS can do versus JavaScript; Write the 42-card list and rules (created).
- **What I did:** Built small test pages: a hidden checkbox with `:checked` to select a card, `:target` links to switch between views, and a CSS counter to count selected cards. Built a JavaScript prototype of the same card game to see what I would lose by leaving JavaScript out. Started listing cards for three champions.
- **Problem and how I solved it:** The CSS-only versions worked for selecting and switching, but CSS could not roll dice or run an opponent. I wrote down what each approach could and could not do.
- **Decision and trade-off:** The graded site stays CSS-only because the course allows HTML and CSS only. The JavaScript prototype is only a comparison and is not part of the submitted site.
- **Next:** Define the card pools and rules.

## 2026-10-14 (approx.): card list and rules, first card component

- **Kanban cards worked:** Write the 42-card list and rules (started); Build the card component and flip (created).
- **What I did:** Drafted the three pools of 14 cards, each with dice and a post-mortem effect that keeps working after the card is played. Sketched the card layout and began the card component in HTML and CSS.
- **Problem and how I solved it:** Some card effects were too similar between champions. I changed numbers and effect types so each pool plays differently: Phoenix adds dice, Gambler takes risks, Tide Warden stacks flat bonuses.
- **Decision and trade-off:** Cards use CSS gradients, not images, so there are no asset files to break and no art to source.
- **Next:** Finish the card face and the flip.

## 2026-10-15 (approx.): card component and flip

- **Kanban cards worked:** Build the card component and flip (started).
- **What I did:** Built the card front and back, and the 3D flip with `transform-style: preserve-3d`, `rotateY` and `backface-visibility: hidden`. Made the flip trigger on hover and on keyboard focus.
- **Problem and how I solved it:** The back of the card showed through on some tests. Adding the `-webkit-` prefix for `backface-visibility` and hiding the back face fixed it.
- **Decision and trade-off:** The flip also works with the keyboard so that keyboard users can read each card's post-mortem.
- **Next:** Write the proposal.

## 2026-10-16 (approx.): card list finished, proposal and wireframes

- **Kanban cards worked:** Write the 42-card list and rules (done); Proposal and 8 wireframes (started).
- **What I did:** Finished the 42-card list and rules, then started the proposal and the eight wireframes for the pages.
- **Problem and how I solved it:** The wireframes needed a clear page flow. I settled on Home, Lobby, Deck, Battle, Rules and Team, with the Strategy Table inside the Deck and Battle pages.
- **Decision and trade-off:** The proposal described a two-player battler. I later changed the game, so the proposal and wireframes still show the original design.
- **Next:** Finish the proposal and set up the site files.

## 2026-10-17 (approx.): proposal done, project set-up

- **Kanban cards worked:** Build the card component and flip (done); Proposal and 8 wireframes (done); Page skeletons and Design tokens and stylesheets (created).
- **What I did:** Finished the card component, the proposal and the eight wireframes. Planned the folder structure, the seven stylesheets and the design tokens (colours, type scale, spacing, motion timings).
- **Problem and how I solved it:** I needed a consistent look across pages without a framework. I decided to put every colour, size and timing in CSS custom properties on `:root` so one change updates every page.
- **Decision and trade-off:** Stylesheets link in a fixed order (base, utilities, layout, components, animations) with no `@import`.
- **Next:** Build all the pages and run checks.

## 2026-10-18: build day

- **Kanban cards worked:** Page skeletons, header, mobile menu, footer; Design tokens and stylesheets; Deck page: 42-card library and pool tabs; Strategy Table and dice animation; Model comparison cards; QA pass and first six defect fixes; README, journal and QA notes (started).
- **What I did:** Built the pages and stylesheets from the proposal, the CSS-only mobile menu, the pool tabs, the Strategy Table with its dice animation, and the model comparison cards. Ran scripted browser checks at 360, 768 and 1280px.
- **Problems solved:** see "Defects found and fixed" in `QA_NOTES.md`, including the Rules page overflow, upside-down dice, and a contrast miss on the graveyard state label.
- **Decisions and trade-offs:**
  - Deck selection is CSS-only (checkboxes, counters, `:has()`). CSS cannot block an 8th card, so the page warns instead.
  - Header and footer markup is repeated on each page because there are no includes without JavaScript or PHP.
  - Checkbox k is paired with card k by position (`nth-of-type`), which is the fragile pattern I would refactor first.
  - Dice results are preset and the "players online" number is a labelled demo, because the course allows HTML and CSS only.
- **Next:** Finish the deck counter and test everything on the live site.
- **AI assistance:** disclosed in the README.

## 2026-10-19 (morning to afternoon): deck counter fix

- **Kanban card worked:** Deck counter updates live as cards are added and removed.
- **What I did:** Tested the Deck page and found the counter stuck at "0 / 7" however many cards I ticked.
- **Problem and how I solved it:** A stray `counter-reset` on the counter element reset the count before it was shown. I removed it, kept `counter-increment` on the checked boxes and `counter(picked)` in `::after`, and tested 0 to 9 cards and back to 0 at 360px and 1280px. The saved screenshot is in `kanban-deck-counter-bug.png`.
- **Decision and trade-off:** I kept the counter as a CSS counter instead of switching to JavaScript, which the course does not allow.
- **AI assistance:** disclosed in the README.

## 2026-10-19 (evening): scope change from battler to score runs

- **Kanban cards worked:** Scope change; Play pages; Lobby overflow fix and QA of all views.
- **What I did:** Changed the game from a two-player battler to a solo four-round score run. Removed opponent and lives rules, converted the 19 cards that only affected an opponent so they help your own roll, pre-computed all 120 results for each champion, and rewrote the Home, Lobby, Deck, Rules and Play pages. Walked all 121 views of each champion's run at 360px and fixed a Lobby overflow.
- **Problem and how I solved it:** A real PvP or PvE game needs live logic, which HTML and CSS cannot do. Making the choices links between pre-computed results gives a playable game with no scripts.
- **Decision and trade-off:** Four rounds, not seven, because each extra round multiplies the pages by three. The dice for each path are fixed, so the game replays the same way for the same choices.
- **AI assistance:** disclosed in the README.

## 2026-10-19 (night): upload and live-site check

- **Kanban cards worked:** Upload to CODD and GitHub; Record walkthrough video.
- **What I did:** Uploaded the site to CODD and checked the live pages. The live Deck page still showed "0 / 7" with seven cards selected.
- **Problem and how I solved it:** The page on CODD used an old copy of `css/deck.css`. I uploaded the fixed stylesheet, hard-refreshed the page, and the counter then worked on the live site.
- **Decision and trade-off:** I checked the live URL, not only my own computer, because that is what the grader will see.
- **Next:** Push to GitHub with commits, record the walkthrough video and fill `links.txt`.
