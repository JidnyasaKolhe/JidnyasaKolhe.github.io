# Jidnyasa Kolhe — Cinematic Data Analytics Portfolio

An award-style, single-page interactive portfolio for **Jidnyasa Kolhe**, Senior Data Analyst (Business Intelligence, Category Analytics & Strategy). It reworks the cinematic sticky-scroll "Mostar" template concept into a data-driven narrative: *raw data → structure → insight → strategy → measurable impact.*

Built as **one self-contained `index.html`** (vanilla HTML + CSS + JS). No build step, no framework, no bundler. It runs by opening the file or serving the folder.

---

## Folder structure

```
portfolio/
├── index.html      # The entire site: markup, styles, and scripts (self-contained)
└── README.md       # This file
```

Optional (drop in if you want a real résumé file — see "Résumé" below):
```
└── Jidnyasa-Kolhe-Resume.pdf
```

---

## Run it

Because everything is in one file, you have three options.

**1. Open directly** — double-click `index.html`. All animation is self-contained (no external JS libraries), so the full cinematic scroll works even fully offline; only the Google Fonts load from the network (system fonts substitute when offline).

**2. Local dev server** (recommended — avoids any `file://` quirks):
```bash
# Python (built in on macOS/Linux)
python3 -m http.server 8080
# then open http://localhost:8080

# or Node
npx serve .
```

**3. Production** — no build needed. Upload `index.html` to any static host.

There is **no `npm install`, no build, and no production compile step** — the "production build" *is* the file. If you later port it to the Next.js stack described in the brief, standard `npm install` / `npm run dev` / `npm run build` would apply, but this deliverable intentionally stays vanilla to preserve animation fidelity and portability.

---

## Deployment

Any static host works. Pick one:

- **Netlify** — drag the `portfolio` folder onto the Netlify dashboard, or `netlify deploy --dir=portfolio --prod`.
- **Vercel** — `vercel` from the folder (framework preset: "Other").
- **GitHub Pages** — push the folder to a repo, enable Pages on the branch/root.
- **Cloudflare Pages / S3 / any CDN** — upload `index.html`.

No environment variables are required for the site to run. See "Contact form" for the one optional variable if you wire a form backend.

### Environment variable example
```
# .env  (only needed if you enable a form backend such as Resend)
FORM_ENDPOINT=https://formspree.io/f/xxxxxxx
```
The static site itself reads nothing from the environment; this is only for a backend function you might add.

---

## Sections

1. **Cinematic Hero** — sticky viewport with a code-rendered, depth-layered "data landscape": gradient golden-hour sky, starfield, receding contour ridges, a glowing data-river in the valley, and two gateway structures (built from glowing grid panels + KPI lights) that frame the giant serif **JIDNYASA / KOLHE** and slide outward as the camera pushes through on scroll. Pointer parallax on desktop; cream pill links; towers simplified away on mobile for clarity.
2. **Positioning** — three pillars: Business Intelligence, Commercial Strategy, Analytical Engineering.
3. **Impact Metrics** — six resume-verified metrics with count-up + sparkline reveals (80%, 20%, 25+, 40+, 25%, 30%).
4. **Experience** — three case studies (The Paper Store, Inmar Intelligence, One Roof Technology) with an animated reporting pipeline and a campaign-measurement funnel.
5. **Expertise Matrix** — capability grouped by outcome (no percentage bars).
6. **Selected Work** — six sanitized project cards, each opening an accessible case-study modal.
7. **Education** — analytical progression timeline.
8. **Contact** — actions, contact list, and a validating form that never falsely claims a message was sent.

---

## Content configuration — how to edit

Almost all copy lives as plain HTML in `index.html`, so editing is find-and-replace friendly. Two things are data-driven in JavaScript:

- **Selected Work cards + modals** — edit the `PROJECTS` array near the top of the `<script>` block. Each object has `title, sub, tools, problem, sources, approach, deliverable, value`. Add or remove objects and the grid + modals update automatically.
- **Résumé content** — edit `buildResumeHTML()` in the same script (see below).

To change **metrics**, edit the `data-to` and `data-suffix` attributes on `<span class="counter">` in the Impact section. To change **accent meanings**, edit the CSS variables at the very top (`--cyan-accent`, `--green-accent`, `--gold-accent`, `--coral-accent`).

---

## Résumé (Download button)

The **Download Résumé** button generates a clean, ATS-friendly résumé from the same source-of-truth data (`buildResumeHTML()`), so it works immediately with **no external file**. It downloads as `Jidnyasa-Kolhe-Resume.html`, which opens in any browser and prints to PDF.

**To serve your own PDF instead:** drop `Jidnyasa-Kolhe-Resume.pdf` next to `index.html`, then change the résumé links to:
```html
<a href="Jidnyasa-Kolhe-Resume.pdf" download> … </a>
```
and remove (or leave) the `downloadResume` handler in the script.

---

## Contact form behavior (honest by design)

The form **validates in the browser** (name, valid email, message required) and shows a polished success state — but it does **not** pretend to send anything, because no backend is wired. The status message explicitly says the message was *not* sent and points to the direct email.

**To enable real delivery**, open the `CONTACT FORM` section of the script and follow the clearly-marked **BACKEND INTEGRATION POINT**. Uncomment one option:
- **Formspree** — one `fetch` to your form ID.
- **Netlify Forms** — add `name="contact" data-netlify="true"` to the `<form>`.
- **Resend / custom API** — POST to your endpoint.

