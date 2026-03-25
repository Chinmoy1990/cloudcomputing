I've got your Week 6 notes — it covers two big lectures: **Design of Key-Value Stores (with Cassandra)** and **Design of HBase**. Let me go through everything thoroughly.

---

# WEEK 6 — Cloud Computing and Distributed Systems
## Prof. Rajiv Misra, IIT Patna

---

# LECTURE 16: Design of Key-Value Stores

---

## 1. Key-Value Abstraction — Kya hai ye cheez?

Professor ne samjhaya ki aajkal jo bhi internet services hain — sab ke sab basically ek key aur uski value ke around ghoomti hain.

**Examples diye unhone:**
- **Flipkart** — item number (key) → product info like cost, name, manufacturer (value)
- **EaseMyTrip** — flight number (key) → flight details, seat availability (value)
- **Twitter** — tweet id (key) → tweet content, who sent it (value)
- **Online Banking** — account number (key) → balance, owner info (value)

Toh basically har business mein ye key-value pattern dikhta hai. Isliye cloud storage systems ne isko as an abstraction provide karna shuru kiya.

---

## 2. Key-Value Store as a Data Structure

Professor bole — ye koi naya concept nahi hai. Ye basically ek **dictionary data structure** hai — insert, lookup, delete by key. Jaise hash table ya binary tree.

**Lekin problem kya hai?** Ye sab single server pe chalte hain. Jab data bohot bada ho jaaye, toh ek server kaam nahi karega. Isliye **distributed system** chahiye.

Yaad karo **Distributed Hash Tables (DHT)** jo P2P systems mein padhe the — key-value stores bhi wahi techniques reuse karte hain.

---

## 3. RDBMS vs Key-Value Store — Comparison

Professor ne pehle RDBMS recall karaya:
- **RDBMS** = Relational Database Management System (e.g., MySQL)
- Data tables mein stored hota hai, **schema-based** (structured)
- Har row ka ek **primary key** hota hai
- **SQL** se query karte hain
- **Joins** support hote hain (multiple tables ka Cartesian product)

**Example diya:** Users table aur Blog table. Users mein blog_id ek **foreign key** hai jo Blog table ke primary key se link karta hai. SQL query se join karke dono tables ka combined data nikal sakte hain.

---

## 4. Mismatch with Today's Workloads — Aaj RDBMS kaam kyun nahi karta?

Professor ne clearly bataya ki aaj ka data RDBMS ke liye fit nahi hota:
- **Data is large and unstructured** — schema banana mushkil hai
- **Lots of random reads and writes** — millions of clients se aa rahe hain
- **Sometimes write-heavy** — reads se zyada writes
- **Foreign keys rarely needed**
- **Joins infrequent**

---

## 5. Needs of Today's Workloads

Modern storage systems ko chahiye:
- **Speed**
- **No Single Point of Failure (SPoF)**
- **Low TCO** (Total Cost of Operation + Ownership)
- **Fewer system administrators**
- **Incremental Scalability**
- **Scale OUT, not Scale UP**

### Scale Out vs Scale Up:
- **Scale Up** = purani machines replace karo zyada powerful se → expensive, traditional approach
- **Scale Out** = incrementally naye commodity (COTS) machines add karo → cheaper, aaj sab companies yahi karte hain

---

## 6. Key-Value / NoSQL Data Model

**NoSQL = "Not Only SQL"**

Basic API operations:
- **get(key)** — value fetch karo
- **put(key, value)** — value update karo

### Tables ka concept:
- Cassandra mein → **Column Families**
- HBase mein → **Table**
- MongoDB mein → **Collection**

### RDBMS se kaise alag hai:
- **Unstructured** — no schema, kuch columns kuch rows mein missing ho sakte hain
- **No foreign keys**
- **Joins usually not supported**
- But **index tables** ho sakte hain (RDBMS jaisa)

---

## 7. Column-Oriented Storage

Professor ne important distinction bataya:
- **RDBMS** — poori row ek saath store hoti hai (row-oriented)
- **NoSQL** — column together store hota hai (column-oriented)

**Kyun useful hai?** Range queries fast hoti hain. Example: "Past month mein updated blog_ids do" — sirf last_updated column search karo, poora database fetch nahi karna padega.

