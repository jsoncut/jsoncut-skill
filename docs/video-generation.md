# Video Generation

Create videos by sending a job with `type: "video"` and a config object.

## Config Structure

```json
{
  "type": "video",
  "config": {
    "width": 1920,
    "height": 1080,
    "fps": 30,
    "format": "mp4",
    "clips": [
      {
        "duration": 3,
        "layers": [],
        "transition": { "name": "fade", "duration": 1 }
      }
    ]
  }
}
```

### Top-Level Properties

| Property             | Type    | Required | Default | Description                                 |
| -------------------- | ------- | -------- | ------- | ------------------------------------------- |
| `width`              | integer | No       | 1280    | Video width in pixels (1–4096)              |
| `height`             | integer | No       | 720     | Video height in pixels (1–4096)             |
| `fps`                | integer | No       | 25      | Frames per second (1–120)                   |
| `format`             | string  | No       | `"mp4"` | `"mp4"` or `"mov"`                          |
| `fast`               | boolean | No       | false   | Fast processing mode (for preview)          |
| `clips`              | array   | Yes      | —       | Array of clip objects (max 100)             |
| `defaults`           | object  | No       | —       | Default properties for clips/layers         |
| `audioFilePath`      | string  | No       | —       | Background audio file path                  |
| `loopAudio`          | boolean | No       | false   | Loop background audio if shorter than video |
| `outputVolume`       | number  | No       | 1       | Final output volume (0–1)                   |
| `keepSourceAudio`    | boolean | No       | false   | Keep audio from video layers                |
| `clipsAudioVolume`   | number  | No       | 1       | Volume for clip audio vs tracks (0–1)       |
| `audioTracks`        | array   | No       | —       | Independent audio tracks                    |
| `audioNorm`          | object  | No       | —       | Audio normalization/ducking                 |
| `autoSubtitles`      | boolean | No       | false   | Auto-generate subtitles (Pro/Enterprise)    |
| `autoSubtitlesStyle` | object  | No       | —       | Subtitle styling                            |

---

## Clips

Each clip is a scene in the video. Clips play sequentially.

```json
{
  "duration": 5,
  "layers": [
    { "type": "fill-color", "color": "#1a1a2e" },
    {
      "type": "title",
      "text": "Welcome",
      "fontSize": 64,
      "textColor": "#ffffff"
    }
  ],
  "transition": {
    "name": "fade",
    "duration": 1
  }
}
```

| Property     | Type        | Required | Default       | Description                              |
| ------------ | ----------- | -------- | ------------- | ---------------------------------------- |
| `duration`   | number      | No       | from defaults | Clip duration in seconds (min 0.1)       |
| `layers`     | array       | Yes      | —             | Array of layers (max 20 per clip)        |
| `transition` | object/null | No       | —             | Transition to next clip. `null` disables |

---

## Layer Types

Layers render **bottom to top** within each clip.

### Video Layer

```json
{
  "type": "video",
  "path": "/video/user123/clip.mp4",
  "resizeMode": "contain-blur",
  "cutFrom": 0,
  "cutTo": 10,
  "mixVolume": 0.8
}
```

| Property        | Type          | Required | Default          | Description                                                               |
| --------------- | ------------- | -------- | ---------------- | ------------------------------------------------------------------------- |
| `path`          | string        | Yes      | —                | Video file path or public URL                                             |
| `resizeMode`    | string        | No       | `"contain-blur"` | `"contain"`, `"contain-blur"`, `"cover"`, `"stretch"`                     |
| `cutFrom`       | number        | No       | 0                | Start time in source video (seconds)                                      |
| `cutTo`         | number        | No       | —                | End time in source video (seconds)                                        |
| `start`         | number        | No       | —                | When layer appears within clip                                            |
| `stop`          | number        | No       | —                | When layer disappears within clip                                         |
| `width`         | number        | No       | —                | Relative width (0–1). Cannot use with `resizeMode`                        |
| `height`        | number        | No       | —                | Relative height (0–1). Cannot use with `resizeMode`                       |
| `position`      | string/object | No       | —                | Position in frame                                                         |
| `mixVolume`     | number        | No       | 1                | Audio volume (0–1)                                                        |
| `zoomDirection` | string/null   | No       | —                | Ken Burns: `"in"`, `"out"`, `"left"`, `"right"`, `"up"`, `"down"`, `null` |
| `zoomAmount`    | number        | No       | 0.1              | Zoom intensity (0–1)                                                      |

