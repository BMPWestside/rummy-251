# Rummy 251 — your own app

A phone app for your rummy nights. It runs free on GitHub Pages, saves every game to a Google Sheet you own, and installs on your home screen like a normal app. Everyone you share the sheet with can sign in and see the same game live (new rounds show up on other phones within about 4 seconds).

**Files in this folder**

| File | What it is |
|---|---|
| `index.html` | The app |
| `config.js` | Your two settings (Client ID and Sheet ID) — the only file you edit |
| `manifest.webmanifest`, `sw.js`, `` | Make it installable and let it open without signal |

Setup takes about 20 minutes. Do the steps in this order.

---

## 1. Make the Google Sheet

1. Go to **sheets.google.com** → **Blank spreadsheet**. Name it `Rummy 251`.
2. Copy the **Sheet ID** from the address bar — the long part between `/d/` and `/edit`:
   `https://docs.google.com/spreadsheets/d/`**`1AbC...xyz`**`/edit`
3. **Share** → add your friends' Gmail addresses:
   - **Editor** for anyone who should be able to enter scores
   - **Viewer** for anyone who should only watch

You don't need to add any tabs. The app creates a **Current** tab (the game in progress) and a **History** tab (one row per finished game: date, rounds, winner, pot, final scores, money) the first time you sign in.

## 2. Put the app on GitHub

1. Sign in at **github.com** → **+** (top right) → **New repository**.
2. Name it `rummy-251`, set it to **Public**, tick **Add a README file**, then **Create repository**.
3. **Add file** → **Upload files** → select all 9 files (index.html, config.js, manifest.webmanifest, sw.js, README.md and the 4 icon PNGs) and drag them in → **Commit changes**.
4. **Settings** → **Pages** → under *Build and deployment*, Source: **Deploy from a branch**, Branch: **main**, folder **/ (root)** → **Save**.
5. Wait a minute and refresh. GitHub shows your address:
   `https://YOUR-USERNAME.github.io/rummy-251/`

## 3. Set up Google sign-in (console.cloud.google.com)

1. Go to **console.cloud.google.com** → project picker at the top → **New project** → name it `Rummy 251` → **Create**, and make sure it's selected.
2. **APIs & Services** → **Library** → search **Google Sheets API** → **Enable**.
3. **APIs & Services** → **OAuth consent screen** (also called *Google Auth Platform*) → **Get started**:
   - App name `Rummy 251`, your email as support email
   - Audience: **External**
   - Contact email: yours → agree → **Create**
4. **Audience** → **Test users** → **Add users** → add your own Gmail and every friend's Gmail (up to 100). Only these people can sign in. Leave the app in **Testing** — no Google review needed.
5. **Clients** (or *Credentials* → **Create credentials** → **OAuth client ID**):
   - Application type: **Web application**, name `Rummy 251 web`
   - **Authorized JavaScript origins** → add `https://YOUR-USERNAME.github.io`
   - **Authorized redirect URIs** → add `https://YOUR-USERNAME.github.io/rummy-251/` (with the slash at the end)
   - **Create**, then copy the **Client ID** (ends in `.apps.googleusercontent.com`)

## 4. Connect the two

On GitHub, open `config.js` → pencil (Edit) → paste your two values:

```js
window.RUMMY_CONFIG = {
  clientId: '1234567890-abc123.apps.googleusercontent.com',
  sheetId: '1AbC...xyz'
};
```

Then **Commit changes**. Both are safe in a public repo: the Client ID isn't a password, and the sheet only opens for accounts you shared it with.

## 5. Install it on phones

1. Open `https://YOUR-USERNAME.github.io/rummy-251/`.
2. Tap **Sign in with Google**. You'll see **"Google hasn't verified this app"** — that's normal for a private app. Tap **Continue**, then allow access to Google Sheets.
3. Add it to the home screen:
   - **iPhone (Safari):** Share → **Add to Home Screen**
   - **Android (Chrome):** ⋮ menu → **Install app** (or *Add to Home screen*)

---

## Good to know

- **Staying signed in.** Google's sign-in lasts an hour at a time. When the app opens it renews this quietly. If it runs out mid-game, a red bar appears ("Saved on this phone only · tap to reconnect"). Keep playing — nothing is lost. Tap the bar when you're ready, and the phone sends its scores to the sheet.
- **No signal.** Scores keep saving on the phone and go up to the sheet when the connection comes back.
- **Two phones entering scores at once.** The last one saved wins, so have one person enter scores and everyone else watch.
- **Viewers.** Friends with Viewer access on the sheet see the live game but can't change it.
- **Season table.** Built from the History tab. Deleting a row there removes that game from the season. Don't edit the *Data* column.
- **Setup tab.** Under *Rules and points* there are **Open sheet** and **Sign out** buttons.
- **Updates.** When the app changes, upload the new `index.html` to GitHub, replacing the old one. Leave `config.js` alone.
- **Permission scope.** The app asks for Google Sheets access because the sheet is shared between friends. It only reads and writes the one sheet set in `config.js`.

## If something goes wrong

| What you see | Fix |
|---|---|
| `Error 400: redirect_uri_mismatch` | The redirect URI in step 3.5 must match your app address exactly, including `/rummy-251/` and the final slash. |
| `Error 403: access_denied` | That Gmail isn't on the Test users list (step 3.4). |
| "Can't open the score sheet" | Check `sheetId` in `config.js`, and that the sheet is shared with the account you signed in with. |
| Still the sign-in screen after setup | `config.js` still has the PASTE placeholders, or GitHub Pages hasn't finished updating (wait a minute and refresh). |
