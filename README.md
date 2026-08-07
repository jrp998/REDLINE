# REDLINE

Hyrox / CrossFit / hybrid training timer. Big-tile tap-through splits, PB ghost pacer, workout generators, and a tap-to-build workout editor. Runs entirely offline, installs to your home screen like a native app.

---

## 1. Put it on GitHub Pages

1. Create a new repository on GitHub — call it `redline` (public is fine; Pages needs public unless you're on a paid plan).
2. Upload **every file in this folder** to the root of the repo:

```
index.html
sw.js
manifest.json
icon-192.png
icon-512.png
icon-maskable-512.png
apple-touch-icon.png
favicon.png
.nojekyll
README.md
```

   Easiest route: on the repo page choose **Add file → Upload files**, drag the whole lot in, then **Commit changes**.

   > `.nojekyll` is a zero-byte file that stops GitHub from running Jekyll over the site. If your uploader drops it, create it manually with **Add file → Create new file**, name it `.nojekyll`, leave it empty, commit.

3. Go to **Settings → Pages**.
4. Under *Build and deployment*, set **Source** to `Deploy from a branch`, **Branch** to `main` and folder to `/ (root)`. Save.
5. Wait a minute or two. Your app is live at:

```
https://<your-username>.github.io/redline/
```

HTTPS is automatic on Pages, which is required — service workers and install prompts won't run over plain HTTP.

---

## 2. Install it on your devices

Open the URL above on each device, then:

- **Android / Chrome** — tap the ⋮ menu → *Install app* (or *Add to Home screen*). You'll get a prompt automatically on most visits.
- **iPhone / iPad** — you must use **Safari**. Tap the Share button → *Add to Home Screen*. (Chrome on iOS can't install PWAs.)
- **Desktop Chrome / Edge** — click the install icon in the address bar, or ⋮ menu → *Install REDLINE*.

Once installed it launches full screen with no browser chrome, and works with no signal — handy in a gym basement.

### Your data is per-device

There's no account and no server. Each device keeps **its own** history and PBs in local storage. Installing on your phone and your laptop gives you two independent logs; a race saved on one will not appear on the other. Clearing the browser's site data (or deleting the app on iOS) erases that device's history.

To move a session elsewhere, use **Export CSV** on the finish screen.

---

## 3. Updating the app

When you want to ship a change:

1. Edit `index.html`.
2. Bump the version in **two** places so devices know something changed:
   - `index.html` → near the top of the app script: `var APP_VERSION='1.1.1';`
   - `sw.js` → line 4: `const CACHE_VERSION = '1.1.1';`
3. Commit / re-upload both files. Pages redeploys in about a minute.

On each device: open REDLINE → **⚙ Settings → App → Check**. It reports either *You're on the latest version* or *Update ready*, and the button turns into **Reload** to apply it.

Devices also pick up updates on their own — the service worker fetches a fresh `index.html` whenever you're online, so a relaunch after a deploy will usually already be current. The Check button is for when you want to force it.

**If you forget to bump `CACHE_VERSION`,** old cached assets may stick around. Bumping it is what triggers the swap.

---

## What's in it

**Race presets** — Halfrox and FitFest HYROX (both UniActive Wollongong sims) and full HYROX with Open/Pro loads.

**Training generator** — set a duration and focus, and a five-stop slider takes you from an exact race simulation through to a pure gym day. Equipment multi-select; bodyweight always available.

**CrossFit** — 14 benchmark WODs plus a generator for AMRAP, For time, Rounds for time and EMOM. AMRAP loops the round and scores by rounds completed; EMOM advances itself every minute.

**Build your own** — tap movements from an icon palette (search or browse by category), drag to reorder, tap any row to edit reps, distance, sets, rest or load, and repeat the whole thing ×2/×3/×5 for rounds.

**During a race** — one big tile per segment, tap to advance. PB ghost pacer counts down your best-ever split for that movement. Three timing modes: count-up with splits, countdown targets, or goal pace with a live projected finish.

**After** — splits with run pace, PBs per workout and per station, trend charts, effort rating and notes, PNG finish card and CSV export.

**Back up / restore** — Settings → Data. Writes a JSON file of everything; restore with merge (skips duplicates) or replace. This is how you move history between devices.

---

## Notes

- **Everything is in `index.html`** — CSS, all JavaScript, the Anton font (subset, base64) and 40 hand-drawn SVG icons. No build step, no dependencies, no CDN.
- **Data won't sync between devices** by design. If you later want that, the storage layer is a single `Store` object near the top of the app script and is a clean place to bolt on Firebase.
- **Benchmark WOD loads** were checked against published prescriptions (Fran 95/65 lb, Grace and Isabel 135/95 lb, Diane 225/155 lb, Jackie's thrusters an empty 45/35 lb bar, Helen's kettlebell 53/35 lb, Karen's ball 20/14 lb, Barbara's 3-minute rests). Metric conversions are rounded to the nearest gym plate.
- **HYROX** and **CrossFit** are trademarks of their respective owners. This is a personal training tool and isn't affiliated with or endorsed by either.
