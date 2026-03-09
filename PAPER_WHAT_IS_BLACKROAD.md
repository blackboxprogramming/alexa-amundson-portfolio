# What Is BlackRoad OS: A Modular AI Infrastructure Platform Built from 1,700+ Components

**Alexa Amundson — BlackRoad OS, Inc.**
*March 2026 | Verified from source code across 16 GitHub orgs, 8 Gitea orgs, and 4 Raspberry Pis*

---

## Abstract

BlackRoad OS is a modular, self-hosted AI infrastructure platform composed of independently deployable components spanning Cloudflare Workers, Raspberry Pi services, cloud VPS daemons, CLI tools, browser extensions, and research engines. This document maps every verified working component across all 17 organizations (16 GitHub orgs + personal + 8 Gitea orgs), shows how they connect, and explains the modular architecture that makes a single-operator platform possible.

---

## 1. The Modular Architecture

BlackRoad OS is not a monolith. It is **hundreds of small, independently deployable modules** organized into seven layers:

```
┌─────────────────────────────────────────────────────────────┐
│                    EDGE LAYER (Cloudflare)                   │
│  130+ Workers · 95+ Pages · 40 KV · 7 D1 · 10 R2 · 18 Tunnels │
├─────────────────────────────────────────────────────────────┤
│                    GATEWAY LAYER (Alice Pi)                  │
│  48+ domains · Nginx (26 sites) · Pi-hole DNS · Tunnels     │
├─────────────────────────────────────────────────────────────┤
│                   AI INFERENCE LAYER (Cecilia)               │
│  16 Ollama models · Hailo-8 (26 TOPS) · CECE API · TTS      │
├─────────────────────────────────────────────────────────────┤
│                   STORAGE LAYER (Octavia)                    │
│  1TB NVMe · Gitea (187 repos) · NATS · Docker Swarm         │
├─────────────────────────────────────────────────────────────┤
│                ORCHESTRATION LAYER (Distributed)             │
│  Task queues · Agent registry · Job scheduler · Health beats │
├─────────────────────────────────────────────────────────────┤
│                  IDENTITY LAYER (PS-SHA-∞)                   │
│  Hash-chained journals · Agent attestation · Audit ledger    │
├─────────────────────────────────────────────────────────────┤
│                   RESEARCH LAYER (Papers)                    │
│  776 sections · 30+ papers · 11 proofs · 10 reasoning agents │
└─────────────────────────────────────────────────────────────┘
```

Each layer is composed of small, replaceable modules. Nothing depends on everything. Any component can fail without bringing down the system.

---

## 2. Edge Layer — Cloudflare Workers (Verified Deployed)

These are real, deployed Cloudflare Workers with verified source code:

### Core Platform Workers

| Worker | Stack | What It Actually Does |
|--------|-------|----------------------|
| **blackroad-os-api** | Hono/TypeScript | Agent identity & governance API. 8 KV namespaces (AGENCY, AGENTS, LEDGER, ORGS, INTENTS, POLICIES, CLAIMS, DELEGATIONS). Creates agents, declares intents, enforces policies, tracks claims/attestations on a hash-chained ledger. |
| **blackroad-os-mesh** | Hono/Durable Objects | Real-time WebSocket server. MeshRoom manages connections, presence tracking, heartbeats, broadcast/direct messaging between agents. The nervous system. |
| **blackroad-tools** | Hono/TypeScript | 4 tool categories on D1: Agent tools (registry CRUD), Memory tools (hash-chained store/retrieve), Reasoning tools (trinary logic: true/unknown/false with contradiction quarantine), Coordination tools (pub/sub, task delegation). |
| **blackroad-memory-api** | JS/D1 | Memory system v2.0. Hash-chained entries (PS-SHA-infinity). CRUD on `/entries` with filtering by action, tags. Each entry links to previous via hash. |
| **blackroad-claude-agent** | Hono/TypeScript | CECILIA AI agent. Claude-powered chatbot with KV conversation state, D1 agent registry. System prompt defines Cecilia as the orchestrator. |
| **blackroad-status-hub** | Hono/TypeScript | Health dashboard at status.blackroad.io. Pings 18 worker endpoints, queries D1 agent registry for stats by core/zone/status/health. |
| **blackroad-infra-manager** | Hono/TypeScript | Infrastructure API at infra.blackroad.io. Full CRUD for DNS records, worker routes, workers, zones. `/setup/:subdomain` quick provisioning. Wraps Cloudflare API. |
| **blackroad-io** | JS | Platform dashboard v3.1.0. Serves `/platform` dashboard, `/api/platform/health`, `/api/platform/inventory`, `/api/platform/workers` (fetches from CF API). |

