<p align="center">
  <img src="docs/logo.png" alt="JsonCut" width="120" />
</p>

<h1 align="center">JsonCut Skill</h1>

<p align="center">
  AI agent skill for seamless integration with <a href="https://jsoncut.com">JsonCut</a> — automated creation of videos and images via API.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/API-REST-blue?style=flat-square" alt="REST API" />
  <img src="https://img.shields.io/badge/Output-Images%20%26%20Videos-orange?style=flat-square" alt="Images & Videos" />
  <img src="https://img.shields.io/badge/Format-JSON-green?style=flat-square" alt="JSON Config" />
  <img src="https://img.shields.io/badge/HTML%2FCSS-Supported-purple?style=flat-square" alt="HTML/CSS" />
  <img src="https://img.shields.io/badge/Transitions-65+-red?style=flat-square" alt="65+ Transitions" />
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="MIT License" />
</p>

---

## What is this?

This repository contains structured documentation files that teach AI agents (Claude Code, Codex, OpenClaw, Cursor, Windsurf, etc.) how to interact with the **JsonCut API** to generate images and videos programmatically.

## How to use

### Claude Code

Place this repo in your project or reference it. Claude Code automatically reads `AGENTS.md` as the entry point.

### Cursor / Windsurf

Add the contents of `AGENTS.md` to your custom instructions or rules, or include this repo in your workspace.

### Codex / OpenClaw

Reference the `AGENTS.md` file in your system prompt or agent configuration.

### Any AI Agent

Point the agent to `AGENTS.md` — it contains the complete overview with links to detailed docs.

## What's Covered

| Topic            | File                                                   | Description                                       |
| ---------------- | ------------------------------------------------------ | ------------------------------------------------- |
| API Reference    | [`docs/api.md`](docs/api.md)                           | Endpoints, auth, file management, job lifecycle   |
| Image Generation | [`docs/image-generation.md`](docs/image-generation.md) | 6 layer types, positioning, effects, defaults     |
| Video Generation | [`docs/video-generation.md`](docs/video-generation.md) | 16+ layer types, 65 transitions, audio, subtitles |
| HTML Layers      | [`docs/html-layers.md`](docs/html-layers.md)           | HTML/CSS rendering, JS libraries, animations      |
| Examples         | [`docs/examples.md`](docs/examples.md)                 | Ready-to-use image & video examples               |

## Repository Structure

```
jsoncut-skill/
├── AGENTS.md                    # Entry point for AI agents
├── CLAUDE.md                    # Claude Code convention file
├── README.md                    # This file
└── docs/
    ├── api.md                   # API endpoints, auth, files, jobs
    ├── image-generation.md      # Image config, layers, positioning
    ├── video-generation.md      # Video config, clips, transitions, audio
    ├── html-layers.md           # HTML/CSS rendering (shared)
    └── examples.md              # Ready-to-use examples
```

## Links

|                   |                                              |
| ----------------- | -------------------------------------------- |
| **Homepage**      | [jsoncut.com](https://jsoncut.com)           |
| **API**           | [api.jsoncut.com](https://api.jsoncut.com)   |
| **Dashboard**     | [app.jsoncut.com](https://app.jsoncut.com)   |
| **Documentation** | [docs.jsoncut.com](https://docs.jsoncut.com) |
