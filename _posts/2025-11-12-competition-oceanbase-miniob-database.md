---
layout: single
classes: wide
title: "OceanBase Database Competition (3rd Edition) — Analysis"
date: 2025-11-12
categories: [competitions]
excerpt: "A complete analysis of the OceanBase Database Competition (3rd Edition), including problem requirements, code implementation, architecture, and key design insights."
---

# 🏆 OceanBase Database Competition (3rd Edition) 

This article analyzes the OceanBase Database competition, a hands-on challenge that requires participants to build a miniature relational database from scratch. Unlike typical coding contests, this competition focuses on core DBMS architecture — including SQL parsing, execution, storage, and indexing.

---

# 1. Competition Overview

**Competition:** OceanBase Database Competition (3rd Edition)  
**Official page:** https://open.oceanbase.com/competition

The OceanBase Database Competition is part of the **National Collegiate Computer System Ability Challenge**, a nationwide contest for university students in China.  
It is jointly organized by the System Capability Training Expert Group, the National Computer Education Research Association, and leading universities, and hosted by OceanBase.

The competition aims to promote **technical excellence** in data-intensive systems and to bridge the gap between academic concepts and real-world distributed database engineering. Students compete in teams of **1–3 members**, and previous top-20 finalists cannot re-enter to ensure fairness.

The competition consists of **two stages**:

1. **Preliminary Round — MiniOB (a teaching-oriented relational database)**  
2. **Final Round — OceanBase (a production-grade distributed SQL database)**

---

# 2. Preliminary Round — MiniOB

The preliminary stage is built on **MiniOB**, a lightweight relational database engine designed for teaching DBMS fundamentals. It exposes essential components—SQL parsing, planning, execution, storage, and indexing—allowing contestants to extend real database internals.

---

## 2.1 Official Overview

The organizers provide a list of MiniOB tasks here:  
https://oceanbase.github.io/miniob/game/miniob_topics/

Topics include:

- SQL parsing (SELECT / INSERT / UPDATE / DELETE)  
- expression evaluation  
- metadata and catalog management  
- slotted-page layout  
- heap file implementation  
- B+ Tree indexing  
- iterator-based execution operators  

These tasks gradually guide contestants **from zero experience to implementing DBMS kernel modules**.

**Reference implementation:** https://github.com/S-1-T/miniob  

---

## 2.2 Technical Analysis of the Preliminary Round

### 1. System Architecture and SQL Pipeline

A SQL query flows through:

1. Parsing (Flex/Bison)  
2. Logical planning  
3. Physical planning  
4. Execution using a Volcano iterator model  
5. Storage engine (heap files + B+ Tree indexing)  
6. SEDA-based request scheduling  

MiniOB mirrors real RDBMS architecture while remaining small enough for students to extend.

---

### 2. SEDA Architecture

MiniOB uses a **staged event-driven architecture (SEDA)**:

- each stage has its own thread pool  
- events move through stages asynchronously  
- `done()` signals event completion  

This teaches non-blocking concurrency and modular performance design.

---

### 3. Engineering Challenges

#### • SQL Parser & Error Handling  
Distinguishing syntax errors from semantic errors, performing metadata validation, and returning correct error codes is essential.

#### • Metadata Management  
Operations like `DROP TABLE` reveal the importance of catalog consistency and error propagation.

#### • Extending the Type System  
Example: Adding a native `DATE` type requires modifying lexing, grammar, type checking, predicate evaluation, and storage.

#### • Index Behavior  
B+ Tree split logic, parent pointer updates, and page invariants must be correct to avoid silent corruption.

---

### 4. Debugging Lessons

Common pitfalls include:

- missing `break` statements  
- incorrect use of `%s`/`std::string`  
- metadata inconsistencies causing silent errors  
- SEDA events not calling `done()`  
- incorrect block layouts in indexes  

Debugging MiniOB teaches students how subsystems interact and how bugs propagate across the engine.

---

### 5. Skills Gained

- understanding DBMS architecture beyond textbooks  
- navigating unfamiliar codebases  
- enforcing storage and metadata invariants  
- debugging multicomponent systems  
- reasoning about correctness vs performance  

---

## 2.3 Recommended Development Notes

For readers who want a more narrative, step-by-step view of working with MiniOB, I highly recommend a four-part development diary written by Zheng Jinghong (郑璟泓). The series follows one student’s journey through the 2021 OceanBase Database Competition preliminary round and is very close to what a real beginner-to-intermediate MiniOB path looks like.

