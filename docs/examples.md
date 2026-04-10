# Examples

Complete, ready-to-use examples for the JsonCut API.

---

## Image Examples

### Social Media Post (Instagram Square)

```json
{
  "type": "image",
  "config": {
    "width": 1080,
    "height": 1080,
    "format": "png",
    "layers": [
      {
        "type": "gradient",
        "gradient": {
          "type": "linear",
          "colors": ["#667eea", "#764ba2"],
          "direction": "diagonal"
        }
      },
      {
        "type": "rectangle",
        "x": 60,
        "y": 60,
        "width": 960,
        "height": 960,
        "fill": "#ffffff",
        "borderRadius": 24,
        "opacity": 0.95
      },
      {
        "type": "image",
        "path": "https://picsum.photos/800/400.jpg",
        "x": 100,
        "y": 100,
        "width": 880,
        "height": 440,
        "fit": "cover",
        "borderRadius": 16
      },
      {
        "type": "text",
        "text": "Exciting News!",
        "x": 100,
        "y": 590,
        "fontSize": 42,
        "color": "#1e293b",
        "googleFont": "Montserrat:700"
      },
      {
        "type": "text",
        "text": "We're thrilled to announce our latest product launch. Stay tuned for more details.",
        "x": 100,
        "y": 660,
        "width": 880,
        "fontSize": 22,
        "color": "#64748b",
        "wrap": true,
        "lineHeight": 1.5,
        "googleFont": "Open Sans:400"
      },
      {
        "type": "rectangle",
        "x": 100,
        "y": 880,
        "width": 250,
        "height": 56,
        "fill": "#667eea",
        "borderRadius": 28
      },
      {
        "type": "text",
        "text": "Learn More",
        "x": 160,
        "y": 895,
        "fontSize": 20,
        "color": "#ffffff",
        "googleFont": "Montserrat:600"
      }
    ]
  }
}
```

### Image with Watermark

```json
{
  "type": "image",
  "config": {
    "width": 1200,
    "height": 800,
    "layers": [
      {
        "type": "image",
        "path": "https://picsum.photos/1200/800.jpg",
        "width": 1200,
        "height": 800,
        "fit": "cover"
      },
      {
        "type": "image",
        "path": "/image/user123/logo.png",
        "position": "bottom-right",
        "width": 150,
        "height": 50,
        "opacity": 0.5
      }
    ]
  }
}
```

### Quote Design

```json
{
  "type": "image",
  "config": {
    "width": 1200,
    "height": 700,
    "layers": [
      {
        "type": "image",
        "path": "https://picsum.photos/1200/700.jpg",
        "width": 1200,
        "height": 700,
        "fit": "cover"
      },
      {
        "type": "gradient",
        "gradient": {
          "type": "linear",
          "colors": ["rgba(0,0,0,0.7)", "rgba(0,0,0,0.3)"],
          "direction": "vertical"
        }
      },
      {
        "type": "rectangle",
        "position": "center",
        "width": 900,
        "height": 400,
        "fill": "#ffffff",
        "opacity": 0.1,
        "borderRadius": 20
      },
      {
        "type": "text",
        "text": "\"The only way to do great work is to love what you do.\"",
        "position": "center",
        "width": 800,
        "fontSize": 36,
        "color": "#ffffff",
        "align": "center",
        "wrap": true,
        "lineHeight": 1.6,
        "googleFont": "Playfair Display:700"
      },
      {
        "type": "text",
        "text": "— Steve Jobs",
        "position": {
          "x": 0.5,
          "y": 0.72,
          "originX": "center",
          "originY": "center"
        },
        "fontSize": 20,
        "color": "#e2e8f0",
        "googleFont": "Open Sans:400"
      }
    ]
  }
}
```

### Sale Banner

```json
{
  "type": "image",
  "config": {
    "width": 1200,
    "height": 400,
    "layers": [
      {
        "type": "gradient",
        "gradient": {
          "type": "linear",
          "colors": ["#ef4444", "#dc2626", "#b91c1c"],
          "direction": "horizontal"
        }
      },
      {
        "type": "text",
        "text": "MEGA SALE",
        "position": {
          "x": 0.35,
          "y": 0.5,
          "originX": "center",
          "originY": "center"
        },
        "fontSize": 72,
        "color": "#ffffff",
        "googleFont": "Montserrat:900"
      },
      {
        "type": "circle",
        "position": {
          "x": 0.8,
          "y": 0.5,
          "originX": "center",
          "originY": "center"
        },
        "width": 180,
        "height": 180,
        "fill": "#ffffff"
      },
      {
        "type": "text",
        "text": "50%\nOFF",
        "position": {
          "x": 0.8,
          "y": 0.48,
          "originX": "center",
          "originY": "center"
        },
        "fontSize": 36,
        "color": "#ef4444",
        "align": "center",
        "googleFont": "Montserrat:800"
      }
    ]
  }
}
```