---

# LECTURE 16 (Part 2): Design of Apache Cassandra

---

## 8. Cassandra — Introduction

- **Distributed key-value store**
- Datacenter mein chalti hai (aur across DCs bhi)
- **Originally Facebook ne design ki**, baad mein open-source → Apache project
- Companies using it: IBM, Adobe, HP, eBay, Twitter, Netflix, PBS Kids
- Netflix Cassandra se video playback position track karti hai

---

## 9. Inside Cassandra: Key → Server Mapping

Cassandra **Chord-like ring-based DHT** use karti hai, lekin **bina finger tables aur routing ke**.

Key-to-server mapping **Partitioner** karta hai.

**Example:** Ring mein m=7, nodes mapped hain. Client K13 padhna chahta hai → Partitioner hash karega → nearest server (N16) pe store hoga. **Replicas** bhi banti hain — next consecutive servers pe (N32, N45). Toh K13 ki 3 copies hain.

**One ring per datacenter.**

---

## 10. Data Placement Strategies

**Two strategies:**

### (a) SimpleStrategy — uses Partitioner
- **RandomPartitioner** — Chord jaisi hash partitioning
- **ByteOrderedPartitioner** — key ranges servers ko assign karta hai (range queries ke liye accha, e.g., "sab Twitter users a se b tak do")

### (b) NetworkTopologyStrategy — multi-datacenter ke liye
- 2 ya 3 replicas per DC
- Pehla replica Partitioner se, baaki clockwise ring pe different rack pe

---

## 11. Snitches

Snitches IPs ko racks aur DCs se map karte hain. Config file: `cassandra.yaml`

Types:
- **SimpleSnitch** — topology aware nahi (rack-unaware)
- **RackInferring** — IP address ke octets se topology guess karta hai (e.g., 101.102.103.104 → x.DC.rack.node)
- **PropertyFileSnitch** — config file use karta hai
- **EC2Snitch** — AWS ke liye (EC2 Region = DC, Availability Zone = rack)

---

## 12. Writes in Cassandra

- Client ek **coordinator node** ko write bhejta hai
- Coordinator per-key, per-client, ya per-query ho sakta hai
- **Per-key coordinator** ensures writes are **serialized** for that key
- Coordinator Partitioner se query **sab replica nodes** ko bhejta hai
- Jab **X replicas** respond karte hain → coordinator client ko acknowledgement deta hai

### Hinted Handoff Mechanism (Always Writable):
- Agar koi replica down hai → coordinator baaki sab ko write karta hai + locally store karta hai jab tak down replica wapas aaye
- Agar SAB replicas down → coordinator front-end pe writes **buffer** karta hai (kuch ghanton tak)

### Multi-DC:
- Har DC ka ek ring
- Per-DC ek coordinator elect hota hai (dusre DCs se coordinate karne ke liye)
- Election **Zookeeper** se hoti hai (Paxos-like consensus)

---

## 13. Writes at a Replica Node — Step by Step

Jab replica ko write milti hai:

**Step 1:** Disk pe **commit log** mein likhta hai (failure recovery ke liye)

**Step 2:** **Memtable** mein changes karta hai
- Memtable = in-memory representation of key-value pairs
- Append-only data structure → fast
- Cache hai jo key se search ho sakti hai
- **Write-back** approach (write-through nahi)

**Step 3:** Jab memtable full ya old ho jaaye → **disk pe flush** hota hai:
- **SSTable** (Sorted String Table) — key-value pairs sorted by key
- SSTables **immutable** hain — ek baar bani toh change nahi hoti
- **Index file** — (key, position in SSTable) pairs
- **Bloom filter** — efficient search ke liye

---

## 14. Bloom Filter — Bohot Important Concept!

Professor ne detail mein samjhaya:
- **Compact way** to represent a set of items
- Check karna cheap hai ki item set mein hai ya nahi
- **False positives possible** — item set mein nahi hai lekin "hai" bol sakta hai
- **False negatives NEVER** — agar item hai toh kabhi "nahi hai" nahi bolega

