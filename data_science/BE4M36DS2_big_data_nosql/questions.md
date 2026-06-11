# BE4M36DS2 — Big Data / NoSQL (DS2) — Questions

Past-exam questions from `../../frequently_asked_questions/oi_wiki_committee_archive.md` (3 entries), cross-referenced to `summary.md`, plus anticipated questions from the official topic structure.

## Real past-exam questions (OI-Wiki archive)

### MapReduce / Hadoop / HDFS — asked 22.6.2023 (examiner: Lisý) → §2 ⭐
- "MapReduce (architektura, funkce, toky dat, exekuce, použití). Hadoop (MapReduce, HDFS)." The examiner specifically wanted: **input/output formats of mapper and reducer functions**, a **drawn communication schema**, emphasis on the **shuffle phase**, **what MapReduce is good for**, and the canonical **word-count/histogram** example. For Hadoop: framework for distributed systems, **HDFS appears as one coherent filesystem**. → §2.2–2.5, examples.md A

### Big Data fundamentals — asked 26.1.2022 (examiner: Krátký) → §1 ⭐
- "Management BigData (CAP, distribuce, škálování, replikace) + wide-column databáze." Warning from the report: Krátký **digs into side remarks** and turns it into a debate — *"why are Big Data unsuitable for relational DBs?"* and *"what's the difference between relational and wide-column?"*; he plays devil's advocate ("we can do that with an RDBMS too"). Prepare structured arguments. → §1.2–1.6, §4.2, examples.md B

### XPath / XQuery — asked 22.6.2023 (examiner: Krátký) → §6 (legacy)
- "XPath (výrazy cest, osy), XQuery (konstrukce, FLWOR)" — define **axes**, show a **FLWOR** construct on a self-invented example. Not in the current official topic list but evidently still asked — keep the §6 basics ready. → examples.md L

---

## Anticipated questions (official topic structure)

### §1 Big Data / NoSQL / distribution ⭐
- 4–5 V characteristics; NoSQL motivation & the four types (with example systems and use cases).
- Vertical vs horizontal scaling (trade-offs); fallacies of distributed computing; shared-nothing cluster.
- Sharding strategies (hash → consistent hashing, range, directory, entity, geo); replication master–slave vs p2p; combining both; consensus (Paxos/Raft) roles.
- **CAP theorem**: define C/A/P precisely, the 2-of-3 statement, CA/CP/AP examples, and the "real reading" (P is mandatory → C vs A trade-off). ACID vs BASE.
- Consistency levels; **quorum rules `W+R>N`, `W>N/2`** with N=3 examples.
- **Amdahl's law, Little's law, message cost model** — formulas + small numeric examples.
- Polyglot persistence with an e-commerce example.

### §2 MapReduce / Hadoop ⭐ (proven exam topic)
- Map and Reduce function signatures; logical phases (map–shuffle–reduce); draw the execution architecture (master/workers, splits, regions, partition function); combine function and when it's valid; fault tolerance (heartbeats, rescheduling, stragglers/backup tasks); use cases & limits.
- HDFS: assumptions (write-once/read-many, failure as norm), blocks (128 MB) + replication, NameNode vs DataNodes, why data never flows through the NameNode.

### §3 SPARQL
- Query forms (SELECT/ASK/CONSTRUCT/DESCRIBE); BGP = subgraph matching; FILTER; solution modifiers; datasets (FROM/GRAPH). → also OSW §3.4–3.5

### §4 Stores ⭐
- Redis: data types + typical operations, TTL/EXPIRE/PERSIST, RDB vs AOF persistence, use cases.
- Cassandra: keyspace (replication strategy/RF), column family, **partition key vs clustering key**, CQL CRUD, ring/gossip/consistent hashing, **tunable consistency**.
- MongoDB: documents/collections/BSON, CRUD methods, query operators (`$gt,$in,$or,$elemMatch…`), update operators (`$set,$inc,$push…`), projection, sort/limit/skip, **aggregation pipeline** (`$match→$group→$sort`), replica sets + sharding.

### §5 Graph data ⭐
- Adjacency matrix vs adjacency list vs incidence matrix (+ Laplacian) — pros/cons table.
- Data locality: spatial locality, BFS layout, **matrix bandwidth**, BMP NP-hardness, **Cuthill–McKee** heuristic.
- **1D vs 2D partitioning** of the adjacency matrix for distributed BFS; communication bounds (`P` vs `R+C`).
- Neo4j: property graph model, **traversal framework** (description: order/expanders/uniqueness/evaluators; traverser).
- Cypher: path patterns, read (`MATCH/WHERE`), write (`CREATE/MERGE/DELETE/SET/REMOVE`), general (`RETURN/WITH/ORDER BY/LIMIT`) clauses; example query.