### HTML Layer — Data Dashboard

```json
{
  "type": "image",
  "config": {
    "width": 1200,
    "height": 800,
    "layers": [
      {
        "type": "html",
        "html": "<div class='dashboard'><h1>Monthly Revenue</h1><div class='stats'><div class='stat'><span class='value'>$48.2K</span><span class='label'>Revenue</span></div><div class='stat'><span class='value'>1,234</span><span class='label'>Orders</span></div><div class='stat'><span class='value'>+23%</span><span class='label'>Growth</span></div></div></div>",
        "css": ".dashboard { padding: 60px; background: linear-gradient(135deg, #0f172a, #1e293b); height: 100%; box-sizing: border-box; font-family: 'Inter', sans-serif; color: white; } h1 { font-size: 36px; margin-bottom: 40px; } .stats { display: flex; gap: 40px; } .stat { background: rgba(255,255,255,0.1); padding: 30px 40px; border-radius: 16px; } .value { display: block; font-size: 42px; font-weight: 700; } .label { font-size: 16px; opacity: 0.6; margin-top: 8px; display: block; }",
        "googleFonts": ["Inter"],
        "width": 1200,
        "height": 800
      }
    ]
  }
}
```

---

## Video Examples

### Social Media Video (TikTok/Reels)

```json
{
  "type": "video",
  "config": {
    "width": 1080,
    "height": 1920,
    "fps": 30,
    "audioFilePath": "https://example.com/background-music.mp3",
    "loopAudio": true,
    "outputVolume": 0.8,
    "defaults": {
      "duration": 3,
      "transition": { "name": "fade", "duration": 0.8 },
      "layer": { "googleFont": "Montserrat:700" }
    },
    "clips": [
      {
        "layers": [
          {
            "type": "fill-color",
            "color": "#0f172a"
          },
          {
            "type": "title",
            "text": "5 Tips for Productivity",
            "fontSize": 64,
            "textColor": "#ffffff",
            "position": "center"
          }
        ],
        "transition": { "name": "ZoomInCircles", "duration": 1 }
      },
      {
        "layers": [
          {
            "type": "image",
            "path": "https://picsum.photos/1080/1920.jpg",
            "resizeMode": "cover",
            "zoomDirection": "in",
            "zoomAmount": 0.1
          },
          {
            "type": "title",
            "text": "1. Start Your Day Early",
            "fontSize": 48,
            "textColor": "#ffffff",
            "position": "bottom",
            "outlineColor": "#000000",
            "outlineWidth": 3
          }
        ],
        "transition": { "name": "GlitchDisplace", "duration": 0.5 }
      },
      {
        "layers": [
          {
            "type": "image",
            "path": "https://picsum.photos/1080/1921.jpg",
            "resizeMode": "cover",
            "zoomDirection": "out",
            "zoomAmount": 0.1
          },
          {
            "type": "title",
            "text": "2. Plan Your Tasks",
            "fontSize": 48,
            "textColor": "#ffffff",
            "position": "bottom",
            "outlineColor": "#000000",
            "outlineWidth": 3
          }
        ]
      }
    ]
  }
}
```

### Product Demo (Landscape)

```json
{
  "type": "video",
  "config": {
    "width": 1920,
    "height": 1080,
    "fps": 30,
    "audioTracks": [
      {
        "path": "/audio/user123/bg-music.mp3",
        "mixVolume": 0.2,
        "start": 0
      },
      {
        "path": "/audio/user123/voiceover.mp3",
        "mixVolume": 1,
        "start": 1
      }
    ],
    "audioNorm": {
      "enable": true,
      "gaussSize": 5,
      "maxGain": 30
    },
    "defaults": {
      "transition": { "name": "Dreamy", "duration": 1 }
    },
    "clips": [
      {
        "duration": 4,
        "layers": [
          {
            "type": "title-background",
            "text": "Introducing Product X",
            "fontSize": 72,
            "textColor": "#ffffff",
            "googleFont": "Montserrat:700",
            "background": {
              "type": "linear-gradient",
              "colors": ["#667eea", "#764ba2"],
              "direction": "diagonal"
            }
          }
        ]
      },
      {
        "duration": 6,
        "layers": [
          {
            "type": "video",
            "path": "/video/user123/product-demo.mp4",
            "resizeMode": "contain-blur",
            "cutFrom": 0,
            "cutTo": 6
          },
          {
            "type": "subtitle",
            "text": "Easy to use, powerful results",
            "fontSize": 28,
            "textColor": "#ffffff",
            "position": "bottom",
            "start": 1,
            "stop": 5
          }
        ],
        "transition": { "name": "CircleCrop", "duration": 1.2 }
      },
      {
        "duration": 4,
        "layers": [
          {
            "type": "fill-color",
            "color": "#0f172a"
          },
          {
            "type": "title",
            "text": "Try it Today!",
            "fontSize": 64,
            "textColor": "#ffffff",
            "googleFont": "Montserrat:700",
            "position": "center",
            "style": "word-by-word"
          }
        ]
      }
    ]
  }
}
```