**Resize modes:**

- `"contain"` — letterbox (black bars)
- `"contain-blur"` — letterbox with blurred background (default)
- `"cover"` — fill frame, may crop
- `"stretch"` — stretch to fill, may distort

**Note:** If `clip.duration` is set and differs from video length, the video is sped up or slowed down to match.

### Image Layer

```json
{
  "type": "image",
  "path": "https://picsum.photos/1920/1080.jpg",
  "resizeMode": "cover",
  "zoomDirection": "in",
  "zoomAmount": 0.1
}
```

| Property        | Type          | Required | Default   | Description                                      |
| --------------- | ------------- | -------- | --------- | ------------------------------------------------ |
| `path`          | string        | Yes      | —         | Image file path or public URL                    |
| `resizeMode`    | string        | No       | `"cover"` | `"contain"`, `"cover"`, `"stretch"`              |
| `position`      | string/object | No       | —         | Position in frame                                |
| `width`         | number        | No       | —         | Relative width (0–1)                             |
| `height`        | number        | No       | —         | Relative height (0–1)                            |
| `start`         | number        | No       | —         | When layer appears                               |
| `stop`          | number        | No       | —         | When layer disappears                            |
| `zoomDirection` | string/null   | No       | `"in"`    | Ken Burns direction (images zoom in by default!) |
| `zoomAmount`    | number        | No       | 0.1       | Ken Burns intensity (0–1)                        |

Supports: PNG, JPG, JPEG, WebP, GIF (WebP/GIF auto-converted to PNG).

**Important:** Images have Ken Burns `zoomDirection: "in"` enabled by default. Set `zoomDirection: null` to disable.

### Image Overlay Layer

Same as image layer but designed for overlays (logos, watermarks).

```json
{
  "type": "image-overlay",
  "path": "/image/user123/logo.png",
  "position": "bottom-right",
  "width": 0.15,
  "height": 0.08
}
```

| Property        | Type          | Required | Default | Description             |
| --------------- | ------------- | -------- | ------- | ----------------------- |
| `path`          | string        | Yes      | —       | Image file path or URL  |
| `position`      | string/object | No       | —       | Position in frame       |
| `width`         | number        | No       | —       | Relative width (0–1)    |
| `height`        | number        | No       | —       | Relative height (0–1)   |
| `start`         | number        | No       | —       | When overlay appears    |
| `stop`          | number        | No       | —       | When overlay disappears |
| `zoomDirection` | string/null   | No       | —       | Ken Burns direction     |
| `zoomAmount`    | number        | No       | 0.1     | Ken Burns intensity     |

**Note:** WebP is NOT supported for image overlays. Use PNG, JPG, or GIF.

### Title Layer

```json
{
  "type": "title",
  "text": "Welcome to My Video",
  "style": "fade-in",
  "fontSize": 64,
  "textColor": "#ffffff",
  "googleFont": "Montserrat:700",
  "position": "center"
}
```

