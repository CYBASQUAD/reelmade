# Reelmade

Landing page for **Reelmade** — a done-for-you promo video service for small businesses. $19 per video, 24-hour turnaround, WhatsApp-based ordering. AI handles the heavy lifting; a human reviewer in Bali finishes every video before it ships.

---

## At a glance

| | |
|---|---|
| **Live site** | https://cybasquad.github.io/reelmade/ |
| **Repo** | https://github.com/CYBASQUAD/reelmade |
| **Hosting** | GitHub Pages (deploy from `main` branch, root) |
| **Stack** | Single-file static site — `index.html` only. No build step, no framework, no dependencies beyond Google Fonts. |
| **Contact channel** | WhatsApp only: `+62 813 3085 8293` |
| **Owner** | Fouad (project) |

---

## Product summary (for context)

- **Offer:** one promo video, $19 flat, delivered in 24 hours
- **No subscriptions, no tiers, no logins, no dashboard**
- **Channel:** orders happen entirely via WhatsApp deep-links from the site
- **Output:** vertical 9:16 reel by default (Instagram / TikTok / Shorts), with 1:1 and 16:9 on request, captions included, customer owns the file
- **Positioning:** "AI Speed. Human Taste." — explicitly positioned against pure-AI generic output and against slow/expensive agencies
- **Audience:** non-technical SMB owners (cafés, salons, gyms, real estate, hotels, online coaches, etc.) who don't want to learn another tool

These are strategic decisions, not technical defaults. See **Strategic guardrails** before changing any of them.

---

## File structure

```
.
├── index.html              The entire site. Inline CSS + inline JS.
├── README.md               You are here.
├── directives.md           Internal project memory / scratch notes.
├── .gitignore
├── hero/                   Uncompressed 1080x1920 reels for the hero rotation.
│   ├── hero-reel.mp4       (~52 MB) Reelmade brand reel — "AI Speed. Human Taste."
│   ├── coach.mp4           (~32 MB) Online coach personal brand reel
│   ├── trave_agent_sara.mp4 (~28 MB) Travel agent service reel
│   └── generic_barbershop.mp4 (~14 MB) Barbershop local business reel
├── hero-reel.mp4           (~7 MB) Compressed 720p version, used by samples tile
├── coach.mp4               (~4 MB) Compressed sample
├── trave_agent_sara.mp4    (~4 MB) Compressed sample
├── generic_barbershop.mp4  (~3 MB) Compressed sample
└── originals/              Local-only backup of pre-compression videos. Gitignored.
```

**Why two copies of every reel:** the hero is the first impression, so it streams the original-quality file from `/hero/`. The samples section below the fold uses 720p CRF-26 compressed versions in the repo root — compression is invisible at thumbnail size, and the page weight drops dramatically (127 MB → 18 MB for the sample set).

---

## Local development

There is no build step. To work on it:

```bash
git clone git@github.com:CYBASQUAD/reelmade.git
cd reelmade
# Open index.html directly, or serve it:
python3 -m http.server 8000
# → http://localhost:8000
```

Any local HTTP server works (Python, `npx serve`, VS Code Live Server, etc.). Opening `index.html` via `file://` mostly works too, but autoplay and video paths behave better over HTTP.

---

## Deployment

GitHub Pages is configured to serve from `main` branch, root directory.

**To deploy:** push to `main`. GitHub rebuilds within ~30–60 seconds.

```bash
git add index.html
git commit -m "..."
git push
```

**Check build status:**
```bash
gh api repos/CYBASQUAD/reelmade/pages/builds/latest --jq '{status, error: .error.message, commit, updated_at}'
```

**Verify the deploy:**
```bash
curl -sI https://cybasquad.github.io/reelmade/ | grep -i last-modified
```

Browser cache TTL is 600s. If you don't see your change, hard-refresh (`Cmd+Shift+R`) or open incognito.

---

## Brand system

All styling is inline in `<style>` inside `index.html`. CSS variables at the top define the brand palette and tokens.

### Palette

| Token | Hex | Use |
|---|---|---|
| `--baliCream` | `#f4f2ef` | Page background, light surfaces |
| `--baliOrangeDark` | `#944914` | Primary accent, CTAs, brand color |
| `--baliOrangeDark-hover` | `#b86820` | CTA hover state |
| `--baliOrangeLight` | `#ecdcd1` | Tint backgrounds (Soul section, FAQ, stats band) |
| `--baliBlack` | `#1c1612` | Headings, dark surfaces (nav CTA, video frame, footer) |
| `--baliDarkGray` | `#3a322b` | Body text |
| `--baliGray` | `#c8c8c8` | Hairlines, borders |
| `--baliBlue` | `#4dbdd0` | Secondary accent (currently unused after the meme-flow removal — kept for future) |

### Typography

- **Headings:** `Fraunces` — Google Font, weights 400/600/700, with italic variants. The italic terracotta `<em>` inside headings is a signature pattern (e.g. *"Human Taste."*).
- **Body:** `DM Sans` — Google Font, weights 300/400/500/600.
- These are loaded via a single Google Fonts `<link>` in `<head>`.

