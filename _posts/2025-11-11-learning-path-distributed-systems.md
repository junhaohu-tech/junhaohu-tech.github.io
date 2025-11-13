---
layout: single
classes: narrow
title: "Distributed Systems Learning Path"
date: 2025-11-11
categories: [learning-path]
excerpt: "A complete roadmap for learning distributed systems — from beginner fundamentals to advanced research and open-source engineering."
---

Building intuition and skill in **distributed systems** takes time.  
This roadmap is organized into three levels — **Beginner**, **Intermediate**, and **Advanced** — and is designed for students who eventually want to **do research** or **build real systems** in distributed systems and databases.

---

## 🧩 Big Picture

For the purposes of learning, we can roughly break the space into four areas:

- **Distributed storage** – file systems, databases, key–value stores, table stores  
- **Distributed computation** – large-scale data processing frameworks  
- **Distributed communication & coordination** – RPC, consensus, service coordination  
- **Distributed machine learning** – training and inference at scale  

A solid learning path touches all four, with progressively more depth.

---

## 🧠 Prerequisites

Before diving in:

- Be comfortable with at least one systems-oriented language:  
  **Go**, **Java**, **C++**, or **Rust** (Python is fine for scripting and prototyping).  
- Have basic background in:
  - operating systems (threads, processes, scheduling)  
  - networking (sockets, RPC, basic protocols)  
  - data structures and algorithms  

You **don’t** need to be an expert; you will reinforce these along the way.

---

## 🧱 Knowledge Map

### 1. Distributed Storage

**Architecture types**

- Central coordinator vs. decentralized peer-to-peer  
- Shared-nothing vs. shared-storage  

**System families**

- Distributed file systems  
- Distributed databases  
- Distributed key–value stores  
- Distributed table stores and column stores  

---

### 2. Distributed Computation

- Hadoop MapReduce  
- Apache Spark  
- Apache Flink  
- How they differ in:
  - execution model  
  - fault tolerance  
  - latency vs. throughput trade-offs  

---

### 3. Distributed Communication & Coordination

- RPC frameworks: gRPC, Thrift  
- Coordination systems: ZooKeeper, etcd  
- Consensus protocols: Raft, Paxos, Zab  

---

### 4. Distributed Machine Learning

- Spark MLlib  
- Parameter servers  
- Distributed TensorFlow / PyTorch  
- Data-parallel vs. model-parallel patterns  

You don’t need to master all of these at once. The rest of this roadmap gives a sequence of **concrete resources** to follow.

---

# 🧩 Beginner Level — CS Bachelor Level

Goal:  
> Build core intuition for **replication, consistency, and failure** while implementing small systems.

---

### 0. High-level overview

**Distributed Systems for Fun and Profit**  
- 🔗 <https://book.mixu.net/distsys/>  
- Short, highly readable book.  
- Explains key ideas like replication, consistency, and design trade-offs behind systems such as Dynamo, Bigtable, and Hadoop.  

---

### 1. Core course: MIT 6.824 Distributed Systems

- 📄 Course page: <https://pdos.csail.mit.edu/6.824/schedule.html>  
- 🎥 YouTube: <https://www.youtube.com/@6.824>  
- 🎥 Bilibili:  
  - <https://www.bilibili.com/video/BV1x7411M7Sf/>  
  - <https://www.bilibili.com/video/BV1CU4y1P7PE/>  

**Why this course**

- Teaches fundamentals through **Go** implementations.  
- Labs cover:
  - MapReduce
  - Raft consensus
  - Replicated key–value servers  
- You will **feel** how failures and timeouts behave in practice.

👉 If you’re new to Go:  
- Start with the official tour: <https://go.dev/tour/welcome/1>

---

### 1′. Optional: CMU 15-440 Distributed Systems

- 📄 Course homepage: <https://www.cs.cmu.edu/~dga/15-440/S14/>  

A solid undergraduate-level course.  
If you want a smoother ramp-up, one option is:  

> **15-440 → 6.824 → 15-712 (later at intermediate/advanced).**

---

### 1″. Conceptual complement: Cambridge Concurrent and Distributed Systems

- 📄 Course materials: <https://www.cl.cam.ac.uk/teaching/2021/ConcDisSys/materials.html>  
- 🎥 YouTube playlist: <https://www.youtube.com/watch?v=UEAMfLPZZhE&list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB>  