| Property            | Type          | Required | Default     | Description                                            |
| ------------------- | ------------- | -------- | ----------- | ------------------------------------------------------ |
| `text`              | string        | Yes      | —           | Title text (keep it short)                             |
| `style`             | string        | No       | `"fade-in"` | `"fade-in"`, `"word-by-word"`, `"letter-by-letter"`    |
| `animationDuration` | number        | No       | —           | Duration for word/letter animations                    |
| `textColor`         | string        | No       | `"#ffffff"` | Hex color                                              |
| `fontSize`          | integer       | No       | —           | Font size (8–500 px)                                   |
| `fontPath`          | string        | No       | —           | Custom font path                                       |
| `googleFont`        | string        | No       | —           | Google Font, e.g. `"Roboto:700"`                       |
| `position`          | string/object | No       | —           | Position in frame                                      |
| `start`             | number        | No       | —           | When title appears                                     |
| `stop`              | number        | No       | —           | When title disappears                                  |
| `zoomDirection`     | string/null   | No       | —           | Ken Burns (disabled for word-by-word/letter-by-letter) |
| `zoomAmount`        | number        | No       | 0.1         | Ken Burns intensity                                    |
| `outlineColor`      | string        | No       | `"#000000"` | Outline color                                          |
| `outlineWidth`      | number        | No       | —           | Outline width in pixels                                |
| `outlineStyle`      | string        | No       | `"outline"` | `"outline"`, `"shadow"`, `"glow"`                      |

### Subtitle Layer

Fixed slide-in effect from left with background. For more control, use `title` layer instead.

```json
{
  "type": "subtitle",
  "text": "This is a subtitle",
  "textColor": "#ffffff",
  "fontSize": 32,
  "position": "bottom"
}
```

| Property     | Type          | Required | Default     | Description                    |
| ------------ | ------------- | -------- | ----------- | ------------------------------ |
| `text`       | string        | Yes      | —           | Subtitle text                  |
| `textColor`  | string        | No       | `"#ffffff"` | Hex color                      |
| `fontSize`   | integer       | No       | —           | Font size (8–500 px)           |
| `fontPath`   | string        | No       | —           | Custom font path               |
| `googleFont` | string        | No       | —           | Google Font                    |
| `position`   | string/object | No       | —           | Position (commonly `"bottom"`) |
| `start`      | number        | No       | —           | When subtitle appears          |
| `stop`       | number        | No       | —           | When subtitle disappears       |

### News Title Layer

Breaking news–style banner.

```json
{
  "type": "news-title",
  "text": "BREAKING: Major Update Released",
  "textColor": "#ffffff",
  "backgroundColor": "#d02a42",
  "fontSize": 36,
  "position": "bottom"
}
```

| Property          | Type          | Required | Default     | Description             |
| ----------------- | ------------- | -------- | ----------- | ----------------------- |
| `text`            | string        | Yes      | —           | News title text         |
| `textColor`       | string        | No       | `"#ffffff"` | Text color              |
| `backgroundColor` | string        | No       | `"#d02a42"` | Banner background color |
| `fontSize`        | integer       | No       | —           | Font size               |
| `fontPath`        | string        | No       | —           | Custom font             |
| `googleFont`      | string        | No       | —           | Google Font             |
| `position`        | string/object | No       | —           | Position                |
| `start`           | number        | No       | —           | When it appears         |
| `stop`            | number        | No       | —           | When it disappears      |

### Title Background Layer

Title with a full-screen background.

```json
{
  "type": "title-background",
  "text": "Chapter 1",
  "textColor": "#ffffff",
  "fontSize": 72,
  "position": "center",
  "background": {
    "type": "linear-gradient",
    "colors": ["#667eea", "#764ba2"],
    "direction": "diagonal"
  }
}
```

| Property     | Type          | Required | Description                                |
| ------------ | ------------- | -------- | ------------------------------------------ |
| `text`       | string        | Yes      | Title text                                 |
| `textColor`  | string        | No       | Hex color                                  |
| `fontSize`   | integer       | No       | Font size                                  |
| `fontPath`   | string        | No       | Custom font                                |
| `googleFont` | string        | No       | Google Font                                |
| `position`   | string/object | No       | Position                                   |
| `background` | object        | No       | Full-screen background (random if omitted) |

**Background object:**
| Property | Type | Description |
|----------|------|-------------|
| `type` | string | `"fill-color"`, `"linear-gradient"`, `"radial-gradient"` |
| `color` | string | For `fill-color` type |
| `colors` | array | For gradient types |
| `direction` | string | For `linear-gradient`: `"horizontal"`, `"vertical"`, `"diagonal"` |

