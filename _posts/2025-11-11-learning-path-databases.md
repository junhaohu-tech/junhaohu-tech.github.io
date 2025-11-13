---
layout: single
classes: wide
title: "Database Learning Path"
date: 2025-11-11
categories: [learning-path]
excerpt: "A structured roadmap for learning databases—from SQL fundamentals to relational engines, NoSQL systems, and advanced internals and research topics."
---

This learning path is for students who want to go beyond “I know SQL” and
build a **system-level understanding of databases** — enough to:

- design schemas and write correct, efficient queries  
- understand how relational engines and storage engines work  
- use NoSQL systems such as MongoDB and Redis appropriately  
- connect database concepts with **distributed systems** and modern data platforms  

The roadmap is divided into three levels:

- **Beginner** 
- **Intermediate** 
- **Advanced** 

---

## 1. What exactly is a “database”?

Before diving into resources, it's helpful to clarify some overloaded terms.

### SQL vs. MySQL

- **SQL** is a *language* (Structured Query Language).  
- **MySQL** is a relational **database management system (DBMS)** that uses SQL.

---

### MySQL vs. “database”

MySQL is one specific **relational** database system.  
Other relational engines include PostgreSQL, Oracle, SQL Server, etc.

---

### SQL vs. SQL databases

- **SQL** is the language.  
- A **SQL database** is any relational engine that uses SQL (e.g., MySQL, PostgreSQL, SQLite).

---

### SQL databases vs. NoSQL databases

- **SQL databases:** relational, schema-based, strong consistency by default.  
- **NoSQL databases:** document, key–value, column-family, graph models; more flexible or scalable.

NoSQL is not a “replacement” for SQL — it is a complementary model.  
Choose based on workload and consistency requirements.

---

### MySQL vs. PostgreSQL

Both are relational databases:

- **MySQL**: widely used, strong for read-heavy workloads.  
- **PostgreSQL**: object-relational, highly extensible, standards-compliant.

Either is fine for learning.

---

## 2. What we want to learn

A complete database learning path spans multiple systems:

- **SQL language**
- **Relational engines** (MySQL, PostgreSQL)
- **Document stores** (MongoDB)
- **In-memory data structures** (Redis)
- **Distributed systems foundations**

---

## Prerequisites

- Basic programming experience (Python, Java, Go, C++, etc.)  
- Command-line familiarity  
- Understanding of basic data structures  

---

# 🔰 Beginner Level — CS Bachelor Level

**Goal:**  
Learn SQL basics, relational databases, NoSQL overview, and get hands-on experience.

---

### 1. Learn SQL basics

**Resources**

- W3Schools SQL: <https://www.w3schools.com/sql/>  
- Tutorial mirror 1: <https://www.w3schools.cn/sql/>  
- Tutorial mirror 2: <https://www.runoob.com/sql/sql-tutorial.html>  

Recommended practice after completing basics:  
Install MySQL locally and practice queries.

---

### 2. Introductory SQL books

Choose **one**:

#### (A) *SQL Essentials* / *MySQL Essentials*

Example download (one edition):  
<https://drive.google.com/file/d/1QJv4JF4DWJdO7V_ZxWLzhis4yDuDxvno/view>

Topics covered:

- SELECT queries  
- JOINs and aggregations  
- schema design  
- inserts/updates/deletes  
- user and permission basics  

Workflow: read → type every query → run it.

#### (B) *SQL Basics (Beginner’s Tutorial)*

Example link:  
<https://drive.google.com/file/d/12I-7TYkq8rGxjyeCOV_NX2r0sR3LxGht/view>

Uses PostgreSQL; highly visual, very beginner-friendly.

---

### 3. MongoDB: Learn with MongoDB University

- <https://learn.mongodb.com/>

This is MongoDB’s official training platform.  
Take **MongoDB for Developers** (7-week pace):

You will learn:

- installation & basic CRUD  
- data modeling  
- building a blog application  
- assignments + quizzes each week  

Optional deeper reading:

**MongoDB: The Definitive Guide**  
<https://drive.google.com/file/d/1CgQL2LjAOvAM8g6aVoY4_d5pWx9A5XL3/view>

---

### 4. General database theory

Read selected chapters (not cover-to-cover) from:

**Database System Concepts**  
<https://drive.google.com/file/d/11rIdRiVYrFChEiqmYCuVpN6A4wgTErMw/view>

Focus on:

- relational model  
- storage & indexing  
- transactions  
- DB architecture

---

### 5. Redis basics

Pick **one**:

- **Redis Beginner’s Guide**  
  <https://drive.google.com/file/d/1aavGDWBzlZwbwDciAe8VLBisW5yJ9eG4/view>

- **Redis Handbook**  
  (overview of data types, operations, usage patterns)

Optional video course:  
Redis 6 from Beginner to Advanced  
<https://www.bilibili.com/video/BV1Rv41177Af/>

---

### 6. Distributed systems perspective

**Distributed Systems for Fun and Profit**  
<https://book.mixu.net/distsys/>

A short and accessible overview of distributed thinking — essential context for distributed databases.

---

### 7. Optional: HBase

**HBase: The Definitive Guide**  
<https://drive.google.com/file/d/1pHaX4c0iPsL24OKhg46ZcSjz8ccwlJIm/view>

---

