# HTML Layers

HTML layers render HTML/CSS content as part of your image or video. This works for both image and video generation.

## Two Modes

### 1. Inline HTML (`html` property)

Provide HTML markup directly:

```json
{
  "type": "html",
  "html": "<div style='color: white; font-size: 48px; text-align: center;'>Hello World</div>",
  "width": 800,
  "height": 400
}
```

### 2. URL Screenshot (`src` property)

Screenshot any HTTPS URL:

```json
{
  "type": "html",
  "src": "https://example.com/dashboard",
  "width": 1920,
  "height": 1080,
  "wait": 3
}
```

`html` and `src` are **mutually exclusive** — use one or the other.

---

## Properties

| Property          | Type          | Required        | Default             | Description                                    |
| ----------------- | ------------- | --------------- | ------------------- | ---------------------------------------------- |
| `html`            | string        | One of html/src | —                   | Inline HTML (max 100KB)                        |
| `src`             | string        | One of html/src | —                   | URL to screenshot (HTTPS only, max 2048 chars) |
| `css`             | string        | No              | —                   | Additional CSS styles (max 50KB)               |
| `tailwindcss`     | boolean       | No              | false               | Enable TailwindCSS CDN                         |
| `googleFonts`     | array         | No              | —                   | Google Font families to load (max 10)          |
| `wait`            | number        | No              | 0                   | Seconds to wait before screenshot (0–5)        |
| `enableLibraries` | boolean       | No              | false               | Enable JS libraries (see below)                |
| `x`               | number        | No              | 0                   | X position in pixels                           |
| `y`               | number        | No              | 0                   | Y position in pixels                           |
| `width`           | number        | No              | canvas/video width  | Render width in pixels                         |
| `height`          | number        | No              | canvas/video height | Render height in pixels                        |
| `position`        | string/object | No              | —                   | Position preset or relative coords             |
| `opacity`         | number        | No              | 1                   | Transparency (0–1) — **image only**            |
| `rotation`        | number        | No              | 0                   | Rotation in degrees — **image only**           |
| `blur`            | number        | No              | —                   | Blur radius in pixels — **image only**         |

**Note:** In video HTML layers, `x`/`y`/`opacity`/`rotation`/`blur` are not available. Use `position` for placement.

### Video-Only Properties

| Property   | Type    | Default | Description                                      |
| ---------- | ------- | ------- | ------------------------------------------------ |
| `animated` | boolean | false   | Record CSS/JS animations as video frames         |
| `duration` | number  | 5       | Animation recording duration in seconds (max 30) |
| `start`    | number  | —       | When layer appears within clip                   |
| `stop`     | number  | —       | When layer disappears within clip                |

---

## Included JavaScript Libraries

Set `enableLibraries: true` to access these libraries:

| Library          | Version  | Global Variable | Best For                                             |
| ---------------- | -------- | --------------- | ---------------------------------------------------- |
| **TailwindCSS**  | Play CDN | —               | Utility-first CSS classes                            |
| **GSAP**         | 3.x      | `gsap`          | Complex animations & timelines                       |
| **Lottie Web**   | 5.x      | `lottie`        | After Effects JSON animations                        |
| **Chart.js**     | 4.x      | `Chart`         | Data visualizations (bar, line, pie, etc.)           |
| **Motion**       | 12.x     | `Motion`        | Spring physics, keyframes, stagger                   |
| **Anime.js**     | 4.x      | `anime`         | Lightweight animation engine                         |
| **D3.js**        | 7.x      | `d3`            | Advanced data visualizations (maps, graphs, charts)  |
| **Lucide Icons** | 1.x      | `lucide`        | 1500+ open-source SVG icons                          |

**Notes:**

- `enableLibraries: true` also automatically enables TailwindCSS
- Libraries are auto-loaded — do not include `<script src>` tags (duplicates are auto-removed)
- TailwindCSS is also available standalone with `tailwindcss: true` (without other libraries)