### Kaise kaam karta hai:
- K hash functions hain
- Har hash function ek bit position pe map karta hai (0 to 127 type bitmap)
- **Insert:** sab hash bits ko 1 kar do
- **Check:** agar sab hash bits 1 hain → true (maybe present). Agar koi 0 hai → definitely not present.

### False Positive Rate:
- 4 hash functions, 100 items, 3200 bits → FP rate = **0.02%** (bohot kam!)
- Bits badhao → FP rate aur kam

---

## 15. Compaction

- Time ke saath SSTables aur logs accumulate hote hain
- **Compaction** = SSTables merge karta hai (ek key ke updates combine)
- **Periodically** aur **locally** har server pe chalta hai

---

## 16. Deletes in Cassandra

- Item turant delete NAHI hota
- Ek **tombstone** log mein add hota hai
- Jab compaction tombstone ko encounter karta hai → tab actually delete hota hai

---

## 17. Reads in Cassandra

- Write jaisa hi, lekin coordinator **X replicas** ko contact karta hai (same rack mein)
- Jo replicas past mein **sabse fast respond** kiye the, unko send karta hai
- X replicas respond karein → coordinator **latest timestamped value** return karta hai
- Background mein coordinator baaki replicas se bhi value fetch karta hai
- Agar values different hain → **read repair** initiate hota hai (eventually sab replicas update)
- Replica pe ek row multiple SSTables mein split ho sakti hai → **reads slower than writes** (but still fast)

---

## 18. Membership — Gossip-Style Failure Detection

Har server coordinator ban sakta hai, toh sabko pata hona chahiye ki kaun kaun alive hai.

### Gossip Protocol:
- Nodes periodically apni **membership list gossip** karte hain (random neighbors ko)
- Receive karne pe local list update hoti hai
- Agar kisi ka heartbeat **Tfail** se purana hai → node **failed** mark

### Accrual Failure Detector:
- **PHI** value output karta hai representing suspicion level
- PHI(t) = -log(CDF(t_now - t_last)) / log10
- Historical inter-arrival times consider karta hai
- Practice mein **PHI = 5** → detection time **10-15 seconds**

---

## 19. Cassandra vs RDBMS — Performance

Data > 50 GB pe:

| | MySQL | Cassandra |
|---|---|---|
| Write latency | 300 ms | **0.12 ms** |
| Read latency | 350 ms | **15 ms** |

**Orders of magnitude faster!** Lekin kya khoye? → **Consistency** (CAP theorem)

---

# CAP THEOREM — Bohot Important for Exam!

---

## 20. CAP Theorem

**Proposed by Eric Brewer (Berkeley), proved by Gilbert & Lynch (NUS, MIT)**

Distributed system mein **at most 2 out of 3** guarantee kar sakte ho:

1. **Consistency (C)** — sab nodes same data dekhein, read latest written value return kare
2. **Availability (A)** — system hamesha operations allow kare, operations jaldi complete hon
3. **Partition-tolerance (P)** — network partitions ke baawajood system kaam kare

---

## 21. Why Each Matters:

### Availability:
- Amazon pe 500ms latency badhne se **20% revenue drop**
- Har added millisecond = **$6M yearly loss** at Amazon
- 1 second se zyada wait → user ka mind kahi aur chala jaata hai (cognitive drift)
- SLAs mainly latency pe focus karte hain

### Consistency:
- Bank account multiple devices se access karo → updates dikh chahiye sabko
- Flight booking → hazaaron customers ke updates sab clients ko accessible hone chahiye

### Partition-tolerance:
- Internet disconnect ho sakta hai (router outage, undersea cable cut, DNS fail)
- Datacenter ke andar bhi partition ho sakta hai (rack switch outage)
- Fir bhi system normally kaam karna chahiye

---

## 22. CAP Tradeoff — Which Systems Choose What?

| System | Guarantees | Compromises |
|---|---|---|
| **RDBMS (non-replicated)** | C + A | Not partition tolerant |
| **Cassandra, RIAK, Dynamo, Voldemort** | A + P | Consistency (eventual) |
| **HBase, HyperTable, BigTable, Spanner** | C + P | Availability |

Cloud mein partition-tolerance zaroori hai → choice hai **consistency ya availability** mein.

---

## 23. Eventual Consistency

