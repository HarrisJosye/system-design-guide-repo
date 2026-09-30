# The Definitive Guide to Modern System Design & Architecture

**A plain-language guide to how large systems are built, why they break, and how to fix them**

---

## How to read this guide

This guide explains how big software systems work: banking systems, AI chatbots, IoT platforms and SaaS products. It is written so that a junior developer can follow it, but it doesn't skip the hard parts. When a technical word appears for the first time, it is explained in plain words. The glossary below collects the most common ones.

**Start with Module 0.** It covers six problems that come up in almost every large system: handling lots of reads, handling lots of writes, keeping reads and writes apart, delivering data in real time, staying reliable, and running long jobs. Modules 1–6 then show those same ideas in real industries.

Every module has the same parts:

1. **Ideas and trade-offs.** What the concept is, and what you give up to get it.
2. **Python code.** Short, working examples.
3. **Case study.** A realistic system with rough numbers.
4. **Diagrams.** A simple block diagram at the start of each concept, plus bigger architecture diagrams. Click any diagram on the website to open it full screen and zoom in.
5. **Animation plan.** A step-by-step description of how you could animate the idea, for teaching.
6. **Review questions.** Questions a senior engineer would ask in a design review.

**About the numbers:** they are rough estimates, used to show *how* to reason about size and speed. They are not benchmarks. Always measure your own system. Cloud examples use Azure first, with AWS or GCP names where it helps.

**Three ideas appear again and again.** If you remember only three things, remember these:

1. **The database is the final judge.** Locks and caches make things faster, but they can fail. Rules enforced by the database, such as "only update this row if the version is still 7", are what actually keep data correct.
2. **Messages can arrive twice.** Networks retry. So every action that can be repeated must be safe to repeat. This property is called *idempotency*.
3. **Counters beat clocks.** Clocks on different machines disagree. A number that only goes up (a version, a term, an epoch) is a safer way to tell old from new: the higher number wins, and the older actor gets rejected.

## Glossary: words you'll see often

| Word | Plain meaning |
|---|---|
| **Latency** | How long one request takes, for example 50 ms |
| **Throughput** | How many requests the system handles per second |
| **p99 (99th percentile)** | The time within which 99 out of 100 requests finish. It shows how the slowest requests behave, not just the average |
| **TPS / RPS** | Transactions per second / requests per second |
| **Node** | One machine, virtual machine or container in a system |
| **Replica** | A copy of data kept on another node |
| **Primary** | The one copy that accepts writes. Replicas copy from it |
| **Shard / partition** | One slice of a big dataset. Each slice lives on different machines, so the work is split |
| **Cache** | A fast, temporary copy of data (often in memory, such as Redis) to avoid asking the slower database |
| **Queue / log** | A durable list of messages waiting to be processed. Kafka and Azure Event Hubs are common examples |
| **Idempotent** | Safe to repeat. Doing it twice has the same effect as doing it once |
| **Transaction** | A group of database changes that all succeed together or all fail together |
| **ACID** | The four promises a transaction makes: all-or-nothing, rules kept, no interference from others, and saved permanently |
| **Consistency (strong)** | Everyone sees the latest data right away |
| **Eventual consistency** | Copies may be briefly out of date but catch up soon |
| **Stale data** | An old copy that hasn't caught up yet |
| **Replication lag** | How far behind a replica is |
| **Throttling / rate limit** | Deliberately limiting how many requests a client can make |
| **Backpressure** | Slowing down the sender when the receiver can't keep up, instead of piling up work |
| **Timeout** | Giving up on a call after a set time, so you don't wait forever |
| **Retry with backoff and jitter** | Trying again after a failure, waiting a little longer each time, plus a small random delay so clients don't all retry at the same moment |
| **Circuit breaker** | A switch that stops calling a failing service for a while, so it can recover |
| **SLO** | A reliability target, for example "99.95% of requests succeed within 300 ms" |
| **RPO / RTO** | How much data you can afford to lose (RPO) and how long recovery may take (RTO) |
| **CDC (change data capture)** | Reading the database's own change log to publish every change as an event |
| **Outbox** | A table where you save "events to send" inside the same transaction as your data change, so sending can't be forgotten |
| **Event** | A record that something happened, for example "FundsDebited" |
| **Consensus** | A way for several machines to agree on one answer even if some of them fail. Raft is the best-known method |
| **Quorum / majority** | More than half the machines. Most agreement methods need a majority |
| **Split brain** | Two parts of a system both think they're in charge, and both make changes |
| **Fencing token** | A number that only goes up. It is attached to writes so the database can reject writes from an old owner |
| **Embedding / vector** | A list of numbers that represents the meaning of text, so similar texts have similar numbers |
| **LLM** | Large language model, the kind of AI that generates text |
| **Token (LLM)** | A small piece of text (part of a word) that an LLM reads or writes |
| **WAL (write-ahead log)** | A file where the database writes every change *before* updating its main data, so it can recover after a crash |
| **Tenant** | One customer organization in a shared (multi-tenant) system |

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

## Two editions

The main guide (the `modules/` folder and the website) is the **plain-language edition**, written so a junior developer can follow it, with a glossary at the top. The earlier, denser wording is kept in [`advanced/system-design-guide-advanced.md`](advanced/system-design-guide-advanced.md) for readers who want the terse version. Both have the same code and diagrams.

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
advanced/                  the earlier, more technical wording of the same guide
```

The Markdown in `modules/` is the source to edit. `docs/index.html` and the SVGs are generated snapshots, so they won't update automatically when you change a module.