### Content & Feature Workers

| Worker | What It Does |
|--------|-------------|
| **blackroad-features-api** | Feature flag service |
| **blackroad-metrics-api** | Metrics collection endpoint |
| **blackroad-pdf-api** | PDF generation worker |
| **blackroad-audit** | Audit trail logging |
| **blackroad-codex-worker** | Code indexing API |

### Domain Workers (48+ domains)

Each domain routes through Cloudflare to either a Worker, Pages project, or Pi tunnel:
- `blackroad.io` → platform dashboard Worker
- `status.blackroad.io` → status-hub Worker
- `infra.blackroad.io` → infra-manager Worker
- `api.blackroad.io` → os-api Worker
- `agents.blackroad.io` → agent proxy (Alice → Aria)
- `git.blackroad.io` → Gitea on Octavia (via tunnel)
- `dashboard.blackroad.io` → dashboard proxy (Alice → Octavia)
- Plus 40+ more subdomains and standalone domains

### Cloudflare Data Stores

| Type | Count | Key Instances |
|------|-------|---------------|
| KV Namespaces | 40 | AGENTS, AGENCY, LEDGER, TEMPLATES, CONTENT, STATUS_CACHE |
| D1 Databases | 7 | blackroad-os-main, blackroad-memory-db, apollo-agent-registry, blackroad-continuity |
| R2 Buckets | 10 | Object storage for assets, backups |
| Tunnels | 18 (6 active) | blackroad-pi (8 conn), cecilia, codex-infinity, lucidia, shellfish |

---

## 3. Gateway Layer — Alice (Pi 400)

Alice routes all external traffic to internal services:

### Nginx Reverse Proxy (26 sites)

| Domain | Proxies To |
|--------|-----------|
| agents.blackroad.io | Aria :4010 |
| api.blackroad.io | Local :8001 |
| dashboard.blackroad.io | Octavia :3000 |
| fleet.blackroad.io | Local fleet dashboard |
| status.blackroad.io | Local status service |
| blackroad.io | Local landing |
| lucidia.earth | Local Lucidia service |
| Plus 19 more sites | Various Pi services |

### Running Services

| Service | Type | Function |
|---------|------|----------|
| road-control | PM2/Node | Deployment orchestrator |
| blackroad-task-queue-v2 | systemd | SQLite-backed distributed task queue |
| blackroad-task-worker | systemd | Task execution daemon |
| blackroad-agents | systemd | Agent coordination service |
| blackroad-agents-proxy | systemd | Proxy to Aria agent API |
| blackroad-operator | systemd | Automation operator |
| blackroad-status | systemd | Health monitoring |
| blackroad-salesforce-agent | systemd | CRM integration agent |
| Pi-hole | systemd | DNS ad-blocking for entire network |
| PostgreSQL 13 | systemd | Relational database (blackroad DB) |
| Qdrant | systemd | Vector database (:6333/:6334) |
| Cloudflared | systemd | Tunnel with 8 connections |
| GitHub Actions Runner | systemd | CI/CD executor |
| roadnet-failover | systemd | Network failover handler |

---

## 4. AI Inference Layer — Cecilia (Pi 5)

### Ollama Models (16 deployed)

| Model | Size | Role |
|-------|------|------|
| cece (custom) | 637 MB | Primary agent personality |
| cece2 (custom) | 637 MB | Agent personality v2 |
| tinyllama | 637 MB | Fast routing/classification (17.4 tok/s) |
| llama3.2:1b | 1.3 GB | Compact general-purpose (7.9 tok/s) |
| llama3.2:3b | 2.0 GB | Mid-range general (5.0 tok/s) |
| deepseek-coder:1.3b | 776 MB | Code generation (9.3 tok/s) |
| qwen2.5-coder:3b | 1.9 GB | Quality code gen (4.5 tok/s) |
| codellama:7b | 3.8 GB | Full code model |
| llama3:8b | 4.9 GB | Large general-purpose |
| qwen3:8b | 5.2 GB | Large general-purpose |
| OpenELM-1.1B | 672 MB | Apple's efficient model |
| OpenELM-3B | 1.9 GB | Apple's mid-range model |

