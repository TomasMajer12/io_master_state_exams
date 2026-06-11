# BE4M36DS2 — Worked Examples

Exam-style problems with solutions; cross-references to `summary.md`. The state-exam FAQ shows examiners want **drawn schemas** (MapReduce flow) and **argumentation** (NoSQL vs RDBMS).

---

## A. MapReduce word count — full data flow [§2.2–2.3] ⭐ (asked 2023)

Input: 2 documents `d1="a b a"`, `d2="b c"`. Walk through (this is what the examiner wanted drawn):

```
                MAP                       SHUFFLE (group+sort by key)        REDUCE
d1 → map(d1,"a b a") → (a,1)(b,1)(a,1) →  a:[1,1]   →  reduce(a,[1,1]) → (a,2)
d2 → map(d2,"b c")   → (b,1)(c,1)      →  b:[1,1]   →  reduce(b,[1,1]) → (b,2)
                                          c:[1]     →  reduce(c,[1])   → (c,1)
```
- **Map I/O format**: `(docId, text) → list of (word, 1)`. **Reduce I/O**: `(word, [counts]) → (word, sum)`.
- **Shuffle** = grouping+sorting intermediate pairs by key and moving them mapper→reducer (the network-heavy phase).
- **Combine** (optional): pre-sum locally on each mapper (`(a,[1,1])→(a,2)` on mapper 1) — valid because sum is commutative+associative; cuts shuffle traffic.
- **What MapReduce is good for**: batch processing of huge, append-only files on commodity clusters with automatic fault tolerance (histograms, inverted index, log analysis); not for interactive/iterative jobs.

## B. "Why are Big Data unsuitable for RDBMS?" [§1.2] ⭐ (Krátký's dispute, 2022)

Argue along these axes (he will push back with "we can do that relationally too"):
1. **Scaling economics** — RDBMS scales vertically (cost grows superlinearly, hard ceiling); NoSQL scales horizontally on commodity nodes.
2. **Distributed joins/transactions** — ACID across shards demands 2PC/coordination → latency, availability loss (CAP); aggregate stores avoid cross-node joins by design.
3. **Schema rigidity vs variety** — semi/unstructured data and evolving schemas fit document/wide-column models; ALTER TABLE on billions of rows is painful.
4. **Wide-column vs relational** specifically: rows in a column family may have *different* columns — sparse data stored compactly (no NULL columns); partition+clustering keys give locality by access pattern; writes go to LSM/SSTables (write-optimized) vs B-tree in-place updates.
5. Concede the counterpoint honestly: for moderate volumes + strong-consistency needs, RDBMS *is* right — that's polyglot persistence [§1.8].

## C. CAP scenarios [§1.5]

Network partition splits the cluster into two halves; a client writes to side A and reads from side B.
- **CP system**: side B refuses reads (or A refuses the write) — consistent but not available.
- **AP system**: B answers with stale data — available but not consistent (eventual consistency reconciles later).
- **CA**: only meaningful without partitions (single node / theoretical cluster) — on partition it must stop.

## D. Quorum arithmetic [§1.6] ⭐

`N=3` replicas.
- `W=2, R=2`: `W+R=4>3` ⇒ strong consistency; tolerates 1 node down for both reads & writes.
- `W=1, R=1`: `W+R=2≤3` ⇒ fast + highly available, but a read can miss the latest write (eventual).
- `W=3, R=1`: reads fast and always-consistent, writes fail if any node is down.
- Write-write conflicts prevented when `W > N/2` (no two disjoint write quora). Cassandra exposes exactly this per-operation (ONE/QUORUM/ALL).

## E. Amdahl / Little / message cost [§1.7] ⭐

