# Junhao Hu — Notes on Distributed Systems, Databases, and Systems Research

This repository hosts my research-oriented technical blog, built on **academicpages**, where I document my readings, experiments, and reflections in **Distributed Systems**, **Database Systems**, and **Operating Systems**.  
The goal of this site is to build a long-term knowledge base that bridges theory, system design, and hands-on implementations.

---

## 🔍 Research Interests

My current research interests include:

### **Distributed Systems**
- consensus protocols (Raft, Multi-Paxos, Zab)
- strongly-consistent and weakly-consistent replication
- fault-tolerant storage systems
- distributed transaction models  
- cloud-native system design & observability

### **Database Systems**
- storage engine internals (B+ trees, LSM-trees, MVCC)
- query optimization and execution engine design
- distributed SQL & NewSQL systems (TiDB, CockroachDB, OceanBase)
- HTAP engine architecture
- performance optimization and cost-based scheduling

### **Systems & OS**
- memory management and virtual memory
- asynchronous I/O and zero-copy mechanisms
- OS–hardware co-design (DMA, SIMD, NIC offloading)
- kernel scalability & concurrency control

This site reflects my process of understanding how real-world systems are built, optimized, scaled, and reasoned about.

---

## 📘 Learning Paths

Structured guides synthesizing what I’ve learned from courses, papers, and production systems:

- **Distributed Systems Learning Path**  
- **Database Systems Learning Path**

These roadmaps integrate:
- foundational theory  
- canonical courses (MIT 6.824, CMU 15-445/721)  
- industry papers (Google, Amazon, Meta, Alibaba, PingCAP)  
- code reading from open-source systems  

---

## 📝 Research Paper Notes

Concise, structured reviews of papers from SOSP, OSDI, NSDI, VLDB, SIGMOD, and other top venues.

Recent notes include:

- *“How to Copy Memory? Coordinated Asynchronous Copy as a First-Class OS Service”* — SOSP 2025  
- *“CortenMM: Efficient Memory Management with Strong Correctness Guarantees”* — SOSP 2025  

Each note includes:
- motivation and problem framing  
- key challenges  
- new abstractions and system design  
- correctness arguments  
- evaluation takeaways  
- Q&A from conference presentations  
- my reflections  

---

## 🏆 Competition & System Project Write-Ups

I also analyze real-world system competitions as case studies in system design:

- **OceanBase Database Competition (MiniOB + OceanBase Kernel)**
- **Alibaba Middleware Performance Challenge**
- **TiDB Hackathon 2022**

These posts examine:
- system architecture  
- performance bottlenecks  
- data structure and scheduling decisions  
- trade-offs and lessons for system builders  

---

## 🧪 Ongoing Work

I am gradually building deeper notes on:
- MVCC implementation strategies  
- Raft log replication & leader election edge cases  
- B+ tree vs. LSM tree trade-offs  
- memory-copy offloading mechanisms  
- database benchmarking methodology  

Future posts will include code-level explorations of TiKV, OceanBase, and CockroachDB.

---

## 🧰 Tech Stack

- Built with **Jekyll** + **academicpages**
- Hosted on **GitHub Pages**
- Markdown + minimal custom SCSS
- Local development:

```bash
bundle install
bundle exec jekyll serve