### Hardware Acceleration

- **Hailo-8** (26 TOPS, FW 4.23.0, Serial HLLWM2B233704667)
- 9 YOLO vision models on Hailo for real-time inference
- PCIe M.2 M-Key module

### Services

| Service | Port | Function |
|---------|------|----------|
| cece-api | 3100 | CECE AI gateway (Node.js) |
| cece-heartbeat | — | Health monitoring daemon |
| Ollama proxy | 11435 | Python proxy to Ollama |
| TTS API | 5001 | Text-to-speech service |
| Monitoring API | 5002 | System monitoring |
| PostgreSQL 17 | 5432 | Relational database |
| MinIO | 9000 | S3-compatible object storage |
| InfluxDB | 8086 | Time-series metrics |
| Nginx | 80 | HTTP reverse proxy |
| hailort | — | Hailo runtime service |

---

## 5. Storage & Code Layer — Octavia (Pi 5)

### Gitea (Self-Hosted Git)

**187 repositories across 8 organizations:**

| Organization | Repos | Focus |
|-------------|-------|-------|
| blackroad-os | 115+ | Core platform, Workers, dashboards, integrations |
| roadchain | 21 | Blockchain, billing, deployment, monitoring |
| lucidia | 14 | AI assistant, math engines, metaverse |
| infrastructure | 8 | Deploy scripts, IaC, Pi operations |
| platform | 8 | Dashboards, web apps, webhooks |
| tools | 5 | CLI (Go, Rust, npm, Python), SDK |
| agents | 3 | Agent dial, NLP, coordination |
| services | — | Microservices |

**Languages:** TypeScript (10), Shell (10), HTML (11), JavaScript (6), Go (2), Python (1), Rust, C#

### Services

| Service | Port | Function |
|---------|------|----------|
| Gitea | 3100/2222 | Git hosting for 187 repos |
| NATS | 4222/8222 | Messaging bus |
| Ollama | 11434 | 9 AI models |
| OctoPrint | 5000 | 3D printer controller |
| blackroad-agent | 8787 | Agent daemon |
| blackroad-dashboard | 3000 | Dashboard service |
| blackroad-worker-hailo | — | Hailo job queue processor |
| Docker Swarm | 2377 | Container orchestration |
| InfluxDB | 8086 | Time-series metrics |

### NVMe Storage (1TB)
- `/mnt/nvme/quantum_discoveries/` — 11 computational experiment datasets
- `/mnt/nvme/blackroad/` — Gitea data, Claude Time system, cloud configs
- `/mnt/nvme/models/` — Hailo .hef models (YOLOv5, ResNet)

---

## 6. Orchestration Layer — Distributed Across Fleet

### Task Queue System (Alice)
- SQLite-backed with 4 queue types
- Dead-letter routing for failed tasks
- Visibility timeouts prevent duplicate processing
- Automatic retry with exponential backoff
- Worker daemon with heartbeat monitoring

### Agent Registry (Cloudflare + Pis)
- D1 database tracks agent identity, capabilities, health
- KV namespaces for fast lookup (AGENTS, AGENCY, LEDGER)
- PS-SHA-∞ hash chains for append-only audit
- Trinary logic engine: true (+1) / unknown (0) / false (-1)
- Contradiction quarantine — conflicting claims isolated, not discarded

### Job Scheduling (Cecilia/Octavia)
- blackroad-os-operator: Fastify + BullMQ (Redis-backed)
- Cron scheduling for periodic tasks
- LLM chat endpoint (`/chat`) proxies to Ollama
- Health/ready/version endpoints

### Network Coordination
- roadnet-failover on all nodes — automatic network recovery
- WireGuard mesh maintains connectivity across LAN + cloud
- NATS messaging (Octavia) for async inter-service communication

---

## 7. Identity Layer — PS-SHA-∞

The cryptographic identity system that ties everything together:

```
PS-SHA-∞(M_t) = H(M_t ‖ PS-SHA-∞(M_{t-1}) ‖ t ‖ context)
```

