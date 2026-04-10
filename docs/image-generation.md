# Image Generation

Create images by sending a job with `type: "image"` and a config object.

## Config Structure

```json
{
  "type": "image",
  "config": {
    "width": 1080,
    "height": 1080,
    "backgroundColor": "#ffffff",
    "format": "png",
    "quality": 90,
    "layers": []
  }
}
```

### Top-Level Properties

| Property          | Type    | Required | Default     | Description                                                                    |
| ----------------- | ------- | -------- | ----------- | ------------------------------------------------------------------------------ |
| `width`           | integer | Yes      | —           | Canvas width in pixels (1–4096)                                                |
| `height`          | integer | Yes      | —           | Canvas height in pixels (1–4096)                                               |
| `backgroundColor` | string  | No       | transparent | Hex, rgb, or named color                                                       |
| `background`      | string  | No       | —           | **DEPRECATED** — alias for `backgroundColor`, kept for backwards compatibility |
| `format`          | string  | No       | `"png"`     | `"png"`, `"jpeg"`, `"jpg"`, `"webp"`                                           |
| `quality`         | integer | No       | 90          | Quality for JPEG/WebP (1–100)                                                  |
| `layers`          | array   | Yes      | —           | Array of layer objects (max 50)                                                |
| `defaults`        | object  | No       | —           | Default properties for layers                                                  |

### Output Formats

- **PNG** — lossless, supports transparency, largest files
- **JPEG** — lossy, no transparency, smaller files
- **WebP** — lossy, supports transparency, smallest files

---

## Layer Types

Layers render **bottom to top** — first layer in the array is the background.

### 1. Image Layer

Display an uploaded image or image from a public URL.

```json
{
  "type": "image",
  "path": "https://images.unsplash.com/photo-example.jpg",
  "x": 50,
  "y": 50,
  "width": 400,
  "height": 300,
  "fit": "cover",
  "opacity": 1,
  "rotation": 0,
  "borderRadius": 20,
  "blur": 0
}
```

| Property       | Type          | Required | Default  | Description                                                     |
| -------------- | ------------- | -------- | -------- | --------------------------------------------------------------- |
| `path`         | string        | Yes      | —        | Internal path (`/...`) or public URL                            |
| `x`            | number        | No       | 0        | X position in pixels                                            |
| `y`            | number        | No       | 0        | Y position in pixels                                            |
| `width`        | number        | No       | —        | Width in pixels                                                 |
| `height`       | number        | No       | —        | Height in pixels                                                |
| `position`     | string/object | No       | —        | Position preset or relative coords (overrides x/y)              |
| `fit`          | string        | No       | `"fill"` | How image fits: `cover`, `contain`, `fill`, `inside`, `outside` |
| `opacity`      | number        | No       | 1        | Transparency (0–1)                                              |
| `rotation`     | number        | No       | 0        | Rotation in degrees                                             |
| `blur`         | number        | No       | 0        | Blur radius in pixels                                           |
| `borderRadius` | number        | No       | 0        | Corner radius in pixels                                         |

**Fit modes:**

- `cover` — crop to fill dimensions exactly
- `contain` — fit within dimensions, preserving ratio
- `fill` — stretch to fill (may distort)
- `inside` — like contain but never enlarges
- `outside` — scale to cover dimensions

### 2. Text Layer

Add text with custom fonts and styling.

```json
{
  "type": "text",
  "text": "Hello World",
  "x": 100,
  "y": 200,
  "fontSize": 48,
  "fontFamily": "Arial",
  "color": "#ffffff",
  "align": "center",
  "opacity": 1
}
```

| Property          | Type          | Required | Default     | Description                                                                                                                                                                  |
| ----------------- | ------------- | -------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `text`            | string        | Yes      | —           | Text content (supports `\n` for line breaks)                                                                                                                                 |
| `x`               | number        | No       | 0           | X position in pixels                                                                                                                                                         |
| `y`               | number        | No       | 0           | Y position in pixels                                                                                                                                                         |
| `position`        | string/object | No       | —           | Position preset or relative coords (overrides x/y)                                                                                                                           |
| `width`           | number        | No       | —           | Width for text wrapping                                                                                                                                                      |
| `height`          | number        | No       | auto        | Height in pixels. Only used for blur effect calculations, not for text rendering. Auto-calculated as `fontSize × 1.3` (single line) or `lines × fontSize × lineHeight × 1.3` |
| `fontSize`        | number        | No       | 24          | Font size in pixels                                                                                                                                                          |
| `fontFamily`      | string        | No       | `"Arial"`   | Font family name                                                                                                                                                             |
| `fontPath`        | string        | No       | —           | Path to uploaded custom font file                                                                                                                                            |
| `googleFont`      | string        | No       | —           | Google Font, e.g. `"Roboto:600"`                                                                                                                                             |
| `color`           | string        | No       | `"#000000"` | Text color                                                                                                                                                                   |
| `align`           | string        | No       | `"left"`    | `"left"`, `"center"`, `"right"`                                                                                                                                              |
| `wrap`            | boolean       | No       | false       | Enable text wrapping (requires `width`)                                                                                                                                      |
| `lineHeight`      | number        | No       | 1.2         | Line height multiplier                                                                                                                                                       |
| `backgroundColor` | string        | No       | —           | Background color behind text                                                                                                                                                 |
| `opacity`         | number        | No       | 1           | Transparency (0–1)                                                                                                                                                           |
| `rotation`        | number        | No       | 0           | Rotation in degrees                                                                                                                                                          |
| `blur`            | number        | No       | 0           | Blur radius                                                                                                                                                                  |