### Breaking News

```json
{
  "type": "video",
  "config": {
    "width": 1920,
    "height": 1080,
    "fps": 30,
    "keepSourceAudio": true,
    "defaults": {
      "transition": { "name": "wipeUp", "duration": 0.5 }
    },
    "clips": [
      {
        "duration": 3,
        "layers": [
          {
            "type": "fill-color",
            "color": "#b91c1c"
          },
          {
            "type": "title",
            "text": "BREAKING NEWS",
            "fontSize": 96,
            "textColor": "#ffffff",
            "position": "center",
            "googleFont": "Montserrat:900",
            "style": "letter-by-letter",
            "animationDuration": 1.5
          },
          {
            "type": "audio",
            "path": "/audio/user123/breaking-sfx.mp3",
            "mixVolume": 0.8
          }
        ]
      },
      {
        "duration": 8,
        "layers": [
          {
            "type": "video",
            "path": "/video/user123/news-footage.mp4",
            "resizeMode": "cover",
            "cutFrom": 0,
            "cutTo": 8
          },
          {
            "type": "news-title",
            "text": "Major Technology Breakthrough Announced",
            "fontSize": 32,
            "textColor": "#ffffff",
            "backgroundColor": "#d02a42",
            "position": "bottom",
            "start": 1
          }
        ]
      }
    ]
  }
}
```

### Video with HTML Overlay (Animated Lower Third)

```json
{
  "type": "video",
  "config": {
    "width": 1920,
    "height": 1080,
    "fps": 30,
    "clips": [
      {
        "duration": 8,
        "layers": [
          {
            "type": "video",
            "path": "/video/user123/interview.mp4",
            "resizeMode": "cover"
          },
          {
            "type": "html",
            "html": "<div class='lower-third'><div class='name'>Jane Smith</div><div class='title'>CEO, Tech Corp</div></div>",
            "css": ".lower-third { position: absolute; bottom: 80px; left: 60px; animation: slideIn 0.5s ease-out forwards; } .name { background: #3b82f6; color: white; padding: 12px 24px; font-size: 28px; font-weight: 700; font-family: 'Montserrat', sans-serif; } .title { background: #1e293b; color: #e2e8f0; padding: 8px 24px; font-size: 18px; font-family: 'Montserrat', sans-serif; } @keyframes slideIn { from { transform: translateX(-100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }",
            "googleFonts": ["Montserrat"],
            "animated": true,
            "duration": 8,
            "width": 1920,
            "height": 1080,
            "start": 1,
            "stop": 7
          }
        ]
      }
    ]
  }
}
```

### Slideshow with Ken Burns

```json
{
  "type": "video",
  "config": {
    "width": 1920,
    "height": 1080,
    "fps": 30,
    "audioFilePath": "https://example.com/gentle-music.mp3",
    "loopAudio": true,
    "defaults": {
      "duration": 4,
      "transition": { "name": "fade", "duration": 1.5 }
    },
    "clips": [
      {
        "layers": [
          {
            "type": "image",
            "path": "https://picsum.photos/1920/1080.jpg",
            "resizeMode": "cover",
            "zoomDirection": "in",
            "zoomAmount": 0.15
          }
        ]
      },
      {
        "layers": [
          {
            "type": "image",
            "path": "https://picsum.photos/1921/1080.jpg",
            "resizeMode": "cover",
            "zoomDirection": "right",
            "zoomAmount": 0.1
          }
        ]
      },
      {
        "layers": [
          {
            "type": "image",
            "path": "https://picsum.photos/1922/1080.jpg",
            "resizeMode": "cover",
            "zoomDirection": "out",
            "zoomAmount": 0.12
          }
        ],
        "transition": null
      }
    ]
  }
}
```

---