**Implemented in:**
- `blackroad-memory-api` Worker — every memory entry hash-chains to previous
- `blackroad-os-api` Worker — agent claims and attestations on hash-chained ledger
- `blackroad-tools` Worker — reasoning tool records with contradiction preservation
- Research papers — formal specification in `ps-sha-infinity/spec.md`

**Properties:**
- Append-only (never erases — "P.S." = Post-Script)
- Causal dependency (each entry references parent hash)
- Paraconsistent (contradictions preserved, not resolved)
- Agent-signed (actors attest to their own entries)

---

## 8. Client Layer — User-Facing Tools

### Browser Extensions
| Tool | What It Does |
|------|-------------|
| **context-bridge** | Stores AI context in GitHub Gist, injects into ChatGPT/Claude/Copilot with one click. Solves the cross-session memory problem. |
| **blackroad-chrome-extension** | Quick-access popup to all BlackRoad GitHub orgs, Cloudflare dashboard, agent visualization |

### CLI Tools
| Tool | Language | What It Does |
|------|----------|-------------|
| **lucidia-cli** | Python/Textual | TUI with terminal web browser, sandboxed VFS, Ollama AI agents (5 personalities), mini apps (calc, crypto, weather), process manager |
| **blackroad-deploy** | Node.js | Railway-like deployment CLI. Auto-detects language (Node/Python/Go/Rust/Docker), deploys via SSH, manages Cloudflare DNS, webhook push-to-deploy |
| **blackroad-auto-cleanup** | Shell | Batch management of 1,500+ repos: merge Dependabot PRs, fix CI, consolidate repos |
| **blackroad-dashboards** | Shell | 4+ terminal dashboards: agent status grid, live infra monitor (pings Pis/droplets/Cloudflare/GitHub), system monitor with progress bars |
| **blackroad-scripts** | Shell | 400+ automation scripts in home directory |

### Web Interfaces
| Interface | Stack | What It Does |
|-----------|-------|-------------|
| **blackroad-os-web** | Next.js 16/React 19/Zustand | Frontend across 5 domains. Auth, workspace, conversations UI. Supabase backend. |
| **blackroad-os-prism-enterprise** | Express + React/Vite | Enterprise dashboard with Stripe billing, E2E tests (Playwright), LLM bridge |
| **blackroad-simple-launch** | Static HTML + Stripe | Payment funnel: 3 tiers ($29/$58/$199/mo), webhook handler, welcome email |
| **BlackRoad-Operating-System** | HTML/CSS/JS | Windows 95-style web OS interface at blackroad.systems |
| **blackroad-metaverse** | Three.js | 3D world with first-person controls, 6 biomes, 3 AI agent avatars, day/night cycle, particle effects |

---

## 9. AI Research Layer

### Reasoning Engines (lucidia-core)
10 domain expert agents totaling 5,481 lines:

| Agent | Lines | Specialization |
|-------|-------|---------------|
| Physicist | 867 | Physical reasoning, quantum mechanics |
| Mathematician | 760 | Proof construction, number theory |
| Geologist | 654 | Earth science modeling |
| Engineer | 599 | Systems engineering |
| Painter | 583 | Visual/spatial reasoning |
| Chemist | 569 | Chemical systems |
| Analyst | 505 | Data analysis |
| Architect | 392 | System design |
| Speaker | 302 | Natural language |
| Poet | 250 | Creative generation |

### Mathematical Engines (lucidia-math)
1,052+ lines across two modules:
- **Forge:** consciousness modeling (650 lines), unified geometry (402 lines), fractals, proofs, dimensions
- **Lab:** geometry engine (492 lines), Amundson equations (284 lines), trinary logic, prime explorer, quantum finance

### Quantum Simulator (quantum-math-lab)
Pure Python + NumPy quantum circuit simulator: Hadamard, Pauli-X/Y/Z, CNOT gates. 10 unsolved problem summaries.

---

## 10. The RoadChain Ecosystem (Gitea: roadchain/)

21 repositories implementing blockchain-adjacent services:

