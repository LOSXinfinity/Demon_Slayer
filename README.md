# Demon Slayer — Sun & Moon Reveal

![Demo Preview](assets/demo-preview.gif)

An interactive hero banner built around **Yoriichi Tsugikuni** (Sun Breathing) and his brother **Kokushibo** (Moon Breathing). Move your cursor across the screen and a comet-trail mask carves through one image to reveal the other — framed as a Sun vs. Moon showdown, complete with floating embers, ambient lightning, and vertical kanji typography.

Built with a single HTML5 `<canvas>`. **No frameworks, no build step, no dependencies.**

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🌙 **Comet-trail reveal** | 60-point smoothed trail masks the hidden image into a tapering streak that follows the cursor |
| 💜 **Violet cursor glow** | Pulsing radial glow with inner/outer rings marks the reveal point |
| 🔥 **Ember particles** | Full-screen drifting ember field — ambient red near Yoriichi, shifting to violet near Kokushibo |
| ⚡ **Thunder & lightning** | Jagged, branching bolts strike at semi-random intervals with screen flash (purple/white alternating) |
| 🌑 **Configurable darkness** | Transparent black overlay for moodier atmosphere |
| 🖋️ **Vertical kanji + wordmark** | 継国縁壱 (Tsugikuni Yoriichi) & 月ノ呼吸 (Moon Breathing) framed beside gradient "Sun & Moon" title |
| 📦 **Self-contained** | Images embedded as base64 data URIs — runs offline (only Google Fonts loaded externally) |

---

## 🚀 Quick Start

```bash
# Clone and open directly in browser
git clone https://github.com/yourusername/demon-slayer-sun-moon-reveal.git
cd demon-slayer-sun-moon-reveal
open index.html  # or double-click the file
```

**No install step, no server required.** An internet connection is only needed the first time to load Google Fonts (Cinzel, Shippori Mincho, Cormorant Garamond, Bebas Neue).

---

## 📸 Screenshots

| Default View (Yoriichi / Sun) | Revealing Kokushibo (Moon) |
|:---:|:---:|
| ![Yoriichi](assets/screenshot-yoriichi.png) | ![Kokushibo](assets/screenshot-kokushibo.png) |

*Move your mouse/touch to reveal the Moon Breathing wielder beneath the Sun Breathing progenitor.*

---

## 🎮 Interaction

| Platform | Action |
|----------|--------|
| **Desktop** | Move mouse across the screen |
| **Touch** | Drag finger across the image |
| **Hint** | "▶ MOVE TO REVEAL UPPER MOON ONE ◀" fades on first interaction |

---

## 🛠️ Customization

### Using Your Own Images

Replace the two `Image` sources near the top of the `<script>` block in `index.html`:

```js
const bottom = new Image(); // revealed on hover (Kokushibo / Moon)
const top    = new Image(); // shown by default (Yoriichi / Sun)

bottom.src = './images/kokushibo.jpg';
top.src    = './images/yoriichi.jpg';
```

> `top` = visible at rest; `bottom` = revealed inside the cursor's trail.

The drawing loop waits for **both** images to load before starting:

```js
let loaded = 0;
const onLoad = () => { if (++loaded === 2) draw(); };
bottom.onload = onLoad;
top.onload = onLoad;
```

### Tuning Knobs

All configuration lives at the top of the `<script>` block:

| Parameter | Default | Effect |
|-----------|---------|--------|
| `TRAIL_LENGTH` | `64` | Longer = longer comet tail |
| `HEAD_RADIUS` | Auto-scaled (`~26%` of min dimension) | Radius of revealed circle |
| Smoothing factor | `0.14` | Lower = more lag/drift |
| `OVERLAY` | `'rgba(0,0,0,0.30)'` | Higher alpha = darker scene |
| Glow colors | Violet gradients | Cursor-head glow + rings |
| Ember tint | Red → violet near cursor | Particle color shift |
| `nextStrike` timing | `450–1550ms` | Lower = more frequent lightning |
| Bolt jaggedness | `w*0.22` / `6` detail | Higher = wilder bolts |

**Example tweaks:**

```js
// Calmer storm
nextStrike = performance.now() + 1400 + Math.random() * 3200;

// Darker mood
const OVERLAY = 'rgba(0,0,0,0.5)';

// Crimson reveal instead of violet
// Swap rgba(200,150,255…) / rgba(140,70,240…) 
// toward rgba(255,170,90…) / rgba(255,60,40…)
```

---

## ⚙️ How It Works

```
top image (Yoriichi)     ──► visible canvas (+ overlay)
trail circles             ──► offscreen canvas (mask)
bottom image (Kokushibo)  ──► offscreen canvas (source-in composite)
offscreen result          ──► visible canvas
violet glow + rings       ──► visible canvas (lighter blend)
lightning bolts + flash   ──► visible canvas (independent timer)
ember particles           ──► visible canvas (lighter blend, every frame)
```

1. **Top image** (Yoriichi) drawn to visible canvas, darkened with overlay
2. Smoothed cursor position pushed to `trail` array (capped at `TRAIL_LENGTH`)
3. **Offscreen canvas**: each trail point drawn as shrinking/fading black circle
4. **Bottom image** (Kokushibo) composited with `globalCompositeOperation = 'source-in'` — only appears where trail circles exist
5. Offscreen result drawn over visible canvas
6. Violet radial gradient + two rings rendered at trail head (`lighter` blend)
7. Lightning system fires jagged bolts (recursive midpoint displacement) with screen flash
8. Ember particles drift continuously, tinted by distance from reveal point

---

## 🌐 Browser Support

| Browser | Support |
|---------|---------|
| Chrome | ✅ Current |
| Edge | ✅ Current |
| Firefox | ✅ Current |
| Safari | ✅ Current |

Requires Canvas 2D and `requestAnimationFrame` (universally supported). Touch supported via `touchmove`.

---

## 📁 Project Structure (Suggested)

If splitting the single file into a modular project:

```
demon-slayer-reveal/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── reveal.js
└── images/
    ├── yoriichi.jpg    # default (top)
    └── kokushibo.jpg   # revealed (bottom)
```

---

## 📄 Credits & Licensing

- **Code**: Free to use and modify for your own projects (MIT-style)
- **Artwork & Characters**: *Kimetsu no Yaiba* created by **Koyoharu Gotouge**; all character art and trademarks belong to their respective rights holders. The embedded images are fan/demo assets — **replace with art you have rights to use before publishing.**
- **Kanji**: 継国縁壱 (Tsugikuni Yoriichi) & 月ノ呼吸 (Moon Breathing) — standard Japanese. Verify rendering on target devices; CJK glyphs depend on system/browser font stack.
- **Fonts**: Google Fonts (Cinzel, Shippori Mincho, Cormorant Garamond, Bebas Neue)

---

> *The sun and the moon, forever chasing each other across the sky.* ⚔️

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.