- **Part 1 – Environment & Framework**
- https://deepz.cc/2021/10/miniob-1/
  Covers setting up the development environment, cloning the official MiniOB repository, using VS Code with Remote SSH, and getting a high-level understanding of the MiniOB architecture and the “must-do” preliminary problems.   

- **Part 2 – SEDA Architecture**
- https://deepz.cc/2021/10/miniob-2/
  Explains how MiniOB uses a staged event-driven architecture (SEDA): requests are split into stages, each with its own thread pool and event queue, and each event must call `done()` when a stage finishes processing it. This is a great way to learn how MiniOB structures its request pipeline beyond a simple thread-per-connection model.   

- **Part 3 – Metadata Validation & Error Handling**
- https://deepz.cc/2021/10/miniod-3/
  Focuses on implementing `DROP TABLE` correctly, and on adding metadata validation so that querying a dropped or non-existent table returns a proper error instead of silently succeeding. The diary emphasizes that a DBMS must not only run valid queries, but also reject invalid ones with clear failure modes.   

- **Part 4 – Adding a `DATE` Type**
- https://deepz.cc/2021/10/miniob-4/
  Walks through extending MiniOB with a native `DATE` type: modifying the lexer and parser, storing `YYYY-MM-DD`, validating date strings, handling `WHERE` conditions on dates, and debugging subtle issues (such as using `%s` with `std::string` and a missing `break` in a switch). This part captures the “real feel” of database kernel development.   

These diaries complement this write-up: while this article focuses on the system-level structure of MiniOB, the diary shows what it is like to work through the competition tasks in practice—debugging, reading code, and gradually building confidence in database internals.


---

# 3. Final Round — OceanBase

The final round transitions from MiniOB to **OceanBase**, a distributed SQL database used in production at large financial institutions.

Contestants must design and implement a **bypass import (direct path load)** pipeline.

---
## 3.1 Official Overview of the Final Round

The final round transitions from the teaching-oriented MiniOB system to the **enterprise-grade distributed SQL database** OceanBase.  
Instead of extending a small relational engine, contestants work with real storage components and system-level APIs to implement a **bypass import (direct path load)** mechanism.

### Problem Background

OceanBase’s existing data import path relies on converting input text into large batches of `INSERT` statements. These statements must pass through:

- SQL parsing  
- semantic analysis  
- transaction management  
- logging  
- compaction triggers  

This results in a long execution path and limited import throughput.

Traditional DBMSs (e.g., Oracle, DB2) provide a “direct path load,” bypassing SQL and transactions and writing directly into **SSTable** storage files. OceanBase does not yet have this feature.

### Final Round Challenge

Contestants must design and implement a **high-performance bypass load pipeline** that:

1. Parses CSV files  
2. Converts rows into internal OceanBase data formats  
3. Writes data directly into SSTable blocks  
4. Achieves large performance improvements over batch INSERT  
5. Maintains correctness and storage invariants  

This requires deep understanding of OceanBase internals, distributed storage principles, and high-throughput data ingestion.

References:  
- https://zhuanlan.zhihu.com/p/617520132  
- https://zhuanlan.zhihu.com/p/677020265  
- https://open.oceanbase.com/blog/2325423616

---

## 3.2 Technical Analysis of the Final Round

The final round shifts from “implementing components” to “designing an end-to-end high-performance system.”  
Below is the analysis structured similarly to the preliminary round.

---

### 1. System Architecture and Data Ingestion Pipeline

A bypass-import pipeline typically follows this flow:

1. **Input Parsing**  
   - Read CSV / TSV input  
   - Validate schema, types, delimiters  
   - Map columns to table schema

2. **Row Conversion**  
   - Convert textual rows into OceanBase’s internal row representation  
   - Perform type casting, null checks, boundary validation  

3. **Partition Routing**  
   - Determine which partition / tablet each row belongs to  
   - Use partition keys and partition metadata  
   - Avoid data skew and cross-node traffic

4. **Block Building**  
   - Accumulate rows into **SSTable blocks / micro-blocks**  
   - Preserve sorting requirements (if any)  
   - Align data with storage engine’s merge and compaction expectations

5. **Direct SSTable Writing**  
   - Write macro-blocks to storage files  
   - Update metadata to expose new data to query engine  
   - Trigger compaction or rely on background merge

6. **Cluster Coordination**  
   - Handle replicas, network distribution  
   - Ensure consistency across multi-replica clusters  

This architecture mimics ingestion pipelines used in distributed warehouses such as ClickHouse, BigQuery, TiDB Lightning, and HDFS bulk loaders.

From a professor’s perspective, this stage demonstrates **systems-level design in a distributed database kernel**.