Taught by **Martin Kleppmann** (author of *Designing Data-Intensive Applications*).  
Great for developing a clean conceptual model of concurrency and communication.

---

### 2. Database fundamentals: CMU 15-445

**CMU 15-445 Intro to Database Systems**

- 🎥 YouTube: <https://www.youtube.com/watch?v=vdPALZ-GCfI&list=PLSE8ODhjZXjbj8BMuIrRcacnQh20hmY9g>  
- 🎥 Bilibili: <https://www.bilibili.com/video/av85655193/>  

Covers:

- storage and buffer pool  
- index structures  
- query execution  
- transactions and MVCC  

Labs use **bustub** in C++.  
If you are not a C++ user, you can still follow lectures and read the code at a higher level.

---

### 3. Core reading: Designing Data-Intensive Applications (DDIA)

**Designing Data-Intensive Applications** by Martin Kleppmann  

- Online copy: <https://ddia.qtmuniao.com/#/>  
- Slides (one example): <https://drive.google.com/file/d/1s2MfNjS5RH6DK0gXYMzVZKlzsnacZ6Uq/view>  

What you get:

- unified big-picture view of storage, streams, and batch/online systems  
- explanations for *why* systems like Kafka, Cassandra, and Spanner look the way they do  
- many concrete design trade-offs and real-world stories  

---

### 3′. Optional textbook: Distributed Systems – Concepts and Design

**Distributed Systems: Concepts and Design** (Coulouris et al.)  

- PDF example: <https://drive.google.com/file/d/19QgnFpwS5nJHzPC_weV_N9BoBACiC3oc/view>  

Contains a traditional textbook treatment of distributed systems.  
Good as a reference to clarify terms and models.

---

### 3″. Short notes: Notes on Distributed Systems for Young Bloods

- Article: <https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/>  

Short, practical notes on what makes distributed systems hard in real life.

---

# 🧱 Intermediate Level — Ready for Professional Work

Goal:  
> Connect coursework to **theoretical models** and **real research papers**.

---

### 1. Roadmap paper: Distributed Systems Theory for the Distributed Systems Engineer

- Article: <https://www.the-paper-trail.org/post/2014-08-09-distributed-systems-theory-for-the-distributed-systems-engineer/>  

This is a *paper roadmap* — it lists key results (CAP, FLP, Paxos, etc.) and tells you why they matter for engineers.

---

### 2. Advanced database systems: CMU 15-721

- Course site: <https://15721.courses.cs.cmu.edu/spring2020/schedule.html>  
- Bilibili: <https://www.bilibili.com/video/BV1VE411f7kP/>  

A graduate-level seminar focusing on:

- reading and presenting papers  
- understanding the design of modern database systems  
- thinking like a systems researcher

---

### 2′. Seminar-style DS course: Stanford CS244b

- Course site: <http://www.scs.stanford.edu/20sp-cs244b/>  

Discussion-based, centered around classic and modern distributed systems papers.  
Good for training your **paper reading + presentation** skills.

---

### 3. Textbook: Distributed Systems (Tanenbaum & van Steen)

- Book site: <https://www.distributed-systems.net/index.php/books/ds3/>  

Nine core chapters:

1. Introduction  
2. Architecture  
3. Processes  
4. Communication  
5. Naming  
6. Coordination  
7. Consistency  
8. Fault Tolerance  
9. Security  

Comes with figures and some Python examples.  
It’s a good “second textbook” once you already know the basics.

---

### 4. Practical storage book: Large-Scale Distributed Storage Systems

- PDF example: <https://drive.google.com/file/d/1f27FfjzHU9hmwgVKLnUo5LNj-8oOnH6e/view>  

Short and practical discussion of:

- different kinds of storage systems  
- their architectures  
- where the bottlenecks come from

---

### 4′. Theory deep dive (optional): Nancy Lynch – Distributed Algorithms

- PDF example: <https://drive.google.com/file/d/1h_cbTqzzhIek6qRpkhFLuTaCkqpiBDSF/view>  

Mathematically rigorous and challenging.  
Best approached once you already have some intuition about consensus and failures.

---

# 🧬 Advanced Level — Research & Top-tier Engineering

Goal:  
> Read and implement real systems; understand research problems and trade-offs at scale.