### Layout constants

- `--content-max: 1280px` — max content width
- `--gutter: 48px` desktop, `24px` mobile — outer padding
- `section { padding: 80px 0 }` — vertical rhythm between sections

### Component patterns

- **Pill buttons** with letter-spaced uppercase labels (`.btn-primary`, `.btn-white`, `.btn-outline-white`, `.btn-secondary`)
- **Trust strip** with dot-separator bullets (`.hero-trust`, `.cta-included`)
- **Section eyebrow + serif title with italic punch line + body sub** — the consistent section opener pattern
- **Cards with dashed borders → now removed for samples (real videos)**; industry tiles use a solid border that turns terracotta on hover

---

## Page anatomy (top to bottom)

1. **Nav** (`<nav>`) — fixed, blurred cream bar with brand wordmark + "Get Started" CTA
2. **Hero** (`.hero`) — eyebrow + h1 ("AI Speed. *Human Taste.*") + body + 2 CTAs + trust strip + phone-framed video on the right
3. **Stats band** (`.stats-section`) — `$19 / 24h / 1 / 0` in giant Fraunces serif
4. **Industry quick-pick** (`#industries`) — 12 niches in 2-row grid; each card is a WhatsApp deep-link with a niche-specific pre-filled message
5. **Video types** (`#formats`) — 6 format cards (Offer Ad, Product Showcase, Brand Intro, AI Presenter, Real Estate Walkthrough, Restaurant Reel); also WhatsApp deep-links
6. **Samples** (`#samples`) — 4 playable reels (brand reel, coach, travel agent, barbershop)
7. **Soul** (`.soul-section`) — "AI does the building / Humans do the thinking" two-column comparison + founder note
8. **FAQ** (`#faq`) — 6 collapsible `<details>` items
9. **CTA** (`#contact`) — closing section, terracotta background, "Your next customer is scrolling right now."
10. **Footer** — brand wordmark + copyright line

---

## Key behaviors and how they work

### 1. Hero video — random rotation per page load

The hero video element has no hardcoded `src`. Instead it declares the rotation set via a `data-reels` attribute:

```html
<video id="heroReel" autoplay muted loop playsinline preload="metadata"
       data-reels="hero/hero-reel.mp4,hero/coach.mp4,hero/trave_agent_sara.mp4,hero/generic_barbershop.mp4">
</video>
```

A tiny inline script near the bottom of `<body>` picks one at random and assigns it:

```js
var reels = (video.dataset.reels || '').split(',').map(s => s.trim()).filter(Boolean);
var idx = Math.floor(Math.random() * reels.length);
video.src = reels[idx];
video.play().catch(() => {});
```

Refresh = new random reel (1/4 chance of repeat). Different visitors at the same time will see different reels. No tracking, no cookies, no localStorage.

`preload="metadata"` means only the metadata of the selected reel streams initially (a few hundred KB); the full file streams when playback starts.

### 2. Hero video — tap-to-unmute

Browsers block autoplay of videos with sound. The hero must start `muted` to autoplay. A small pill button in the bottom-right corner of the phone frame toggles audio:

```js
btn.addEventListener('click', () => {
  video.muted = !video.muted;
  btn.dataset.muted = video.muted ? 'true' : 'false';
  if (!video.muted) video.play().catch(() => {});
});
```

CSS swaps the icon and label based on `data-muted` attribute on the button.

### 3. Industry quick-pick → WhatsApp deep-links

Every card in the industry grid is an `<a>` to `https://wa.me/<number>?text=<urlencoded-message>`. The message is pre-filled per niche:

```
https://wa.me/6281330858293?text=Hi%20Reelmade%2C%20I%20run%20a%20caf%C3%A9...
```

When the user taps, WhatsApp opens (on mobile) or wa.me web (on desktop) with the message ready to send. **There is no form, no backend, no database.** The entire intake funnel is WhatsApp.

The same pattern is used for the format cards (`Offer Ad`, etc.) and for the main CTAs.

### 4. Animated SVG icons (industry grid)

Each industry card has an inline SVG icon with hover animations. Some are uniform (lift + scale on hover); six niches have bespoke keyframe animations:

| Niche | Animation |
|---|---|
| Café | Steam wisps rise from cup |
| Gym | Dumbbell pumps up and down |
| Yoga | Figure breathes (scale + tilt) |
| Salon | Scissor blades snip |
| Tour | Palm sways |
| Retail | Shopping bag swings |
| Services | Wrench rotates |
| Something else | Sparkle twinkles + rotates |

These are pure CSS `@keyframes` on `:hover` — no JS.

### 5. FAQ

Pure semantic HTML `<details>` / `<summary>`. Custom CSS hides the default marker and swaps `+` for `–` when open. No JS.

### 6. Daily rotation alternative (not currently in use)

There is a previous version of the hero rotation script that used calendar-day rotation:

```js
var dayIndex = Math.floor(Date.now() / 86400000) % reels.length;
```

`86400000 = 24 * 60 * 60 * 1000`. Replace the `Math.random()` line with the day-index line to switch back. Same reel for all visitors on the same UTC day, rotates each midnight UTC.

