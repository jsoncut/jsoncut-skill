# JsonCut Agent Skill

You are interacting with the **JsonCut API** — a media processing platform that creates images and videos programmatically via a REST API.

**Base URL:** `https://api.jsoncut.com`

## Quick Reference

| Action           | Method | Endpoint                          |
| ---------------- | ------ | --------------------------------- |
| Upload a file    | `POST` | `/api/v1/files/upload`            |
| Create a job     | `POST` | `/api/v1/jobs`                    |
| Check job status | `GET`  | `/api/v1/jobs/{jobId}`            |
| Download result  | `GET`  | `/api/v1/files/{fileId}/download` |
| Validate config  | `POST` | `/api/v1/jobs/validate`           |
| Get image schema | `GET`  | `/api/v1/schemas/image`           |
| Get video schema | `GET`  | `/api/v1/schemas/video`           |

## Authentication

Every request requires an API key in the `x-api-key` header:

```
x-api-key: jc_live_your_api_key_here
```

## Core Workflow

1. **Upload** source files (images, videos, audio, fonts) or use public URLs directly
2. **Create a job** with `type: "image"` or `type: "video"` and a JSON `config`
3. **Poll** `GET /api/v1/jobs/{jobId}` until `status` is `COMPLETED`
4. **Download** the result via `GET /api/v1/files/{outputFileId}/download`

## Documentation Files

Read these files for detailed instructions:

| File                                                 | Contents                                                      |
| ---------------------------------------------------- | ------------------------------------------------------------- |
| [docs/api.md](docs/api.md)                           | API endpoints, authentication, file management, job lifecycle |
| [docs/image-generation.md](docs/image-generation.md) | Image creation: layers, positioning, text, effects            |
| [docs/video-generation.md](docs/video-generation.md) | Video creation: clips, transitions, layer types, audio        |
| [docs/html-layers.md](docs/html-layers.md)           | HTML/CSS rendering with JS libraries (images & videos)        |
| [docs/examples.md](docs/examples.md)                 | Ready-to-use examples for common use cases                    |

## Key Rules

- **Job configs** are JSON objects passed in `config` field when creating a job
- **Image jobs** require `type: "image"` — config needs `width`, `height`, and `layers` array
- **Video jobs** require `type: "video"` — config needs `clips` array (each clip has `layers`)
- **File paths** can be internal paths (from upload response) starting with `/` or public URLs ending with a file extension
- **Layers render bottom to top** — first layer in array is the background
- Always **validate** your config with `/api/v1/jobs/validate` before creating a job
- Files are **temporary** (default 1 hour TTL, max 48 hours)