**Font options:**

- `fontFamily` — use any system font (Arial, Helvetica, etc.)
- `fontPath` — use an uploaded font file (`.ttf`, `.otf`, `.woff`, `.woff2`)
- `googleFont` — use any Google Font, format: `"FontName:weight"` (e.g. `"Montserrat:700"`)
- Cannot combine `fontPath` and `googleFont` on the same layer

**Text wrapping:**

- Set `wrap: true` and specify `width` (required for wrapping)
- Use `lineHeight` to control line spacing (e.g. `1.4`)
- Manual line breaks with `\n` work with or without `wrap`

### 3. Rectangle Layer

Solid or outlined rectangular shapes.

```json
{
  "type": "rectangle",
  "x": 50,
  "y": 50,
  "width": 300,
  "height": 200,
  "fill": "#3b82f6",
  "stroke": "#1e40af",
  "strokeWidth": 2,
  "borderRadius": 12,
  "opacity": 0.9
}
```

| Property       | Type          | Required | Default | Description                        |
| -------------- | ------------- | -------- | ------- | ---------------------------------- |
| `x`            | number        | No       | 0       | X position in pixels               |
| `y`            | number        | No       | 0       | Y position in pixels               |
| `width`        | number        | No       | 100     | Width in pixels                    |
| `height`       | number        | No       | 100     | Height in pixels                   |
| `position`     | string/object | No       | —       | Position preset or relative coords |
| `fill`         | string        | No       | —       | Fill color                         |
| `stroke`       | string        | No       | —       | Border color                       |
| `strokeWidth`  | number        | No       | —       | Border width in pixels             |
| `opacity`      | number        | No       | 1       | Transparency (0–1)                 |
| `rotation`     | number        | No       | 0       | Rotation in degrees                |
| `blur`         | number        | No       | 0       | Blur radius                        |
| `borderRadius` | number        | No       | 0       | Corner radius                      |

### 4. Circle Layer

Circles and ellipses.

```json
{
  "type": "circle",
  "x": 200,
  "y": 200,
  "width": 100,
  "height": 100,
  "fill": "#ef4444",
  "stroke": "#ffffff",
  "strokeWidth": 3
}
```

| Property      | Type          | Required | Default | Description                                |
| ------------- | ------------- | -------- | ------- | ------------------------------------------ |
| `x`           | number        | No       | 0       | X position in pixels                       |
| `y`           | number        | No       | 0       | Y position in pixels                       |
| `width`       | number        | No       | 50      | Width (equal to height for perfect circle) |
| `height`      | number        | No       | 50      | Height                                     |
| `position`    | string/object | No       | —       | Position preset or relative coords         |
| `fill`        | string        | No       | —       | Fill color                                 |
| `stroke`      | string        | No       | —       | Border color                               |
| `strokeWidth` | number        | No       | —       | Border width                               |
| `opacity`     | number        | No       | 1       | Transparency (0–1)                         |
| `blur`        | number        | No       | 0       | Blur radius                                |

### 5. Gradient Layer

Linear or radial color gradients.

```json
{
  "type": "gradient",
  "x": 0,
  "y": 0,
  "width": 1080,
  "height": 1080,
  "gradient": {
    "type": "linear",
    "colors": ["#667eea", "#764ba2"],
    "direction": "diagonal"
  }
}
```

| Property   | Type          | Required | Default       | Description            |
| ---------- | ------------- | -------- | ------------- | ---------------------- |
| `x`        | number        | No       | 0             | X position in pixels   |
| `y`        | number        | No       | 0             | Y position in pixels   |
| `width`    | number        | No       | canvas width  | Width in pixels        |
| `height`   | number        | No       | canvas height | Height in pixels       |
| `position` | string/object | No       | —             | Position preset        |
| `gradient` | object        | Yes      | —             | Gradient configuration |
| `opacity`  | number        | No       | 1             | Transparency (0–1)     |
| `rotation` | number        | No       | 0             | Rotation in degrees    |
| `blur`     | number        | No       | 0             | Blur radius            |