### Slide-in Text Layer

Animated text that slides into view.

```json
{
  "type": "slide-in-text",
  "text": "SLIDE IN",
  "fontSize": 0.1,
  "textColor": "#ffffff",
  "position": "center"
}
```

| Property      | Type          | Required | Description                                               |
| ------------- | ------------- | -------- | --------------------------------------------------------- |
| `text`        | string        | Yes      | Text content                                              |
| `fontSize`    | number        | No       | **Relative** font size (0–1, where 1 = full video height) |
| `textColor`   | string        | No       | Hex color                                                 |
| `charSpacing` | number        | No       | Character spacing                                         |
| `fontPath`    | string        | No       | Custom font                                               |
| `googleFont`  | string        | No       | Google Font                                               |
| `position`    | string/object | No       | Position                                                  |

### Background Layers

```json
{ "type": "fill-color", "color": "#1a1a2e" }
```

```json
{
  "type": "linear-gradient",
  "colors": ["#667eea", "#764ba2"],
  "direction": "horizontal"
}
```

```json
{ "type": "radial-gradient", "colors": ["#667eea", "#764ba2"] }
```

```json
{ "type": "rainbow-colors" }
```

All background layers support `start`/`stop` for timing within a clip. Gradient directions: `"horizontal"`, `"vertical"`, `"diagonal"`.

### Pause Layer

Black screen.

```json
{ "type": "pause" }
```

### HTML Layer

Render HTML/CSS content in video. See [html-layers.md](html-layers.md) for full details.

```json
{
  "type": "html",
  "html": "<div style='color: white; font-size: 48px;'>Dynamic Content</div>",
  "animated": true,
  "duration": 5,
  "width": 1920,
  "height": 1080
}
```

---

## Transitions

Transitions play between clips. Set on the **outgoing** clip.

```json
{
  "transition": {
    "name": "fade",
    "duration": 1
  }
}
```

Set `"transition": null` to disable transition for a specific clip.

### All Available Transitions (65)

**Basic:**
`fade`, `fadecolor`, `fadegrayscale`

**Wipe:**
`wipeLeft`, `wipeRight`, `wipeUp`, `wipeDown`, `directionalwipe`, `directionalwarp`, `Directional`

**Geometric:**
`circle`, `circleopen`, `CircleCrop`, `hexagonalize`, `kaleidoscope`, `pinwheel`, `GridFlip`, `randomsquares`, `Mosaic`, `BowTieHorizontal`, `BowTieVertical`

**3D:**
`cube`

**Zoom:**
`SimpleZoom`, `CrossZoom`, `DreamyZoom`, `ZoomInCircles`

**Motion:**
`rotate_scale_fade`, `Bounce`

**Digital/Glitch:**
`GlitchDisplace`, `GlitchMemories`, `crosshatch`, `windowblinds`, `windowslice`, `pixelize`

**Distortion:**
`displacement`, `crosswarp`, `Swirl`, `LinearBlur`

**Organic:**
`WaterDrop`, `ripple`, `luminance_melt`, `wind`, `undulatingBurnOut`, `burn`

**Creative:**
`Dreamy`, `ColourDistance`, `colorphase`, `multiply_blend`, `CrazyParametricFun`, `polar_function`, `perlin`, `angular`, `squeeze`, `swap`, `morph`

**Special:**
`ButterflyWaveScrawler`, `DoomScreenTransition`, `InvertedPageCurl`, `PolkaDotsCurtain`, `Radial`, `StereoViewer`, `doorway`, `flyeye`, `heart`, `luma`, `squareswire`

### Transition Duration Guidelines

- Quick cuts: 0.2–0.5s
- Standard: 0.8–1.5s
- Dramatic: 2–3s
- Text transitions: 0.5–1s

### Audio Curves in Transitions

```json
{
  "transition": {
    "name": "fade",
    "duration": 1,
    "audioOutCurve": "tri",
    "audioInCurve": "tri"
  }
}
```

