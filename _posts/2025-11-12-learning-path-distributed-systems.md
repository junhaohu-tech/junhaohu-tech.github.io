---
title: "Distributed Systems Learning Path"
date: 2025-11-12
categories: [learning-path]
excerpt: "A complete roadmap for learning distributed systems — from beginner fundamentals to advanced research and open-source engineering."
---

# 🌍 Distributed Systems Learning Path

## 🧩 Knowledge Structure

Distributed Systems can be divided into four broad categories:

### 1. Distributed Storage
**Architecture Types**
- Centralized control node architecture  
- Fully decentralized (peer-to-peer) architecture  

**System Types**
- Distributed File Systems  
- Distributed Databases (complex file systems)  
- Distributed Key-Value Systems  
- Distributed Table Systems  

### 2. Distributed Computation
- Hadoop MapReduce  
- Apache Spark  
- Apache Flink  
- Comparison: Hadoop vs Spark vs Flink  

### 3. Distributed Communication
- RPC frameworks (gRPC, Thrift)  
- Coordination systems (ZooKeeper, etcd, Raft)

### 4. Distributed Machine Learning
- Spark MLlib  
- Distributed TensorFlow

---

## 🧠 Prerequisite
- Be fluent in at least one programming language (Go, Python, Java, or C++)  

---

# 🧩 Beginner Level  — roughly CS-bachelor level

