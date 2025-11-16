---
layout: single
classes: wide
title: "TiDB Hackathon 2022  — Analysis"
date: 2025-11-15
categories: [competitions]
excerpt: "A high-level analysis of TiDB Hackathon 2022, examining its open-ended, innovation-driven challenges across TiDB/TiKV kernel development, distributed-systems engineering, and large-scale application design."
---

# 🏆 TiDB Hackathon 2022
A detailed analysis of TiDB Hackathon 2022’s Application Track, focusing on the top three award-winning projects—DataDance, Yunji, and Mirror. This write-up summarizes their architectures, design trade-offs, and engineering challenges from academic, system-engineering, and participant perspectives.

---

# 1. Competition Overview

**Competition:** TiDB Hackathon 2022  
**Track:** Application Group  
**Theme:** *Possibility at Scale*  
**Official page:** https://tidb.net/events/hackathon2022  

TiDB Hackathon is the flagship annual event organized by PingCAP and the TiDB open-source community.  
It is widely regarded as one of the most influential hackathons in the database and distributed-systems ecosystem.

The **2022 edition** brought together:

- **303 participants**  
- **86 teams**  
- engineers, researchers, database kernel developers, and open-source contributors  

The Application Track challenges participants to build **real-world, large-scale distributed applications** powered by TiDB’s HTAP capabilities, TiFlash, TiKV, and related ecosystem tools.

The hackathon requires teams to:

- Propose an idea (RFC)  
- Build a working prototype in 48 hours  
- Demonstrate scalability & real engineering depth  
- Deliver a polished live demo  

Evaluation criteria:

1. Innovation  
2. Engineering completeness  
3. Scalability  
4. Architectural soundness  
5. Community usefulness  
6. Demo quality  

---

# 2. The Winning Projects (Application Track)

This write-up examines three award-winning projects:

### 🥇 First Prize — **DataDance**  
https://github.com/datadance-fun/DataDance  

### 🥈 Second Prize — **Yunji**  
https://github.com/VelocityLight/yunji  

### 🥈 Second Prize — **Mirror**  
https://github.com/mirror-data/mirror  

---

# 3. Project Analyses

## 3.1 First Prize — DataDance  
**Goal:**  
A real-time data transformation platform (dbt-like + incremental ETL) built on TiDB.

### Key Features
- SQL-based workflow definitions  
- DAG transformation graph  
- Incremental recomputation with minimal deltas  
- TiCDC-triggered updates  
- Materialized outputs stored in TiDB/TiFlash  
- Web UI for pipeline visualization  

### Architecture Overview

User SQL → Parser → DAG Builder → Task Scheduler → Executors
↓ ↑
Metadata (TiDB)


### Strengths
- Very strong engineering completeness for a 48-hour hackathon  
- Clear real-world use case  
- Seamless integration with TiDB HTAP  
- Professional UI + strong system story  

---

## 3.2 Second Prize — Yunji  
https://github.com/VelocityLight/yunji  

**Goal:**  
A multi-tenant, cloud-native user analytics platform for real-time metrics & behavioral analysis.

### Key Features
- Multi-tenant isolation  
- Event ingestion → normalization → TiDB  
- Real-time user profile store  
- Metrics dashboard backed by TiFlash  
- Plugin-based analytics  

### Architecture Overview

Event Stream → Collector → Normalizer → TiDB
↓
Analytics API
↓
Dashboard / UI

### Strengths
- Clean schema design for event analytics  
- HTAP-aware architecture  
- Balanced full-stack system  
- High practical value  

---

## 3.3 Second Prize — Mirror  
https://github.com/mirror-data/mirror  

**Goal:**  
A developer-friendly visualization and introspection tool for TiDB clusters.

### Key Features
- Visual schema explorer  
- Column statistics & profiling  
- Query insight dashboards  
- Real-time table previews  
- ER diagram & dependency mapping  

### Architecture Overview

Frontend (React) → API Gateway → TiDB Information Schema / Stats
↓
Profiling & Insights Engine

### Strengths
- Excellent UX and presentation polish  
- Fills a needed gap in TiDB developer tooling  
- Lightweight, usable, easy to integrate  

---

# 4. Technical Analysis

## 4.1 Professor / Research Perspective
A researcher may focus on:

- HTAP execution patterns (TiDB + TiFlash)  
- DAG-based incremental view maintenance (DataDance)  
- Multi-tenant metadata modeling (Yunji)  
- Schema and stats introspection (Mirror)  
- Hybrid streaming/batch pipelines  
- Query pushdown and cost-based execution  

These projects represent modern directions in distributed SQL systems.

---

## 4.2 Systems Engineer Perspective

### Engineering topics of interest:
- Pipeline ingestion architectures  
- Metadata schema design  
- Consistency models for incremental pipelines  
- Avoiding full recomputation via deltas  
- TiFlash as analytical accelerator  
- Performance considerations under concurrency  

### Common design trade-offs:
- SQL vs DSL for workflow definitions  
- Metadata stored inside TiDB vs external store  
- Push vs pull data refresh  
- Horizontal scaling patterns  

---

## 4.3 Contestant Perspective  
Things a participant would care about:

- What project ideas are competitive?  
- What can be realistically built in 48 hours?  
- UI vs backend effort allocation  
- How much demo polish is needed?  
- What factors matter most to judges?  

### Observed Winning Patterns
- Clean, end-to-end user story  
- Practical use case deeply tied to TiDB  
- Strong visual demo  
- Real code, not just concepts  
- Architectural clarity  

---

# 5. High-Scoring Architecture Patterns

Across DataDance, Yunji, and Mirror, common patterns emerge:

### 1. TiDB as the primary metadata & transactional store  
Used for: schemas, pipeline state, configs, offsets, profiles.

### 2. TiFlash as analytical compute  
Used for: aggregations, segmentation, statistics, dashboards.

### 3. DAG or streaming execution models  
(DataDance and Yunji)

### 4. Strong frontend polish  
Yunji & Mirror show UI matters.

### 5. Cloud-native deployment  
Most teams used Docker to simplify demo.

---

# 6. Community Code References

### 🥇 DataDance (First Prize)  
https://github.com/datadance-fun/DataDance  

### 🥈 Yunji (Second Prize)  
https://github.com/VelocityLight/yunji  

### 🥈 Mirror (Second Prize)  
https://github.com/mirror-data/mirror  

These repositories serve as excellent references for:

- incremental computation  
- schema design  
- clean data ingestion pipelines  
- scalable HTAP architectures  
- developer-oriented tooling  

---

# 7. Reflection & Conclusion

TiDB Hackathon 2022 Application Track brings together innovation in:

- distributed SQL  
- HTAP workloads  
- real-time data transformations  
- cloud-native SaaS analytics  
- developer observability tools  

From a **research angle**, these projects demonstrate practical prototypes of active research areas such as incremental view maintenance, hybrid streaming workloads, and HTAP optimization.

From a **systems-engineering angle**, they illustrate clean architecture boundaries, correct use of TiDB/TiFlash, and realistic distributed workload patterns.

From a **contestant angle**, the lesson is clear:

> **The strongest projects do not reinvent TiDB—they extend it in impactful, meaningful, and highly usable ways.**

TiDB Hackathon remains a unique environment where distributed systems innovation meets real-world engineering constraints, producing ideas and prototypes that often evolve into community contributions.