---

## How to update common things

### Change the WhatsApp number
- Find/replace `6281330858293` across `index.html`. Currently appears in ~20 places (industry cards, format cards, CTAs).

### Update pricing or turnaround
- `$19` appears in: hero sub, stats band, formats cards, FAQ, CTA, all WhatsApp pre-fills, and meta description in `<head>`.
- `24` / `24h` / `24 hours` appears in: hero sub, stats band, FAQ, CTA, meta description.

### Add or change an industry niche
1. Add a new `<a class="industry-card">` block inside `.industry-row`
2. Match the existing pattern: WhatsApp link with pre-filled message, inline SVG icon, name, hint
3. The grid auto-stacks to clean rows: 2 cols mobile → 3 → 4 → 6 desktop. If you change the count from 12, check the breakpoint logic so you don't end up with orphan rows.

### Add or replace a sample reel
1. Drop the new MP4 into the repo root (compressed) — keep it under 10 MB ideally
2. Add a `.sample-tile` block to `.samples-grid` referencing the new file
3. If it should also appear in the hero rotation, place the original-quality copy in `/hero/` and add its filename to the `data-reels` attribute on `#heroReel`

### Compress a video (when adding new content)
```bash
ffmpeg -i input.mp4 \
  -vf "scale=720:1280" \
  -c:v libx264 -crf 26 -preset medium \
  -c:a aac -b:a 96k \
  -movflags +faststart \
  output.mp4
```
Typical reduction: 80–90% with no perceptible quality loss at thumbnail size.

### Update a FAQ question
Each is a `<details class="faq-item">` block inside `#faq`. The `<summary>` is the question; the `<div class="faq-body">` is the answer.

### Edit copy in the closing CTA
Look for `<!-- CTA -->` in `index.html`. Headline, body, the "what's included" mini-strip, and the button label are all there.

### Change the stats numbers
Look for `<!-- STATS BAND -->`. Each stat is `<div class="stat-num">` (big Fraunces) + `<div class="stat-label">` (small caps). The italic `<em>` accents on `$` and `h` use the brand orange.

---

## Strategic guardrails

These choices were intentional and matter to the brand. Do not change them without alignment:

1. **One price, no tiers, no comparison table.** $19 flat. A pricing table implies choice; this site sells decisiveness.
2. **No subscription, no logins, no dashboard.** The entire user experience is "send a WhatsApp." Adding a self-serve flow breaks the value proposition.
3. **Single contact channel: WhatsApp.** No email. No contact form. The previous version had an email button (`BaliBot@alabali.live`) that was a leak from a different project and has been removed.
4. **Editorial brand (Fraunces + cream + terracotta), not generic SaaS.** A user who lands on Reelmade and bounces will still remember it. Killing the serif or switching to a white-background-blue-accent palette would make the site indistinguishable from competitors (Captions, HeyGen, MakeUGC, etc.).
5. **AI + Human framing.** "Pure AI is generic. Pure agencies are expensive and slow." This is the positioning. Don't drop the human side.
6. **Hero video is the proof.** Compressed thumbnails are fine for the samples section. The hero must stay original quality.

---

## Known limitations and follow-ups

- **No analytics.** No GA4, no Plausible, no events. If you want to measure WhatsApp click-throughs, the cleanest path is `wa.me` link tagging via UTMs or a redirect through a tracker.
- **Repo size.** ~150 MB of video assets in git. Fine for now (GitHub Pages soft limit is 1 GB, file hard limit is 100 MB). If repo size becomes a problem, move the hero originals to object storage (Cloudflare R2, Bunny CDN) and update `data-reels` URLs.
- **No real metrics in the stats band.** Numbers shown ($19, 24h, 1, 0) are deal terms, not performance. When you have data — "200 reels delivered," "average watch %" — swap them in. Same Fraunces treatment, different content.
- **No real testimonials or case studies.** The Soul section has a founder note ("Hi, I'm Fouad...") but no third-party social proof. As soon as you have 3–5 satisfied customers, add a dedicated case-studies row above the FAQ.
- **No favicon image.** Currently a generated SVG data-URI in `<head>`. Replace with a proper file when there's a logo asset.
- **No multilingual support.** Page is English-only; voiceover languages are advertised in the FAQ.
- **`trave_agent_sara.mp4` filename has a typo** ("trave" missing the second `l`). UI labels it correctly as "Travel Agent" — but the filename is shipped this way. Renaming requires updating both the `data-reels` attribute on the hero, the `<video src>` in the samples section, and the file itself.

---

## Handover checklist

- [ ] Confirm GitHub Pages settings: Settings → Pages → Source = `main` branch, root folder
- [ ] Confirm WhatsApp number ownership / WhatsApp Business setup for receiving orders
- [ ] Set up basic analytics if needed (Plausible recommended — privacy-friendly, single script tag)
- [ ] Decide on a real contact email (currently no email channel on the site)
- [ ] Plan compression workflow for future video uploads
- [ ] Optional: move hero videos off-repo to a CDN if repo size becomes a concern