| # | Resource | Type / Focus | Link & Description |
|---|-----------|---------------|--------------------|
| 0 | **Distributed Systems for Fun and Profit** | Overview | [book.mixu.net/distsys](https://book.mixu.net/distsys/) — A short and highly readable book covering key ideas such as replication, consistency, and design trade-offs behind real-world systems like Dynamo, BigTable, and Hadoop. |
| 1 | **MIT 6.824 Distributed Systems** | Core Course | [Course page](https://pdos.csail.mit.edu/6.824/schedule.html) • [YouTube](https://www.youtube.com/@6.824) • [Bilibili 1](https://www.bilibili.com/video/BV1x7411M7Sf/) • [Bilibili 2](https://www.bilibili.com/video/BV1CU4y1P7PE/) — The most recommended course. Learn Go ([Go tour](https://go.dev/tour/welcome/1)) and implement labs from scratch: MapReduce, Raft (consensus), replicated key-value servers. |
| 1′ | **CMU 15-440 Distributed Systems** | Undergraduate Course | [Course homepage](https://www.cs.cmu.edu/~dga/15-440/S14/) — Great undergraduate-level introduction using Go. Recommended sequence: finish 15-440 first, then continue to 15-712. |
| 1″ | **Cambridge Concurrent and Distributed Systems** | Conceptual Overview | [Course materials](https://www.cl.cam.ac.uk/teaching/2021/ConcDisSys/materials.html) • [YouTube playlist](https://www.youtube.com/watch?v=UEAMfLPZZhE&list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB) — Taught by *Martin Kleppmann*, author of *DDIA*. |
| 2 | **CMU 15-445 Intro to Database Systems** | Databases | [YouTube](https://www.youtube.com/watch?v=vdPALZ-GCfI&list=PLSE8ODhjZXjbj8BMuIrRcacnQh20hmY9g) • [Bilibili](https://www.bilibili.com/video/av85655193/) — Learn about buffer pools, indexes, query optimization, transactions, and MVCC. Labs use **bustub** (C++). Focus on concepts if you’re not a C++ user. |
| 3 | **Designing Data-Intensive Applications (DDIA)** | Core Reading | [Online book](https://ddia.qtmuniao.com/#/) • [Slides](https://drive.google.com/file/d/1s2MfNjS5RH6DK0gXYMzVZKlzsnacZ6Uq/view) — Explains how and *why* systems like Kafka, Cassandra, and Spanner evolved. Ideal to read after MIT 6.824. |
| 3′ | **Distributed Systems: Concepts and Design** by Coulouris et al. | Optional | [PDF](https://drive.google.com/file/d/19QgnFpwS5nJHzPC_weV_N9BoBACiC3oc/view) — Textbook reference, good theoretical foundation. |
| 3″ | **Notes on Distributed Systems for Young Bloods** | Optional | [Article](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/) — Practical field notes; short and approachable. |


---

# 🧱 Intermediate Level  — ready for professional work

| # | Resource | Type / Focus | Link & Description |
|---|-----------|---------------|--------------------|
| 1 | **Distributed Systems Theory for the Distributed Systems Engineer** | Paper Roadmap | [Article](https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/) — Summarizes major theoretical results (CAP, FLP, Paxos) with pointers to seminal papers. Great bridge between coursework and research. |
| 2 | **CMU 15-721 Advanced Database Systems** | Graduate Course | [Course site](https://15721.courses.cs.cmu.edu/spring2020/schedule.html) • [Bilibili](https://www.bilibili.com/video/BV1VE411f7kP/) — Research-style seminar focusing on paper reading and system design. |
| 2′ | **Stanford CS244b Distributed Systems** | Seminar | [Course site](http://www.scs.stanford.edu/20sp-cs244b/) — Discussion-based course with classic papers and student presentations. |
| 3 | **Distributed Systems (3rd ed.) — Tanenbaum & van Steen** | Textbook | [Book site](https://www.distributed-systems.net/index.php/books/ds3/) — Nine chapters: Introduction, Architecture, Processes, Communication, Naming, Coordination, Consistency, Fault Tolerance, Security. Comes with Python examples and downloadable figures. |
| 4 | **Large-Scale Distributed Storage Systems: Principles and Architecture** | Practical Book | [PDF](https://drive.google.com/file/d/1f27FfjzHU9hmwgVKLnUo5LNj-8oOnH6e/view) — Explains high-level designs of different storage systems (file, KV, table) and their bottlenecks. Short and readable. |
| 4′ | **Nancy Lynch — Distributed Algorithms** | Optional Deep Theory | [PDF](https://drive.google.com/file/d/1h_cbTqzzhIek6qRpkhFLuTaCkqpiBDSF/view) — Theoretical foundation of distributed algorithms; mathematically rigorous and challenging but definitive. |


---

# 🧬 Advanced Level  — research / top-tier engineering

| # | Resource | Type / Focus | Link & Description |
|---|-----------|---------------|--------------------|
| 1 | **The Google & Amazon Papers** | Foundational Papers | [Summary article](https://blog.csdn.net/u011510825/article/details/122816587) • [GFS](https://drive.google.com/file/d/1xPIAG96AC--OpntxPymlMFOgbjOteL1W/view) • [MapReduce](https://drive.google.com/file/d/1km2uZSV0UFcdPsHUOI-rQCwJQxWBQdlw/view) • [Bigtable](https://drive.google.com/file/d/1161vjEDJ33tWqGcATGZ5xpoEkuwwGlwU/view) • [Dynamo](https://drive.google.com/file/d/1RHkdJEEUKYoCJ5_D6jMmkQvORQOVMpu0/view) — These four classics define modern distributed storage and computation. |
| 2 | **PingCAP TiDB Talent Plan** | Hands-On Practicum | [tidb.net/talent-plan](https://tidb.net/talent-plan) — Build a mini TiDB/TiKV following guided labs. Covers Go/Rust programming, consensus, and distributed database internals. Perfect next step after MIT 6.824. |
| 3 | **Paper Collections** | Reading Lists | - [Readings in Databases](https://github.com/rxin/db-readings)  <br> - [Awesome Distributed Systems](https://github.com/theanalyst/awesome-distributed-systems)  <br> - [Qix DS list](https://github.com/ty4z2008/Qix/blob/master/ds.md)  <br> - [ascrutae gist](https://gist.github.com/ascrutae/7fbc3681ff6e7f68fc908e196eac980e) |
| 4 | **Consensus & Transactions Papers** | Deep Dive | - *Raft*: simplified consensus design (see Diego Ongaro’s PhD thesis for full details)  <br> - *ZooKeeper*: introduces Zab consensus protocol  <br> - *Multi-Paxos*: optimization reducing network rounds  <br> - *Percolator*: transactional layer on Bigtable  <br> - *Megastore*: precursor to Spanner using Paxos  <br> - *Consensus on Transaction Commit* (Jim Gray & Lamport): Paxos-based 2-phase commit generalization  <br> - *A Note on Distributed Systems* ([paper](https://citeseerx.ist.psu.edu/doc/10.1.1.41.7628))  <br> - *A Brief Tour of FLP Impossibility* ([link](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)) |
| 5 | **Open-Source Projects to Study** | Code Reading | **Storage**: [Hadoop](https://github.com/apache/hadoop) (Java), [SeaweedFS](https://github.com/seaweedfs/seaweedfs) (Go), [MinIO](https://github.com/minio/minio) (Go), [TiDB](https://github.com/pingcap/tidb) (Go)  <br> **Consensus**: [etcd](https://github.com/etcd-io/etcd) (Go), [ZooKeeper](https://github.com/apache/zookeeper) (Java)  <br> **Computation**: [Spark](https://github.com/apache/spark) (Scala), [Flink](https://github.com/apache/flink) (Java), [Ray](https://github.com/ray-project/ray) (Python/C++) |
| 6 | **Prerequisite Reinforcement** | Background | Revisit computer architecture, OS, and networking fundamentals.|


---

# 🧭 Summary Table

| Level | Description | Goal |
|-------|--------------|------|
| **Beginner** | Learn key ideas — replication, consensus, MapReduce — through hands-on labs and core readings. | Build solid intuition and implementation skills. |
| **Intermediate** | Understand theoretical models and architecture trade-offs in real systems. | Read and discuss academic papers; connect design and theory. |
| **Advanced** | Implement or contribute to real-world distributed systems. | Achieve research-level or industry-grade expertise. |


---