| Component | What It Does |
|-----------|-------------|
| **roadchain** | Python blockchain implementation |
| **road-control** | Deployment orchestrator (running on Alice) |
| **road-deploy** | Service deployment agent |
| **roadbilling** | Billing and invoicing system |
| **roadgateway** | API gateway |
| **roadcli** | Command-line interface |
| **roadcommand-enhanced** | Enhanced command center |
| **roadlog-monitoring** | Log aggregation and monitoring |
| **roadmarket** | Marketplace interface |
| **roadpad** | Python notepad/documentation tool |
| **roadstudio** | Development studio |
| **roadvpn** | VPN management |
| **roadworld** | Virtual world engine |
| **roadcoin-repo** | Cryptocurrency token |
| **road-registry-api** | Service registry |

---

## 11. Infrastructure as Code

### Docker (100+ Dockerfiles, 35+ Compose files)

**blackroad-priority-stack** deploys 4 services via Docker Compose:
- Headscale (self-hosted mesh VPN)
- Keycloak (identity/SSO)
- vLLM (local AI inference)
- EspoCRM (customer relationship management)

### CI/CD (2,400+ GitHub Actions workflows)
- Monorepo matrix builds
- Playwright E2E tests
- Auto-deploy to Cloudflare
- Dependabot auto-merge
- Cross-repo coordination

### Monitoring
- Prometheus node exporters on all Pis (:9100)
- InfluxDB time-series on Cecilia + Octavia
- blackroad-monitor daemons on all nodes
- status-hub Worker pings 18 endpoints

---

## 12. The Modular Principle

**Why this architecture works for one person:**

Every component is small enough to understand in one sitting. The largest single codebase is the Prism Enterprise monorepo. Most Workers are under 500 lines. Most shell scripts are under 100 lines. Most Pi services are single-process daemons.

**The module boundaries:**
- Edge (Cloudflare) handles TLS, routing, DDoS, static assets — zero ops burden
- Gateway (Alice) handles DNS and reverse proxy — one nginx config
- Inference (Cecilia) handles AI — one Ollama instance, one Hailo driver
- Storage (Octavia) handles state — one Gitea, one NVMe
- Each Worker is independently deployable via `wrangler deploy`
- Each Pi service is independently restartable via `systemctl`

**What connects them:**
- Cloudflare Tunnels (zero-trust ingress, no open ports)
- WireGuard mesh (encrypted inter-node communication)
- NATS messaging (async pub/sub)
- PS-SHA-∞ hash chains (identity and audit across all services)
- Nginx reverse proxy (domain → service routing)

**What fails gracefully:**
- Aria offline → agents proxy fails, but core platform unaffected
- Gematria SSH down → WireGuard still routes, services still reachable
- Any single Worker down → status-hub detects, other Workers unaffected
- NVMe full → SD card boot continues, just no new experiment data

---

## 13. The 16 GitHub Organizations

| Organization | Repos | Focus |
|-------------|-------|-------|
| **BlackRoad-OS** | 1,236 | Core platform — Workers, Pages, agents, dashboards, integrations, domain sites |
| **BlackRoad-OS-Inc** | 21 | Corporate core — tokenless gateway (125 tests), FastAPI (22 tests), br CLI (37 tools), SDK, Salesforce LWC |
| **BlackRoad-AI** | 16 | AI infrastructure — unified API gateway, cluster orchestration, memory bridge, vLLM, model registry |
| **BlackRoad-Security** | 16 | Security tools — SIEM (10 detection rules), threat intel (STIX 2.1), identity provider (TOTP MFA), encryption suite, password manager |
| **BlackRoad-Hardware** | 14 | IoT/hardware — IoT gateway (MQTT/HTTP/CoAP/Modbus), sensor network (Z-score anomaly), fleet tracker, device registry |
| **BlackRoad-Foundation** | 14 | Business tools — CRM, HR system, event manager, grant tracker, analytics dashboard, ticket system |
| **BlackRoad-Media** | 13 | Media processing — video transcoder, streaming hub (HLS/RTMP/DASH), podcast platform, newsletter engine, image optimizer |
| **BlackRoad-Interactive** | 12 | Game/creative — ECS game engine (ASCII renderer), 2D physics engine, 3D wireframe renderer, audio synthesizer, particle system |
| **BlackRoad-Archive** | 11 | Data preservation — IPFS tracker, document archive (FTS5), backup manager, web archiver, artifact registry |
| **BlackRoad-Labs** | 10 | ML research — ML pipeline (5-stage), feature store, experiment tracker, data pipeline, notebook server |
| **BlackRoad-Cloud** | 10 | Cloud-native — K8s operators, service mesh (circuit breaker), Terraform modules, secrets operator, load balancer |
| **BlackRoad-Gov** | 9 | Governance — compliance framework, digital identity (KYC), FOIA request manager, policy tracker, budget tracker |
| **Blackbox-Enterprises** | 8 | Enterprise forks — n8n, Airbyte, Prefect, Temporal, Activepieces, Huginn |
| **BlackRoad-Ventures** | 8 | Investment tools — deal flow tracker, portfolio tracker, LP portal, startup metrics |
| **BlackRoad-Studio** | 7 | Creative tools — color palette generator (WCAG), motion library (24 easings), writing studio |
| **BlackRoad-Education** | 7 | Learning — AI tutoring platform, adaptive quiz (spaced repetition), code challenge platform |
| **blackboxprogramming** (personal) | 108 | Origin repos — simulation-theory, quantum-math-lab, lucidia, context-bridge, universal-computer |

