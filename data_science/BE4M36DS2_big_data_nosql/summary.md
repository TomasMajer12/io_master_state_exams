# BE4M36DS2 — Big Data / NoSQL Database Systems (DS2) — Study Summary

**Language:** English-primary (slides are English), key Czech terms in parentheses for the oral exam.
**Source materials:** DS2 lecture + lab slides (Y. Prokop, based on M. Svoboda's course) in `materials/` — introduction, data formats, NoSQL types, principles, Redis, MongoDB, Cassandra, Neo4j/Cypher, SPARQL, MapReduce/Hadoop, advanced aspects. Cross-checked against *NoSQL Distilled* (Sadalage & Fowler), the original MapReduce/GFS papers (Dean & Ghemawat), and official docs (Redis/MongoDB/Cassandra/Neo4j) — flagged inline.

> **⭐ Exam intel** (see `questions.md`): real past questions hit **MapReduce/Hadoop** (mapper/reducer input-output formats, shuffle phase, use case — examiner wanted the schema drawn), **Big Data fundamentals** (CAP, distribution, scaling, replication — examiner Krátký digs into "why not relational?" and wide-column vs relational), and (older curriculum) XPath/XQuery. Prepare to **draw the MapReduce data flow** and to **argue why RDBMS doesn't fit Big Data**.

**Structure.** Sections 1–5 mirror the five official sub-topics; §6 supplementary (data formats), §7 cross-links, §8 references. Worked examples in `examples.md`.

---

## 1. Big Data, NoSQL, scaling, distribution, CAP, consistency, performance

> **Official sub-topic:** *Big Data (V characteristics, trends), NoSQL (motivation, features). Scaling (vertical, horizontal, network fallacies, cluster). Distribution models (sharding, replication, master-slave and p2p). CAP theorem (properties, ACID, BASE). Consistency (strong, eventual, read, write, quora). Performance tuning (Amdahl, Little, message cost). Polyglot persistence.*

### 1.1 Big Data — the V characteristics (*charakteristiky V*)

- **Volume** — scale of data (terabytes → zettabytes); beyond what one machine/RDBMS handles.
- **Velocity** — speed of generation/processing (batch → streaming).
- **Variety** — heterogeneous formats: structured / semi-structured (JSON, XML — "no or little schema") / unstructured.
- **Veracity** — quality & trustworthiness (noise, bias, imprecision).
- (+ **Value** — business value that must be revealed.)
- Trends: data lakes for diverse raw data, streaming, cloud clusters.

### 1.2 NoSQL — motivation & features

- **Why not RDBMS?** ⭐ (Krátký's favorite argument): rigid schema vs variety; vertical-scaling cost limits vs volume; **impedance mismatch** (objects/aggregates vs normalized relations); join/transaction overhead across a cluster; licensing. NoSQL = "not only SQL": schema flexibility, horizontal scalability, relaxed (tunable) consistency, aggregate orientation.
- **Four types** (by data model):
  | Type | Unit | Examples | Best for |
  |---|---|---|---|
  | **Key-value** | opaque value under a key | Redis, Riak, Memcached | caches, sessions, IoT |
  | **Document** | examinable document (JSON/BSON) in collections | MongoDB, CouchDB | flexible-schema entities, queries inside the value |
  | **Wide column** (column family) | rows with varying columns grouped in families | Cassandra, HBase | huge write-heavy tables, time series |
  | **Graph** | nodes + relationships | Neo4j, OrientDB | highly connected data, traversals |
- First three are **aggregate-oriented** (an aggregate = unit of storage & distribution); graph DBs are the opposite (fine-grained relationships, hard to shard).

### 1.3 Scaling (*škálování*)

- **Vertical (up/down)** — bigger machine (CPUs, RAM, disks). + simple, strong consistency, no distribution issues; − hard performance ceiling, **exponentially growing cost**, proactive provisioning, **vendor lock-in**, deployment downtime.
- **Horizontal (out/in)** — more nodes in a **cluster**. + commodity hardware, flexible, no single point of failure; − complexity: distribution, synchronization, consistency, failure recovery.
- **Cluster** = interconnected commodity nodes, **shared-nothing architecture** (each node has own CPU/memory/disk + OS; interaction only by messages); nodes may be heterogeneous.
- **Fallacies of distributed computing** (false assumptions) ⭐: network is reliable; latency is zero; bandwidth is infinite; network is secure; topology doesn't change; there is one administrator; network is homogeneous; transport cost is zero.
- Caveat: a standalone node can still be the better option (e.g. **graph databases** — graphs are hard to split).

### 1.4 Distribution models: sharding & replication ⭐

Two **orthogonal** techniques (use either or both):

**Sharding** (*horizontal partitioning*) — *different* data on different nodes (motivation: volume + performance). Distribute **aggregates**; keep data accessed together, together. Strategies:
- **mapping-based** (random/round-robin + central directory index),
- **hash sharding** (hash(key) → shard; problem: adding a node reshuffles almost everything → **consistent hashing** on a ring moves only a fraction),
- **range-based** (key ranges; beware **hotspots**), **directory-based**, **entity/relationship-based** (user + their posts together), **geography-based**, **functional**, **time-based**, combined.
- Hard parts: routing reads *and* writes; cluster changes; incomplete node knowledge; failures; partitions.

**Replication** — *the same* data on multiple nodes (motivation: performance + fault tolerance). **Replication factor** = number of copies (3 is typical — then nodes do *not* hold identical data and a **replica-placement strategy** is needed).
- **Master–slave** (*primární–sekundární*): one master handles **all writes**, propagates to slaves; **reads from any node** → great for read-intensive loads; master = bottleneck & single point of failure (new master elected/appointed on failure); obsolete reads possible during propagation.
- **Peer-to-peer**: all nodes equal, **any node serves reads and writes** → no bottleneck, both ops scale; but concurrent independent writes ⇒ **conflict resolution / synchronization** needed.
- **Consensus protocols** for replica agreement & leader election: **Paxos, Raft**, Viewstamped Replication, ZooKeeper's Zab.
- Combinations: sharding + master–slave (multiple masters, one per shard; roles can overlap) or sharding + p2p (anything anywhere). Cluster metadata maps shards → nodes/ranges.

### 1.5 CAP theorem ⭐

For a distributed (sharded + replicated) system with single-aggregate reads/writes, the three **CAP properties**:
- **Consistency** — operations appear **atomic/linearizable**: a total order exists such that each op looks executed at a single instant on one node; practically: *after a write, all readers see the same data*.
- **Availability** — every request received by a **non-failing** node must produce a response.
- **Partition tolerance** — the system keeps operating even when node sets get isolated (arbitrarily many lost messages).

**Theorem (Brewer/Gilbert-Lynch): at most 2 of the 3 can be guaranteed.**
- **CA** — RDBMS, single-node systems (on partition the system must stop serving).
- **CP** — consistency preserved, some requests refused during partitions (e.g. distributed locking, MongoDB-style primaries).
- **AP** — always answers, possibly stale (Cassandra, CouchDB, DNS, web caches).
- **The real reading** ⭐: in a cluster, **partition tolerance is a must** (failures can't be reliably detected) ⇒ the practical trade-off is **consistency vs availability**, and it's a *spectrum*, not binary — relaxing consistency buys availability.

**ACID vs BASE:**
- **ACID** (RDBMS, pessimistic, consistency-first): Atomicity (all-or-nothing), Consistency (valid state → valid state), Isolation (no uncommitted effects visible), Durability.
- **BASE** (NoSQL, optimistic, availability-first): **B**asically **A**vailable (partial, never total failure), **S**oft state (in flux), **E**ventual consistency (a consistent state is reached *eventually*). Vague by design; enables scalability ACID can't.

### 1.6 Consistency levels & quora ⭐

- **Strong consistency**: every read returns the latest write. **Eventual**: replicas converge if writes stop; often sufficient (a 1-min-stale news article; a double-booked room solvable in the real world).
- **Read/write consistency problems**: stale reads (read an old replica), lost updates / **write-write conflicts** (concurrent writes), read-your-writes violations.
- **Quorum mechanics** (with replication factor **N**, write quorum **W**, read quorum **R**):
  - a write succeeds after **W** replicas ack; a read consults **R** replicas;
  - **`W + R > N`** ⇒ read & write sets overlap in ≥1 replica ⇒ **strong consistency** (the read sees the newest version);
  - **`W > N/2`** prevents conflicting concurrent write quora (write-write conflicts);
  - typical: `N=3, W=2, R=2`; tune per operation: `W=N,R=1` (fast reads), `W=1,R=N` (fast writes), `W+R≤N` → eventual consistency but higher availability/latency.
  - *(The quorum slides didn't extract from the PDF; rules verified against standard literature — also exactly how Cassandra's tunable consistency works, §4.2.)*

### 1.7 Performance tuning ⭐

- **Linear scalability** assumption: n nodes process n× the data per second — holds only if tasks split into balanced units.
- **Amdahl's law**: max speedup with parallelizable fraction `P` on `N` processors: `S = 1/((1−P) + P/N)`; as `N→∞`, **`S → 1/(1−P)`**. Example: 300-min job, 25 min serial (`P=0.916`) ⇒ max speedup `1/0.084 ≈ 11.9×`. ⇒ the *serial* part dominates the limit.
- **Little's law**: in a stable queueing system, **`L = λW`** (average # in system = arrival rate × average time in system) — independent of distributions/service order. Example: 4 customers/hr × 0.25 hr ⇒ 1 customer on average.
- **Message cost model**: **`C = a + b·N`** (fixed setup cost `a` + per-byte cost `b` × size `N`). Gigabit Ethernet: `a≈0.3 ms`, `b=1 s/125 MB`. 100×10 KB messages = 38 ms vs 10×100 KB = 11 ms ⇒ **batch into fewer, bigger messages**.

### 1.8 Polyglot persistence (*polyglotní perzistence*)

Different problems → different databases; a single engine for everything is sub-optimal. E.g. e-commerce: shopping cart in a highly-available KV store, catalog in a document store, "products bought by friends" in a graph DB, transactions in RDBMS. Wrap stores in **services** so they can evolve independently. (Analogy: polyglot programming.)

---

## 2. MapReduce and Hadoop (HDFS) ⭐ (asked at the state exam)

> **Official sub-topic:** *MapReduce (architecture, functions, data flow, execution, use cases). Hadoop (MapReduce, HDFS).*

### 2.1 What MapReduce is

A **programming model + implementation** (Google 2004, Dean & Ghemawat) for automatic parallelization of large-scale computation on clusters of commodity machines. Motivation: PageRank over tens of billions of pages — huge append-only files scattered over ~10⁵ machines, expensive data movement, **failures are the norm**. Classification: **message passing** interaction; **data parallelism**; automatic fault handling. (Successors: Apache **Spark**, Flink.)

### 2.2 The two functions (know the I/O formats! — the examiner asked) ⭐

- **Map**: `(key, value) → list of (key', value')` — one input **record** in, a list of **intermediate key-value pairs** out (different domain allowed; duplicate keys fine).
- **Reduce**: `(key', list of all values for key') → (key', smaller list of values)` — usually same domain.
- Everything else — input distribution, scheduling, monitoring, communication, **failure handling** — is the framework's job.

**Canonical example — word count:**
```
map(docId, text):    for each word w in text: emit(w, 1)
reduce(w, counts):   emit(w, sum(counts))
```

### 2.3 Logical phases & data flow (draw this!) ⭐

1. **Map phase** — Map runs on every input record; intermediate pairs emitted.
2. **Shuffle phase** — intermediate pairs are **grouped and sorted by key** and transferred from mappers to the right reducers (the only all-to-all communication; the expensive part).
3. **Reduce phase** — Reduce runs per intermediate key; outputs written.

### 2.4 Execution architecture

**Master–slave (coordinator–workers):**
- **Master**: schedules Map/Reduce tasks to idle workers, tracks job progress, keeps file metadata.
- **Workers**: store data splits; execute tasks.
- **Job submission**: client gives Map+Reduce implementations, input file(s), output directory → master locates **splits**.
- **Map task** (1 split × 1 worker, preferably the one already storing the split — *move computation to data*): input reader parses split → records; Map applied; intermediate pairs stored **locally**, organized into **regions** by a **partition function** (e.g. `hash(key) mod #reducers`).
- **Reduce task**: pulls its region from **all** mappers (remote read), **merge-sorts & groups** by key, applies Reduce, writes its own output file.
- **Full function stack**: input reader → **Map** → **partition** → **compare** (sort order) → **combine** (optional) → **Reduce** → output writer.
- **Combine function** (optional, local pre-reduction on the mapper): cuts shuffle traffic; valid when the reduction is **commutative + associative** (e.g. sum, max).
- **Fault tolerance**: master **pings/heartbeats** workers; failed worker's tasks are reset and rescheduled. Master failure: checkpoints (or just resubmit the job). **Stragglers** → speculative **backup executions** of the last in-progress tasks.
- **Counters** track progress (predefined + custom).
- **Use cases**: word frequency/histograms, grep, inverted index construction, log analysis, sorting (TeraSort), PageRank-style link analysis — *batch* jobs over huge, rarely-updated files; NOT for interactive/low-latency or iterative workloads (→ Spark).

### 2.5 Apache Hadoop

Open-source (Apache, Java) implementation derived from Google MapReduce + **GFS**. Components:
- **HDFS** (Hadoop Distributed File System) — high-throughput distributed FS.
- **YARN** — cluster resource management & job scheduling.
- **Hadoop MapReduce** — YARN-based MapReduce implementation.

**HDFS** ⭐:
- **Assumptions**: very large files; **streaming access**; batch processing; **write-once, read-many**; **failure is the norm** → automatic detection/recovery.
- Files split into **blocks** (default **128 MB**, configurable), each block **replicated** (default factor 3; rack-aware placement) → read throughput + fault tolerance.
- **Master–slave**: **NameNode** (master) keeps the **namespace** + block locations, mediates all metadata ops; **DataNodes** store blocks and serve reads/writes directly — **user data never flows through the NameNode**. DataNodes send heartbeats.
- Appears to clients as **one coherent file system** (the FAQ answer: "HDFS se tváří jako jeden celistvý filesystem").

---

## 3. SPARQL (DS2 angle)

> **Official sub-topic:** *SPARQL (subgraph matching, graph patterns, datasets, filters, solution modifiers, query forms).*

> **Full treatment in OSW** `../BE4M33OSW_ontologies_semantic_web/summary.md` §3.4–3.5 — same language, DS2 asks the *querying mechanics*:

- **Data model**: RDF triples (subject, predicate, object) = a directed labeled graph; **SPARQL queries = subgraph matching** (find all mappings of query variables to RDF terms such that the pattern is a subgraph of the data).
- **Triple pattern** = triple with variables (`?x :invented ?i`); **Basic Graph Pattern (BGP)** = set of triple patterns matched **conjunctively** (a join on shared variables). **Solution** = mapping `μ: variables → RDF terms`.
- **Graph patterns / algebra**: `UNION` (disjunction), `OPTIONAL` (left outer join), `FILTER` (selection; `FILTER NOT EXISTS` / `MINUS` for negation), property paths, `GRAPH ?g {…}` for named graphs.
- **Datasets**: default graph + named graphs; `FROM` / `FROM NAMED` select the active dataset.
- **Filters**: comparisons, `regex`, string/numeric/RDF-term functions (`lang`, `datatype`, `isIRI`, `str`, `strstarts`, …).
- **Solution modifiers**: `ORDER BY`, `LIMIT`, `OFFSET`, `DISTINCT`, `REDUCED`, projection; aggregation `GROUP BY`/`HAVING` + `COUNT/SUM/AVG/MIN/MAX/GROUP_CONCAT/SAMPLE`.
- **Query forms**: **SELECT** (binding table), **ASK** (boolean), **CONSTRUCT** (new RDF graph from a template), **DESCRIBE** (resource description, implementation-defined).

---

## 4. Redis, Cassandra, MongoDB ⭐

> **Official sub-topic:** *Redis (data types, operations, TTL, persistence). Cassandra (keyspaces, column families, CRUD). MongoDB (CRUD, update/query operators, projection, modifiers, aggregations).*

### 4.1 Redis (key-value store)

- **In-memory** data-structure store (very low latency) with optional disk **persistence**; single-threaded core (atomic ops); master–slave replication; Redis Cluster for sharding; **Pub/Sub** + Streams.
- **Keys**: arbitrary binary strings (even `""`); design for clarity vs length. Logical **databases** addressed by number.
- **Value data types** ⭐:
  | Type | Operations (examples) |
  |---|---|
  | **String** (binary-safe; also numbers) | `SET k v [EX s]`, `GET`, `APPEND`, `INCR/DECR` (atomic counters), `TYPE`, `EXISTS` |
  | **List** (ordered, both-end ops) | `LPUSH/RPUSH`, `LPOP/RPOP`, `LRANGE`, `LLEN` (queues/stacks) |
  | **Set** (unordered, unique) | `SADD`, `SMEMBERS`, `SISMEMBER`, `SINTER/SUNION/SDIFF` |
  | **Hash** (field→value map) | `HSET`, `HGET`, `HGETALL`, `HDEL` (objects, settings) |
  | **Sorted set (ZSet)** (members with scores) | `ZADD`, `ZRANGE`, `ZRANGEBYSCORE`, `ZRANK` (leaderboards) |
  | (+ bitmaps, HyperLogLog, streams) | |
- **TTL / expiration**: `EXPIRE key seconds`, `TTL key`, `PERSIST key`; or `SET k v EX s` — auto-deletion; the basis for caching/sessions.
- **Persistence** ⭐: **RDB** (point-in-time binary **snapshots** at intervals — compact, fast restart, may lose last writes) vs **AOF** (append-only log of every write — replayed on restart, fsync policies, safer but larger/slower); can combine both; or pure cache (no persistence).
- **Use cases**: caching, sessions, counters, rate limiting, recommendations, settings, queues (lists), leaderboards (zsets).

### 4.2 Cassandra (wide column store)

- **Decentralized peer-to-peer** architecture: **ring topology**, **consistent hashing** for data placement, **gossip protocol** for membership — no master, no single point of failure; massive linear write scalability (Apple: ~300k nodes, ~100 PB).
- **Data model**:
  - **Keyspace** = top-level container (≈ database), defines **replication strategy** + **replication factor (RF)** (SimpleStrategy — adjacent ring nodes; NetworkTopologyStrategy — per data center).
  - **Table / column family** = collection of *similar but not identical* rows — each row can have **different columns** (wide-column trait).
  - **Primary key = partition key + clustering key** ⭐: the **partition key** determines *which node* stores the row (hashed onto the ring); **clustering columns** determine the *sort order within* a partition. Partition-key design must spread load (avoid hotspots).
- **CRUD** in **CQL** (SQL-like): `CREATE KEYSPACE … WITH replication = {…}`; `CREATE TABLE`; `INSERT INTO … VALUES …` (actually an *upsert*); `SELECT … WHERE partition_key = …` (queries must generally restrict the partition key); `UPDATE`; `DELETE` (writes **tombstones**).
- **Tunable consistency** ⭐: consistency level **per operation** (ONE, QUORUM, ALL, LOCAL_QUORUM…) — the quorum machinery of §1.6 made concrete; **read repair** fixes stale replicas during reads. Writes go to a commit log + memtable → flushed to **SSTables** (LSM-tree → write-optimized).

⭐ **Wide-column vs relational — the Krátký debate** (he counters every point with "to můžeme i s relační DB"; have *systemic* arguments ready, not feature lists):

| | Relational | Wide-column (Cassandra) |
|---|---|---|
| Schema | fixed columns per table; ALTER is global | **each row its own columns**; sparse data stored sparsely (no NULL storage) |
| Data layout | row store, B-tree indexes — read-optimized | partition-per-node + **LSM/SSTables — write-optimized**, sequential disk I/O |
| Scaling unit | scale-up (shared storage / complex sharding bolted on) | **shard-per-partition-key is the data model itself** — linear scale-out is native, not added |
| Consistency | ACID transactions, joins | **tunable per operation**; no joins — denormalize, model query-first |
| Failure model | failover of a primary | p2p ring, no SPOF, gossip |

The honest concession (he pushes until you say it): yes, a sharded RDBMS *can* be built — but you then **manually** reimplement partitioning, rebalancing, and replication that Cassandra gives natively, and you lose cross-shard joins/transactions anyway — so the relational advantages you were defending are gone exactly at scale. The trade is **consistency+joins ↔ availability+linear writes** (CAP, §1.5), not "feature X".

### 4.3 MongoDB (document store)

- **JSON/BSON document database**: documents (max 16 MB) in **collections**; flexible schema; `_id` primary key (auto ObjectId); secondary **indexes**; **replica sets** (master–slave with automatic failover) + **sharding**; comprehensive **aggregation framework**; geospatial queries. (Vs Redis: Redis = in-memory KV with limited querying; MongoDB = disk-based with rich queries inside documents.)
- **CRUD** ⭐:
  - `db.coll.insertOne({…})`, `insertMany([…])`
  - `db.coll.find(query, projection)`, `findOne`
  - `db.coll.updateOne(filter, update)`, `updateMany`, `replaceOne`
  - `db.coll.deleteOne(filter)`, `deleteMany`
- **Query operators**: comparison `$eq, $ne, $gt, $gte, $lt, $lte, $in, $nin`; logical `$and, $or, $not, $nor`; element `$exists, $type`; array `$all, $elemMatch, $size`; evaluation `$regex`. Nested fields via dot notation `"address.city"`.
- **Update operators**: `$set`, `$unset`, `$inc`, `$mul`, `$rename`, `$min/$max`, array `$push, $pop, $pull, $addToSet`.
- **Projection**: second argument of `find` — `{name: 1, _id: 0}` (include/exclude fields).
- **Cursor modifiers**: `.sort({f:1})`, `.limit(n)`, `.skip(n)`, `.count()`.
- **Aggregation pipeline** ⭐: documents flow through **stages**: `$match` (filter) → `$group` (`_id` + accumulators `$sum, $avg, $min, $max, $push`) → `$sort`, `$project`, `$limit`, `$skip`, `$unwind` (flatten arrays), `$lookup` (left join). Example:
  ```js
  db.orders.aggregate([
    { $match: { status: "A" } },
    { $group: { _id: "$cust_id", total: { $sum: "$amount" } } },
    { $sort: { total: -1 } }
  ])
  ```

---

## 5. Graph data: structures, locality, partitioning, Neo4j, Cypher

> **Official sub-topic:** *Graph structures (adjacency matrix, adjacency list, incidence matrix). Data locality (BFS layout, bandwidth minimization, Cuthill-McKee). Graph partitioning (1D, 2D). Neo4j (traversal framework). Cypher (graph matching, read/write/general clauses).*

### 5.1 Graph data structures (`G=(V,E)`, `n=|V|`, `m=|E|`)

| Structure | Definition | Pros | Cons |
|---|---|---|---|
| **Adjacency matrix** | `n×n` boolean `A`, `A_ij=1` iff edge (variants: directed, weighted) | O(1) edge add/remove/check | **O(n²) space** (bad for sparse graphs), node addition expensive, neighbors in O(n) |
| **Adjacency list** | vector of `n` pointers to neighbor lists (often compressed) | fast neighbor retrieval, cheap node addition, compact for sparse graphs | edge-existence check is slow (sorted lists → O(log n), but log-insert) |
| **Incidence matrix** | `n×m` boolean: rows=nodes, columns=edges | represents **hypergraphs** (one edge joins many nodes) | `n·m` bits |
| **Laplacian matrix** | `n×n`: diagonal = degrees, `−1` for edges, 0 else | **spectral analysis** of graph structure (eigenvalues — cf. SAN spectral clustering) | |

### 5.2 Data locality ⭐

Idea: physical memory layout determines **spatial locality** — after accessing an item, nearby items are likely accessed next (graph traversals!). Reorder rows/columns of the adjacency matrix to improve **cache hit ratio**.
- **BFS Layout (BFSL)**: pick a start node, BFS-traverse, relabel vertices in visit order → optimal for traversals *from that node*, weak from others.
- **Matrix bandwidth**: bandwidth of a row = max distance of nonzero elements straddling the diagonal; bandwidth of the matrix = max over rows. **Low bandwidth = nonzeros clustered near the diagonal = cache-friendly**. **Bandwidth Minimization Problem (BMP) is NP-hard** → approximations.
- **Cuthill–McKee (1969)**: heuristic relabeling for sparse matrices — start from the **minimum-degree** node, BFS, visiting lower-degree neighbors first; produces a low-bandwidth ordering. (Reverse variant RCM common.)

### 5.3 Graph partitioning (for distributed BFS) ⭐

Graphs too big for one machine → distribute over a shared-nothing cluster by **partitioning the adjacency matrix**:
- **1D partitioning**: matrix **rows** randomly assigned to `P` processors — each vertex + its outgoing edges owned by one processor. Distributed BFS: each processor merges edge lists of its **frontier** `F` → set of neighbors `N` → **messages to all other processors** to extend their frontiers (potentially all `P` communicate).
- **2D partitioning**: processors arranged in an `R×C` **mesh**; adjacency matrix divided into `C` block columns and `R·C` block rows; each processor owns `C` blocks. **Benefit: each node communicates with ≤ `R+C` processors instead of all `P`** (frontier expansion = row messages; ownership updates = column messages). 1D = 2D with `C=1`.
- Query types in graph DBs: **sub-graph** queries (pattern search, sub-graph isomorphism), **super-graph** queries, **similarity** (approximate matching) queries.

### 5.4 Neo4j

- **Native graph DB**: **property graph** model = **directed labeled multigraph**: **nodes** (with **labels** for categorization + **properties** key→value) and **relationships** (always with start/end node + **type** + properties; traversable equally fast in **either direction** — no need for inverse edges). ACID transactions; master–slave replication; index-free adjacency.
- **Traversal framework** ⭐ — programmatic traversals described by:
  - **Traversal description** (immutable builder): **order** (depth-first / breadth-first), **expanders** (which relationship types + directions to follow), **uniqueness** (visit nodes/relationships once — NODE_GLOBAL, RELATIONSHIP_GLOBAL, …), **evaluators** (per-position decision: `INCLUDE_AND_CONTINUE`, `EXCLUDE_AND_PRUNE`, … e.g. depth limits).
  - **Traverser** = the lazily-evaluated result: iterates **paths** (or end nodes / relationships) as the graph is explored.

### 5.5 Cypher

Declarative pattern-matching query language (inspired by SQL clauses + SPARQL patterns). **Clauses chain together** — the intermediate result of one feeds the next.
- **Path patterns**: `(a:ACTOR {name:"X"})-[r:PLAYS]->(m:MOVIE)` — node patterns `(var:Label {prop: val})`, relationship patterns `-[var:TYPE*1..3]->` (direction, types, variable length).
- **Read clauses**: `MATCH` (graph pattern matching = sub-graph matching; `OPTIONAL MATCH`), **`WHERE`** (filtering sub-clause).
- **Write clauses**: `CREATE` (nodes/relationships), `MERGE` (match-or-create), `DELETE` (`DETACH DELETE` removes a node + its relationships), `SET` (labels/properties), `REMOVE`.
- **General clauses**: `RETURN` (projection; `DISTINCT`, aggregation `count/collect/avg…`), `ORDER BY`, `LIMIT`/`SKIP`, **`WITH`** (pipe intermediate results, enables multi-part queries), `UNWIND`.
- Example:
  ```cypher
  MATCH (m:MOVIE)-[:PLAY]->(a:ACTOR)
  WHERE m.title = "Medvídek"
  RETURN a.name, a.year ORDER BY a.year
  ```

---

## 6. Supplementary: data formats (lectures 1–2)

- **Structured vs semi-structured vs unstructured** data; semi-structured = "self-describing", schema optional.
- **JSON** (objects/arrays/scalars; JSON Schema; BSON = binary JSON in MongoDB), **XML** (elements/attributes; DTD/XSD), **CSV/TSV**, binary formats (**Avro, Parquet, ORC** — columnar, splittable, compressed; for Hadoop pipelines), **RDF** serializations (→ OSW).

⭐ **XPath / XQuery** — *flagged legacy, but Krátký asked it verbatim in 2023* (axes + FLWOR on a self-invented example):

- **XPath path expressions**: steps `axis::node-test[predicate]`, abbreviated `/bookstore/book[@year>2000]/title`. **Axes** (the part he wants enumerated): `child` (default), `descendant`, `descendant-or-self` (`//`), `parent` (`..`), `ancestor`, `ancestor-or-self`, `self` (`.`), `attribute` (`@`), `following-sibling`, `preceding-sibling`, `following`, `preceding`.
- **XQuery FLWOR** (For–Let–Where–Order by–Return) — have a tiny example ready:
  ```xquery
  for $b in doc("books.xml")//book
  let $p := $b/price
  where $p > 500 and $b/@year >= 2010
  order by $b/title
  return <cheap>{ $b/title/text() }</cheap>
  ```
  `for` iterates a sequence, `let` binds without iterating, `where` filters, `order by` sorts, `return` constructs (possibly new XML) per binding.
- Format choice drives: splittability (MapReduce!), schema evolution, compression, human readability.

## 7. Cross-subject links
- **SPARQL, RDF** ↔ **OSW** §3 (identical language; OSW adds semantics/entailment).
- **Spectral analysis of the Laplacian** ↔ **SAN** §6.6 (spectral clustering).
- **Consistent hashing, distributed BFS, NP-hardness (bandwidth minimization)** ↔ **PAL/TAL/KO** (graphs, complexity).
- **Master–slave vs p2p, consensus** ↔ general distributed-systems knowledge; **MapReduce data parallelism** ↔ SSU SGD mini-batching (data parallel ideas).

## 8. References (verified)
- P. Sadalage, M. Fowler: *NoSQL Distilled*, Pearson 2013 (distribution models, CAP, polyglot persistence — the course's base book).
- J. Dean, S. Ghemawat: *MapReduce: Simplified Data Processing on Large Clusters*, OSDI 2004; S. Ghemawat et al.: *The Google File System*, SOSP 2003.
- E. Brewer's CAP conjecture; Gilbert & Lynch (2002) formal proof.
- Quorum rules (`W+R>N`, `W>N/2`): standard distributed-systems literature (verified; e.g. DDIA — M. Kleppmann, *Designing Data-Intensive Applications*).
- Official docs: [Redis](https://redis.io/docs/) (data types, persistence RDB/AOF), [MongoDB](https://www.mongodb.com/docs/manual/) (CRUD, aggregation), [Cassandra](https://cassandra.apache.org/doc/) (architecture, CQL), [Neo4j](https://neo4j.com/docs/) (traversal framework, Cypher), [Hadoop/HDFS](https://hadoop.apache.org/docs/stable/) (architecture).
- Cuthill & McKee (1969); Buluç & Madduri on distributed-memory BFS (1D/2D partitioning).
- Course pages: <https://cw.fel.cvut.cz/wiki/courses/b4m36ds2/>.