---

### 1. The “Google & Amazon” papers

These four papers are foundational:

- **GFS** – Google File System  
- **MapReduce** – programming model + runtime  
- **Bigtable** – wide-column storage  
- **Dynamo** – highly available key–value store  

Example links / collections:

- Overview article (Chinese): <https://blog.csdn.net/u011510825/article/details/122816587>  
- GFS: <https://drive.google.com/file/d/1xPIAG96AC--OpntxPymlMFOgbjOteL1W/view>  
- MapReduce: <https://drive.google.com/file/d/1km2uZSV0UFcdPsHUOI-rQCwJQxWBQdlw/view>  
- Bigtable: <https://drive.google.com/file/d/1161vjEDJ33tWqGcATGZ5xpoEkuwwGlwU/view>  
- Dynamo: <https://drive.google.com/file/d/1RHkdJEEUKYoCJ5_D6jMmkQvORQOVMpu0/view>  

---

### 2. Hands-on practicum: TiDB Talent Plan

**PingCAP TiDB Talent Plan**

- <https://tidb.net/talent-plan>  

Guided labs to implement components inspired by **TiDB/TiKV** using Go or Rust:

- storage engine  
- Raft-based replication  
- simple distributed transactions  

Perfect follow-up after MIT 6.824.

---

### 3. Curated reading lists

Some good collections:

- **Readings in Databases**: <https://github.com/rxin/db-readings>  
- **Awesome Distributed Systems**: <https://github.com/theanalyst/awesome-distributed-systems>  
- **Qix DS list**: <https://github.com/ty4z2008/Qix/blob/master/ds.md>  
- **ascrutae gist** (Chinese notes and links): <https://gist.github.com/ascrutae/7fbc3681ff6e7f68fc908e196eac980e>  

Pick a small set of papers at a time and **write notes** after reading.

---

### 4. Consensus & transactions papers

Examples worth reading (at least in summary):

- **Raft** – understandable consensus; read the paper and optionally the thesis  
- **ZooKeeper** – Zab protocol; coordination service design  
- **Multi-Paxos** – practical Paxos in repeated settings  
- **Percolator** – transaction layer on top of Bigtable  
- **Megastore** – partitioned stores with per-partition consensus  
- **Consensus on Transaction Commit** – Paxos-based two-phase commit  
- **A Note on Distributed Systems**: <https://citeseerx.ist.psu.edu/doc/10.1.1.41.7628>  
- **A Brief Tour of FLP Impossibility**: <https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/>  

For PhD applications, writing **paper notes** on a subset of these is excellent evidence of depth.

---

### 5. Open-source projects to study

You don’t need to understand every line of code. Start by tracing:

- the write path  
- the read path  
- how failures are detected and handled  

**Storage:**

- Hadoop: <https://github.com/apache/hadoop> (Java)  
- SeaweedFS: <https://github.com/seaweedfs/seaweedfs> (Go)  
- MinIO: <https://github.com/minio/minio> (Go)  
- TiDB: <https://github.com/pingcap/tidb> (Go)  

**Consensus & coordination:**

- etcd: <https://github.com/etcd-io/etcd> (Go)  
- ZooKeeper: <https://github.com/apache/zookeeper> (Java)  

**Computation:**

- Spark: <https://github.com/apache/spark> (Scala)  
- Flink: <https://github.com/apache/flink> (Java)  
- Ray: <https://github.com/ray-project/ray> (Python/C++)  

---

### 6. Reinforce prerequisites as needed

As you go deeper, it’s normal to circle back to:

- computer architecture  
- operating systems  
- networking  
- compilers / runtimes  

Distributed systems sit on top of all of these.

---

## 🧭 Summary

| Level          | Focus                                             | Main Goal                                      |
|----------------|---------------------------------------------------|-----------------------------------------------|
| **Beginner**   | Labs + core concepts (6.824, 15-445, DDIA)        | Build intuition and implementation skills     |
| **Intermediate** | Paper roadmaps and advanced courses            | Connect theory with real systems              |
| **Advanced**   | Classic papers, open-source, and research topics | Think and work like a systems researcher/engineer |

This roadmap is not meant to be followed rigidly.  
Pick a starting point that fits your background, **alternate between theory and implementation**, and write down what you learn — that’s how the knowledge becomes your own.
