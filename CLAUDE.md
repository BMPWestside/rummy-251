# Rummy 251

A phone scorer for rummy nights. It runs as a static site on GitHub Pages (published from `main`, root) and stores games in a Google Sheet through Google sign-in.

## How changes are deployed
Claude makes the changes, commits and pushes to `main`. GitHub Pages republishes within a minute or two. The owner doesn't upload anything by hand.

## Files
- `index.html` is the whole app: markup, styles and script in one file, with the logo images embedded.
- `config.js` holds the Google OAuth Client ID and Sheet ID. **Never overwrite or blank it.**
- `manifest.webmanifest`, `sw.js` and `icon-*.png` / `apple-touch-icon.png` let the app be installed on a phone. The service worker fetches from the network first, so a push shows up on the next app open.
- `rummy251-app-icon-1024.png` is a large copy of the app icon, kept for the owner. The app doesn't use it.

## Storage (Google Sheets)
- `Current!A1` holds the game in progress as JSON, and `Current!B1` holds the time it was last changed.
- The `History` tab has one row per finished game. Its columns are Ended, Rounds, Winner, Pot, Final scores, Money, and Data, which is JSON the app reads back.
- Don't change these shapes without migrating existing data.

## House rules the app enforces
- **Scores:** Drop is 25, middle drop 50 and full count 80. A typed score must be 2–79. 0 means Rummy (R), and there is exactly one R per round.
- **Out:** A player who reaches 251 or more is out.
- **Rejoin:** Rejoining is allowed only in the gap right after the round the player went out, at the highest active score + 1, and only if that score is 226 or less.
- **Give up:** It is offered to the smallest group of top scorers who can all rejoin lower by giving up together. It is not available with only 2 players left.
- **Two players left:** If one drops or middle-drops, the other automatically gets R.
- **High scores:** Over 200 there is no middle drop. 226–250 is compulsory: no drop or middle drop, and typed 25 or 50 is blocked too.
- **Seating and dealing:** The seat order comes from the card draw. The 1st seat picks first and holds the money. The deal moves one seat each round, and rejoiners sit on the dealer's left.
- **Pot:** A$20 per player plus A$20 per rejoin.
- **Split:** Compulsory players get A$20 back. The rest is shared at A$20 per drop above the lowest drop count, and the remainder is split equally. If only one player still has drops, that player takes A$20 per drop and everyone else shares the rest equally. A manual split must add up to the pot.
- **Names:** Names are shown as initials. Players with the same initials get the second letter of their first name added (SuM / SaM).
- **Layout:** The page itself never scrolls.
