# Alexa Amundson

**Platform & Infrastructure Engineer**

Lakeville, MN | amundsonalexa@gmail.com | (507) 828-0842 | [blackroad.io](https://blackroad.io) | [GitHub](https://github.com/blackboxprogramming)

---

## Summary

Platform engineer who designed and single-handedly built a 6-node distributed compute mesh spanning Raspberry Pis, cloud VPS, and Cloudflare edge — orchestrated by custom task queues, a real-time sync daemon, distributed tracing, and fleet management across 130+ Workers, 95+ Pages projects, 48+ domains, and 29 AI models. Previous career in regulated financial services (FINRA Series 7/63/65) generating $23M+ in sales with data pipeline engineering that drove 75% of lead generation. Brings both production infrastructure skills and compliance-aware, data-driven execution from high-stakes environments.

---

## Technical Skills

**Distributed Systems:** Custom distributed task queues (SQLite-backed, 4 queue types, dead-letter routing, visibility timeouts, automatic retry with backoff), append-only journal replication (SHA-256 chained JSONL), lock-free concurrent sync (1ms polling, checkpoint tracking), heartbeat-based failure detection, NATS messaging, Docker Swarm

**Cloud & Edge:** Cloudflare (130+ Workers, 95+ Pages, 40 KV, 7 D1, 10 R2, 18 Tunnels), DigitalOcean (DOKS, VPC, Managed PostgreSQL), Raspberry Pi fleet (4 active nodes), Hailo-8 neural accelerators (52 TOPS)

**Orchestration & IaC:** Terraform (Cloudflare, DigitalOcean, Railway modules), Ansible (9+ roles), Docker (100+ Dockerfiles, 35+ Compose, multi-stage builds), Docker Swarm, Portainer, PM2, systemd, GitHub Actions (self-hosted runners on Pi), Gitea (100+ repos, 7 orgs)

**Observability:** Custom distributed tracing (trace ID propagation, parent-child span hierarchies, service dependency graphs), per-service metrics aggregation, full-text log search, cost tracking engine (per-resource billing with budget enforcement), InfluxDB, Grafana, OpenTelemetry, Node Exporter

**Networking:** WireGuard mesh VPN (6-peer hub-spoke), Pi-hole DNS, Nginx, Caddy, SSH ProxyJump, Tailscale/Headscale, Cloudflare Tunnels (Zero Trust), NFS, WebSockets

**Languages:** Bash (400+ production scripts), Python (FastAPI, agent systems), TypeScript/Node.js 22 (Hono, Zod), SQL, Excel VBA

**Databases:** PostgreSQL, SQLite, InfluxDB, Qdrant (vector DB), MinIO S3, Redis, Cloudflare D1/KV/R2

**Security:** Tokenless API gateway, OPA/Rego policy enforcement, Keycloak SSO, Zero Trust tunnels, SHA-256 journal chaining, cryptographic agent identity

---

## Experience

### Founder & Chief Architect — BlackRoad OS
*Remote | June 2024 – Present*

#### Fleet Orchestration & Mesh Networking

Designed and operate a 6-node hybrid compute mesh (4 Raspberry Pis + 2 DigitalOcean VPS) connected via WireGuard VPN with hub-spoke topology, serving 48+ domains through Cloudflare edge infrastructure.

- Built a **distributed task queue** (472 lines, SQLite-backed) with 4 queue types — default (10K capacity, 30s visibility timeout, 24h retention), inference (5K, 5m timeout), batch (1K, 10m timeout), priority (100, 60s timeout) — dead-letter routing after max retries, worker daemon mode with heartbeat tracking, and automatic retry with backoff
- Built a **task orchestrator daemon** (246 lines) continuously polling a task marketplace, routing by priority (urgent/high/medium/low), matching tasks to runners by skill, dispatching to GitHub Actions across the fleet, and cleaning up zombie agents on timeout
- Built an **agent registry** (297 lines) managing 11+ agents across 3 types (hardware nodes, AI models, human operators) with SSH connectivity testing, capability discovery, cryptographic identity, and bulk fleet health checks
- Engineered **mesh replication** with selective sync — full memory to local Pis, journal-only to cloud for bandwidth optimization — SSH health checks (2-5s timeout), bidirectional rsync, atomic append-only writes
- Operate 20+ persistent services across the mesh via PM2, systemd, and Docker

#### Cloudflare Edge Platform

- Deployed and manage **130+ Cloudflare Workers**, **95+ Pages projects**, **40 KV namespaces**, **7 D1 databases**, **10 R2 buckets**, and **18 tunnels** routing 48+ domains
- Workers handle API routing, PDF processing, metrics collection, memory systems, and per-node infrastructure management
- Built multi-domain deployment pipeline deploying 16+ sites (static + Next.js) to Pages via Wrangler CLI

#### Observability & Cost Management

- Built a **distributed tracing system** (487 lines) with trace ID propagation, parent-child span hierarchies, millisecond-precision duration tracking, per-service error rate analysis, full-text log search, service dependency graph generation, and configurable retention
- Built a **cost tracking engine** (451 lines) with per-resource billing — CPU ($0.001/hr), GPU/Hailo ($0.01/hr), inference ($0.0001/request), tokens ($0.000001/token), storage, network — per-project budgets with 80% alert thresholds and invoice generation
- Deployed InfluxDB and Node Exporter across fleet for time-series metrics

#### Infrastructure as Code & Containers

- Wrote **Terraform modules** for Cloudflare Workers, Cloudflare Pages, Railway, and DigitalOcean with firewall rules and SSH key management
- Maintain **100+ Dockerfiles** (multi-stage, Node 22 Alpine) and **35+ docker-compose configs** across production/staging/dev
- Operate Docker Swarm cluster with Portainer dashboard; maintain **1,326+ GitHub repos** (107 personal + 1,218 in BlackRoad-OS org) plus self-hosted Gitea (100+ repos, 7 orgs) with GitHub Actions runners on Pi nodes

#### Edge AI & Hardware

- Deployed **29 Ollama models across 4 nodes** (4 custom fine-tuned) with model load balancing, health tracking, and token counting
- Integrated **2x Hailo-8 AI accelerators** (52 TOPS combined) for edge neural inference (YOLOv5, ResNet)
- Configured 4 Pis with role-specific workloads: Alice (gateway/DNS/PostgreSQL/Qdrant/48 domains), Cecilia (16 AI models/Hailo-8/TTS/MinIO), Octavia (1TB NVMe/Gitea/NATS/Docker Swarm), Aria (Portainer/Headscale/audio)

#### Real-Time Distributed Sync

- Built a **memory sync daemon** (433 lines) enabling 100+ concurrent AI instances to share state — 1ms polling, lock-free JSONL, per-instance checkpoint tracking, dead-instance cleanup, broadcast TTL management
- Designed **PS-SHA journal architecture** with SHA-256 hash chaining for audit trails, real-time sync, and crash recovery

---

### Internal Annuity Wholesaler / Senior Sales Analyst — Securian Financial
*St. Paul, MN | July 2024 – June 2025*

- Generated **$23M+ in annuity sales** (92% of annual goal vs. 74% territory median) while architecting financial modeling infrastructure and data quality pipelines
- Engineered Excel-based product optimization calculator analyzing 5+ annuity structures with inflation-adjusted return projections and scenario modeling across 5-7 year horizons, updated every 15 days tracking rate changes
- Built **data quality pipeline** identifying 50%+ CRM error rate through email bounce analysis, personally validating records to drive **75% of new lead generation**
- Implemented behavioral analytics tracking email open rates and cross-referencing product interest signals, increasing advisor engagement **33% month-over-month**
- Architected CRM data governance framework auditing **6,000+ Salesforce records** for FINRA 2210 compliance
- Selected as technical product analysis presenter at LPL Financial conferences and Securian Winter Sales Conferences, representing Securian to 24,000+ advisor network

---

### Financial Advisor — Ameriprise Financial
*Minneapolis, MN | July 2023 – June 2024*

- Identified **$18.4M in convertible AUM** by building Excel-based customer segmentation models with VBA macros targeting high-LTV prospects through cohort analysis
- Built macro-driven pipeline visualization dashboards analyzing age cohorts and asset brackets to prioritize conversion opportunities
- Retained **$3.1M in at-risk assets** through risk-based reallocation strategies guided by quantitative modeling
- Ranked **#1 in qualified appointments** achieving 97% client satisfaction

---

### Licensed Real Estate Agent / Investment Analyst — eXp Realty
*White Bear Lake & Apple Valley, MN | May 2022 – December 2023*

- Built lead qualification system analyzing click-rate patterns to identify buyer intent, converting behavioral signals into pre-approval pipeline
- Developed appraisal gap risk models comparing CMAs against financing constraints (FHA restrictions, bridge capital requirements) to eliminate unviable deals
- Executed **1,000+ outreach touchpoints** achieving **10% conversion** to qualified leads

---

## Licenses & Certifications

- **FINRA:** SIE, Series 7, Series 63, Series 65 (CRD# 7794541, held through Securian Financial 2023-2025)
- **Insurance:** Life & Health (Minnesota)
- **Real Estate:** Minnesota Real Estate License (2022-2023)
- **Trademarks (USPTO):** BLACKROAD OS, BLACKBOXPROGRAMMING, ALICE

---

## Education

**University of Minnesota — Twin Cities**
B.A., Strategic Communication: Advertising & Public Relations | December 2022
Relevant Coursework: Real Estate Finance, Risk Management, Leadership, Project Management

---

## Awards

National Speech & Debate Finalist | Securian Winter Sales Conference Presenter (2x) | LPL Due Diligence Presenter | Ameriprise Sales Training Award | Enterprise MSP Sales Award (3x)

---

## Infrastructure Summary

| Layer | Scale |
|-------|-------|
| Edge | 130+ Workers, 95+ Pages, 40 KV, 7 D1, 10 R2, 18 Tunnels |
| Compute | 4 Pis + 2 VPS (6 nodes) |
| AI | 29 models, 52 TOPS (2x Hailo-8), Qdrant vector DB |
| Storage | 1.25TB+ distributed |
| Network | WireGuard mesh (6 peers), 48+ domains, Pi-hole DNS |
| Code | 400+ scripts, 240+ Node.js projects, 100+ Dockerfiles |
| Git | 1,326+ GitHub repos + 100+ self-hosted Gitea |
| Queues | 16,100 message capacity / 4 types |
| Services | 20+ persistent (PM2, systemd, Docker) |