**GitHub total: 1,520 repos across 16 orgs + 108 personal = 1,628**
**Gitea total: 187 repos across 8 orgs**
**Grand total: 1,815 repositories**

---

## 14. Component Count

| Category | Count | Verified |
|----------|-------|----------|
| Cloudflare Workers (deployed) | 130+ | Wrangler |
| Cloudflare Pages (deployed) | 95+ | Wrangler |
| GitHub repos (16 orgs + personal) | 1,628 | GitHub API |
| Gitea repos (8 orgs) | 187 | Gitea API |
| Total repositories | **1,815** | Combined |
| Ollama models (fleet-wide) | 29 | Ollama API |
| Custom fine-tuned models | 4 | Modelfiles |
| Nginx sites (Alice) | 26 | sites-enabled |
| Systemd services (fleet) | 40+ | systemctl |
| PM2 processes (Alice) | 3 | pm2 list |
| Shell scripts (home dir) | 400+ | Filesystem |
| Docker configs | 100+ Dockerfiles | Filesystem |
| GitHub Actions workflows | 2,400+ | Repository scan |
| Custom domains | 48+ | DNS verified |
| KV namespaces | 40 | Cloudflare API |
| D1 databases | 7 | Cloudflare API |
| R2 buckets | 10 | Cloudflare API |
| Research papers | 30+ | Git repos |
| Mathematical proofs | 11 | simulation-theory |
| Reasoning agents | 10 | lucidia-core |
| Hailo-8 accelerators | 2 (52 TOPS) | Hardware |
| Quantum experiment datasets | 11 | NVMe |

---

## 15. Real Code with Tests (Verified)

Not everything is a scaffold. These repos have real implementations with test suites:

| Repo | Tests | Stack | What It Does |
|------|-------|-------|-------------|
| BlackRoad-OS-Inc/blackroad-core | 125 unit tests | TypeScript/Worker | Tokenless AI gateway — routes to Ollama/Anthropic/OpenAI, policy engine, fallback chains |
| BlackRoad-OS-Inc/blackroad-gateway | 20 tests | TypeScript/Worker | Trust boundary — auth, rate limiting, request validation |
| BlackRoad-OS-Inc/blackroad-api | 22 tests | Python/FastAPI | REST API — agents, tasks, memory, chat with Celery workers |
| BlackRoad-OS-Inc/blackroad-sf | Apex tests | Salesforce LWC | Agent__c, AgentTask__c, AgentMemory__c objects for 193+ agents |
| BlackRoad-AI/blackroad-ai-cluster | SQLite persistence | Python | Distributed inference — node registry, priority job scheduling, GPU affinity |
| BlackRoad-AI/blackroad-ai-memory-bridge | Unit tests | Python | Vector store — 64-dim embeddings, cosine similarity, episodic→semantic consolidation |
| BlackRoad-Security/blackroad-siem | 10 detection rules | Python (stdlib) | Event ingestion, correlation (brute-force/lateral movement), HTML reports |
| BlackRoad-Security/blackroad-identity-provider | Unit tests | Python (stdlib) | JWT HS256, PBKDF2 310K iterations, TOTP MFA, account lockout |
| BlackRoad-Interactive/blackroad-game-engine | Collision tests | Python | ECS architecture, MovementSystem, CollisionSystem, EventBus, SQLite save |
| BlackRoad-Interactive/blackroad-physics-engine | Simulation tests | Python | 2D rigid body, AABB collision, restitution, friction |
| BlackRoad-Labs/blackroad-ml-pipeline | Pipeline tests | Python | 5-stage ML pipeline: ingest→features→train→eval→deploy, rollback |

