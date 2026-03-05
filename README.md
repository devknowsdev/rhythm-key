# Rhythm & Key — PWA Music Annotator

A browser-based, offline-capable music annotation tool with:
- **Rhythm Calculator** — 16-step kick/snare sequencer that matches your pattern against 21 genre database entries, with audio playback, BPM slider, and detailed genre info per match
- **Key Finder** — Piano keyboard (C3–C5) for selecting notes, with real-time key detection across all 24 major/minor keys, diatonic chords, cadences, and common progressions

## Deploy to Cloudflare Pages via GitHub

**No build step required.** This is a static site — publish as-is.

### 1. Push to GitHub

```bash
git init
git add .
git commit -m "Initial deploy"
git remote add origin https://github.com/YOUR_USERNAME/rhythm-key.git
git push -u origin main
```

### 2. Connect to Cloudflare Pages

1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com) → **Workers & Pages**
2. Click **Create application** → **Pages** → **Connect to Git**
3. Select your GitHub repo
4. **Build settings:**
   - Framework preset: `None`
   - Build command: *(leave empty)*
   - Build output directory: `/` (root)
5. Click **Save and Deploy**

### 3. After deploy

- Your app will be live at `https://YOUR_PROJECT.pages.dev`
- Custom domain: Add in Cloudflare Pages → Custom Domains
- Every `git push` to `main` auto-deploys

---

## Update Cache on Deploy

When making changes, bump the `CACHE_VERSION` string in `sw.js`:

```js
const CACHE_VERSION = 'rhythm-key-v2'; // increment this
```

This forces all users to get fresh assets.

---

## File Structure

```
├── index.html        — Full app (HTML + CSS + JS, no build step)
├── manifest.json     — PWA manifest
├── sw.js             — Service worker (offline support)
├── icon-192.png      — PWA icon
├── icon-512.png      — PWA icon  
├── _headers          — Cloudflare cache headers
└── README.md
```

## Extending the Rhythm Database

All rhythm data lives in the `RHYTHMS` array in `index.html`. Each entry:

```js
{
  id: 'unique-id',
  name: 'Display Name',
  subtitle: 'Short description',
  kick:  [1,0,0,0, 1,0,0,0, ...], // 16 values, 1=hit 0=rest
  snare: [0,0,0,0, 1,0,0,0, ...],
  bpmRange: [min, max],
  bpmTypical: [low, high],
  genres: ['Genre 1', 'Genre 2'],
  origin: ['Country/City'],
  instruments: ['Instrument 1', ...],
  conventions: 'Paragraph about the genre...',
}
```

Pattern notation → array conversion:
- `x---|--x-|----|x---` = `[1,0,0,0, 0,0,1,0, 0,0,0,0, 1,0,0,0]`