---

### 2. Core Engineering Challenges

#### • Challenge 1 — Partition-Aware Parallelism  
A large cluster may have dozens or hundreds of partitions.  
A high-performance loader must:

- detect target partition per row  
- group rows by partition  
- spawn per-partition worker threads  
- avoid cross-partition contention

**Key insight:**  
Parallelism is not “one big thread pool,” but **per-partition concurrency** to minimize lock contention and network hops.

#### • Challenge 2 — Conversion into Internal Formats  
OceanBase stores rows in tightly packed, serialized structures.  
Building these correctly requires:

- matching internal type layouts  
- nullability checks  
- byte-level serialization  
- maintaining memory alignment  

This is a direct test of “reading real-world database code” ability.

#### • Challenge 3 — Building SSTable Blocks Correctly  
Constructing valid SSTable files requires:

- correct macro-block structure  
- correct micro-block encoding  
- ascending row keys (if ordered)  
- correct checksum / metadata fields  
- compatibility with compaction engine  

A single wrong byte may corrupt the block or crash compaction.

This is where system engineers and professors focus heavily on **storage invariants**.

#### • Challenge 4 — IO Saturation & Throughput Optimization  
Key performance bottlenecks include:

- disk write throughput  
- network routing to servers  
- memory allocator overhead  
- thread synchronization  
- per-row conversion cost

Top teams achieved massive speedups by:

- using large batch sizes  
- minimizing per-row dynamic allocation  
- leveraging zero-copy parsing  
- tuning thread/partition scheduling  
- compressing blocks efficiently

#### • Challenge 5 — Correctness Under Parallelism  
Even though bypass-load skips SQL and transactions, correctness still matters:

- two worker threads cannot write conflicting blocks  
- blocks must not violate partition boundaries  
- metadata must reflect full import state  
- ingest must be idempotent (or at least predictable)  

This shows deep understanding of **distributed consistency and storage durability**.

---

### 3. Debugging Stories and Lessons Learned

Contestants frequently encounter real system-level bugs:

- **SSTable blocks unreadable** → due to row key mismatch or corrupted checksum  
- **Cluster shows partial data** → metadata updates incomplete  
- **Import stalls** → thread starvation or partition skew  
- **Data silently lost** → incorrect block offsets or buffer reuse bugs  
- **Merge crashes** → because ingest pipeline produced invalid storage layout  

The debugging mindset shifts from “fix my code” to **“validate invariants across multiple subsystems.”**

This is exactly the type of experience that PhD advisors value.

---

### 4. Skills Gained from the Final Round

The final round teaches a very different set of system skills than the MiniOB stage:

- **Distributed systems thinking**: partition routing, network paths, replica coordination  
- **Storage engine internals**: SSTable format, blocks, compaction, metadata  
- **High-performance ingestion design**: batching, parallel file writers, CPU–IO balancing  
- **Debugging in real kernels**: logs, hexdump, storage metadata inspection  
- **Architectural reasoning**: trade-offs between correctness, performance, and implementation complexity  

Overall, contestants move from “DBMS component-level understanding” to **real distributed database engineering**.

---

## 3.3 Recommended Reading & Write-ups

These write-ups provide high-quality insights into the final round challenge:

- **2022 Final Round Experience (very detailed analysis):**  
  https://zhuanlan.zhihu.com/p/617520132

- **2023 Final Round Solution Breakdown:**  
  https://zhuanlan.zhihu.com/p/677020265

- **Official OceanBase Final Round Blog:**  
  https://open.oceanbase.com/blog/2325423616

Together, they illustrate design trade-offs, bottlenecks, and real implementation strategies used by top-performing teams.

---

# 5. Reflection and Conclusion

Participating in both stages of the competition—from MiniOB to the full OceanBase kernel—provides a holistic view of database systems engineering.

### 1. From Components to Systems
MiniOB teaches component-level DBMS architecture.  
OceanBase requires designing system-level ingestion pipelines.

### 2. The Value of Invariants
Both stages emphasize metadata consistency, structured storage layout, and predictable error handling.

### 3. Engineering Realism
Contestants learn how real systems fail, how to debug multi-module issues, and how to reason about performance and correctness simultaneously.

### 4. What I Would Do Differently
- instrument earlier  
- adopt stronger testing strategies  
- define clearer module boundaries  
- profile before optimizing  

### 5. Closing Thoughts
This competition bridges academic foundations with industry-grade distributed systems.  
It provides a rigorous, hands-on understanding of database internals—from SQL to parser, storage, distributed ingestion, and performance engineering.