---

## 16. What BlackRoad OS Actually Is

BlackRoad OS is a **modular AI infrastructure platform** organized into 16 divisions:

| Division | Org | What It Handles |
|----------|-----|----------------|
| **Core Platform** | BlackRoad-OS (1,236 repos) | Workers, Pages, agents, dashboards, domain sites |
| **Corporate Engineering** | BlackRoad-OS-Inc (21 repos) | Tokenless gateway, API, CLI, SDK, Salesforce — the production core with tests |
| **AI Infrastructure** | BlackRoad-AI (16 repos) | Model gateway, cluster orchestration, memory bridge, inference optimization |
| **Security** | BlackRoad-Security (16 repos) | SIEM, threat intel, identity provider, encryption, incident response |
| **Hardware/IoT** | BlackRoad-Hardware (14 repos) | IoT gateway, sensor network, fleet tracking, firmware updates |
| **Business Tools** | BlackRoad-Foundation (14 repos) | CRM, HR, events, grants, analytics, ticketing |
| **Media** | BlackRoad-Media (13 repos) | Video transcoding, streaming, podcasts, newsletters, image optimization |
| **Creative/Interactive** | BlackRoad-Interactive (12 repos) | Game engine, physics engine, audio synthesis, 3D rendering |
| **Archive/Data** | BlackRoad-Archive (11 repos) | IPFS tracking, document archive, backups, web archiving |
| **ML Research** | BlackRoad-Labs (10 repos) | ML pipelines, feature store, experiment tracking |
| **Cloud-Native** | BlackRoad-Cloud (10 repos) | K8s operators, service mesh, Terraform, secrets management |
| **Governance** | BlackRoad-Gov (9 repos) | Compliance, digital identity, FOIA, policy tracking |
| **Enterprise Integrations** | Blackbox-Enterprises (8 repos) | Forks: n8n, Airbyte, Prefect, Temporal, Activepieces, Huginn |
| **Ventures** | BlackRoad-Ventures (8 repos) | Deal flow, portfolio tracking, LP portal, startup metrics |
| **Creative Studio** | BlackRoad-Studio (7 repos) | Color palettes, motion library, writing tools |
| **Education** | BlackRoad-Education (7 repos) | AI tutoring, adaptive quizzes, code challenges |
| **Personal/Research** | blackboxprogramming (108 repos) | Simulation theory, quantum lab, Lucidia, context-bridge |

**The physical infrastructure:**
1. **The edge** (Cloudflare) — 130+ Workers, 95+ Pages, 40 KV, 7 D1, 10 R2, 18 Tunnels handling all public traffic
2. **The gateway** (Alice Pi) — 48+ domains, 26 nginx sites, Pi-hole DNS, PostgreSQL, Qdrant
3. **The brain** (Cecilia Pi) — 16 Ollama models, Hailo-8 (26 TOPS), CECE API, MinIO, PostgreSQL
4. **The vault** (Octavia Pi) — 1TB NVMe, Gitea (187 repos), NATS messaging, Docker Swarm
5. **The identity layer** (PS-SHA-∞) — append-only hash-chained audit across all services
6. **The reasoning layer** (Lucidia) — 10 domain expert agents, math engines, quantum simulator
7. **The research layer** (simulation-theory) — 776 sections, 11 proofs, 50+ academic citations

**It is not one product. It is a platform for building products.** Each of the 16 divisions handles one concern. Each repo is a module. Each module is independently deployable. The platform grows by adding modules, not by scaling a monolith.

**1,815 repositories. 16 organizations. 6 compute nodes. 130+ Workers. 29 AI models. One person.**

The modularity is why that's possible.

---

*Verified from source code across 16 GitHub organizations (1,628 repos), 8 Gitea organizations (187 repos), 4 Raspberry Pi nodes, 2 cloud VPS, and 130+ Cloudflare Workers. March 9, 2026.*

---

*Copyright 2024-2026 BlackRoad OS, Inc. All Rights Reserved.*
