# Long Hours — intermittent fasting tracker

A standalone app for your iPhone. No account, no backend, no analytics.
Everything you log is stored in `localStorage` on the phone itself and never
leaves it. Once installed it opens and runs in airplane mode.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app — HTML, CSS and JS inlined into one file |
| `manifest.webmanifest` | Name, icon and `display: standalone` so it opens without Safari chrome |
| `sw.js` | Service worker; caches the app so it works with no network |
| `icons/` | Home screen and favicon images |

## Installing on iPhone

iOS only offers **Add to Home Screen** from Safari, and only for a page served
over `https://`. A file opened from the Files app can't be installed. So the
files need a web address once — after that the app is cached on the phone and
the address is never used again.

Any of these work. Pick whichever is least friction for you:

- **Drag the folder onto a static host.** Netlify, Cloudflare Pages, GitHub
  Pages, Azure Static Web Apps — all free, all give you HTTPS immediately.
- **Serve it from a machine you already trust** and reach it over HTTPS.
- **Use a tunnel** (`cloudflared tunnel --url http://localhost:8080`) to expose
  a local folder over HTTPS for the two minutes it takes to install.

Then, on the phone:

1. Open the URL **in Safari** (not Chrome — only Safari can install).
2. Tap the **Share** button.
3. Scroll down and tap **Add to Home Screen**.
4. Tap **Add**.

You'll get a Long Hours icon. Opening it launches full screen, with no address
bar, and it works offline from then on.

## Once it's installed

You can delete the hosted copy. The service worker has cached everything.

## Two things worth knowing

**Storage is per-context.** On iOS, the installed home screen app keeps its own
storage, separate from Safari. Anything you logged while testing in Safari won't
appear in the installed app. Log your real data after installing.

**Back up occasionally.** Because the phone holds the only copy, deleting the
app deletes your history with it. The footer has **Back up**, which writes a
`.json` file to Files, and **Restore**, which reads one back. Worth doing every
month or so, and definitely before deleting or replacing the phone.

## Updating later

Replace `index.html`, bump `CACHE` in `sw.js` to a new value (`long-hours-v2`),
and reload the app twice. Your logged data survives — it lives in
`localStorage`, not in the cache.