**Gradient object:**
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `type` | string | `"linear"` | `"linear"` or `"radial"` |
| `colors` | array | — | Array of color strings (min 2) |
| `direction` | string | `"diagonal"` | `"horizontal"`, `"vertical"`, `"diagonal"` (linear only) |

### 6. HTML Layer

Render HTML/CSS or screenshot a URL. See [html-layers.md](html-layers.md) for full details.

```json
{
  "type": "html",
  "html": "<div style='padding: 20px; background: #1e293b; color: white; border-radius: 12px;'><h1>Custom HTML</h1></div>",
  "width": 400,
  "height": 200,
  "position": "center"
}
```

---

## Positioning

Three ways to position any layer:

### 1. Pixel Coordinates (x/y)

```json
{ "x": 100, "y": 50 }
```

Origin is top-left (0,0). X increases right, Y increases down.

### 2. Position Presets (string)

```json
{ "position": "center" }
```

| Value            | Location           |
| ---------------- | ------------------ |
| `"center"`       | Center of canvas   |
| `"top"`          | Top center         |
| `"bottom"`       | Bottom center      |
| `"top-left"`     | Upper left corner  |
| `"top-right"`    | Upper right corner |
| `"center-left"`  | Middle left        |
| `"center-right"` | Middle right       |
| `"bottom-left"`  | Lower left corner  |
| `"bottom-right"` | Lower right corner |

### 3. Relative Position (object)

```json
{
  "position": {
    "x": 0.5,
    "y": 0.5,
    "originX": "center",
    "originY": "center"
  }
}
```

| Property  | Type   | Range | Description                                                         |
| --------- | ------ | ----- | ------------------------------------------------------------------- |
| `x`       | number | 0–1   | Horizontal position (0 = left, 1 = right)                           |
| `y`       | number | 0–1   | Vertical position (0 = top, 1 = bottom)                             |
| `originX` | string | —     | `"left"`, `"center"`, `"right"` — which part of element aligns to x |
| `originY` | string | —     | `"top"`, `"center"`, `"bottom"` — which part of element aligns to y |

**Important:** If both `x`/`y` and `position` are set, `position` takes precedence.

---

## Visual Effects

### Opacity

- Range: 0 (invisible) to 1 (fully opaque)
- Useful values: 0.9 subtle, 0.7 semi-transparent, 0.5 half, 0.3 watermarks

### Rotation

- Positive = clockwise, negative = counter-clockwise
- Range: -360° to 360°
- Rotates around element center

### Blur

- Value in pixels
- Useful ranges: 1–2 subtle, 3–5 noticeable, 6–10 strong, 10+ heavy

### Common Patterns

**Glass morphism effect:**

```json
{
  "layers": [
    {
      "type": "gradient",
      "gradient": { "colors": ["#667eea", "#764ba2"] },
      "width": 1080,
      "height": 1080
    },
    {
      "type": "rectangle",
      "x": 140,
      "y": 240,
      "width": 800,
      "height": 600,
      "fill": "#ffffff",
      "opacity": 0.15,
      "blur": 10,
      "borderRadius": 24
    }
  ]
}
```

**Neon glow text:**

```json
{
  "layers": [
    { "type": "rectangle", "width": 800, "height": 400, "fill": "#000000" },
    {
      "type": "text",
      "text": "NEON",
      "fontSize": 72,
      "color": "#00ffff",
      "position": "center",
      "blur": 15,
      "opacity": 0.4
    },
    {
      "type": "text",
      "text": "NEON",
      "fontSize": 72,
      "color": "#00ffff",
      "position": "center",
      "blur": 5,
      "opacity": 0.7
    },
    {
      "type": "text",
      "text": "NEON",
      "fontSize": 72,
      "color": "#ffffff",
      "position": "center"
    }
  ]
}
```

---

## Defaults System

Set default properties to avoid repetition across layers:

```json
{
  "width": 1080,
  "height": 1080,
  "defaults": {
    "layer": {
      "opacity": 0.9
    },
    "layerType": {
      "text": {
        "fontSize": 24,
        "color": "#333333",
        "fontFamily": "Helvetica"
      },
      "image": {
        "fit": "cover"
      }
    }
  },
  "layers": [...]
}
```

**Priority (highest wins):**

1. Individual layer properties
2. `defaults.layerType.{type}` — type-specific defaults
3. `defaults.layer` — all-layer defaults

---

## Common Dimensions

| Use Case          | Width | Height |
| ----------------- | ----- | ------ |
| Instagram Post    | 1080  | 1080   |
| Instagram Story   | 1080  | 1920   |
| Facebook Post     | 1200  | 630    |
| Twitter/X Post    | 1200  | 675    |
| YouTube Thumbnail | 1280  | 720    |
| LinkedIn Post     | 1200  | 627    |
| Blog Header       | 1200  | 600    |
| Open Graph        | 1200  | 630    |
