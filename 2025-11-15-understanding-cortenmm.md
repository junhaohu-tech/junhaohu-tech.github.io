---
layout: single
classes: wide
title: "CortenMM: Efficient Memory Management with Strong Correctness Guarantees — SOSP 2025 Paper Review"
date: 2025-11-15
categories: [paper-notes]
excerpt: "A review of CortenMM, a new OS memory manager that eliminates VMA bottlenecks and enables scalable, provably correct memory operations."
---

# 🔍 1. Overview

**Paper:** *CortenMM: Efficient Memory Management with Strong Correctness Guarantees*  
**Conference:** SOSP 2025  
**Authors:** Shanghai Jiao Tong University, Huawei  

CortenMM proposes a **new OS memory management architecture** that replaces Linux’s traditional **VMA (Virtual Memory Area)** abstraction with a lightweight, per-page entry model that simplifies correctness, avoids locking bottlenecks, and significantly improves multi-core scalability.  

This review follows the same format as the previous *Copier* analysis and focuses on:

- What problem CortenMM solves  
- Why existing OS architectures struggle  
- How CortenMM achieves correctness + performance  
- System architecture & mechanisms  
- Evaluation highlights  
- Q&A  
- Reflections  

---

# 🧠 2. Background & Motivation

Modern OSes (Linux, BSD, etc.) use a **dual-layer memory management model**:

1. **VMA layer (software abstraction)**
   - Interval-tree over contiguous virtual memory regions
   - Stores permissions, flags, mappings
   - Shared among threads of a process
   - Requires synchronization

2. **Page Table layer (hardware abstraction)**
   - Per-page mapping data used during address translation
   - Fine-grained and hierarchical, but opaque to OS-level policies

This split causes several issues.

## 🚧 **Problem 1 — VMA is a coarse-grained global structure**

Even when threads access *distinct* regions, they might:

- Modify or split the same VMA node  
- Access the VMA tree protected by **mmap_lock**
- Serialize operations that should be independent

This becomes a bottleneck for:

- Multi-threaded memory allocators  
- Large mmap workloads  
- JIT compilers  
- High-performance runtime systems  
- NUMA-aware applications  

## 🚧 **Problem 2 — VMA → Page Table translation is expensive**

Every mmap/munmap / CoW / mprotect / page fault must:

- Traverse VMAs  
- Then update page tables  
- Then flush TLBs  

The two-layer design adds complexity and latency.

## 🚧 **Problem 3 — Correctness reasoning is hard**

Correctness invariants must be maintained across:

- VMA interval tree  
- Multi-level page table  
- Shared vs private mappings  
- CoW and permissions  
- Kernel vs user operations  

CortenMM’s insight:

> **What if we eliminate the VMA layer entirely and make per-PTE metadata the only authoritative source of memory mappings?**

---

# 🏗️ 3. CortenMM Architecture

CortenMM introduces **a unified memory management model** based entirely on:

- Per-page metadata
- Directly associated with PTE structure
- Without VMA
- Fully concurrent
- Lock-light or lock-free
- Formally verified correctness properties

---

# 🔑 4. Key Concepts & Mechanisms

## 4.1 Per-PTE Metadata (The Heart of CortenMM)

Instead of storing mapping properties in large VMAs, CortenMM adds:

- Small (8-byte) metadata per PTE  
- Allocated *on demand*  
- Aligned with page table entries  
- No memory overhead for unmapped regions  
- Enables constant-time lookup  

Each PTE holds:

- permissions  
- sharing type  
- flags for operations  
- versioning for concurrency  
- state transitions  

This **collapses VMA and page table into a single layer**, improving both simplicity and performance.

---

## 4.2 Interval Skiplist for Fast Range Operations

Since VMAs are gone, the system still needs to support range-based operations such as:

- `mmap`
- `munmap`
- `mprotect`
- CoW initialization
- range invalidations

CortenMM uses a **Concurrent Interval Skiplist**, enabling:

- true parallel operations  
- range splitting and merging  
- O(log n) insertion and lookup  
- no global locks  
- scalable on multicore systems  

---

## 4.3 Strong Correctness Properties

The new model simplifies reasoning:

