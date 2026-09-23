# ATLAS-Dashboard-mk_1 — demo page

One file. No build step, no dependencies, no framework.

```
index.html      the page, video id already set to Ieu1-mhcGyk
CNAME           only if you point nth-atlas-llp.com at it (see below)
```

## Publishing it

**GitHub Pages will not serve from a private repo on a free account.**
`ATLAS_PRE_FILTER_DASH` is private, so this page needs its own public repo —
which is the right shape anyway, since the page shares nothing with the
dashboard's source.

1. Create a public repo, e.g. `atlas-dash-demo`.
2. Commit `index.html` at the root.
3. Settings → Pages → Source: *Deploy from a branch* → `main` / `(root)`.

Live in a minute or two at `https://<user>.github.io/<repo>/`.

If you would rather keep it inside an existing public repo, put `index.html` in
a `docs/` folder and set Pages to `main` / `/docs`.

## Your own domain

You have `nth-atlas-llp.com`. To use it:

1. Add a file called `CNAME` next to `index.html` containing exactly:
   ```
   nth-atlas-llp.com
   ```
2. At your DNS host, point the domain at GitHub:
   - apex (`nth-atlas-llp.com`) → four A records:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - or `www` → CNAME to `<user>.github.io`
3. Settings → Pages picks the domain up; tick **Enforce HTTPS** once the
   certificate is issued (usually within the hour).

## Editing the chapters

```html
<li><button data-t="34">0:34</button><span>Sign-in, and the navigation dashboard</span></li>
```

`data-t` is **seconds**, and clicking the row loads the video starting there.
The label and the number are separate, so keep them in step yourself — or
re-run `build_demo_mk2.ps1`, which prints this block with the real offsets
after every cut.

Current values come from the mk2 cut: the poster frame and card 01 occupy the
first 6.5 s, footage runs to 34.5 s, then card 02 for 10 s.

## What it does that a plain embed does not

- **Click-to-load.** No iframe, no Google request, no cookie until someone
  presses play. A normal `<iframe>` embed contacts Google on every page load
  whether or not anyone watches.
- **`youtube-nocookie.com`**, so even after playing, no tracking cookie is set
  for a viewer who is not signed in.
- **The poster comes from YouTube**, so there is no image to commit. It probes
  `maxresdefault.jpg` and falls back to `hqdefault.jpg` — maxres does not exist
  for uploads under 1280×720, and the naive version of this stretches a grey
  120×90 placeholder across the frame.
- **Keyboard reachable.** The play control is a real `<button>`, so tab-and-enter
  works and a screen reader announces it.
- **`rel=0`** so YouTube does not offer unrelated channels at the end, and
  `playsinline` so iOS does not seize fullscreen.

## What it deliberately does not do

No analytics, no web fonts, no framework. The only third-party request is to
YouTube, and only after a click.

## One thing to do on YouTube

Upload `frame_00.png` as the **custom thumbnail**. It is already 1920×1080 and
857 KB, inside the 2 MB limit. Without it YouTube picks a frame automatically,
and the poster on this page — which is pulled from YouTube — would be whatever
it chose.