Cassandra ka model:
- Agar writes ruk jaayein → sab replicas **eventually converge** kar jayengi
- Agar writes chalte rahein → system hamesha converge karne ki koshish karta hai
- **Stale values return** ho sakte hain (back-to-back writes mein)
- Low writes ke period mein system jaldi converge karta hai

**Origin:** Amazon's Dynamo, LinkedIn's Voldemort

---

## 24. ACID vs BASE

| RDBMS (ACID) | Key-Value Stores (BASE) |
|---|---|
| **A**tomicity | **B**asically **A**vailable |
| **C**onsistency | **S**oft-state |
| **I**solation | **E**ventual Consistency |
| **D**urability | Prefers Availability over Consistency |

---

## 25. Consistency Levels in Cassandra

Client har operation ke liye consistency level choose kar sakta hai:

| Level | Meaning | Speed | Consistency |
|---|---|---|---|
| **ANY** | Koi bhi server (replica bhi nahi hona zaroori) | Fastest | Weakest |
| **ONE** | Kam se kam 1 replica | Fast | Cannot tolerate failure |
| **QUORUM** | Majority across all DCs | Medium | Strong |
| **LOCAL_QUORUM** | Majority in coordinator's DC only | Faster than QUORUM | Local strong |
| **EACH_QUORUM** | Majority in every DC | Hierarchical | Strong per DC |
| **ALL** | Sab replicas | Slowest | Strongest |

---

## 26. Quorum — How it Ensures Consistency

**Quorum = Majority (> 50%)**

Key insight: **Koi bhi do quorums mein at least ek common server hoga.**

Example: 5 replicas, quorum = 3. Client 1 red quorum mein write karta hai. Client 2 blue quorum se read karta hai. Blue quorum mein kam se kam ek server pe latest write hogi → client ko latest value milega.

**Quorum conditions:**
1. **W + R > N** (write replicas + read replicas > total replicas)
2. **W > N/2**

### Application ke hisaab se values:
- **(W=1, R=1)** — very few writes and reads
- **(W=N, R=1)** — read-heavy workloads
- **(W=N/2+1, R=N/2+1)** — write-heavy workloads
- **(W=1, R=N)** — write-heavy, mostly one client writing per key

---

## 27. Newer Consistency Models (Eventual → Strong)

Professor ne ek spectrum dikhaya:

**Eventual ←→ Strong**

Beech mein aate hain:

### (a) Per-key Sequential
- Ek key ke liye sab operations ka **global order** hona chahiye
- Scalability problem hai

### (b) CRDTs (Commutative Replicated Data Types)
- Data structures jinme **commutated writes same result** dete hain
- Example: value = integer, sirf +1 operation allowed → order matter nahi karta
- Servers ko consistency ki chinta nahi

### (c) Red-Blue Consistency
- Client transactions ko **red** aur **blue** operations mein separate karo
- **Blue** = commutative, any order across DCs
- **Red** = same order at each DC

### (d) Causal Consistency
- Reads ko **information flow ka partial order** respect karna chahiye
- Example: A writes K1=33 → B reads K1 (gets 33) → B writes K2=55 → C reads K2 (gets 55) → C reads K1 → **MUST return 33** (causal path hai)

### (e) Strong Consistency Models:
- **Linearizability** — operation instantaneously visible to all clients (real-time)
- **Sequential Consistency** (Lamport) — sab processors ka ek reasonable sequential order mil jaaye jo sanity maintain kare

### NewSQL:
- Newer key-value stores jo ACID support karte hain: **Hyperdex** (Cornell), **Spanner** (Google), **Transaction Chains** (Microsoft Research)

---

# LECTURE 17: Design of HBase

---

## 28. HBase — Kya Hai?

- **Open-source NoSQL database**
- **Distributed column-oriented** data store
- **Horizontally scalable** to 1000s of commodity servers + petabytes of storage
- Runs on top of **HDFS** (Hadoop Distributed File System) — HDFS se scalability, fault tolerance, high availability milti hai
- **Google's BigTable** ka implementation — Yahoo ne open-source kiya
- Works with structured, unstructured, semi-structured data

### Scale Out (Horizontal Scaling):
Professor ne chariot analogy diya — zyada horses add karte jao → zyada power. Yahi horizontal scaling hai — zyada commodity servers add karo.

