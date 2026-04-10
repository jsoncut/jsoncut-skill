# JsonCut API Reference

## Base URL

```
https://api.jsoncut.com
```

## Authentication

Every request requires an API key in the `x-api-key` header:

```bash
curl -H "x-api-key: jc_live_your_api_key_here" https://api.jsoncut.com/api/v1/files
```

- Keys are prefixed with `jc_live_`
- Obtain keys from the dashboard at [app.jsoncut.com](https://app.jsoncut.com)
- Two permission levels: **Read-Only** (view/download) and **Full Access** (all operations)
- Missing/invalid key returns `401`, insufficient permissions returns `403`

---

## Complete Workflow

### Step 1: Upload Files (if needed)

Upload source files or use public URLs directly in your config.

```bash
curl -X POST https://api.jsoncut.com/api/v1/files/upload \
  -H "x-api-key: jc_live_..." \
  -F "file=@image.jpg" \
  -F "category=image"
```

**Parameters:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `file` | file | Yes | The file to upload (max 100MB) |
| `category` | string | Yes | `image`, `video`, `audio`, or `font` |
| `ttlHours` | number | No | Time-to-live in hours (default: 1, max: 48) |

**Supported file types:**
| Category | Extensions |
|----------|-----------|
| Image | `.png`, `.jpg`, `.jpeg`, `.gif`, `.webp` |
| Video | `.mp4`, `.webm`, `.mov` |
| Audio | `.mp3`, `.wav`, `.m4a`, `.aac` |
| Font | `.ttf`, `.otf`, `.woff`, `.woff2` |

**Response:**

```json
{
  "success": true,
  "data": {
    "id": "file-id-here",
    "storageUrl": "/image/2024-01-15/user123/image.jpg",
    "category": "image",
    "originalName": "image.jpg",
    "size": 245000,
    "mimeType": "image/jpeg"
  }
}
```

Use the `storageUrl` value as the `path` in your job config.

### Step 2: Create a Job

```bash
curl -X POST https://api.jsoncut.com/api/v1/jobs \
  -H "x-api-key: jc_live_..." \
  -H "Content-Type: application/json" \
  -d '{
    "type": "image",
    "config": {
      "width": 800,
      "height": 600,
      "layers": [
        { "type": "rectangle", "width": 800, "height": 600, "fill": "#3b82f6" },
        { "type": "text", "text": "Hello World", "fontSize": 48, "color": "#ffffff", "position": "center" }
      ]
    }
  }'
```

**Request body:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | `"image"` or `"video"` |
| `config` | object | Yes | The generation configuration (see image/video docs) |

**Response:**

```json
{
  "success": true,
  "data": {
    "id": "job-id-here",
    "type": "image",
    "status": "PENDING",
    "createdAt": "2024-01-15T12:00:00Z"
  }
}
```

### Step 3: Poll Job Status

```bash
curl https://api.jsoncut.com/api/v1/jobs/{jobId} \
  -H "x-api-key: jc_live_..."
```

**Job statuses:**
| Status | Description |
|--------|-------------|
| `PENDING` | Job is queued |
| `PROCESSING` | Job is being processed |
| `COMPLETED` | Job finished — download the result |
| `FAILED` | Job failed — check `error` field |

**Completed response includes:**

```json
{
  "success": true,
  "data": {
    "id": "job-id-here",
    "status": "COMPLETED",
    "outputFileId": "output-file-id",
    "inputFileIds": ["input-file-id-1"]
  }
}
```

### Step 4: Download Result

```bash
curl https://api.jsoncut.com/api/v1/files/{outputFileId}/download \
  -H "x-api-key: jc_live_..." \
  -o result.png
```

---

## File Paths in Configs

There are two ways to reference files in your job `config`:

### 1. Internal paths (from upload)

Use the `storageUrl` from the upload response:

```json
{ "path": "/image/2024-01-15/user123/photo.jpg" }
```

### 2. Public URLs

Use any publicly accessible URL with a file extension:

```json
{ "path": "https://images.unsplash.com/photo-123456.jpg" }
```

Public URLs are automatically downloaded and validated. URLs **must** end with a recognized file extension (`.jpg`, `.png`, `.mp4`, etc.).

---

## Validate Before Creating

Always validate your config first to catch errors without consuming tokens:

```bash
curl -X POST https://api.jsoncut.com/api/v1/jobs/validate \
  -H "x-api-key: jc_live_..." \
  -H "Content-Type: application/json" \
  -d '{
    "type": "image",
    "config": { ... }
  }'
```

**Response (always HTTP 200):**

```json
{
  "success": true,
  "data": {
    "isValid": true,
    "errors": []
  }
}
```

Check `isValid` — if `false`, the `errors` array describes what's wrong.

---

## Fetch Schemas

Get the full JSON schema for validation or reference:

```bash
# Image schema
curl https://api.jsoncut.com/api/v1/schemas/image

# Video schema
curl https://api.jsoncut.com/api/v1/schemas/video

# List all schemas
curl https://api.jsoncut.com/api/v1/schemas
```

---

## Additional Endpoints

### List User Files

```bash
GET /api/v1/files?page=1&limit=10
```

### Get File Metadata

```bash
GET /api/v1/files/{fileId}
```

### Delete a File

```bash
DELETE /api/v1/files/{fileId}
```

### List User Jobs

```bash
GET /api/v1/jobs?page=1&limit=10&status=COMPLETED
```

### Cancel a Pending Job

```bash
DELETE /api/v1/jobs/{jobId}
```

### Generate Public Download Link

```bash
POST /api/v1/files/{fileId}/public-link
```

Returns a time-limited public URL that requires no authentication.

### Get Video Duration

```bash
POST /api/v1/tools/video/duration
Content-Type: application/json
{ "path": "/video/user123/clip.mp4" }
```

### Get Audio Duration

```bash
POST /api/v1/tools/audio/duration
Content-Type: application/json
{ "path": "/audio/user123/track.mp3" }
```

### Bulk Duration (Video/Audio)

```bash
POST /api/v1/tools/video/duration/bulk
POST /api/v1/tools/audio/duration/bulk
Content-Type: application/json
{ "paths": ["/video/user123/clip1.mp4", "/video/user123/clip2.mp4"] }
```

### Validate Files

```bash
POST /api/v1/files/validate/image   # Validate image format
POST /api/v1/files/validate/video   # Validate video format
POST /api/v1/files/validate/audio   # Validate audio format
```

### Get Supported File Types

```bash
GET /api/v1/files/supported-types
```

---

## Rate Limits

| Type            | Limit | Window     |
| --------------- | ----- | ---------- |
| API Requests    | 1,000 | 15 minutes |
| File Uploads    | 10    | 1 minute   |
| Job Creation    | 10    | 1 minute   |
| Concurrent Jobs | 3     | —          |

Rate limit headers are included in every response:

- `X-RateLimit-Limit` — max requests
- `X-RateLimit-Remaining` — remaining requests
- `X-RateLimit-Reset` — reset timestamp

Exceeding limits returns HTTP `429`.

---

## Token Costs

| Operation   | Cost                                  |
| ----------- | ------------------------------------- |
| File upload | 1 token                               |
| Image job   | 2 + (1 × number of layers)            |
| Video job   | 10 base + (5 × output MB), minimum 20 |

Tokens are only charged on **successful completion** — failed jobs don't consume tokens.

---

## Processing Times

| Type                       | Typical Duration              |
| -------------------------- | ----------------------------- |
| Simple image (1-3 layers)  | 2–10 seconds                  |
| Complex image (10+ layers) | 10–30 seconds                 |
| Short video (<30s)         | 30–90 seconds                 |
| Longer video (1-5 min)     | 2–5 minutes per output minute |

---

## Error Handling

All error responses follow this format:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error description"
  }
}
```

| HTTP Status | Meaning                                      |
| ----------- | -------------------------------------------- |
| `400`       | Invalid request (bad config, missing fields) |
| `401`       | Invalid or missing API key                   |
| `403`       | Insufficient permissions (read-only key)     |
| `404`       | Resource not found                           |
| `429`       | Rate limit exceeded                          |
| `500`       | Server error                                 |