## Workflow Example (cURL)

Complete end-to-end workflow for generating an image:

```bash
# 1. Create the job
JOB_RESPONSE=$(curl -s -X POST https://api.jsoncut.com/api/v1/jobs \
  -H "x-api-key: jc_live_your_key" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "image",
    "config": {
      "width": 1080,
      "height": 1080,
      "backgroundColor": "#1e293b",
      "layers": [
        {
          "type": "text",
          "text": "Hello World",
          "fontSize": 64,
          "color": "#ffffff",
          "position": "center",
          "googleFont": "Montserrat:700"
        }
      ]
    }
  }')

# 2. Extract job ID
JOB_ID=$(echo $JOB_RESPONSE | jq -r '.data.id')

# 3. Poll until complete
while true; do
  STATUS_RESPONSE=$(curl -s https://api.jsoncut.com/api/v1/jobs/$JOB_ID \
    -H "x-api-key: jc_live_your_key")
  STATUS=$(echo $STATUS_RESPONSE | jq -r '.data.status')

  if [ "$STATUS" = "COMPLETED" ]; then
    OUTPUT_FILE_ID=$(echo $STATUS_RESPONSE | jq -r '.data.outputFileId')
    break
  elif [ "$STATUS" = "FAILED" ]; then
    echo "Job failed"
    exit 1
  fi

  sleep 2
done

# 4. Download result
curl -s https://api.jsoncut.com/api/v1/files/$OUTPUT_FILE_ID/download \
  -H "x-api-key: jc_live_your_key" \
  -o output.png

echo "Image saved to output.png"
```

---

## Workflow Example (Python)

```python
import requests
import time

API_KEY = "jc_live_your_key"
BASE_URL = "https://api.jsoncut.com/api/v1"
HEADERS = {"x-api-key": API_KEY, "Content-Type": "application/json"}

# 1. Create job
job = requests.post(f"{BASE_URL}/jobs", headers=HEADERS, json={
    "type": "image",
    "config": {
        "width": 1080,
        "height": 1080,
        "backgroundColor": "#1e293b",
        "layers": [
            {
                "type": "text",
                "text": "Hello World",
                "fontSize": 64,
                "color": "#ffffff",
                "position": "center",
                "googleFont": "Montserrat:700"
            }
        ]
    }
}).json()

job_id = job["data"]["id"]

# 2. Poll until complete
while True:
    status = requests.get(f"{BASE_URL}/jobs/{job_id}", headers=HEADERS).json()
    if status["data"]["status"] == "COMPLETED":
        output_file_id = status["data"]["outputFileId"]
        break
    elif status["data"]["status"] == "FAILED":
        raise Exception("Job failed")
    time.sleep(2)

# 3. Download result
result = requests.get(f"{BASE_URL}/files/{output_file_id}/download", headers=HEADERS)
with open("output.png", "wb") as f:
    f.write(result.content)
```

---

## Workflow Example (JavaScript/Node.js)

```javascript
const API_KEY = 'jc_live_your_key';
const BASE_URL = 'https://api.jsoncut.com/api/v1';

async function generateImage() {
  // 1. Create job
  const jobResponse = await fetch(`${BASE_URL}/jobs`, {
    method: 'POST',
    headers: { 'x-api-key': API_KEY, 'Content-Type': 'application/json' },
    body: JSON.stringify({
      type: 'image',
      config: {
        width: 1080,
        height: 1080,
        backgroundColor: '#1e293b',
        layers: [
          {
            type: 'text',
            text: 'Hello World',
            fontSize: 64,
            color: '#ffffff',
            position: 'center',
            googleFont: 'Montserrat:700',
          },
        ],
      },
    }),
  });
  const job = await jobResponse.json();
  const jobId = job.data.id;

  // 2. Poll until complete
  let outputFileId;
  while (true) {
    const statusResponse = await fetch(`${BASE_URL}/jobs/${jobId}`, {
      headers: { 'x-api-key': API_KEY },
    });
    const status = await statusResponse.json();

    if (status.data.status === 'COMPLETED') {
      outputFileId = status.data.outputFileId;
      break;
    } else if (status.data.status === 'FAILED') {
      throw new Error('Job failed');
    }

    await new Promise((r) => setTimeout(r, 2000));
  }

  // 3. Download result
  const result = await fetch(`${BASE_URL}/files/${outputFileId}/download`, {
    headers: { 'x-api-key': API_KEY },
  });
  const buffer = await result.arrayBuffer();
  require('fs').writeFileSync('output.png', Buffer.from(buffer));
}

generateImage();
```
