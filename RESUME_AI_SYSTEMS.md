# Alexa Amundson

**AI Systems & Infrastructure Engineer**

Lakeville, MN | amundsonalexa@gmail.com | (507) 828-0842 | [blackroad.io](https://blackroad.io) | [GitHub](https://github.com/blackboxprogramming)

---

## Summary

AI infrastructure engineer who architected a hybrid LLM deployment achieving 57-71% cost reduction with sub-200ms latency, then built the distributed platform to run it — 29 models across 4 edge nodes, 52 TOPS of neural acceleration, a tokenless multi-provider gateway, and a 14-module multi-agent orchestration system with 9 specialized roles. Previous career generating $23M+ in financial services sales with FINRA Series 7/63/65 — bringing compliance-aware, quantitative thinking to AI systems design.

---

## Technical Skills

**AI/ML Infrastructure:** Ollama multi-node serving (29 models, 4 nodes), custom model fine-tuning (4 CECE models), Hailo-8 neural accelerators (52 TOPS), round-robin model load balancing, per-instance health tracking, token counting, batch management, RAG pipeline architecture, Qdrant vector DB

**Agent Systems:** Multi-agent orchestration (14 Python modules, 9 roles), capability-based task routing with scoring, peer-to-peer agent calling (SHA-256), wave-based deployment (30K agent scale), fallback chain execution, LangGraph

**AI Gateway:** Tokenless API gateway (Hono/Zod), multi-provider adapters (Anthropic, OpenAI, Ollama, Gemini), OPA/Rego policy enforcement, Keycloak SSO, token bucket rate limiting, vectorized ethics framework

**Distributed Systems:** Custom task queues (SQLite, dead-letter routing, visibility timeouts), real-time sync daemon (1ms polling, 100+ concurrent instances), WireGuard mesh VPN, Docker Swarm, NATS messaging

**Observability:** Distributed tracing (trace ID propagation, span hierarchies), Grafana, OpenTelemetry, per-service metrics, cost tracking engine, service dependency graphs

**Languages:** Python (FastAPI, LangGraph, agent systems), TypeScript/Node.js 22 (Hono, Zod), Bash (400+ scripts), SQL, Excel VBA

**Infrastructure:** Cloudflare (130+ Workers, 95+ Pages), Docker (100+ Dockerfiles), Terraform, Ansible, Kubernetes, Raspberry Pi fleet, DigitalOcean, 1,326+ GitHub repos

---

## Experience

### Founder & Chief Architect — BlackRoad OS
*Remote | June 2024 – Present*

#### Multi-Agent Orchestration System

- Architected **five-layer AI ecosystem** (Interface → Orchestration → Data → Compliance → Compute) enabling deterministic multi-agent coordination across web, mobile, CLI, and edge devices
- Built a **multi-model orchestration platform** with 14 Python agent modules implementing 9 specialized roles — Architect, Researcher, Coder, Reasoner, Mathematician, Coordinator, Memory Keeper, Quantum Analyst, Vision — with capability-based task routing
- Designed **deployment system** scaling from 0 to 30,000 agents across 15 organizations — wave-based deployment with per-org allocation (50-200 agents/repo) distributed by function
- Built **agent communication protocols**: peer-to-peer calling via SHA-256 hashing, broadcast channels, thread-based conversations, message persistence with read tracking
- Implemented **agent orchestrator** (Python) with mythology-themed naming (300+ names), capability assignment, and wave-based fleet deployment
- Configured **6 named AI agents** with provider-specific mappings and fallback chains:
  - Octavia (Architect): anthropic + openai, 8192 tokens
  - Lucidia (Dreamer): anthropic + ollama, 8192 tokens
  - Alice (Operator): ollama only, 4096 tokens
  - Cipher (Sentinel): anthropic only, 4096 tokens
  - Prism (Analyst): openai + ollama, 4096 tokens
  - Planner (Strategist): anthropic + openai + gemini, 16384 tokens

#### Hybrid LLM Deployment & Cost Optimization

- Directed **hybrid LLM deployment strategy** (self-hosted Mistral 7B/Llama 3 + Claude fallbacks) achieving **57-71% inference cost reduction** ($1.0-1.3K vs $3.5K monthly baseline) with sub-200ms latency validated through benchmarks
- Built the **tokenless AI gateway** (Node.js 22, Hono, Zod) — agents never hold API keys, all provider calls route through gateway with rate limiting (token bucket, 100 req/60s), policy engine, and typed error hierarchy
- Built **model endpoint manager** (462 lines) with multi-replica definitions, round-robin load balancing, per-instance health tracking, batch size management, timeout configuration, request logging with latency tracking, and token counting

#### Edge AI Compute (52 TOPS, 29 Models)

- Deployed **29 Ollama models across 4 edge nodes**: 16 on Cecilia (4 custom CECE fine-tunes), 9 on Octavia, 4 on Aria — models include qwen3:8b, llama3:8b, codellama:7b, phi3.5, gemma2:2b, deepseek-coder:1.3b, Apple OpenELM 1B/3B
- Integrated **2x Hailo-8 AI accelerators** (26 TOPS each = 52 TOPS) on Cecilia and Octavia — M.2 M-Key, firmware 4.23.0, hailort service
- Deployed pre-compiled vision models: **YOLOv5** (9MB) for object detection, **ResNet v1.50** (18MB) for image classification
- Built **CECE API Gateway** (Node.js, port 3100) with TTS API, monitoring API, and heartbeat service
- Designed **RAG infrastructure** using Qdrant vector DB with <100ms retrieval latency targets and 80% cache hit rate benchmarks

#### Compliance & Ethics Framework

- Implemented **zero-trust compliance framework** with OPA/Rego policy enforcement, Keycloak SSO, and tokenized PII handling designed for RIA/BD regulatory audit requirements and SEC Rule 204-2 alignment
- Pioneered **vectorized ethics framework** embedding moral and compliance constraints as high-dimensional vectors within policy evaluation — explainability mechanisms absent in pure LLM architectures
- Established **data lineage and immutable transaction logging** with deterministic audit trails via SHA-256 hash-chained journals

#### Real-Time AI Memory & Context

- Built **real-time memory sync daemon** (433 lines) enabling 100+ concurrent AI instances to share state — 1ms polling, lock-free JSONL, per-instance checkpoint tracking, dead-instance cleanup, broadcast TTL
- Built **memory API server** with REST/GraphQL endpoints, full-text indexing, knowledge extraction, graph visualization, session isolation
- Deployed **Qdrant vector database** on Alice for semantic search across agent memory

---

### Internal Annuity Wholesaler / Senior Sales Analyst — Securian Financial
*St. Paul, MN | July 2024 – June 2025*

- Generated **$23M+ in annuity sales** (92% of goal vs. 74% territory median) while building financial modeling infrastructure and behavioral analytics systems
- Engineered product optimization calculator analyzing 5+ annuity structures with inflation-adjusted return projections and scenario modeling across 5-7 year horizons
- Built **data quality pipeline** identifying 50%+ CRM error rate, driving **75% of new lead generation**
- Implemented **behavioral analytics** tracking email open rates and cross-referencing product interest signals, increasing advisor engagement **33% MoM**
- Architected CRM governance framework auditing **6,000+ Salesforce records** for FINRA 2210 compliance
- *Recognized that AI-enabled automation reduces human evaluation risk more effectively than manual compliance review — insight that catalyzed full-time transition to AI systems architecture*

---

### Financial Advisor — Ameriprise Financial
*Minneapolis, MN | July 2023 – June 2024*

- Identified **$18.4M in convertible AUM** through Excel/VBA customer segmentation models with cohort analysis
- Retained **$3.1M in at-risk assets** through quantitative risk-based reallocation
- Ranked **#1 in qualified appointments**, 97% client satisfaction

---

### Licensed Real Estate Agent — eXp Realty
*White Bear Lake & Apple Valley, MN | May 2022 – December 2023*

- Built lead qualification and appraisal gap risk models converting behavioral signals into pipeline
- Executed **1,000+ outreach touchpoints** achieving **10% conversion** to qualified leads

---

## Licenses & Certifications

- **FINRA:** SIE, Series 7, Series 63, Series 65 (CRD# 7794541)
- **Insurance:** Life & Health (Minnesota)
- **Trademarks (USPTO):** BLACKROAD OS, BLACKBOXPROGRAMMING, ALICE

---

## Education

**University of Minnesota — Twin Cities**
B.A., Strategic Communication: Advertising & Public Relations | December 2022

---

## Awards

National Speech & Debate Finalist | Securian Winter Sales Conference Presenter (2x) | LPL Due Diligence Presenter | Ameriprise Sales Training Award | Enterprise MSP Sales Award (3x)

---

## Scale

| Metric | Value |
|--------|-------|
| AI models deployed | 29 (4 custom fine-tuned) |
| Neural accelerator throughput | 52 TOPS (2x Hailo-8) |
| LLM cost reduction | 57-71% ($1.0-1.3K vs $3.5K/mo) |
| AI providers integrated | 4 (Anthropic, OpenAI, Ollama, Gemini) |
| Agent modules | 14 Python, 9 specialized roles |
| Agent scale design | 30,000 across 15 orgs |
| Concurrent AI instances (sync) | 100+ |
| Compute nodes | 6 (4 Pi + 2 cloud) |
| Edge infrastructure | 130+ Workers, 95+ Pages, 18 Tunnels |
