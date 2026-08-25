# Nanophonics — one-pager

Redesign of nanophonics.com. Static site: **no build step, no dependencies, no
third-party requests.** Open `index.html` and it works.

```
index.html                 the whole page
assets/css/style.css       design tokens + all styling
assets/js/main.js          canvas visuals, reveals, nav, accordion, form
assets/fonts/*.woff2       self-hosted variable fonts (Space Grotesk, Inter, JetBrains Mono)
assets/img/favicon.svg     logo mark
assets/img/team/           drop team portraits here (see README.txt inside)
```

Total weight: ~1.4 MB, of which 768 KB is fonts.

---

## Run it locally

```bash
python -m http.server 8000     # then open http://localhost:8000
```

A server is only needed because the fonts are loaded with CORS; opening the
file directly still renders, just with fallback fonts.

## Deploy

Drag the folder into Netlify / Vercel / Cloudflare Pages, or copy it to any
web host. There is nothing to compile.

---

## Design direction

**"Oscilloscope Lab"** — near-black canvas, one saturated accent, grotesk +
mono type pairing, and real signal visualisations instead of stock imagery.
Grounded in the current audio-ML genre (Neural DSP, AudioShake, Supertone,
Krisp), not a generic template.

| Token | Value | Use |
|---|---|---|
| `--bg` | `#08090b` | page |
| `--panel` | `#0e1116` | instrument frames |
| `--acc` | `#d4ff3f` | the single accent — signal lime |
| `--cyan` | `#3fe8ff` | data-viz only, never UI |
| `--f-dsp` | Space Grotesk | headlines |
| `--f-txt` | Inter | body |
| `--f-mon` | JetBrains Mono | labels, numbers, metadata |

Everything lives in the `:root` block at the top of `style.css` — change the
accent there and the whole site follows.

### The visuals are actual maths, not GIFs

Five `<canvas>` animations, all generated at runtime in `main.js`:

- **Hero scope** — a harmonic stack buried in band-limited value noise, with a
  processing edge sweeping left to right. Behind the edge the noise is gone and
  the trace glows; ahead of it, raw input. Readouts (SNR, f₀, latency) track the
  sweep. This is the visual argument for "we turn noise into signal".
- **01 Signal Processing** — one signal decomposed into four components.
- **02 Machine Learning** — an embedding space, points orbiting into three
  clusters with a moving decision boundary.
- **03 Audio Development** — spectrum analyser with a smoothed response curve.
- **04 AI Agents** — a hub dispatching calls to tools and sub-agents of three
  different kinds, several in flight at once, each returning with a result.

Each canvas only animates while it is on screen (`IntersectionObserver`) and
pauses when the tab is hidden, so idle CPU stays at zero.

---

## Things you need to supply

### 1. Nothing outstanding on the team

All four members have their photo, title and copy in place. No placeholders are
left in `index.html`.

Job titles carry no seniority prefix by design: Algorithms Engineer, AI &
Machine Learning Engineer (Marino and Filip), Mobile Engineer.

The originals live in `photos/`. That folder is source material, not part of the
site — you can leave it out when you deploy. To re-cut a portrait, drop a new
file there and re-run the crop (4:5, head high in the frame, 800px wide max).
`photos/filip.jpg` is the superseded square headshot; `filip-nova.jpg` is the one
currently in use.

### 2. A form endpoint

The contact form currently has an empty `action`, so submitting opens the
visitor's mail client pre-filled to `info@nanophonics.com`. It never dead-ends,
but it also never lands in an inbox automatically.

To make it a real POST, put an endpoint in the `action` attribute:

```html
<form class="form" id="contactForm" ... action="https://formspree.io/f/XXXXXXX">
```

`main.js` detects the endpoint and switches to `fetch` + JSON, with success and
error states already wired. Works with Formspree, Basin, Netlify Forms, or your
own handler. A honeypot field (`_gotcha`) is already in place.

### 3. An OG share image

`assets/img/og.png`, 1200×630. Referenced in the `<head>`; until it exists,
link previews just show text.

---

## Notes on content

Copy was carried over from the old site and tightened, with three deliberate
changes:

1. **The team is Ivan Vican, Marino Vican, Filip Dropuljić and Ivan Fabijanović.**
2. **The Tyxit entry lost a stray paragraph.** The old site described Tyxit with
   *"a safe, non-invasive and completely natural way of listening to your baby's
   heartbeat"* — copy belonging to a different product entirely. It is gone.
3. **The hero stats are all derived from claims already on the old site**
   (10+ years in audio, 5 portfolio projects, 4 service areas, iOS + Android).
   Swap in harder numbers if you have them — `index.html`, the `.stats` block.

---

## Accessibility & behaviour

- Skip link, visible focus rings, semantic landmarks, `aria-expanded` on every
  toggle, live region on the form status.
- `prefers-reduced-motion` is honoured: reveals resolve instantly, canvases
  render one static frame, the grain layer is removed.
- Keyboard: everything reachable; Escape closes the mobile menu.
- Verified at 390 px, 900 px and 1440 px.

## Browser support

Modern evergreen browsers. Uses `IntersectionObserver`, CSS nesting-free custom
properties, `backdrop-filter`, and `canvas.roundRect` (with a `rect` fallback).
No IE, no polyfills.

---

## One thing to check with your accountant

The footer used to carry the registry block (Final Fantasy d.o.o., OIB, MB,
IBAN, SWIFT, registered address). You asked for it to come out, so it is gone,
along with the phone number and street address.

Croatian company law (Zakon o trgovačkim društvima) requires a company to state
its firm name, registered seat, register court and registration number, and OIB
on its business documents, and that is generally read to include the company
website. Removing it may leave a gap.

If that matters, the tidiest fix is a small **Impressum** link in the footer
pointing to a short page with the registry details, so the design stays clean
and the obligation is still met. Say the word and I will add it.
