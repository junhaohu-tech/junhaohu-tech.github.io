---
layout: single
classes: wide
title: "Fifth Alibaba Middleware Performance Challenge — Analysis"
date: 2025-11-14
categories: [competitions]
excerpt: "A deep-dive analysis of the 5th Alibaba Middleware Performance Challenge, including problem structure, system-level constraints, performance metrics, and architectural implications for distributed middleware systems."
---

# 🏆 Fifth Alibaba Middleware Performance Challenge
A concise analysis of the 5th Alibaba Middleware Performance Challenge, covering the adaptive load-balancing task in the preliminary round and the in-process message storage engine in the final round.  
This article highlights core algorithms, system design trade-offs, and the practical engineering lessons learned from solving real-world middleware problems.

# 1. Competition Overview

**Competition:** Fifth Alibaba Middleware Performance Challenge  
**Organizer:** Alibaba Cloud / Tianchi Platform  
**Official Page:** https://tianchi.aliyun.com/competition/entrance/231714

The Alibaba Middleware Performance Challenge is a hands-on, engineering-intensive competition designed to simulate the realities of large-scale distributed middleware systems.  
Instead of algorithms or puzzles, this competition evaluates:

- high-performance system design  
- load balancing  
- correctness under concurrency  
- throughput / latency optimization  
- overload control  
- storage engine implementation  

Participants compete in two stages:

1. **Preliminary Round — Adaptive Load Balancing**  
2. **Final Round — Queue-based Message Storage Engine**

Both reflect real internal challenges faced inside Alibaba’s infrastructure.

---

# 2. Preliminary Round — Adaptive Load Balancing

**Challenge Page:**  
https://code.aliyun.com/middlewarerace2019/adaptive-loadbalance

**Task:** Implement a self-adaptive load balancing algorithm that automatically adapts to fluctuating provider capacity, minimizes response latency, and maximizes throughput.

Providers’ service capability fluctuates dynamically.  
The system must avoid overload, starvation, and collapse.

---

## 2.1 Requirements

### Gateway Responsibilities
- dynamically distribute traffic based on real-time backend performance  
- minimize RTT, maximize TPS  
- reject excessive traffic when the system is overloaded  

### Provider Responsibilities
- evaluate its own processing capacity  
- reject requests when overloaded  
- protect its latency and stability  

### System-level Behavior
If **incoming request rate > global capacity**,  
the **Gateway must proactively reject** requests (global backpressure).

---

## 2.2 Evaluation Procedure

PTS (Performance Testing Service) sends HTTP requests: 
PTS → Gateway → Provider → Response

Provider capacity changes across:

- overload  
- equilibrium  
- normal  
- partial degradation  

Performance Metrics:
- **Success Requests** (primary metric)  
- **Max TPS** (tie-breaker)  
- 1024 concurrent connections  
- 1-minute formal test  

---

# 3. Preliminary Round — Full Technical Analysis

This round is a true systems problem involving stability, noisy feedback, and dynamic latency behavior.  
We analyze it from three perspectives.

---

## 3.1 Professor / Research Perspective

This task is mathematically a **feedback control problem** where:

- provider service rates μ(t) are time-varying  
- latency L(t) is a delayed/noisy signal  
- naive greedy strategies oscillate and collapse  

Key principles required:

- EWMA smoothing to filter noisy latency  
- penalty mechanisms to avoid oscillation  
- global overload control  
- stability in feedback loops  
- routing strategies that avoid deterministic oscillation  

The problem structurally mirrors:

- congestion control  
- queueing systems  
- control theory  
- distributed service schedulers  

---

## 3.2 Systems Engineer Perspective

Production-grade load balancers must handle:

- jitter  
- nonlinear performance  
- sudden overload  
- partial failures  
- noisy measurements  
- need for early rejection  

The competition intentionally simulates these real-world issues.

Key engineering behaviors:

### A. Latency is the earliest, most reliable signal  
Latency increases before throughput drops → ideal for early detection.

### B. Penalizing slow providers prevents oscillation  
Immediate switching → thrashing → collapse.

### C. Global overload rejection is mandatory  
This mirrors circuit breaking in Envoy, Nginx, Kafka, etc.

### D. Provider must protect itself  
Reject requests if overloaded to preserve SLA.

---

## 3.3 Contestant Perspective

Common Wrong Strategies:

- static weights  
- round-robin  
- greedy lowest-latency  
- no global rejection  
- hard switching without smoothing  

Winning strategies:

- latency smoothing  
- penalty-based stability  
- capacity estimation  
- weighted random routing  
- global load shedding  

---

# ⭐ **3.4 High-Scoring Algorithm Architecture (Core Formulas)**

The following is the distilled architecture used by top-ranking teams and widely adopted in real-world distributed systems.

---

## **1. Provider Scoring Function**

Each provider is assigned a real-time score:
score_i = EWMA(latency_i) + penalty_i + failure_rate_factor

- **EWMA latency** smooths noise  
- **penalty** discourages routing to recently slow nodes  
- **failure rate factor** penalizes timeouts or rejections  

Lower score = better provider.

---

## **2. Provider Capacity Estimation**