Available curves: `tri` (default), `qsin`, `hsin`, `esin`, `log`, `ipar`, `qua`, `cub`, `squ`, `cbr`, `par`, `exp`, `iqsin`, `ihsin`, `dese`, `desi`, `losi`, `nofade`

---

## Audio

### Background Audio (config-level)

```json
{
  "audioFilePath": "/audio/user123/music.mp3",
  "loopAudio": true,
  "outputVolume": 0.8
}
```

### Audio Tracks (config-level, independent of clips)

```json
{
  "audioTracks": [
    {
      "path": "/audio/user123/music.mp3",
      "mixVolume": 0.3,
      "start": 0
    },
    {
      "path": "/audio/user123/voiceover.mp3",
      "mixVolume": 1,
      "start": 2,
      "cutFrom": 0,
      "cutTo": 30
    }
  ]
}
```

| Property    | Type   | Required | Default | Description                            |
| ----------- | ------ | -------- | ------- | -------------------------------------- |
| `path`      | string | Yes      | —       | Audio file path or URL                 |
| `mixVolume` | number | No       | 1       | Volume (0–1)                           |
| `start`     | number | No       | 0       | Start time in video timeline (seconds) |
| `cutFrom`   | number | No       | 0       | Start position in audio source         |
| `cutTo`     | number | No       | —       | End position in audio source           |

### Audio Layer (in clips)

Requires `keepSourceAudio: true` at config level.

```json
{
  "type": "audio",
  "path": "/audio/user123/sfx.mp3",
  "mixVolume": 0.5,
  "cutFrom": 0,
  "cutTo": 3
}
```

| Property    | Type   | Required | Default | Description                              |
| ----------- | ------ | -------- | ------- | ---------------------------------------- |
| `path`      | string | Yes      | —       | Audio file path or URL                   |
| `mixVolume` | number | No       | 1       | Volume (0–1)                             |
| `cutFrom`   | number | No       | 0       | Start position in audio source (seconds) |
| `cutTo`     | number | No       | —       | End position in audio source (seconds)   |
| `start`     | number | No       | —       | When audio starts within clip (seconds)  |
| `stop`      | number | No       | —       | When audio stops within clip (seconds)   |

### Detached Audio Layer

Audio with clip-relative timing.

```json
{
  "type": "detached-audio",
  "path": "/audio/user123/narration.mp3",
  "start": 1,
  "mixVolume": 1
}
```

| Property    | Type   | Required | Default | Description                                 |
| ----------- | ------ | -------- | ------- | ------------------------------------------- |
| `path`      | string | Yes      | —       | Audio file path or URL                      |
| `start`     | number | No       | 0       | Start time relative to clip start (seconds) |
| `mixVolume` | number | No       | 1       | Volume (0–1)                                |
| `cutFrom`   | number | No       | 0       | Start position in audio source (seconds)    |
| `cutTo`     | number | No       | —       | End position in audio source (seconds)      |
| `stop`      | number | No       | —       | When audio stops within clip (seconds)      |

### Audio Normalization (Ducking)

Automatically lowers background music when voiceover is detected.

```json
{
  "audioNorm": {
    "enable": true,
    "gaussSize": 5,
    "maxGain": 30
  }
}
```

| Property    | Type    | Default | Description                 |
| ----------- | ------- | ------- | --------------------------- |
| `enable`    | boolean | false   | Enable normalization        |
| `gaussSize` | integer | 5       | Gaussian filter size (1–10) |
| `maxGain`   | integer | 30      | Maximum gain in dB          |

Supported formats: MP3, WAV, M4A, AAC.

---

## Auto Subtitles

Automatically generate subtitles from audio (Pro/Enterprise plans).

```json
{
  "autoSubtitles": true,
  "autoSubtitlesStyle": {
    "animation": "word-by-word",
    "textColor": "#ffffff",
    "fontSize": 48,
    "googleFont": "Montserrat:700",
    "position": "bottom",
    "outlineColor": "#000000",
    "outlineWidth": 3,
    "outlineStyle": "outline"
  }
}
```