---

## CSS Support

### Inline Styles

```json
{
  "type": "html",
  "html": "<div style='background: linear-gradient(135deg, #667eea, #764ba2); padding: 40px; color: white; font-size: 32px; border-radius: 16px;'>Styled Content</div>"
}
```

### Separate CSS Property

```json
{
  "type": "html",
  "html": "<div class='card'><h1>Title</h1><p>Description</p></div>",
  "css": ".card { background: #1e293b; padding: 40px; border-radius: 16px; color: white; } .card h1 { font-size: 48px; margin: 0 0 16px; } .card p { font-size: 24px; opacity: 0.8; }"
}
```

### CSS Animations (Videos)

For video HTML layers, use `animated: true` to capture CSS animations:

```json
{
  "type": "html",
  "html": "<div class='pulse'>Hello</div>",
  "css": "@keyframes pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.2); } } .pulse { animation: pulse 2s ease-in-out infinite; font-size: 64px; color: white; text-align: center; }",
  "animated": true,
  "duration": 6,
  "width": 1920,
  "height": 1080
}
```

CSS-only animations do **not** require `enableLibraries` — just `animated: true`.

---

## Google Fonts

Load Google Fonts for use in your HTML:

```json
{
  "type": "html",
  "html": "<div style='font-family: Montserrat; font-weight: 700; font-size: 48px; color: white;'>Beautiful Typography</div>",
  "googleFonts": ["Montserrat", "Open Sans"]
}
```

Up to 10 font families can be loaded per layer.

---

## TailwindCSS

Enable Tailwind utility classes:

```json
{
  "type": "html",
  "html": "<div class='flex items-center justify-center h-full bg-gradient-to-br from-blue-500 to-purple-600'><h1 class='text-6xl font-bold text-white drop-shadow-lg'>Tailwind Styled</h1></div>",
  "tailwindcss": true,
  "width": 1920,
  "height": 1080
}
```

---

## Library Examples

### GSAP Animation (Video)

```json
{
  "type": "html",
  "html": "<div id='box' style='width: 100px; height: 100px; background: #3b82f6; border-radius: 12px;'></div><script>gsap.to('#box', { rotation: 360, duration: 2, repeat: -1, ease: 'power2.inOut' });</script>",
  "enableLibraries": true,
  "animated": true,
  "duration": 6,
  "width": 400,
  "height": 400
}
```

### Chart.js (Static)

```json
{
  "type": "html",
  "html": "<canvas id='chart' width='800' height='400'></canvas><script>new Chart(document.getElementById('chart'), { type: 'bar', data: { labels: ['Jan', 'Feb', 'Mar', 'Apr'], datasets: [{ label: 'Revenue', data: [12, 19, 8, 15], backgroundColor: ['#3b82f6', '#8b5cf6', '#ec4899', '#f59e0b'] }] }, options: { responsive: false, plugins: { legend: { labels: { color: '#fff' } } }, scales: { x: { ticks: { color: '#fff' } }, y: { ticks: { color: '#fff' } } } } });</script>",
  "css": "body { background: transparent; }",
  "enableLibraries": true,
  "width": 800,
  "height": 400
}
```

### Lottie Animation (Video)

```json
{
  "type": "html",
  "html": "<div id='lottie' style='width: 400px; height: 400px;'></div><script>lottie.loadAnimation({ container: document.getElementById('lottie'), renderer: 'svg', loop: true, autoplay: true, path: 'https://assets.lottiefiles.com/packages/lf20_jcikwtux.json' });</script>",
  "enableLibraries": true,
  "animated": true,
  "duration": 5,
  "width": 400,
  "height": 400
}
```

### Anime.js Stagger (Video)

