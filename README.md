<div align="center">

# 🧠 Second Brain

**A local-first, file-system-backed personal management system.**

Manage tasks, assets, knowledge, meetings, and AI-tool usage from one self-hosted application.

[Features](#features) · [Quick Start](#quick-start) · [Meetings and AI](#meetings-and-ai) · [Data Storage](#data-storage) · [Development](#development)

</div>

---

## What is Second Brain?

Second Brain is a full-stack personal management application with a Chinese-language interface. It brings task management, yield tracking, portfolio holdings, ledger bookkeeping, receivables, subscriptions, knowledge management, meeting recording, and AI usage reviews into one self-hosted workspace.

The application has no business database. Core structured records live under `memory/` and use YAML, Markdown with YAML frontmatter, CSV, and JSON. You can inspect, edit, search, diff, and version these files directly instead of being locked into an opaque data store.

Second Brain is also designed for AI-assisted workflows. Tools such as Claude Code, Codex, and Kiro can read the repository's data and skill definitions to create tasks, analyze finances, process meetings, and extend the application.

> Second Brain is local-first, not necessarily offline. Weather, market prices, batch speech recognition, Claude, and optional MCP connectors may access local or remote services depending on your configuration.

## Features

| Area | Current capabilities |
| --- | --- |
| **Home** | Time-based greeting, weather, daily quote, task statistics, reminders, and entry points to the main modules |
| **Tasks** | Multiple workspaces; calendar, kanban, list, and AI review views; search and stage/date/tag/archive filters; task creation, editing, drag-and-drop, archiving, and deletion |
| **Yield tracking** | Segment-aware returns based on principal, APR, dates, and currency; current, month-end, and maturity projections; active and archived positions |
| **Portfolio holdings** | Crypto, US stocks, HK stocks, CN stocks, and cash; price history, asset allocation, portfolio snapshots, and CoinGecko prices |
| **Ledger** | Income and expenses, nested categories, yearly CSV files, daily/monthly/quarterly/yearly budgets, multi-currency reporting, and Feidee import tooling |
| **Receivables** | Personal and customer receivables, partial collections, aging, write-offs, and inclusion in asset totals |
| **Subscriptions** | Monthly, yearly, quarterly, semi-annual, weekly, and custom billing cycles; trials, pauses, cancellations, renewal dates, and reminders |
| **Knowledge** | Quick notes and Markdown notes, HTML file/folder import, full-text search, tags, bidirectional links, attachments, Mermaid, article/slide presentation modes, and presenter outlines |
| **Meetings** | Multiple workspaces, dual-track system/microphone recording, pause/mute, live captions, recording-time AI digests, batch transcription, speaker diarization and correction, meeting minutes, and knowledge export |
| **AI review** | Collect local Claude Code, Codex, Kiro, and AWS Quick logs for a date range, display cross-tool metrics, and generate review documents per task workspace; optionally read chat/mail MCP connectors |
| **Global UI** | Light/dark themes, a collapsible resizable sidebar, responsive layouts, and a `Cmd+K` / `Ctrl+K` command palette with AI chat |

### Command palette

Press `Cmd+K` on macOS or `Ctrl+K` on other platforms to:

- Search and navigate to pages or workspaces
- Quickly create tasks, ledger records, subscriptions, receivables, yield records, and knowledge notes
- Record a manual holding price
- Toggle the theme and sidebar
- Enter AI chat when no command matches

## Data ownership

Core business data is persisted through the file system, with no ORM or business database:

- **Markdown + YAML frontmatter**: tasks, yield records, holdings, receivables, subscriptions, knowledge notes, and review documents
- **YAML**: workspace, kanban, budget, and other metadata
- **CSV**: ledger records
- **JSON**: meeting captions, transcript segments, and other structured artifacts
- **HTML and attachments**: knowledge documents, images, and other assets

There are important boundaries:

- Raw meeting chunks and merged WebM/WAV audio live under `memory/meetings/` but are ignored by Git.
- `.local/` stores machine-specific state such as ASR configuration, encryption keys, downloaded models, connectors, and the meeting trash; it is also ignored by Git.
- A normal Git sync therefore backs up tracked structured records, but **not** raw recordings, downloaded models, or `.local/` configuration. Back those up separately if needed.

## Quick Start

### Prerequisites

The base web application requires:

- [Git](https://git-scm.com/)
- [Bun](https://bun.sh/)

Meeting transcription additionally requires `ffmpeg` and `ffprobe`. On macOS:

```bash
brew install ffmpeg
```

### Install

```bash
git clone https://github.com/domechn/second-brain.git
cd second-brain

# Repository-level tooling dependencies
bun install

# Web application dependencies
cd system
bun install
```

### Development mode

From `system/`:

```bash
bun run dev --host
```

Then open <http://localhost:5173>.

### Production build and server

From `system/`:

```bash
bun run build
bun run start
```

On macOS, you can also use the convenience script from the repository root:

```bash
./start.sh
```

`start.sh` runs `git pull` first, installs missing `system/` dependencies, rebuilds when needed, starts the production server on `PORT` (default `5173`), and opens the browser. Use the manual commands above if startup should not update the repository.

## Desktop app (optional)

`desktop/` is a macOS Electron shell whose main purpose is capturing system audio for meeting recording through CoreAudio loopback. A normal browser can capture audio from a selected tab, but it cannot provide the same system-wide audio capture experience.

The desktop shell still requires the local repository, its `memory/` directory, and Bun. It is not a standalone data application. On first launch it locates the repository, checking `~/github/second-brain` by default and otherwise presenting a folder picker.

```bash
cd desktop
bun install

# Run the Electron shell in development
bun run start

# Package Second Brain.app
bun run package
```

System-audio loopback is intended for macOS 14.2 or newer and requires the relevant microphone/audio permissions. The shell reuses an existing web server on its configured port or installs dependencies, builds, and starts one when necessary. It deliberately does not manage a speech-recognition model.

See [`desktop/README.md`](desktop/README.md) for installation, signing, permissions, and environment-variable details.

## Meetings and AI

These capabilities are configured on demand and are not required for the core task, asset, or knowledge features.

### Batch speech recognition

Final meeting transcripts use a user-configured OpenAI-compatible ASR service. The default configuration points to a local Ollama instance:

- Base URL: `http://127.0.0.1:11434/v1`
- Model: `gemma-4`

The service must support audio input. Its URL, model, and API key can be changed from the ASR settings in the meetings UI. The Electron shell does not start or manage this service.

### Live captions and speaker diarization

- Live captions use a local sherpa-onnx streaming Chinese/English model.
- Optional speaker diarization uses pyannote segmentation and ERes2Net speaker embeddings.
- Both can be installed on demand from the meetings overview. Models are stored under the Git-ignored `.local/` directory.
- Recording continues if either feature is unavailable, and final transcription does not depend on live captions.

### Claude-powered features

The following features use the Claude Agent SDK:

- Immediate and periodic AI digests during recording
- Post-transcription speaker profiles and meeting minutes
- AI usage review generation
- Command-palette AI chat

Installing and signing in to the local `claude` CLI is recommended so the application can reuse its authentication. Chat and mail context is optional: paste a read-only MCP configuration into `/settings/connectors`. The configuration is stored in `.local/mcp.json`.

## Main pages

| Path | Page |
| --- | --- |
| `/` | Home dashboard |
| `/tasks` | Task workspace overview |
| `/tasks/:workspaceId` | Calendar, kanban, list, and review views |
| `/assets` | Asset overview |
| `/assets/finance/:workspaceId` | Yield tracking |
| `/assets/holdings/:workspaceId` | Holdings and snapshots |
| `/assets/ledger/:workspaceId` | Ledger and budgets |
| `/assets/receivables/:workspaceId` | Receivables and collections |
| `/assets/subscriptions/:workspaceId` | Subscription management |
| `/knowledge/:workspaceId` | Knowledge documents, search, and editing |
| `/knowledge/:workspaceId/present` | Presenter console or audience view |
| `/meetings` | Meeting workspaces and local engine status |
| `/meetings/:workspaceId/:meetingId` | Recording, transcription, review, minutes, and export |
| `/review` | AI usage metrics, collection, and review generation |
| `/settings/connectors` | Chat/mail MCP connector settings |

## Data Storage

```text
memory/
├── task-management/
│   └── workspaces/<id>/
│       ├── metadata.yaml
│       ├── tasks/YYYY/MM/DD/task_<ts>.md
│       └── reviews/<period>.md
├── finance/
│   ├── earn/<id>/YYYY/MM/DD/finance_<ts>.md
│   ├── holdings/<id>/{assets,snapshots}/
│   ├── ledger/<id>/{metadata.yaml,budgets.yaml,records/*.csv}
│   ├── receivables/<id>/YYYY/MM/DD/receivable_<ts>.md
│   └── subscriptions/<id>/YYYY/MM/DD/sub_<ts>.md
├── knowledge/
│   └── workspaces/<id>/
│       ├── metadata.yaml
│       ├── documents.yaml
│       ├── notes/
│       ├── quick/
│       ├── presenter-notes/
│       └── assets/
├── meetings/
│   └── workspaces/<id>/meetings/<meeting-id>/
│       ├── meta.yaml
│       ├── chunks/                  # Git-ignored
│       ├── audio/                   # Git-ignored
│       ├── live/captions.json
│       ├── transcript/segments.json
│       └── summary.md
└── ai-usage/
    ├── config.yaml
    └── snapshots/<YYYY-MM>--<host>.md

external/data/
└── exchange_rates.yaml

.local/                              # Machine-local config, keys, models, and trash; Git-ignored
```

## AI Skills

The repository includes reusable Agent Skills under `.agents/skills/`. Agents follow the data formats and workflows in these definitions and operate on the same files as the web application.

| Skill | Purpose |
| --- | --- |
| `create-workspace` | Create and register a task workspace |
| `task-management` | Create and classify tasks from natural language |
| `weekly-summary` | Summarize important completed work for a date range |
| `finance-analysis` | Analyze asset and finance data |
| `finance-yield` | Calculate returns for yield positions |
| `ledger-import` | Export, parse, and import Feidee ledger data |
| `meeting-minutes` | Process meeting transcripts and minutes |
| `creating-knowledge-presentations` | Create or revise knowledge-base HTML presentations |
| `youtube-summarizer` | Turn YouTube content into knowledge material |

## Development

### Common commands

Run these commands from `system/`:

```bash
bun run dev --host    # Development server
bun run typecheck     # React Router type generation + TypeScript checks
bun run build         # Production build
bun run start         # Serve the production build
bun run test:unit     # Vitest unit tests
bun run test:e2e      # Playwright end-to-end tests
bun run test          # Unit + end-to-end tests
```

### Project structure

```text
second-brain/
├── system/                 # React Router 7 full-stack web application
│   └── app/
│       ├── routes/         # Pages, loaders, actions, and resource routes
│       ├── components/     # Shared and domain UI components
│       └── lib/            # Data access, types, and domain logic
├── desktop/                # macOS Electron shell
├── memory/                 # User business data
├── external/data/          # Shared reference data
├── scripts/                # Import and maintenance utilities
├── .agents/skills/         # Agent Skills
├── .local/                 # Machine-local state (Git-ignored)
└── start.sh                # Pull, build when needed, and start production
```

### Tech stack

| Layer | Technology |
| --- | --- |
| Web | React 19, React Router 7 (SSR) |
| Build | Vite 7, TypeScript 5.9, Bun |
| UI | Tailwind CSS 4, Radix UI, Lucide React |
| Editing and rendering | CodeMirror 6, Marked, Mermaid 11, sanitize-html |
| Charts | Recharts 3 |
| Data | js-yaml, gray-matter, PapaParse, file-system storage |
| Testing | Vitest, Testing Library, Playwright |
| Desktop | Electron |

When a feature, workflow, or data contract changes, update the relevant README files and `AGENTS.md` guidance in the same change so the documentation stays aligned with the implementation.

---

<div align="center">

**Your data, your workflows, your system.**

</div>