Then set `const CONFIGURED = true;`.

---

## Asset manifest

This build has **zero binary assets** — everything is inline for portability and performance:

| Asset | Source |
|---|---|
| Fonts | Google Fonts (Fraunces, Inter, IBM Plex Mono) via `<link>`, with system fallbacks |
| Favicon | Inline SVG data-URI in `<head>` |
| Background scenery | **Original procedurally-painted raster art** (6 WebP layers, ~320KB total, embedded as data URIs): golden-hour sky, two rim-lit ridge layers, glowing data-river, two gateway monoliths, chapter dusk scene with data-bar skyline. Painted via seeded-noise canvas rendering (`paint.html` + `paintrun.mjs` regenerate them) — no stock photos, no AI-generation service, no licensing constraints. |
| Charts / diagrams | Inline SVG (sparklines, pipeline, funnel, card visuals) |
| Animation engine | Custom, fully inline — one eased rAF loop drives hero push-through + chapter zoom/split; IntersectionObserver drives reveals. Zero external JS. |

To fully self-host the fonts too, download Fraunces / Inter / IBM Plex Mono into a `/fonts` folder and swap the Google Fonts `<link>` for local `@font-face` rules.

---

## Responsive behavior summary

- **Desktop (>900px):** full cinematic layout, depth-layered hero with camera push-through, pointer parallax, custom cursor, magnetic buttons, 3-column grids.
- **Tablet (~600–900px):** grids collapse to 1–2 columns; nav switches to a full-screen mobile menu at ≤900px.
- **Mobile (<600px):** parallax and cursor disabled; hero meta stacks and centers; decorative panel labels hidden; pipeline becomes a vertical list; single-column grids; touch-friendly targets.
- No horizontal overflow at any tested width (verified 390px, 768px, 1440px).

---

## Accessibility summary

- Logical heading order (`h1` → `h2` → `h3`), semantic landmarks (`header`, `main`, `nav`, `footer`, `section` with `aria-labelledby`).
- **Skip-to-content** link; visible focus outlines on all interactive elements.
- Project modal: `role="dialog"`, `aria-modal`, focus trap, `Esc` to close, focus returned to the trigger.
- Keyboard operable throughout; mobile menu and cards are buttons/links.
- `aria-live` on form status; descriptive labels on inputs and icon buttons.
- Decorative SVG marked `aria-hidden`; meaningful controls have text/labels.
- **Reduced motion:** `prefers-reduced-motion` disables smooth-scroll, parallax, count-ups (values shown instantly), and transitions. Content is fully readable with animation off.
- The page remains understandable without JavaScript (all content is in the HTML; reveals fall back to visible).

---

## Performance summary

- Single HTML file (~100 KB) — one document request; no bundle, no layout-shifting web components.
- All heavy libraries load `defer` and are **feature-detected** (site is fully functional if they never load).
- Animations use **`transform`/`opacity`** only; scroll work is `requestAnimationFrame`-throttled; reveals use **IntersectionObserver** (elements unobserved after firing).
- Pointer parallax uses a single rAF loop that idles when the pointer is still; no perpetual offscreen animation loops.
- Fonts use `display=swap`; SVG art is inline (no image decode cost); favicon is a data-URI.
- `will-change` applied selectively (hero layers, cursor) rather than globally.

---

## Testing checklist

- [x] Renders with no JavaScript console errors (verified headless Chromium — only offline CDN blocks in the sandbox, which is expected).
- [x] Counters animate to correct resume values (80 / 20 / 25+ / 40+ / 25 / 30).
- [x] Résumé download produces a valid file.
- [x] No horizontal overflow on mobile (390px), tablet, or desktop.
- [x] Project modal opens, traps focus, and closes on `Esc` / overlay / button.
- [x] Nav solidifies on scroll; active-section indicator tracks position.
- [x] Mobile menu opens/closes and locks scroll.
- [x] Contact form validates and does **not** claim false submission.
- [ ] Manual: verify hero push-through + chapter zoom/split choreography in Chrome, Firefox, Safari, Edge (no network needed — engine is inline).
- [ ] Manual: `prefers-reduced-motion` toggle in OS settings.
- [ ] Manual: run Lighthouse (Performance / A11y / Best Practices / SEO) on the deployed URL.

---

## Quality-control notes (factual integrity)

Every factual claim is drawn from the résumé content in the brief. **No** fabricated employers, clients, metrics, testimonials, dates, or dashboard screenshots were introduced. Selected-work visuals are abstract/anonymized SVG with an explicit confidentiality note; no proprietary company data is shown. Methodology language distinguishes descriptive analytics, modeling, and experimental-design work, and avoids implying causation the résumé does not support.

---

## Notes & honest caveats

- I did **not** have the original "Mostar" template file or a résumé PDF attached to the session, so I rebuilt an equivalent cinematic scroll engine from scratch and sourced all content from the facts in the brief. If you share the original template, the motion can be matched more precisely to it.
- The Lighthouse targets in the brief (90+/95+) are realistic for this build but should be **measured on your deployed URL** — I can't run Lighthouse here, so I haven't asserted specific scores.
- If you'd prefer the full **Next.js + TypeScript + Tailwind + Framer Motion** repo structure from the brief instead of this single-file build, that conversion can be done as a follow-up.