- Every byte of virtual memory is covered by exactly one metadata entry  
- No overlapping intervals  
- Permission invariants enforced per page  
- Proof-friendly design  

The paper includes a formal model ensuring correctness for:

- page faults  
- TLB shootdowns  
- concurrent operations  
- flag updates  
- resource reclamation  

---

# ⚙️ 5. Multi-Core Scalability Improvements

CortenMM eliminates the classic **mmap_lock bottleneck**.

### How Linux behaves today:
- Multiple threads calling mmap/munmap → forced into global lock contention
- Even disjoint memory regions require synchronization
- VMA tree operations scale poorly beyond 4–8 cores

### How CortenMM changes the game:
- No VMA tree → no global lock  
- Page-level metadata → only fine-grained locks  
- Per-range operations use skiplist with local synchronization  
- On multi-core (up to 64 cores), throughput grows nearly linearly  

---

# 📊 6. Evaluation Highlights

Measured against Linux:

| Workload | Improvement |
|---------|-------------|
| mmap-heavy multithread apps | **3.3× – 11.6× faster** |
| page-fault intensive workloads | **2× – 5× faster** |
| memory allocators (e.g., tcmalloc) | **20% – 60% faster** |
| JIT workloads | major latency reductions |
| general MMLRU / VM subsystem | significantly more parallel |

Memory overhead:

- On-demand metadata → **<2% overhead**
- Worst-case theoretical: ~2×  
- Real workloads: near Linux baseline  

---

# 💬 7. Q&A (from SOSP discussion & public commentary)

### **Q1: Does per-PTE metadata significantly increase memory usage?**

**A1:** No.  
Metadata is allocated **only for mapped pages**.  

- Real workloads: **<2% overhead**  
- Worst case: 2× (but almost never happens)  
- Future optimization: use unused bits in PTE to shrink size further  

---

### **Q2: Must the metadata be stored contiguously?**

**A2:** Logically yes, physically irrelevant.

The metadata array aligns with page table pages for easy indexing.  
Each entry is only **8 bytes**, so the physical requirement poses minimal overhead.

---

### **Q3: Why does Linux scale poorly with many cores in tests?**

**A3:** Because Linux still relies on the global **VMA tree**, protected by **mmap_lock**.

Even when threads operate on unrelated memory areas, they may need to:

- split the same VMA  
- modify flags  
- traverse shared tree nodes  

This unnecessary synchronization is the main source of poor scalability.

---

# 🧭 8. Reflection & Takeaways

### 🔹 **1. The VMA abstraction is outdated for modern multicore systems**
CortenMM shows that coarse-grained VMAs introduce:

- software complexity  
- unnecessary global synchronization  
- correctness pitfalls  

Eliminating VMAs simplifies both design and verification.

### 🔹 **2. Page-level metadata is a powerful unifying abstraction**
A single-layer model:

- makes correctness properties more obvious  
- removes costly translations  
- matches hardware semantics more closely  

It is “closer to the metal.”

### 🔹 **3. OS scalability problems often stem from old abstractions**
The paper follows a common theme seen in modern OS research:

> Remove legacy abstractions → Make fine-grained state the first-class entity.

Like Dune, Arrakis, FlexSC, and now Copier (previous review), CortenMM rethinks long-standing OS interfaces.

### 🔹 **4. A promising direction for next-generation OS memory systems**
CortenMM resembles the shift toward:

- per-object metadata  
- verifiable OS designs  
- concurrency-friendly structures  
- hardware-level alignment  

The design philosophy could influence:

- microkernels  
- unikernels  
- confidential computing VMMs  
- high-performance cloud runtimes  

---

# 📌 9. Summary

CortenMM re-architects OS memory management by:

- Removing VMAs  
- Using per-PTE metadata  
- Introducing a concurrent interval skiplist  
- Ensuring provable correctness  
- Achieving excellent multi-core scalability  

It is one of the most impactful VM subsystem papers in recent years and suggests a future where OSes abandon coarse, process-wide abstractions in favor of per-page, concurrent, verifiable designs.

# References
- SOSP 2025 Session 13 Papers:  
  https://zhuanlan.zhihu.com/column/c_1961542888350549465  
- ACM DOI: https://dl.acm.org/doi/10.1145/3731569.3764836
