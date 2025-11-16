---
layout: single
classes: wide
title: "Copier: Rethinking Memory Copy as a First-Class OS Service — SOSP 2025 Paper Review"
date: 2025-11-15
categories: [paper-notes]
excerpt: "A concise overview of Copier, SOSP 2025’s asynchronous OS-level memory copy service."
---

# Paper Review  
**How to Copy Memory? Coordinated Asynchronous Copy as a First-Class OS Service**  
*Jingkai He, Yunpeng Dong, Dong Du (Shanghai Jiao Tong University);  
Mo Zou, Zhitai Yu, Yuxin Ren, Ning Jia (Huawei Technologies);  
Yubin Xia, Haibo Chen (Shanghai Jiao Tong University)*  
**SOSP 2025**

This review follows an 8-section structure tailored for distributed-systems and OS researchers, engineers, and students seeking to extract high-level ideas and design insights without reading the entire 20-page SOSP paper.

---

# 1. Why This Paper Matters — The Pain Point

In modern operating systems, **memory copy** remains a significant and persistent performance bottleneck.  
It directly impacts:

- system calls (e.g., `recv`, `send`)  
- IPC (Android Binder, pipes, shared memory)  
- high-I/O workloads (databases, KV stores, proxies, RPC servers)  
- memory-intensive applications (protobuf, video codecs, ML pipelines)

Even though memcpy looks trivial, **data movement dominates both CPU cycles and memory bandwidth**—often more than computation itself.

Existing solutions have limitations:

### **Zero-copy**
- requires page alignment  
- fails for multi-copy/CoW scenarios  
- unsafe (TOCTTOU risks)  
- only helps large chunks (Linux ≥10KB, zIO ≥16KB)

### **Hardware-accelerated copy (DMA / SIMD)**
- user applications cannot directly use DMA  
- kernel SIMD (AVX) suffers heavy state-save overhead  
- hard to schedule across subsystems  
- low global utilization

### **Synchronous memcpy**
- blocks the caller  
- cannot overlap communication and computation  
- cannot hide latency

---

# 2. Problem Formulation — What’s Wrong With Today’s OS Model?

Memory copy today is:

- **scattered** across the entire OS and application ecosystem  
- **synchronous**  
- **uncoordinated** (each subsystem uses its own memcpy path)  
- **unoptimizable** at global scale  
- **blind** to hardware heterogeneity (CPU SIMD, DMA engines)

As the authors summarize:

> “Copying is everywhere, but coordinated nowhere.”

The paper argues that memcpy must be elevated into **a first-class, OS-managed asynchronous service**, enabling:

1. **asynchrony** → overlap copy & compute  
2. **global optimization** → eliminate redundant copies  
3. **hardware orchestration** → jointly schedule CPU+DMA  
4. **cross-subsystem coordination** → networking, paging, IPC, and filesystems  

This leads to the core contribution: **Copier**.

---

# 3. Key Idea — Copier: Coordinated Asynchronous Copy as an OS Service

The central idea:

> **Turn memory copy into an OS-level asynchronous service**, similar to async I/O,  
> exposing a unified interface for both applications and kernel subsystems.

### Copier provides:
- asynchronous copy submission  
- dependency tracking  
- copy–use pipelining  
- unified hardware scheduling (AVX + DMA)  
- global copy absorption (eliminate unnecessary middle copies)  
- fairness, isolation, and cgroup integration  
- proactive fault-handling for page faults

### Why “coordinated” and “asynchronous” both matter?
- **asynchronous** makes copy overlap with compute  
- **coordinated** scheduling lets OS globally optimize and eliminate redundant tasks  
- **first-class service** means all subsystems share the same mechanism (network, FS, IPC, VM)

This is the fundamental conceptual advancement.

---

# 4. System Design

Copier’s architecture combines abstractions, dependency tracking, heterogeneous hardware scheduling, and OS-level management.  
Below are the essential components.

---

## 4.1 Copier Abstraction

### **Three queues**
- **Copy Queue** — asynchronous copy requests  
- **Sync Queue** — synchronous promotion via `csync()`  
- **Handler Queue** — completion callbacks  

### **Segment-based Copy**
Breaks large copies into **segments**, enabling:

- fine-grained state updates  
- early consumption of partially copied data  
- true copy–use pipelining  
- better concurrency under multi-client access

### **Task Promotion and `csync()`**
`csync()` allows a client to say: “I am about to use this data; promote required segments now.”

This solves:

- head-of-line blocking  
- dependency delays  
- unpredictable latency

---

## 4.2 Dependency Tracking

Two types of dependencies must be preserved:

### **Cross-Queue Barriers**
Using **syscall trap / return events** to reconcile order between user-mode and kernel-mode tasks.

### **Data Dependency Tracking**
Ensures correctness under reordering, e.g., when `csync()` accelerates segments.

---

## 4.3 Heterogeneous Hardware Coordination

Copier uses **task piggybacking**:

