# The Definitive Guide to Modern System Design & Architecture

**From Senior to Staff/Principal: hard trade-offs, reference architectures, and teaching blueprints**

---

## How to read this guide

**Start with Module 0**, which covers the six recurring scaling concerns: scale reads, scale writes, split reads and writes, real-time data, reliability and long-running jobs. Modules 1–6 then apply them under industry-specific pressure. Each major concept has a simple block diagram at the top of its section.

Each module follows the same structure: **Theory & Trade-offs → Python in Practice → Case Study (with capacity math) → Mermaid Architecture → Animation Blueprint → Staff-level Review Questions.**

Numbers are order-of-magnitude assumptions used to show *how* to reason. They are not benchmarks. Always measure your own hardware, data and access patterns. Cloud mappings use Azure first, with AWS/GCP equivalents where it helps.

Three ideas come up in every module. Keep them in mind as the thread that ties the guide together:

1. **Correctness lives in the storage layer, not in coordination services.** Locks, caches and leader leases are optimizations. Constraints, conditional writes and monotonic version numbers are guarantees.
2. **Every network hop is at-least-once.** "Exactly-once" is at-least-once delivery plus an idempotent side effect, committed atomically with a dedupe record.
3. **Monotonic numbers beat wall clocks.** Fencing tokens (Module 1), Raft terms and device-ownership epochs (Module 3), and page LSNs and directory epochs (Module 4) are the same idea. A higher number wins, and a stale actor gets rejected by the thing it tries to write to.

## Contents

- [Module 0 — Core Building Blocks: The Six Scaling Concerns](modules/00-core-building-blocks-the-six-scaling-concerns.md)
- [Module 1 — High-Throughput & Financial Systems](modules/01-high-throughput-financial-systems.md)
- [Module 2 — AI & LLM Infrastructure at Scale](modules/02-ai-llm-infrastructure-at-scale.md)
- [Module 3 — Distributed Systems Consensus & Resiliency](modules/03-distributed-systems-consensus-resiliency.md)
- [Module 4 — Cloud-Native Data Partitioning & Storage](modules/04-cloud-native-data-partitioning-storage.md)
- [Module 5 — Real-Time Streaming & Fraud Detection](modules/05-real-time-streaming-fraud-detection.md)
- [Module 6 — Parallel-Run Migration](modules/06-parallel-run-migration.md)
- [Epilogue — Design Review Checklist & Further Reading](modules/99-epilogue-checklist-and-reading.md)

## Where each core concept is covered

| Concept | Foundations | Applied in |
|---|---|---|
| Scale reads | [0.1](modules/00-core-building-blocks-the-six-scaling-concerns.md#01-scale-reads) | Module 1 (CQRS read models), Module 2 (caching layers), Module 4 (read replicas) |
| Scale writes | [0.2](modules/00-core-building-blocks-the-six-scaling-concerns.md#02-scale-writes) | Module 1 (sharded ledger, hot accounts), Module 3 (IoT ingest), Module 4 (sharding, WAL) |
| Split reads and writes | [0.3](modules/00-core-building-blocks-the-six-scaling-concerns.md#03-split-reads-and-writes) | Module 1 (CQRS), Module 4 (SQLAlchemy read/write routing) |
| Real-time data | [0.4](modules/00-core-building-blocks-the-six-scaling-concerns.md#04-real-time-data) | Module 2 (SSE token streaming), Module 5 (stream processing and fraud) |
| Reliability | [0.5](modules/00-core-building-blocks-the-six-scaling-concerns.md#05-reliability) | Module 3 (consensus, breakers, degradation), Module 4 (replication, RPO) |
| Long-running jobs | [0.6](modules/00-core-building-blocks-the-six-scaling-concerns.md#06-long-running-jobs) | Module 6 (batch parallel run, statement generation) |

## Reading the diagrams

Every concept section starts with a simple block diagram, and each module also has detailed architecture diagrams (50 in total). There are two ways to read them:

- **On GitHub:** diagrams render inline in the module pages, with an **Open full-size diagram (SVG)** link underneath. Click it, then click **Raw** to open the SVG on its own browser tab. Browser zoom (Ctrl/Cmd and +, or Ctrl/Cmd and scroll) then enlarges only the diagram, and the text stays sharp because it's vector. All SVGs are also listed in [`diagrams/`](diagrams/).
- **On the website (GitHub Pages):** [`docs/index.html`](docs/index.html) is the styled single-page version. Click any diagram, or its **Expand** button, to open a full-screen viewer:

| Action | Mouse / trackpad | Touch | Keyboard |
|---|---|---|---|
| Zoom | Scroll wheel or pinch, at the pointer | Pinch | `+` / `-` |
| Pan | Drag | Drag | Arrow keys |
| Fit to screen | **Fit** button | **Fit** button | `0` |
| Actual size | **100%** button | **100%** button | `1` |
| Zoom in on a point | Double-click (Shift for out) | | |
| Close | **Close** button | **Close** button | `Esc` |

## Publishing the website

1. Push this repository to GitHub.
2. In the repository, go to **Settings → Pages**.
3. Under *Build and deployment*, choose **Deploy from a branch**, then select branch `main` and folder `/docs`. Save.
4. After a minute or two the site is live at `https://<your-username>.github.io/<repository-name>/`.

On free GitHub plans, Pages sites are public even if the repository is private.

## Repository layout

```
README.md                  contents, concept map and reading guide (this page)
modules/                   one Markdown file per module (00–06), rendered by GitHub
diagrams/                  full-size SVG for every diagram, linked from the modules
docs/index.html            styled website with the full-screen diagram viewer
docs/diagrams/             copies of the SVGs served by GitHub Pages
docs/system-design-guide-single-file.md   the whole guide as one Markdown file
```

The Markdown in `modules/` is the source to edit. `docs/index.html` and the SVGs are generated snapshots, so they won't update automatically when you change a module.