# ⚙️ Intermediate Level — Ready for Real Work

**Goal:**  
Understand SQL engine internals, storage engines, transactions, NoSQL internals, and official docs.

---

### 1. CMU 15-445 / 15-645: Database Systems

- Course site: <https://15445.courses.cs.cmu.edu/fall2017/>  
- Lecture recordings: <https://www.bilibili.com/video/BV1LA411H7Gj/>

Covers:

- storage and buffer pools  
- B+-trees  
- query optimization  
- MVCC  
- logging and recovery  

Assignments are based on **BusTub**, a real DBMS implementation.

---

### 2. MySQL official documentation

- <https://dev.mysql.com/doc/>

How to read:

- browse the doc structure first  
- look up specific topics deeply  
- read entire chapters when exploring a new feature  

---

### 3. InnoDB internals

**MySQL Technical Insider: The InnoDB Storage Engine**  
<https://drive.google.com/file/d/1kSQsS9-_QujtdpMVVV2Lnvy0Olv63bXa/view>

Recommended chapters: 2, 4, 5.

---

### 4. MongoDB official documentation

- <https://www.mongodb.com/docs/manual/>

Best source for modeling, aggregation, indexes, deployments.

---

### 5. Redis internals

**Redis Design and Implementation**  
<https://drive.google.com/file/d/1PyPTLFGywqbHEmKvHQrV0rjJD4THPjcJ/view>

Paired with source code reading → excellent for understanding systems internals.

---

### 6. Redis official documentation and references

- Official docs: <https://redis.io/documentation>  
- Data types: <https://redis.io/topics/data-types>  
- Redis in Action:  
  <https://drive.google.com/file/d/1pPgZnvelXPAIxPCz8eMbJLFkwjeddpqo/view>  
- Redis Command Reference:  
  <https://drive.google.com/file/d/1WKBA4pg0uRM24NiThUXW8rAOTNfPR3IT/view>

---

### 7. Distributed systems (again)

MIT 6.824 Distributed Systems  
- <https://pdos.csail.mit.edu/6.824/schedule.html>  
- <https://www.youtube.com/@6.824>  
- <https://www.bilibili.com/video/BV1x7411M7Sf/>

Implement Raft, MapReduce, replicated key-value stores.

---

### 8. PostgreSQL official documentation

- <https://www.postgresql.org/docs/>

Deep, well-structured, and great for understanding relational engine design.

---

# 🚀 Advanced Level — Toward Top-tier Engineering & PhD

**Goal:**  
Performance tuning, indexing, internals, distributed storage, and research papers.

---

### 1. Indexing and performance

**Database Index Design and Optimization**  
<https://drive.google.com/file/d/10SrTxPtZVrI4f93m7AmAXcutWJvYGZKe/view>

Learn:

- how DB engines choose access paths  
- estimating query costs  
- designing and tuning indexes with first principles

---

### 2. High-performance MySQL

**High Performance MySQL**  
<https://drive.google.com/file/d/1nwhSrOXYufqbzIMC99cHQNjL7IZRIWhq/view>

Covers:

- indexing  
- schema design  
- replication  
- sharding  
- hardware & server configuration  
- performance tuning

---

### 3. Advanced Redis books

- **Redis Development and Operations**  
  <https://drive.google.com/file/d/1G2tCdsy5ph10n_eq6Gzv8LiqWLBUIBtg/view>

- **Redis Deep Dive: Core Principles and Practical Applications**  
  <https://drive.google.com/file/d/1VyLppzqXyxseaIWjFKGODg1_zESlFkJO/view>

---

### 4. CMU 15-721 (Advanced Database Systems)

- Example lecture link: <https://www.bilibili.com/video/BV1mJ41147KK/>

Graduate-level seminar focusing on:

- modern DBMS research  
- column stores  
- log-structured storage  
- query optimizers  
- HTAP  
- distributed transactions  

---

### 5. Google’s foundational data systems papers

- **Google File System (GFS)**  
- **MapReduce**  
- **Bigtable**

Example links:  
<https://drive.google.com/file/d/1Gm6ICrMofFzRUd9B4GBvVLY3tZH-z9xX/view>  
<https://drive.google.com/file/d/1wzaVK5PStnudeWA_PTvE5uL-VEw9OaWz/view>  
<https://drive.google.com/file/d/1nhbid-TlM73bCKQ4G3DwNRxTInAlo8fR/view>

---

### 6. Paper reading

**Readings in Databases**  
<https://github.com/rxin/db-readings>

Pick a theme → read 2–3 papers → write notes.

---

## 🧭 Putting it all together

One recommended progression:

1. **Beginner**  
   - Learn SQL via tutorials + one book  
   - Install MySQL/PostgreSQL and practice  
   - Learn MongoDB & Redis basics
2. **Intermediate**  
   - Take CMU 15-445  
   - Read selected chapters of Database System Concepts  
   - Dive into MySQL/MongoDB/Redis documentation  
   - Take MIT 6.824 to connect DBs with distributed systems
3. **Advanced**  
   - Read about indexing and optimization  
   - Study engine internals (InnoDB, Postgres, Redis)  
   - Take CMU 15-721  
   - Start reading DB research papers  

Most importantly: **alternate theory and hands-on practice**.  
Databases become intuitive only when you build, debug, measure, and reason about real systems.