Real-time capacity predicted as:
estimated_capacity_i = k / EWMA(latency_i)


This approximates each provider’s μ(t).  
Lower latency → more remaining capacity.

---

## **3. Global Overload Protection**

Before routing a new request:
if incoming_rate > Σ estimated_capacity_i:
shed requests


This prevents catastrophic collapse  
and reflects real-world backpressure mechanisms.

---

## **4. Routing Decision (Weighted Random)**

Providers are selected probabilistically:

- lower score → higher probability  
- avoids deterministic oscillation  
- stabilizes routing behavior  

---

## **5. Penalty Decay**

Penalties gradually decay:
penalty_i = penalty_i * 0.9

Recovered providers should return gradually to rotation.

---

# 3.5 Community Write-ups & Study Notes

These two excellent resources help deepen understanding:

### Practical Walkthrough  
https://tianchi.aliyun.com/notebook/66591

### Detailed Strategy Notes  
https://tianchi.aliyun.com/notebook/60036

They cover routing instability, latency behavior, workload patterns, and tuning strategies.

---

# 4. Final Round — Queue-Based Message Storage Engine

**Challenge Page:**  
https://code.aliyun.com/middlewarerace2019/mqrace2019

**Task:** Implement a persistent in-process message store supporting:

- append  
- time-window queries  
- time-window aggregation (sum/avg on field `a`)  

This problem is a scaled-down version of RocketMQ / Kafka log storage.

---

## 4.1 Requirements

- message format: `{a: int, t: timestamp}`  
- query `(t1, t2)`  
- support sum/avg in the same window  
- fully custom data layout allowed  

This is essentially a small **time-series database** problem.

---

# 4.2 Technical Analysis — Academic View

Conceptually involves:

- append-only log storage  
- timestamp-ordered indexing  
- prefix-sum acceleration  
- LSM-tree style layout  
- selective scanning via metadata  

---

# 4.3 Technical Analysis — Systems Engineer View

Key design components:

### A. Log Segment Format
[header][entries][metadata]

### B. Indexing
- segment-level min/max timestamps  
- per-entry or per-block timestamp arrays  
- binary search  

### C. Query Execution
1. find relevant segments  
2. binary search for boundaries  
3. scan subset  
4. compute sum/avg via prefix sums  

### D. Durability & Efficiency
- buffered writes vs fsync  
- mmap  
- contiguous memory layout  

---

# 4.4 Community Write-ups & Winner Architectures

### 🥇 Champion Solution  
https://tianchi.aliyun.com/notebook/77159

Highlights:
- multi-level segments  
- prefix sums  
- binary search + optimized scan  
- near zero-per-entry overhead  

---

### 🥈 Runner-up Solution  
https://tianchi.aliyun.com/forum/post/77949

Highlights:
- vector-based design  
- batch writing  
- clean binary search  

---

### 🥉 Third Place Solution  
https://tianchi.aliyun.com/notebook/77274

Highlights:
- dynamic segment splitting  
- timestamp-indexed arrays  
- simple, effective design  

---

# 4.5 Comparison of Top Solutions

| Aspect | Champion | Runner-up | Third Place |
|-------|----------|-----------|-------------|
| Write Path | Most optimized | Stable batch | Straightforward |
| Query Engine | Prefix sums + binary search | Binary search | Linear scan |
| Indexing | Multi-level | Minimal | Lightweight |
| Durability | Tuned | Balanced | Simple |
| Complexity | High | Medium | Low |

---

# 5. Reflection & Conclusion

This competition is an outstanding demonstration of *real systems engineering*, covering:

### **From the Preliminary Round:**
- feedback control in distributed systems  
- adaptive load balancing  
- oscillation prevention  
- global backpressure  
- EWMA & penalty mechanisms  
- dynamic capacity estimation  

### **From the Final Round:**
- log-structured storage design  
- timestamp indexing  
- streaming/analytical workloads  
- prefix-sum based acceleration  
- memory layout optimization  
- file segmenting & metadata design  

---

## Overall Reflection

The competition simulates real challenges encountered in:

- distributed systems  
- high-performance middleware  
- storage engines  
- database internals  

It trains participants to:

- reason about system behavior  
- design under uncertainty  
- balance trade-offs  
- engineer performant low-level components  

For anyone pursuing:

- a PhD in Systems / Databases / OS  
- a career in distributed infrastructure  
- storage engine engineering  
- performance optimization  

**this competition provides exceptional preparation and demonstrable technical depth.**

---

# 6. References

- Adaptive Load Balancer Repo  
  https://code.aliyun.com/middlewarerace2019/adaptive-loadbalance  

- Message Storage Engine Repo  
  https://code.aliyun.com/middlewarerace2019/mqrace2019  

- Preliminary Round Notes  
  https://tianchi.aliyun.com/notebook/66591  
  https://tianchi.aliyun.com/notebook/60036  

- Final Round Winner Write-ups  
  Champion: https://tianchi.aliyun.com/notebook/77159  
  Runner-up: https://tianchi.aliyun.com/forum/post/77949  
  Third Place: https://tianchi.aliyun.com/notebook/77274  










