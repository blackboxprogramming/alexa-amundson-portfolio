# Alexa Amundson

**Technical Founder & Systems Architect**

Lakeville, MN | amundsonalexa@gmail.com | (507) 828-0842 | [blackroad.io](https://blackroad.io) | [GitHub](https://github.com/blackboxprogramming)

---

## Summary

Technical founder who architected BlackRoad OS — a distributed AI orchestration platform spanning 6 compute nodes, 48+ domains, 130+ edge workers, 29 AI models, and a complete stack from task queues to observability to multi-agent coordination. Achieved 57-71% LLM cost reduction with sub-200ms latency. Previous career generating $23M+ in regulated financial services (FINRA Series 7/63/65), building the data pipelines, segmentation models, and compliance frameworks that drove results. Every layer of BlackRoad — networking, storage, compute, AI, monitoring, deployment — designed and operated solo.

---

## Technical Skills

**Architecture:** Distributed systems, microservice orchestration, five-layer AI ecosystem design, tokenless API gateway, capability-based agent routing, append-only journal systems, vectorized ethics framework, zero-trust compliance (OPA/Rego)

**Full Stack:** TypeScript/Node.js 22 (Hono, Zod, Next.js 15, React 19, Zustand), Python (FastAPI, LangGraph), Bash (400+ scripts), SQL, Excel VBA

**Infrastructure:** WireGuard mesh VPN, Cloudflare (130+ Workers, 95+ Pages, 40 KV, 7 D1, 10 R2, 18 Tunnels), Docker (100+ Dockerfiles, Swarm, Compose), Terraform, Ansible, Kubernetes, DigitalOcean

**AI/ML:** Ollama (29 models, 4 custom), Hailo-8 (52 TOPS), multi-provider gateway (Anthropic, OpenAI, Ollama, Gemini), Qdrant vector DB, RAG pipelines, TTS, computer vision (YOLOv5, ResNet)

**Financial:** FINRA compliance (2210, SEC 204-2), risk modeling, portfolio optimization, annuity product engineering, customer segmentation, CRM data governance (Salesforce)

---

## Experience

### Founder & Chief Architect — BlackRoad OS
*Remote | June 2024 – Present*
*(Founded concurrently with Securian role; transitioned to primary focus June 2025)*

Built an end-to-end distributed AI platform from scratch. Every component designed, coded, deployed, and operated solo.

#### The Architecture

Architected **five-layer AI ecosystem** (Interface → Orchestration → Data → Compliance → Compute) enabling deterministic multi-agent coordination across web, mobile, CLI, and edge devices. Directed **hybrid LLM deployment** (self-hosted Mistral 7B/Llama 3 + Claude fallbacks) achieving **57-71% inference cost reduction** ($1.0-1.3K vs $3.5K/mo) with sub-200ms latency.

#### The Platform (TypeScript)
- **blackroad-core**: Tokenless AI gateway (Hono + Zod) — agents never hold API keys, rate limiting (token bucket), policy engine (OPA/Rego), fallback chains, multi-provider adapters (Anthropic, OpenAI, Ollama, Gemini)
- **blackroad-agents**: 6 named agents with capability-based routing, Zod-validated schemas, and provider-specific access control
- **blackroad-operator**: `br` CLI for fleet management — status, invocation, deployment, logs, config
- **blackroad-web**: Next.js 15 dashboard (React 19, Zustand, Tailwind CSS 4) with real-time agent grid and glass-morphism UI
- **blackroad-infra**: Terraform modules (Cloudflare, DigitalOcean, Railway), Docker multi-stage builds, CI templates
- **blackroad-docs**: Architecture docs, ADRs, API specs, runbooks, governance, brand system

#### The Orchestration Layer
- **Task Queue** (472 lines, SQLite): 4 queue types (default 10K, inference 5K, batch 1K, priority 100), dead-letter routing, automatic retry, worker daemon with heartbeat
- **Autonomy Orchestrator** (246 lines): Task marketplace polling, priority routing, skill-based dispatch, zombie cleanup
- **Agent Registry** (297 lines): 11+ agents (hardware/AI/human), SSH testing, capability discovery, cryptographic identity
- **Multi-Agent System** (14 Python modules): 9 specialized roles, capability scoring, 30K-agent deployment design, peer-to-peer calling via SHA-256

#### The Observability Stack
- **Distributed Tracing** (487 lines): Trace ID propagation, span hierarchies, error rates, dependency graphs
- **Cost Tracking** (451 lines): Per-resource billing (CPU/GPU/inference/tokens/storage/network), budgets, invoices
- **Infrastructure Dashboard** (Python, 775 lines): 9-service monitoring, live logs, SQLite backend

#### The Compliance Layer
- **Zero-trust framework**: OPA/Rego policy enforcement, Keycloak SSO, tokenized PII handling for RIA/BD audit requirements and SEC Rule 204-2 alignment
- **Vectorized ethics**: Moral and compliance constraints embedded as high-dimensional vectors in policy evaluation with explainability
- **Immutable audit trails**: SHA-256 hash-chained append-only journals with data lineage tracking

#### The Network
- **WireGuard Mesh**: 6-peer hub-spoke (Anastasia hub → Alice, Cecilia, Octavia, Aria, Gematria)
- **Cloudflare Edge**: 130+ Workers, 95+ Pages, 40 KV, 7 D1, 10 R2, 18 Tunnels, 48+ domains
- **Pi Fleet**: Alice (gateway/DNS/PostgreSQL/Qdrant), Cecilia (16 AI models/Hailo-8/TTS/MinIO), Octavia (1TB NVMe/Gitea/NATS/Swarm), Aria (Portainer/Headscale)

#### The AI Fleet
- 29 Ollama models across 4 nodes (4 custom fine-tuned CECE models)
- 52 TOPS neural acceleration (2x Hailo-8)
- Computer vision (YOLOv5, ResNet) on edge NVMe
- TTS pipeline, CECE API Gateway, Qdrant vector DB
- Model endpoint manager with load balancing and token counting

#### The Automation
- 400+ shell scripts, 240+ Node.js projects, 100+ Dockerfiles, 35+ Compose configs
- Ansible (9+ roles), Terraform modules, GitHub Actions runners on Pi
- **1,326+ GitHub repos** (107 personal + 1,218 BlackRoad-OS org) + self-hosted Gitea (100+ repos, 7 orgs)

---

### Internal Annuity Wholesaler / Senior Sales Analyst — Securian Financial
*St. Paul, MN | July 2024 – June 2025*

- Generated **$23M+ in annuity sales** (92% of goal vs. 74% territory median)
- Engineered product optimization calculator analyzing 5+ annuity structures with scenario modeling across 5-7 year horizons
- Built **data quality pipeline** driving **75% of new lead generation** by identifying 50%+ CRM error rate
- Implemented **behavioral analytics** increasing advisor engagement **33% MoM**
- Architected CRM governance auditing **6,000+ Salesforce records** for FINRA 2210 compliance
- Selected as technical presenter at LPL Financial conferences and Securian Winter Sales Conferences (24,000+ advisor network)
- *Recognized that AI-enabled automation reduces human evaluation risk more effectively than manual compliance review — catalyzed full-time transition to AI systems architecture*

---

### Financial Advisor — Ameriprise Financial
*Minneapolis, MN | July 2023 – June 2024*

- Identified **$18.4M in convertible AUM** via Excel/VBA customer segmentation and cohort analysis
- Retained **$3.1M in at-risk assets** through quantitative reallocation strategies
- Ranked **#1 in qualified appointments**, 97% client satisfaction

---

### Licensed Real Estate Agent — eXp Realty
*White Bear Lake & Apple Valley, MN | May 2022 – December 2023*

- Built lead qualification and appraisal gap risk models
- Executed **1,000+ outreach touchpoints**, **10% conversion** to qualified leads

---

## Licenses & Certifications

- **FINRA:** SIE, Series 7, Series 63, Series 65 (CRD# 7794541)
- **Insurance:** Life & Health (Minnesota)
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

## What I Built — By the Numbers

| What | Scale |
|------|-------|
| Compute nodes | 6 (4 Pi + 2 cloud) |
| LLM cost reduction | 57-71% |
| AI models | 29 deployed, 4 custom fine-tuned |
| Neural compute | 52 TOPS (2x Hailo-8) |
| Edge Workers | 130+ |
| Pages projects | 95+ |
| Domains | 48+ |
| Git repos | 1,326+ GitHub + 100+ self-hosted Gitea |
| Shell scripts | 400+ |
| Node.js projects | 240+ |
| Docker configs | 100+ Dockerfiles, 35+ Compose |
| Agent modules | 14 Python, 9 roles |
| Services | 20+ persistent |
| Storage | 1.25TB+ distributed |
| Financial sales | $23M+ (Securian) |
| AUM identified | $18.4M (Ameriprise) |