- **Amdahl**: 300-min job, 25 min inherently serial ⇒ `P = 275/300 ≈ 0.916`; max speedup `1/(1−P) = 1/0.084 ≈ 11.9×` no matter how many nodes. With `N=10`: `1/(0.084+0.916/10) ≈ 5.7×`.
- **Little**: gas station, λ=4 customers/hr, W=15 min=0.25 hr ⇒ `L = λW = 1` customer on average in the system. If arrivals exceed service capacity, queue (and W) grows — bottleneck detection.
- **Message cost**: `C = a + bN`, gigabit Ethernet `a=0.3 ms`, `b=1/125 ms/KB`: 100 messages ×10 KB = `100(0.3+0.08)=38 ms`; 10×100 KB = `10(0.3+0.8)=11 ms` ⇒ **batch messages**.

## F. Redis modelling [§4.1]

Session cache with 30-min expiry: `SET sess:42 "{json}" EX 1800`; check `TTL sess:42`; cancel expiry `PERSIST sess:42`.
Leaderboard: `ZADD board 1500 "alice"`, `ZADD board 1720 "bob"`; top-3: `ZRANGE board 0 2 REV WITHSCORES`. Atomic page counter: `INCR views:home`.
Persistence choice: cache-only → none; durable counters → AOF (every write logged); fast restart of a big dataset → RDB snapshots; commonly both.

## G. Cassandra table design [§4.2] ⭐

Sensor readings, query = "all readings of sensor X on day D ordered by time":
```sql
CREATE TABLE readings (
  sensor text, date date, ts timestamp, value double,
  PRIMARY KEY ((sensor, date), ts)
) WITH CLUSTERING ORDER BY (ts DESC);
```
- **Partition key `(sensor,date)`** → the whole day of one sensor lives on one node (query hits a single partition; daily bucketing prevents unbounded partitions/hotspots).
- **Clustering key `ts`** → rows sorted by time inside the partition.
- Cassandra modelling rule: **design tables from queries**, not from entities (denormalize; one table per query pattern).

## H. MongoDB queries [§4.3]

```js
// people older than 30 in Prague, return name only, newest first, top 10
db.people.find(
  { age: { $gt: 30 }, "address.city": "Prague" },
  { name: 1, _id: 0 }
).sort({ created: -1 }).limit(10)

// add a tag, increment a counter
db.people.updateOne({ _id: id }, { $addToSet: { tags: "vip" }, $inc: { visits: 1 } })

// average order amount per customer, only completed orders
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$custId", avg: { $avg: "$amount" }, n: { $sum: 1 } } },
  { $sort: { avg: -1 } }
])
```

## I. Cypher queries [§5.5]

```cypher
// co-actors of an actor
MATCH (a:ACTOR {name:"X"})-[:PLAYS]->(m:MOVIE)<-[:PLAYS]-(b:ACTOR)
RETURN DISTINCT b.name

// shortest path up to 5 hops
MATCH p = shortestPath( (a:Person {name:"A"})-[:KNOWS*..5]-(b:Person {name:"B"}) )
RETURN length(p)

// match-or-create + property update
MERGE (c:City {name:"Prague"}) SET c.visited = coalesce(c.visited,0) + 1
```

## J. Bandwidth & Cuthill–McKee mini-run [§5.2]

Path graph 1–3–2 labeled badly: adjacency nonzeros far from diagonal → high bandwidth. CM: start at a **minimum-degree** vertex (an endpoint, degree 1), BFS preferring low-degree neighbors, relabel in visit order → nonzeros hug the diagonal (bandwidth 1) → cache-friendly traversals. Remember: exact bandwidth minimization is **NP-hard**; CM is a heuristic.

## K. 1D vs 2D BFS partitioning [§5.3]

`P=16` processors. 1D: 16 row-blocks; every BFS level may message **all 15** others. 2D with `R=C=4`: each processor communicates within its row (4) and column (4) only — ≤ `R+C−2=6` peers ⇒ less communication; 1D is the `C=1` special case.

## L. XPath/XQuery (legacy question, asked 2023) [§6]

`//book[price>30]/title` — `//` = descendant-or-self axis, predicate filters, `/title` child axis. Axes: child, parent, ancestor, descendant, following-sibling, preceding-sibling, attribute, self.
FLWOR: `for $b in //book let $p := $b/price where $p > 30 order by $b/title return $b/title` (For–Let–Where–Order by–Return).
