# Yosvanii portfolio website

A static website (plain HTML, CSS and JavaScript — no framework, no build step).
Home page = grid of project cards. Click a card to open a project page (video that autoplays, section images and GIFs).

```
portfolio-website/
  index.html          <- the whole site (HTML + CSS + JS + project data)
  vercel.json         <- caching + security headers (optional but included)
  README.md
  assets/
    videos/           <- 20 project videos (.mp4, H.264 + AAC, "fast-start")
    gifs/             <- 130 animated GIFs (slug-01.gif, slug-02.gif ...)
    images/           <- card covers, video posters, section images, portrait
    fonts/            <- Inter, Instrument Serif, JetBrains Mono (self-hosted .woff2)
    icons/            <- favicon.svg
```

## Deploy to Vercel

Vercel settings (same for both options):

| Setting | Value |
|---|---|
| Framework Preset | **Other** |
| Root Directory | the folder that contains `index.html` (this folder) |
| Build Command | *(leave empty)* |
| Output Directory | *(leave empty)* |
| Install Command | *(leave empty)* |

### Option A — GitHub (recommended for this much media)
1. Unzip, create a new GitHub repository and push the contents of this folder (so `index.html` is at the repo root).
2. In Vercel: **Add New → Project → Import** the repository.
3. Use the settings in the table above and click **Deploy**.

### Option B — Vercel CLI
```bash
npm i -g vercel
cd portfolio-website
vercel          # preview deployment
vercel --prod   # production deployment
```
When asked, accept the defaults (no framework, no build command, current directory).

### Size note
The folder is about 420 MB (videos 246 MB, GIFs 171 MB). The largest single file is 25 MB, well below Vercel's per-file limit.
If a deploy is ever rejected because of total size, either deploy through Git (Option A) or upload `assets/videos` and `assets/gifs`
to a CDN/object store and set the single line `var BASE='';` near the top of the script in `index.html`
to that CDN's address (for example `var BASE='https://cdn.example.com/';`). Every asset path in the site goes through that one variable.

## Verify the live site
1. Open the production URL and hard-refresh (Ctrl/Cmd+Shift+R).
2. Home page: the portrait and all project covers appear (click **See more** until all 20 cards show).
3. Open each project: the video starts, the poster shows before it loads, and all GIFs animate.
4. In your browser's DevTools → Network tab, filter by "404" — there should be none.
5. Check the **Email me**, **WhatsApp** and **Book a call** buttons.

## Add or change a project
All projects live in the `var P=[ ... ]` list in `index.html`. Copy one entry, change `id`, `t` (title), `tag`, `lead`, `chips`,
and the file paths, then add the matching files to `assets/`. Use lowercase file names — Vercel's servers are case-sensitive.

## What was and was not checked before packaging
Done in a local test (headless Chromium served by a static server):
- Every `assets/...` path referenced in the code exists, and all 130 GIFs, 20 posters, 20 covers, section images, the portrait and the fonts load with no 404.
- No `/_blob/`, `data:` or external font/CDN references remain.
- All 20 project pages open; the grid shows 6 cards, then 20 after clicking **See more** three times; **Back** removes the video and stops playback.
- No horizontal overflow at 390 px, 768 px and 1280 px widths.
- Autoplay/back-button logic confirmed using a stand-in WebM clip.
- All 130 GIFs have more than one frame; all 20 MP4s decode end-to-end without errors (H.264/AAC, metadata at the start of the file so they stream).

Not verified:
- Real MP4 playback inside a browser. The test browser has no H.264 codec, so playback of the actual `.mp4` files was not tested there.
  They are standard H.264 + AAC files that play in Chrome, Edge, Safari and Firefox, but please press play on a couple of projects after deploying.
- Behaviour on Vercel itself (nothing was deployed from the packaging environment).
- Browsers may start the video muted if they block sound autoplay (for example when a project is opened by a direct link).

## Asset notes
- All videos are the original files you supplied, only re-wrapped (not re-encoded) so the metadata sits at the start for faster streaming. Madison.ai uses the original 24 MB file.
- GIFs are your supplied GIFs, except: Madison.ai GIFs 01, 02, 04, 05, 07 and all Peko and MyCover Hero GIF 01 were shrunk (720 px wide) for loading speed, and
  all GIFs for Krisp, Balance, Extend, CloudCannon, CNA Enroll, MyCover Hero (02–07) and EasyDMARC were cut by me from your videos (3 seconds each).
- Covers/posters for Krisp, Balance, Extend, CloudCannon, CNA Enroll, Madison.ai, MyCover Hero are frames from the videos; Grami Chat, Peko and EasyDMARC covers are the images you supplied.

## Second batch (projects 11-20)
Lucky Carrot, Surfe, JustCall IQ, Titanium, "Hiring Platform", Flowla, Fullview, PushOwl, Voicy and Frame were added from the "New Saas Account Portfolio" folder.
- Videos are the original files, only re-wrapped for fast start.
- Each project has 6 GIFs (3 seconds each) cut from the video. The moments were chosen automatically by picking the most active stretches of the video, and the cover and poster are sharp frames from the video.
- The project called "Hiring Platform" (file: Saas Motion Graphic Explainer Video.mp4) has no name in its file name, so the title is a guess from what is on screen. Change `t` in its entry in `index.html` to the real client name.