---

## 29. HBase vs Cassandra — Key Difference

Dono NoSQL hain, dono BASE model follow karte hain. **LEKIN:**
- **Cassandra** → Availability prefer karti hai (A+P in CAP)
- **HBase** → **Consistency prefer karta hai** (C+P in CAP)

---

## 30. HBase API Functions

- **Get/Put(row)** — ek row read/write
- **Scan(row range, filter)** — range queries support
- **MultiPut** — multiple puts ek saath

---

## 31. HBase Architecture

### Components:
1. **Client** — RegionServers se directly communicate karta hai
2. **HBase Master (HMaster)** — cluster monitor karta hai, sab RegionServer instances pe nazar rakhta hai
3. **Region Servers** — regions serve aur manage karte hain, HDFS DataNode pe run karte hain
4. **Zookeeper** — distributed coordination (fault-tolerant master, data nodes alive check)

### Structure:
- Table → split into **Regions** (row ranges)
- Regions → vertically divided by **Column Families** into **Stores**
- Stores → saved as files on **HDFS**

---

## 32. HBase Data Model — Detail Mein

### Row Key:
- Primary key jaisa
- Records **sorted order** mein stored hain according to row key
- Row keys ka koi data type nahi — always **byte array**

### Column Family:
- Row ke data ko group karta hai
- Har row mein same column families hain (but sab mein data hona zaroori nahi — null ho sakta hai)
- Column Family name + Column name → **Column Qualifier** (colon se separate, e.g., `personaldata:Name`)
- Har column family **separate HFile** mein stored hoti hai → separately access ho sakti hai

### Cell:
- Row key + Column Family + Column Qualifier + Timestamp = **Cell**
- Cell ka data = **Value**
- Full cell with structural info = **Key Value**

### Timestamp:
- Cell values **versioned** hain
- Default mein **timestamp** version number hai
- Read pe timestamp specify nahi kiya → **latest version** return hota hai
- Default **3 versions** retain hoti hain per column family

---

## 33. HBase Architecture — Detailed View

```
Client → HMaster (via Zookeeper)
       → HRegionServer
           → HLog (Write-Ahead Log)
           → HRegion
               → Store + MemStore
                   → StoreFile → HFile → HDFS
```

- **HRegionServer** mein **HLog** hai (write-ahead log)
- Har HRegionServer mein multiple **HRegions**
- Har HRegion mein multiple **Stores**
- Har Store mein **MemStore** (in-memory) + **StoreFiles** (on disk)
- StoreFiles contain **HFiles** → stored in HDFS

---

## 34. HBase Storage Hierarchy

- **HBase Table** → split into multiple **Regions** (replicated across servers)
- **Column Family** = subset of columns with similar query patterns
- **One Store** per combination of ColumnFamily + Region
- **MemStore** per Store — in-memory updates, flushed to disk when full
- **StoreFiles** — where actual data lives, contains HFiles
- **HFile** = SSTable from Google's BigTable

---

## 35. HFile Structure

```
[Data] [Data] ... [Metadata, file info, indices, trailer]
  ↓
[Magic] [Key,Value] [Key,Value] ... [Key,Value]
  ↓
Key = [Key length | Value length | Row length | Row | Col Family length | Col Family | Col Qualifier | Timestamp | Key type]
Value = actual data
```

Example: Row = SSN:000-01-2345, Col Family = Demographic Information, Col Qualifier = Ethnicity

---

## 36. Strong Consistency in HBase — Write-Ahead Log (WAL)

**Bohot important concept!**

Jab client HRegionServer ko keys (k1, k2, k3, k4) bhejta hai:

**Step 1: HLog mein PEHLE likho** (Write-Ahead Log) → failure recovery ke liye

**Step 2: Phir MemStore mein likho**

MemStore full/old hone pe → HFile mein flush → HDFS pe

### Log Replay:
- Failure se recover hone pe / boot-up pe
- HRegionServer + HMaster stale logs **replay** karte hain
- Timestamps se pata chalte hai database kahan tak pahunchi thi
- Replay = edits MemStore mein add karo

---

## 37. Cross-Datacenter Replication

