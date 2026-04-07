<div align="center">

# 🧠 Second Brain

**An AI-native, file-system-backed personal management system.**

All your tasks, finances, portfolios, ledgers, subscriptions, and knowledge — stored as plain text files you fully own and control.

<!-- Add screenshot here: ![Second Brain Dashboard](assets/screenshot-dashboard.png) -->

[Features](#features) · [Why Second Brain](#why-second-brain) · [Quick Start](#quick-start) · [AI Skills](#ai-skills) · [Tech Stack](#tech-stack)

</div>

---

## What is Second Brain?

Second Brain is a full-stack web application for personal life management. It covers **task management**, **finance/earn tracking**, **portfolio holdings**, **ledger bookkeeping**, **subscription tracking**, and **knowledge management** — all in a single, self-hosted app.

What makes it different? **There is no database.** Every piece of data is stored as human-readable plain text — YAML, Markdown with YAML frontmatter, and CSV — right in the repository under `memory/`. You can read your data with `cat`, edit it with `vim`, diff it with `git`, and back it up by pushing to a remote. Your data is never locked in an opaque system.

This architecture also makes Second Brain uniquely suited for AI-assisted workflows. AI coding agents like **Claude Code** and **OpenAI Codex** can directly read and write the same plain-text files, enabling natural-language task creation, automated financial analysis, and zero-friction feature development on the existing codebase.

> **Note:** The UI is in Chinese (中文). The codebase and documentation are in English.

## Features

### 📋 Task Management

Multi-workspace task system with three powerful views:

- **Calendar View** — Month grid with date-range visualization; tasks plotted by due date or start date; color mode toggle (by stage or by project)
- **Kanban View** — Configurable columns with drag-and-drop; batch archive completed tasks
- **List View** — Sortable rows with full metadata; keyboard navigation support

**Additional capabilities:**
- Advanced filtering: full-text search, stage filter, date range, multi-tag filter, archive toggle — all synced to URL params for bookmarking
- Configurable workspaces with custom kanban stages, projects, priorities, and tags
- Auto-emoji title decoration on new tasks
- Auto-set `start_date` when moving to "in progress"
- Task detail side sheet with Markdown editing
- Dashboard with aggregated stats: active tasks, in-progress count, overdue count, 8-week completion trend, upcoming tasks

### 💰 Finance Earn Tracking

Track fixed-income and yield-bearing positions:

- APR-based return calculation with segmented history (investment amount and rate can change over time)
- Linear yield formula: `amount × (APR / 100) × (days / 365)`, accumulated across segments
- Calculate current return, month-end projected return, or maturity return
- Active / archived position lifecycle
- Multi-currency support (USD, CNY, HKD) with automatic conversion
- 7-day expiration warnings

### 📊 Portfolio Holdings

Full portfolio tracker with dashboard analytics:

- Multi-asset types: crypto, US stocks, HK stocks, CN stocks (A股), cash
- Price history tracking per asset with CoinGecko integration (pre-mapped symbols for 16+ major tokens)
- Point-in-time portfolio snapshots recording all holdings, prices, exchange rates, and total value
- Net worth calculation and trend analysis across snapshots
- Asset allocation breakdown with color-coded charts
- Automatic currency inference by asset type (HKD for HK stocks, CNY for CN stocks, USD default)

### 📖 Ledger Bookkeeping

Income and expense tracking with budget management:

- Multi-level category system with nested subcategories (e.g., 饮食 → 咖啡, 餐饮, 零食)
- Budget management with daily / monthly / quarterly / yearly periods
- Budget usage tracking with period-aware aggregation
- Multi-year records stored as yearly CSV files
- CSV import pipeline from Feidee (随手记) with automatic category mapping
- Monthly overview dashboard with income/expense totals, category breakdown, and net calculation
- Multi-currency: USD, CNY, HKD, EUR, JPY, GBP

### 🔄 Subscription Tracking

Manage recurring expenses with billing intelligence:

- Flexible billing cycles: monthly, yearly, quarterly, semi-annual, weekly, or custom (value + unit)
- Subscription lifecycle: active → paused → canceled, with cancel date and service end date
- Trial period detection and tracking
- Auto-calculated next billing date from first billing date + cycle
- Monthly and yearly cost annualization
- Days-until-billing countdown
- Upcoming subscription alerts (30-day window on the assets dashboard)

### 📚 Knowledge Management

A personal wiki with bidirectional linking:

- Markdown notes with YAML frontmatter metadata (title, tags, timestamps, type)
- Two note types: quick notes (速记) for brief entries, full notes (笔记) for long-form documents
- **Bidirectional linking**: outgoing links, backlinks, and related notes — auto-discovered across the workspace
- **CodeMirror 6 editor** with syntax highlighting, bracket matching, search, line numbers, and direct image paste/upload
- **Mermaid diagram rendering** in code blocks (theme-aware: light/dark)
- Full-text search across title, content, and tags with match highlighting and excerpts
- Workspace-specific tag system with custom display labels
- Note insights: auto-extracted headings (with anchor IDs), linked notes, and referenced assets
- Asset management: images and files stored alongside notes

### 🏠 Home Dashboard

A personal command center:

- Time-based greeting (早上好 / 下午好 / 晚上好) with date display
- Weather widget via Open-Meteo API with geolocation, temperature, and Chinese weather descriptions
- Curated quote of the day from a collection of 30+ literary quotes
- Task statistics: total active, in-progress, overdue, workspace count
- Quick navigation cards to all major sections

### 🎨 UI / UX

- **Dark / Light theme** with smooth 300ms transitions, persisted in localStorage, no flash on load
- **Collapsible resizable sidebar** (drag-to-resize, icon-only collapse mode, persistent state)
- Full SSR with client hydration — fast initial load
- **150+ currency exchange rates** built-in for cross-module conversion

## Why Second Brain

### 🗂️ 100% Local File Storage — You Own Your Data

All data is stored as **plain text files** — YAML for metadata, Markdown with YAML frontmatter for records, CSV for ledger entries. No database, no cloud dependency, no vendor lock-in.

- **Read with any tool**: `cat`, `grep`, `vim`, VS Code — your data is always accessible
- **Version control with Git**: every change is tracked, diffable, and reversible
- **Back up anywhere**: push to GitHub, copy to a USB drive, sync with rsync
- **Portable**: move your entire life system by copying a folder

### 🤖 AI-Native Architecture — Built for AI Agents

Second Brain comes with **pre-built AI skills** designed for Claude Code, Codex, and other AI coding agents:

| Skill | What It Does |
|-------|-------------|
| **Task Creation** | Describe a task in natural language → auto-classified stage, priority, projects, and tags |
| **Weekly Summary** | Generate a summary of the 3 most impactful completed tasks in any date range |
| **Finance Yield** | Calculate current, month-end, or maturity returns for investment positions |
| **Ledger Import** | Automated Feidee export → XLSX parse → CSV import with category completion |
| **Workspace Creation** | Create a new task workspace from a description with all required YAML files |

Because the data layer is plain text, AI agents read and write the same files as the web app. No API wrappers, no ORM mappings, no intermediate layers. The AI operates directly on your data with full context.

### 🔧 Zero-Friction Customization — Let AI Build Your Features

This is the killer advantage. Want a new feature? **Just describe it in natural language and let an AI agent implement it.**

The architecture is deliberately simple:
- **React Router loaders/actions** for request handling
- **File-system reads/writes** for persistence
- **No complex abstractions** — straightforward TypeScript you (or an AI) can read in minutes

The flat, readable codebase makes AI-assisted development trivially easy. An AI agent can:
- Understand the full data model by reading a few YAML/Markdown files
- Add new routes, components, or data modules following established patterns
- Modify any feature with full confidence because there's no hidden state

You're not locked into a rigid product roadmap. Your personal management system evolves with your needs, and AI does the heavy lifting.

### 🔓 Full Data Transparency

- Every record is **human-readable** — no opaque database blobs
- `git diff` shows **exactly** what changed and when
- Perfect for **auditing**: review any financial record, task history, or note revision
- No data migrations — add a new YAML field and it just works

## Quick Start

### Prerequisites

- [Bun](https://bun.sh/) (package manager and runtime)

```bash
# Install Bun if you don't have it
curl -fsSL https://bun.sh/install | bash
```

### Install & Run

```bash
# Clone the repository
git clone https://github.com/user/second-brain.git
cd second-brain

# Install dependencies
bun install
cd system && bun install

# Start the development server
bun run dev --host
```

Open **http://localhost:5173** — you're in.

### Other Commands

```bash
# From the system/ directory:
bun run dev --host      # Development server with hot reload
bun run build           # Production build
bun run start           # Serve production build
bun run typecheck       # TypeScript type checking

# From the repository root:
./start.sh              # Convenience: git pull + start dev server
./sync.sh "message"     # Compress images + git commit + push
```

## AI Skills

Second Brain ships with pre-built skills for AI coding agents (Claude Code, Codex, etc.). These skills are defined in `.claude/skills/` and operate directly on the file system.

| Skill | Description | Example |
|-------|-------------|---------|
| **task-management-create** | Create a task from natural language with auto-classification of stage, priority, projects, and tags | *"Track the Twitter API migration, high priority, due Friday"* |
| **weekly-summary** | Generate a weekly summary of the top 3 completed tasks with impact analysis | *"Summarize last week's work"* |
| **finance-yield** | Calculate returns for finance earn positions (current / month-end / maturity) | *"How much has the USDT position earned?"* |
| **ledger-import** | Import transactions from Feidee (随手记) with automatic category and subcategory mapping | Automates browser export → parse → import |
| **create-workspace** | Create a new task workspace with kanban stages, projects, tags, and register in global index | *"Create a workspace for my side project"* |

### How It Works

The AI agent reads the skill definition (which describes the data format, file paths, and classification rules), then reads/writes the same plain-text files that the web app uses. No special API needed — the file system **is** the API.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Framework** | React 19 + React Router 7 (SSR) |
| **Build** | Vite 7 + TypeScript 5.9 (strict) |
| **Styling** | Tailwind CSS 4 + shadcn/ui (Radix primitives) |
| **Editor** | CodeMirror 6 (Markdown) + Mermaid 11 (diagrams) |
| **Charts** | Recharts 3.7 |
| **Data** | js-yaml + gray-matter (YAML/frontmatter) + PapaParse (CSV) |
| **Icons** | Lucide React |
| **Runtime** | Bun (dev + package manager) / Node.js (production) |

## Data Architecture

All data lives under `memory/` as plain text:

```
memory/
├── task-management/
│   └── workspaces/
│       ├── metadata.yaml                          # Workspace index
│       └── <workspace-id>/
│           ├── metadata.yaml                      # Kanban stages, projects, priorities, tags
│           └── tasks/
│               ├── metadata.yaml                  # Kanban ordering
│               ├── archived.yaml                  # Archived task paths
│               └── YYYY/MM/DD/task_<ts>.md        # Task (YAML frontmatter + Markdown body)
│
├── finance/
│   ├── earn/<id>/
│   │   └── YYYY/MM/DD/finance_<ts>.md             # Earn positions (APR, amount, currency, history)
│   │
│   ├── holdings/<id>/
│   │   ├── assets/<name>.md                       # Assets (symbol, type, quantity, cost, price history)
│   │   └── snapshots/YYYY/MM/DD/snapshot_<ts>.md  # Portfolio snapshots
│   │
│   ├── ledger/<id>/
│   │   ├── metadata.yaml                          # Categories and subcategories
│   │   ├── budgets.yaml                           # Budget rules (category, period, amount)
│   │   └── records/YYYY.csv                       # Yearly transaction records
│   │
│   └── subscriptions/<id>/
│       └── YYYY/MM/DD/sub_<ts>.md                 # Subscriptions (billing cycle, status, trial)
│
├── knowledge/
│   └── workspaces/<id>/
│       ├── metadata.yaml                          # Workspace config and tags
│       ├── notes/**/*.md                          # Markdown notes with frontmatter
│       └── assets/**                              # Images and files
│
external/data/
└── exchange_rates.yaml                            # 150+ currency exchange rates
```

### File Formats

- **Tasks, finance records, subscriptions, knowledge notes** → Markdown files with YAML frontmatter
- **Workspace metadata, kanban ordering, budgets** → Pure YAML files
- **Ledger records** → Yearly CSV files (`id,type,date,category_id,subcategory_id,currency,amount,note`)
- **Exchange rates** → YAML map of currency codes to USD rates

## Project Structure

```
second-brain/
├── system/                    # React Router web application
│   ├── app/
│   │   ├── routes/            # Page routes (loaders + actions + UI)
│   │   ├── components/        # UI components (shadcn/ui + custom)
│   │   │   ├── ui/            # shadcn/ui primitives
│   │   │   └── task/          # Task-specific components
│   │   └── lib/               # Domain logic
│   │       ├── *-types.ts     # TypeScript type definitions
│   │       ├── *-data.server.ts  # Server-only file I/O (loaders/actions)
│   │       └── *-helpers.ts   # Display logic, calculations, constants
│   ├── public/                # Static assets
│   └── package.json
│
├── memory/                    # All user data (plain text)
├── external/data/             # Shared reference data
├── scripts/                   # Import/maintenance utilities
├── .claude/skills/            # AI agent skill definitions
├── .github/copilot-instructions.md
├── start.sh                   # Dev startup script
└── sync.sh                    # Image compression + git sync
```

## Routes

| Path | Description |
|------|-------------|
| `/` | Home dashboard — greeting, weather, task stats, quick nav |
| `/tasks` | Task workspace overview — aggregated stats, trends, upcoming |
| `/tasks/:workspaceId` | Task workspace — calendar / kanban / list views |
| `/assets` | Assets dashboard — net worth, allocation, monthly ledger overview |
| `/assets/finance` | Finance earn workspace list |
| `/assets/finance/:workspaceId` | Earn tracker — active/archived positions, yield calculations |
| `/assets/holdings` | Holdings workspace list |
| `/assets/holdings/:workspaceId` | Portfolio — dashboard, assets, snapshots tabs |
| `/assets/ledger` | Ledger workspace list |
| `/assets/ledger/:workspaceId` | Bookkeeping — dashboard, records, budgets tabs |
| `/assets/subscriptions` | Subscription workspace list |
| `/assets/subscriptions/:workspaceId` | Subscription management |
| `/knowledge` | Knowledge workspace list |
| `/knowledge/:workspaceId` | Knowledge workspace — notes, search, editor |

## License

This project is for personal use. License TBD.

---

<div align="center">

**Built with plain text. Powered by AI. Owned by you.**

</div>