| Property       | Type          | Description                              |
| -------------- | ------------- | ---------------------------------------- |
| `animation`    | string        | `"word-by-word"` or `"letter-by-letter"` |
| `textColor`    | string        | Hex color                                |
| `fontSize`     | integer       | 8–500 px                                 |
| `fontPath`     | string        | Custom font                              |
| `googleFont`   | string        | Google Font                              |
| `position`     | string/object | Position                                 |
| `outlineColor` | string        | Outline color                            |
| `outlineWidth` | number        | Outline width                            |
| `outlineStyle` | string        | `"outline"`, `"shadow"`, `"glow"`        |

---

## Positioning (Video)

Same system as image generation, but **sizes are relative (0–1)** for most video layers.

### Position Presets

`"center"`, `"top"`, `"bottom"`, `"top-left"`, `"top-right"`, `"center-left"`, `"center-right"`, `"bottom-left"`, `"bottom-right"`

### Relative Position Object

```json
{
  "position": {
    "x": 0.75,
    "y": 0.25,
    "originX": "right",
    "originY": "top"
  }
}
```

### Ken Burns (Zoom/Pan) Effect

Add motion to static images and videos:

```json
{
  "type": "image",
  "path": "/image/user123/photo.jpg",
  "zoomDirection": "in",
  "zoomAmount": 0.15
}
```

| Direction | Effect        |
| --------- | ------------- |
| `"in"`    | Slow zoom in  |
| `"out"`   | Slow zoom out |
| `"left"`  | Pan left      |
| `"right"` | Pan right     |
| `"up"`    | Pan up        |
| `"down"`  | Pan down      |
| `null`    | No effect     |

`zoomAmount` controls intensity (0–1, default 0.1). Higher = more dramatic.

---

## Defaults System

```json
{
  "defaults": {
    "duration": 3,
    "transition": { "name": "fade", "duration": 1 },
    "layer": {
      "googleFont": "Montserrat:700"
    },
    "layerType": {
      "title": { "fontSize": 48, "textColor": "#ffffff" },
      "subtitle": { "fontSize": 24, "textColor": "#cccccc" }
    }
  },
  "clips": [...]
}
```

**Priority (highest wins):**

1. Individual layer/clip properties
2. `defaults.layerType.{type}` — type-specific defaults
3. `defaults.layer` — all-layer defaults
4. `defaults.duration` / `defaults.transition` — clip defaults

---

## Global Layers

Layers that span across the entire video timeline, rendered over all clips.

```json
{
  "clips": [ ... ],
  "globalLayers": [
    {
      "type": "title",
      "text": "Global Title across Clips",
      "position": "bottom",
      "style": "letter-by-letter",
      "animationDuration": 5,
      "start": 1,
      "stop": 6
    },
    {
      "type": "html",
      "html": "<img src='logo.png' style='width:120px;opacity:0.5;' />",
      "position": "bottom-right",
      "start": 0,
      "stop": 60
    }
  ]
}
```

**Key differences from clip layers:**

- `start`/`stop` are **seconds from video start** (global timeline), not clip-relative
- Animations continue seamlessly across clip boundaries
- Rendered on top of all clips where visible

**Supported layer types in `globalLayers`:**
`title`, `subtitle`, `image`, `image-overlay`, `fill-color`, `radial-gradient`, `linear-gradient`, `rainbow-colors`, `slide-in-text`, `news-title`, `title-background`, `html`

**Not allowed:**
`video`, `audio`, `detached-audio` — use `audioTracks` for full-video audio.

---

## Common Video Dimensions

| Use Case                  | Width | Height |
| ------------------------- | ----- | ------ |
| YouTube / Landscape       | 1920  | 1080   |
| TikTok / Reels / Vertical | 1080  | 1920   |
| Square / Instagram        | 1080  | 1080   |
| Twitter/X Video           | 1280  | 720    |
| LinkedIn Video            | 1920  | 1080   |