- Single **Master** cluster
- Other **Slave** clusters same tables replicate karte hain
- Master cluster **synchronously HLogs** slave clusters ko bhejta hai
- Coordination → **Zookeeper** (file system jaisa use hota hai control info ke liye)
  - `/hbase/replication/state`
  - `/hbase/replication/peers/<peer cluster number>`
  - `/hbase/replication/rs/<hlog>`

---

## 38. Auto Sharding and Distribution

- **Unit of scalability** = Region
- Region = sorted, contiguous range of rows
- Regions **randomly spread** across RegionServers
- **Load balancing aur failover** ke liye move hote hain
- Data grow hone pe **automatically ya manually split** hote hain
- Capacity = cluster nodes vs regions per node

### Example:
Logical table (A to S sorted rows) → split into regions (A-F, G-L, M-S) → each region assigned to RegionServers → Client directly access karta hai Zookeeper/HMaster se info lekar

### Distribution:
Rows ke splits random order mein different RegionServers pe jaate hain. E.g., Region Server 1 pe Keys [A-C] aur [T-Z] dono ho sakte hain. Master ye info maintain karta hai.

---

## 39. Bloom Filter in HBase

- HFile **persist** hone pe Bloom Filters generate hoti hain
- HFile ke **end mein stored** hoti hain
- Memory mein **load** hoti hain
- **Row level** pe check allow karti hain (row + column level bhi possible, but overhead zyada)
- Poore **StoreFiles ko reads se filter** kar sakti hain
- Useful jab data **grouped** ho ya jab **many misses expected** hon (non-existing keys search)

Example: "row-R" kahan hai? Block index se 6 seeks lagenge, but Bloom filter se sirf 2 seeks (kyunki baaki files ko filter out kar diya)

---

## 40. Fold, Store, and Shift

**Important concept for HFile storage!**

- **Logical layout ≠ Physical layout**
- Sab values **full coordinates** ke saath store hote hain: Row Key, Column Family, Column Qualifier, Timestamp
- Columns ko **"row per column"** mein fold karta hai
- **NULLs cost-free** — kuch store nahi hota
- **Multiple versions** = multiple rows in folded table

### Example:
Table mein r1 to r6 rows, cf1 aur cf2 column families. Jab fold hota hai:
- cf1 ka data ek StoreFile mein (e.g., "cf1/1234")
- cf2 ka data alag StoreFile mein (e.g., "cf2/5678")
- Multiple timestamps = multiple entries (time-series data support!)

---

## 41. HBase Conclusion

- Traditional RDBMS → strong consistency, ACID
- Modern workloads → fast response times chahiye (availability)
- CAP theorem → 2 out of 3
- **Cassandra** → A + P (eventual consistency)
- **HBase** → **C + P** (consistency over availability)
- NoSQL/Key-value systems → BASE (Basically Available, Soft-state, Eventual Consistency)

---

# QUICK REVISION TABLE — Cassandra vs HBase

| Feature | Cassandra | HBase |
|---|---|---|
| Based on | Amazon Dynamo + Google BigTable ideas | Google BigTable |
| Open-sourced by | Facebook → Apache | Yahoo → Apache |
| Storage | SSTable (Sorted String Table) | HFile (SSTable equivalent) |
| Runs on | Own ring-based DHT | HDFS |
| CAP Choice | **A + P** | **C + P** |
| Consistency | Eventual (configurable levels) | Strong (Write-Ahead Log) |
| Key mapping | Partitioner (ring-based) | RegionServer (range-based) |
| Membership | Gossip protocol | Zookeeper |
| Scaling | Ring-based, add nodes | Auto sharding, add RegionServers |
| Write path | Commit log → Memtable → SSTable | HLog (WAL) → MemStore → HFile |
| Bloom Filter | Used on SSTables | Used on HFiles |
| Delete mechanism | Tombstone → compaction deletes | Similar approach |
| Replication | Ring-based (consecutive nodes) | HDFS replication (default 3) + cross-DC via HLogs |
| Column storage | Column families | Column families → HFiles |

---

Ye poora Week 6 ka material hai. Koi specific topic aur detail mein chahiye ya practice questions chahiye toh bol de!