```json
{
  "type": "html",
  "html": "<div style='display: flex; gap: 20px; justify-content: center; align-items: center; height: 100%;'><div class='dot' style='width: 30px; height: 30px; background: #3b82f6; border-radius: 50%;'></div><div class='dot' style='width: 30px; height: 30px; background: #8b5cf6; border-radius: 50%;'></div><div class='dot' style='width: 30px; height: 30px; background: #ec4899; border-radius: 50%;'></div></div><script>anime({ targets: '.dot', translateY: [-30, 0], delay: anime.stagger(200), loop: true, direction: 'alternate', easing: 'easeInOutQuad' });</script>",
  "enableLibraries": true,
  "animated": true,
  "duration": 4,
  "width": 400,
  "height": 200
}
```

### Motion (Video)

```json
{
  "type": "html",
  "html": "<div id='box' style='width:120px;height:120px;background:#6366f1;border-radius:16px;'></div><script>Motion.animate('#box',{x:[0,400],rotate:[0,360],scale:[1,1.2,1]},{duration:2,repeat:Infinity,direction:'alternate',easing:'spring(1,80,10,0)'})</script>",
  "enableLibraries": true,
  "animated": true,
  "duration": 5,
  "width": 1920,
  "height": 1080
}
```

### D3.js (Static)

```json
{
  "type": "html",
  "html": "<svg id='chart' width='800' height='400'></svg><script>const data=[30,86,168,234,12,67,120];const svg=d3.select('#chart');svg.selectAll('rect').data(data).join('rect').attr('x',(d,i)=>i*110+10).attr('y',d=>400-d).attr('width',100).attr('height',d=>d).attr('fill','#6366f1').attr('rx',8);</script>",
  "css": "body { background: #0f172a; }",
  "enableLibraries": true,
  "width": 800,
  "height": 400
}
```

### Lucide Icons (Static)

```json
{
  "type": "html",
  "html": "<div style='display:flex;gap:24px;align-items:center;justify-content:center;height:100%;'><i data-lucide='heart' style='width:64px;height:64px;color:#e11d48;'></i><i data-lucide='star' style='width:64px;height:64px;color:#eab308;'></i><i data-lucide='zap' style='width:64px;height:64px;color:#6366f1;'></i></div><script>lucide.createIcons()</script>",
  "css": "body { background: #0f172a; }",
  "enableLibraries": true,
  "width": 800,
  "height": 400
}
```

**Lucide usage:** Add `<i data-lucide='icon-name'>` elements, then call `lucide.createIcons()` in a `<script>` tag. Browse all icons at [lucide.dev/icons](https://lucide.dev/icons).

---

## Transparent Backgrounds

HTML layers support transparent backgrounds — useful for overlays:

```json
{
  "type": "html",
  "html": "<div style='background: transparent; color: white; font-size: 32px;'>Overlay Text</div>",
  "css": "body { background: transparent; }"
}
```

Set `body { background: transparent; }` in your CSS to ensure the background is transparent.

---

## Global HTML Layers (Video)

HTML layers work with `globalLayers` to span across the entire video. `start`/`stop` are seconds from video start:

```json
{
  "clips": [ ... ],
  "globalLayers": [
    {
      "type": "html",
      "html": "<img src='data:image/svg+xml,...' style='width:120px;opacity:0.5;' />",
      "position": "bottom-right",
      "start": 0,
      "stop": 60
    }
  ]
}
```

See [video-generation.md](video-generation.md#global-layers) for full `globalLayers` documentation.

---

## Limitations

- **Max HTML size:** 100KB
- **Max CSS size:** 50KB
- **Max 5 HTML layers** per job
- **Max Google Fonts:** 10 per layer
- **Max wait time:** 5 seconds
- **Max animation duration:** 30 seconds (video)

### Not Supported

- Custom external `<script>` tags (use `enableLibraries` for included libraries)
- `fetch()`, `XMLHttpRequest` (no network access)
- WebSockets, WebRTC
- Web Workers, SharedWorkers
- localStorage, sessionStorage, cookies, indexedDB
- WebGL, Three.js
- Node.js APIs