- large contiguous segments → DMA  
- small / misaligned segments → AVX2  
- DMA tasks *piggyback* onto AVX tasks to hide submission overhead  

This improves:

- bandwidth utilization  
- latency  
- CPU overhead  
- cache behavior

---

## 4.4 Copy Absorption (Global Redundancy Elimination)

Copier can eliminate intermediate copies such as:

Kernel Buffer → Intermediate Buffer → Destination

becoming:

Kernel Buffer → Destination

It achieves this via:

- segment descriptors  
- dependency tracking  
- **lazy copy tasks** (almost never executed; used for dependency absorption)

This is one of the biggest performance wins.

---

## 4.5 Multi-Client Service Management

### **Resource Isolation**
Extends cgroup to track copy length as the resource.

### **Fair Scheduling**
A CFS-like scheduler ensures fair bandwidth across clients.

### **Proactive Fault Handling**
Lock virtual → physical mappings *before* issuing copy, ensuring DMA never faults inside Copier context.

---

# 5. Evaluation — How Much Faster?

### **Macro Benchmarks**
- Up to **158% throughput improvement** through AVX2+DMA coordination  
- For OS services: significant latency reduction for  
  - `recv()` / `send()`  
  - Android Binder IPC  
  - CoW page-fault handling  

### **Application Workloads**
- **Redis latency ↓ up to 43.4%**  
- **TinyProxy throughput ↑ 7.2%–32.3%** via copy absorption  
- **Protobuf, OpenSSL, and video decoding** improved by 3%–33%

### **CPU Efficiency**
- Reduced cache pollution  
- Non-copy code CPI ↓ 4%–16%  
- Copy-as-a-service frees CPU pipeline for useful work  

The evaluation shows the design translates directly into real performance gains across multiple workloads.

---

# 6. Connections to Prior Work

The authors position Copier relative to prior approaches:

### **RowClone / DRAM-internal copy**
- requires hardware modification  
- not applicable to general workloads  

### **Zero-copy networking / filesystem**
- strict size & alignment requirements  
- unsafe for multi-copy scenarios  

### **Userspace DMA frameworks**
- privilege barriers  
- unsafe for general applications  

### **OS-as-a-service trends**
Similar to:
- TAS (TCP acceleration as a service)  
- FlexSC (async syscall)  
- io_uring (async I/O)  

### Copier’s uniqueness:
- works **without hardware changes**  
- coordinated across all OS subsystems  
- globally removes redundant copies  
- hybrid AVX+DMA scheduling  
- safe, correct, dependency-aware  

---

# 7. Key Takeaways for Systems Engineers & DB Developers

### **1. Treat data movement as a first-class resource**
Most high-performance systems are bottlenecked not by compute, but **by copy**.

### **2. Enable asynchronous, batched, segmented copy**
Databases (LSM-tree compaction, snapshotting, RPC serialization) can all benefit from this.

### **3. Provide a copy service in your system**
You can adopt a similar design:

- unified copy queue  
- async API  
- segment descriptors  
- dependency tracking  
- global optimization rules  

### **4. Hardware heterogeneity should be automatically managed**
Avoid exposing DMA/AVX details to application logic.

### **5. Think in terms of global optimization**
Redundant copies should be eliminated across system layers.

---

# 8. How to Read This Paper Efficiently

A recommended route:

### **If you are new to OS papers:**
1. Abstract  
2. Introduction & Motivation figures  
3. Architecture overview  
4. Evaluation summary  

### **If you build databases / RPC / runtimes:**
1. API & queue abstractions  
2. segment-based pipeline  
3. DMA+AVX piggyback scheduling  
4. copy absorption mechanism  

### **Checklist after reading**
You should be able to answer:

- Why is copy a bottleneck today?  
- What does an “OS-level asynchronous copy service” mean?  
- How does Copier maintain correctness under async reordering?  
- How are DMA and AVX scheduled cooperatively?  
- How is global copy absorption possible?  

---

# 9. Q&A (From SOSP 2025 Session Discussion)

### **Q1: Does Copier introduce more context-switch overhead?**  
**A1:** No. Copier runs as kernel background worker threads using shared queues.  
There are *no extra context switches*.

---

### **Q2: Linux kernel cannot freely use AVX instructions. Why can Copier?**  
**A2:**  
Copier batches multiple copy tasks and executes them continuously,  
avoiding frequent AVX state save/restore.  
Thus, it amortizes the cost and remains safe.

---

### **Q3: Why does Copier increase power consumption?**  
**A3:**  
Copier uses polling threads to minimize submission overhead.  
This improves throughput but increases energy usage.  
The authors optimized polling via a NAPI-like mechanism to reduce overhead.

---

# References
- SOSP 2025 Session 13 Papers:  
  https://zhuanlan.zhihu.com/column/c_1961542888350549465  
- ACM DOI: https://dl.acm.org/doi/10.1145/3731569.3764800
