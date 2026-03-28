Bhai samajh gaya, clean format mein deta hun! 🔥

---

# Cloud Computing & Distributed Systems - Quick Revision MCQs

---

## 📚 Week 1

**Q1.** A single-site cloud (datacenter) typically includes:

- a) Only end-user laptops
- b) Only Wi-Fi routers and mobile devices
- c) Compute nodes, switches, network topology, storage nodes, and a front-end
- d) Only a single database server

---

**Q2.** Today's clouds have all EXCEPT:

- a) Massive scale
- b) On-demand access
- c) Guaranteed single-thread execution
- d) Data-intensive nature

---

**Q3.** "Flexible compute + storage infrastructure" enabled by virtualization is:

- a) SaaS
- b) PaaS
- c) DaaS
- d) IaaS

---

**Q4.** PaaS most closely provides:

- a) Bare metal hardware access only
- b) Only network cables and switches
- c) A platform/runtime framework to develop and deploy apps
- d) Only office productivity software

---

**Q5.** Virtualization allows:

- a) One OS only per machine
- b) Eliminating the need for operating systems
- c) Only running applications without OS
- d) Concurrent execution of multiple OSs on the same physical machine

---

**Q6.** The Virtualization layer is also called the:

- a) Compiler
- b) Cache controller
- c) Hypervisor / Virtual Machine Monitor (VMM)
- d) DNS resolver

---

**Q7.** Which is true about a Type 1 (bare-metal) hypervisor?

- a) It runs on top of a full host OS
- b) It cannot support VMs
- c) It is only a user-space application
- d) It directly manages hardware resources and runs guest VMs above it

---

**Q8.** In x86 hardware without virtualization, OS executes privileged instructions in:

- a) Ring 3
- b) Ring 2
- c) Ring 1
- d) Ring 0

---

**Q9.** In hotspot mitigation, a black-box approach is:

- a) Using OS and application-level statistics
- b) Requiring source-code changes
- c) Fully OS-and application-agnostic
- d) Possible only with para-virtualized guests

---

**Q10.** Para-virtualization typically requires modifying approximately:

- a) 30% of guest OS code
- b) 10% of guest OS code
- c) 50% of guest OS code
- d) Less than 2% of guest OS code

---

## 📚 Week 2

**Q11.** In server virtualization, which component directly manages physical hardware?

- a) Guest OS
- b) Open vSwitch
- c) Docker Engine
- d) Hypervisor

---

**Q12.** Docker containers primarily share what with other containers?

- a) Hypervisor
- b) Guest kernel
- c) Host kernel
- d) Physical NIC firmware

---

**Q13.** In Docker networking, external connectivity is provided using:

- a) MPLS label switching
- b) iBGP
- c) VLAN flooding
- d) NAT

---

**Q14.** In SR-IOV, a "virtual function" on the NIC is:

- a) A full physical Ethernet port
- b) A software bridge inside the hypervisor
- c) A simple queue with transmit/receive functionality mapped to a VM
- d) A VLAN tag applied by the controller

---

**Q15.** In Open vSwitch, where do the "smarts" (rule/policy decisions) primarily reside?

- a) NIC hardware pipeline
- b) User space
- c) Physical switch firmware
- d) VM guest OS

---

**Q16.** In Open vSwitch, what is primarily handled in the kernel?

- a) High-level policy computation
- b) Tenant topology design
- c) Fast / common-case packet forwarding
- d) Directory-server lookups

---

**Q17.** Mininet is best described as:

- a) A WAN traffic engineering optimizer
- b) A hardware NIC virtualization standard
- c) A proprietary router configuration platform
- d) A network emulator that creates realistic virtual networks on one machine

---

**Q18.** In SDN, the "logically centralized controller" is primarily intended to:

- a) Replace all physical links with wireless
- b) Eliminate the need for data planes
- c) Run only inside NIC firmware
- d) Keep distributed switching devices simple while centralizing intelligence

---

**Q19.** VL2 uses "oblivious routing," meaning a flow's path:

- a) Always follows the shortest path only
- b) Depends on current traffic matrix measurements
- c) Changes per-packet based on CPU load
- d) Does not depend on the current traffic matrix

---

**Q20.** Google's B4 is characterized as:

- a) A public Internet peering solution
- b) A hypervisor for VMs
- c) A Linux container runtime
- d) A private WAN connecting Google data centers globally using SDN ideas

---

## 📚 Week 3

**Q21.** Primary objective of Leader Election in distributed systems:

- a) To elect multiple coordinators for load balancing
- b) To ensure every process becomes a leader at least once
- c) To minimize message delays
- d) To ensure exactly one non-faulty process is chosen as leader

---

**Q22.** Leader election is categorized as which type of problem?

- a) Deadlock detection problem
- b) Mutual exclusion problem
- c) Symmetry-breaking problem
- d) Load balancing problem

---

**Q23.** A valid use of a leader in a distributed system:

- a) Encrypting all inter-process messages
- b) Increasing physical network bandwidth
- c) Coordinating activities such as spanning tree construction
- d) Eliminating message loss

---

**Q24.** Why is leader election impossible in anonymous rings?

- a) Processes may crash arbitrarily
- b) Messages are delivered out of order
- c) The ring is unidirectional
- d) All processors start in identical states and remain indistinguishable

---

**Q25.** Which assumption enables leader election in ring networks?

- a) Synchronous message delivery
- b) Knowledge of ring size
- c) Bidirectional communication
- d) Unique process identifiers

---

**Q26.** The LCR algorithm elects which process as leader?

- a) Process with smallest identifier
- b) Process closest to initiator
- c) Process that sends the first message
- d) Process with largest identifier

---

**Q27.** Worst-case message complexity of the LCR algorithm:

- a) O(n)
- b) O(n log n)
- c) O(log n)
- d) O(n²)

---

**Q28.** In LCR, when a process receives its own identifier back, it:

- a) Forwards the identifier again
- b) Discards the message
- c) Declares itself leader
- d) Starts a new election

---

**Q29.** Overall message complexity of Hirschberg–Sinclair algorithm:

- a) Θ(n²)
- b) Θ(n)
- c) Θ(log n)
- d) Θ(n log n)

---

**Q30.** In Ring Election protocol, messages are forwarded in which direction?

- a) Randomly
- b) Both clockwise and counter-clockwise
- c) Based on process identifier
- d) Clockwise only

---

**Q31.** Worst-case message complexity of Ring Election protocol (without failures):

- a) N
- b) 2N
- c) N²
- d) 3N − 1

---

**Q32.** ZooKeeper is best described as:

- a) A distributed database for big data storage
- b) A message-passing middleware
- c) A centralized locking kernel for coordination
- d) A distributed computation engine

---

**Q33.** Which ZooKeeper design goal ensures updates are processed in strict order?

- a) Replicated
- b) Fast
- c) Durable
- d) Ordered

---

**Q34.** Why does ZooKeeper require a majority of servers?

- a) To reduce latency
- b) To increase throughput
- c) To avoid split-brain and ensure consistency
- d) To reduce storage overhead

---

**Q35.** Which type of znode is automatically deleted when client session ends?

- a) Persistent
- b) Sequential
- c) Replicated
- d) Ephemeral

---

## 📚 Week 4

**Q36.** "Clock skew" refers to:

- a) Difference in clock frequencies
- b) Total time taken by message round trip
- c) Relative difference in clock values at two processes
- d) The number of clocks in the system

---

**Q37.** "Clock drift" refers to:

- a) Relative difference in clock values (timestamps)
- b) Relative difference in clock frequencies (rates)
- c) Difference in CPU speeds
- d) The error in message timestamps only

---

**Q38.** Physical clocks are synchronized to:

- a) GPS-only time
- b) UNIX epoch time
- c) Lamport time
- d) UTC (Universal Coordinated Time)

---

**Q39.** Which statement about synchronization is correct?

- a) Internal synchronization implies external synchronization
- b) External synchronization implies internal synchronization with same D
- c) External and internal synchronization are identical
- d) External synchronization with D implies internal synchronization with 2D

---

**Q40.** Key problem with naive "ask time server" in asynchronous system:

- a) Server time is always wrong
- b) TCP cannot be used
- c) Message latency is unbounded, so error cannot be bounded
- d) Clocks cannot be adjusted forward

---

**Q41.** In Christian's algorithm, the client measures:

- a) One-way delay exactly
- b) CPU utilization
- c) Round-trip time (RTT)
- d) Only server timestamp precision

---

**Q42.** Berkeley's algorithm differs from Christian's primarily because:

- a) It synchronizes clocks to UTC directly
- b) It requires hardware NIC changes
- c) It works only for logical clocks
- d) It uses a master that polls others and computes an average time

---

**Q43.** DTP achieves bounded precision mainly by:

- a) Using application-level retries
- b) Storing timestamps on disk before replying
- c) Running Paxos for every timestamp
- d) Using physical layer support of network devices

---

**Q44.** The "happened-before" (→) relation forms a:

- a) Total order over all events
- b) Random order over events
- c) Partial order over events
- d) Strict FIFO order across all processes

---

**Q45.** In Lamport timestamps, on receiving message with timestamp Tm, process sets:

- a) local = min(local, Tm)
- b) local = local + Tm
- c) local = Tm − 1
- d) local = max(local, Tm) + 1

---

## 📚 Week 5

**Q46.** Primary goal of a consensus algorithm:

- a) Maximize throughput
- b) Detect failures quickly
- c) Synchronize physical clocks
- d) Ensure all non-faulty processes agree on the same value

---

**Q47.** Consensus is provably solvable in which system model?

- a) Fully asynchronous with unbounded delays
- b) Anonymous message-passing systems
- c) Synchronous with bounded message and processing delays
- d) Systems with Byzantine failures and no bounds

---

**Q48.** Paxos is best described as:

- a) Unsafe but fast
- b) Fully synchronous
- c) Byzantine-fault tolerant
- d) Safe and eventually live

---

**Q49.** In Paxos, who can initiate a new round of consensus?

- a) Only the leader
- b) Only the process with highest ID
- c) Only a majority quorum
- d) Any process at any time

---

**Q50.** Paxos may restart rounds repeatedly because:

- a) Messages are always lost
- b) The protocol violates safety
- c) Acceptors refuse proposals
- d) Leaders may fail or messages may be delayed

---

**Q51.** A Byzantine fault is characterized by a component that:

- a) Stops execution permanently
- b) Fails only by crashing
- c) Misses deadlines only
- d) Behaves arbitrarily, possibly maliciously

---

**Q52.** Which fault model is most difficult to tolerate?

- a) Crash fault
- b) Omission fault
- c) Fail-stop fault
- d) Byzantine fault

---

**Q53.** An SLA consists of:

- a) SLIs only
- b) SLOs only
- c) Hardware guarantees only
- d) SLOs plus consequences for violation

---

**Q54.** The "domino effect" occurs when:

- a) Failures propagate through hardware
- b) Messages are lost permanently
- c) One rollback forces many other processes to roll back
- d) All checkpoints are coordinated

---

**Q55.** Agreement algorithms for Byzantine processes depend heavily on:

- a) Faulty link detection
- b) Message encryption only
- c) Physical clock accuracy
- d) Synchrony assumptions and failure models

---

## 📚 Week 6

**Q56.** The key-value abstraction is best described as:

- a) A directed graph of objects
- b) A log-structured file system
- c) A relational schema with joins
- d) A dictionary data structure mapping keys to values

---

**Q57.** A key reason NoSQL gained popularity:

- a) It requires stronger schemas than SQL
- b) It emphasizes foreign keys and joins
- c) Modern data is often large and unstructured with many random reads/writes
- d) It runs only on a single machine

---

**Q58.** Column-oriented storage is useful mainly because:

- a) It makes joins faster
- b) It eliminates indexes
- c) It guarantees linearizability
- d) It allows faster range searches within a column without fetching all columns

---

**Q59.** Cassandra maps keys to servers using:

- a) Partitioner
- b) Coordinator
- c) Snitch
- d) Bloom Filter

---

**Q60.** "Hinted Handoff" ensures Cassandra is "always writable" by:

- a) Rejecting writes when any replica is down
- b) Forcing all writes to ALL replicas
- c) Disabling replication temporarily
- d) Buffering writes and later delivering them to recovered replicas

---

**Q61.** Cassandra's SSTables are:

- a) Mutable files updated in place
- b) Used only for caching
- c) Only indexes without data
- d) Immutable files sorted by key

---

**Q62.** A Bloom filter in Cassandra guarantees:

- a) No false positives and no false negatives
- b) Possible false negatives but no false positives
- c) Always exact membership check
- d) Possible false positives but no false negatives

---

**Q63.** In CAP theorem, when partitions must be tolerated, you must choose between:

- a) Latency and throughput
- b) Storage and compute
- c) Consistency and availability
- d) Encryption and compression

---

**Q64.** Cassandra prioritizes:

- a) Strong consistency and availability, but not partition tolerance
- b) Strong consistency and partition tolerance, but not availability
- c) Only consistency (ACID)
- d) Eventual consistency, availability, and partition tolerance

---

**Q65.** "Eventual consistency" means if writes stop for a key:

- a) Replicas diverge permanently
- b) All replicas eventually converge to the same value
- c) Reads always return the latest value immediately
- d) The key becomes unavailable

---

**Q66.** LOCAL_QUORUM differs from QUORUM because LOCAL_QUORUM:

- a) Requires all DCs to respond
- b) Requires only one replica
- c) Disables read repair
- d) Waits for quorum only within the coordinator's datacenter

---

**Q67.** Causal consistency requires that reads:

- a) Always return the latest write in real time
- b) Execute in a single global total order
- c) Never return stale values
- d) Respect partial order based on information flow

---

**Q68.** In HBase, if timestamp is not specified during a read:

- a) The oldest version is returned
- b) The operation fails
- c) A random version is returned
- d) The latest version is returned

---

**Q69.** HBase ensures strong write durability using:

- a) Gossip membership
- b) Hinted handoff
- c) Write-ahead log (HLog) before MemStore updates
- d) Quorum reads only

---

## 📚 Week 7

**Q70.** A DHT supports insert/lookup/delete in a:

- a) Single-machine setting only
- b) Only SQL setting
- c) Distributed setting using keys
- d) Only centralized setting

---

**Q71.** In Chord, a key K is stored at:

- a) The peer with the smallest ID
- b) The peer whose ID equals K always
- c) Any random peer for load balancing
- d) The first peer with ID ≥ K (mod 2^m)

---

**Q72.** Virtual nodes are used primarily to:

- a) Reduce disk latency
- b) Prevent node joins
- c) Enforce strong consistency
- d) Improve load balancing by reducing variance

---

**Q73.** The map() function outputs:

- a) Intermediate key/value pairs
- b) Final sorted output files
- c) Only a single global value
- d) Only metadata pointers

---

**Q74.** The reduce() function primarily:

- a) Splits files into chunks
- b) Performs DNS lookup
- c) Encrypts intermediate data
- d) Merges all intermediate values associated with the same key

---

**Q75.** In MapReduce, shuffle refers to:

- a) CPU scheduling inside a node
- b) Encrypting data at rest
- c) Transfer of intermediate map outputs to reducers based on key partitioning
- d) Replicating HDFS blocks

---

**Q76.** In YARN, cluster-wide scheduling is done by:

- a) Node Manager (NM)
- b) Tracker
- c) Application Master (AM)
- d) Resource Manager (RM)

---

**Q77.** The Application Master (AM) mainly handles:

- a) Only disk replication
- b) DNS resolution
- c) Maintaining the global file system namespace
- d) Container negotiation and task failure handling for its job

---

**Q78.** "Stragglers" in MapReduce are:

- a) Tasks that finish first
- b) Failed tasks only
- c) Reducers that never shuffle
- d) Slow tasks that delay job completion

---

**Q79.** A combiner is beneficial when the reduce operation is:

- a) Non-deterministic
- b) Dependent on global ordering only
- c) Only for encryption
- d) Commutative and associative, enabling local partial aggregation

---

## 📚 Week 8

**Q80.** Key limitation of MapReduce that motivated Spark:

- a) Lack of fault tolerance
- b) Inability to run on commodity clusters
- c) Absence of parallelism
- d) High cost of repeatedly writing intermediate data to disk

---

**Q81.** Spark was originally developed at:

- a) Google Research
- b) Microsoft Research
- c) Apache Foundation
- d) UC Berkeley AMPLab

---

**Q82.** An RDD in Spark is best described as:

- a) A mutable table stored in HDFS
- b) A centralized in-memory cache
- c) An immutable, partitioned collection of records
- d) A disk-only dataset

---

**Q83.** RDD fault tolerance is primarily achieved using:

- a) Replication of all data
- b) Write-ahead logging
- c) Distributed locking
- d) Lineage information

---

**Q84.** Which is an example of an action in Spark?

- a) map()
- b) filter()
- c) join()
- d) count()

---

**Q85.** In PageRank, each page contributes to neighbors:

- a) Its full rank to every neighbor
- b) rankₚ × |neighborsₚ|
- c) A constant value
- d) rankₚ / |neighborsₚ|

---

**Q86.** Which Spark library is used for graph processing?

- a) Spark SQL
- b) Spark Streaming
- c) MLlib
- d) GraphX

---

**Q87.** The GAS model in GraphX is used to:

- a) Execute SQL queries
- b) Replicate graph data
- c) Encrypt graph edges
- d) Perform iterative graph computations

---

**Q88.** Apache Kafka is best described as:

- a) A relational database
- b) A batch analytics engine
- c) A real-time distributed messaging system
- d) A file system

---

**Q89.** In Kafka's data model, messages are grouped into:

- a) Streams
- b) Queues
- c) Tables
- d) Topics

---

**Q90.** In Kafka, the server process that stores and serves messages is called:

- a) Producer
- b) Consumer
- c) Connector
- d) Broker

---

**Q91.** Kafka supports which messaging models?

- a) Only queue-based
- b) Only publish-subscribe
- c) Request-reply only
- d) Both queue and publish-subscribe

---

**Q92.** A Kafka topic with replication factor N can tolerate:

- a) N failures
- b) Only one failure
- c) No failures
- d) N−1 broker failures

---
---

# ✅ ANSWERS

| Q | Ans | | Q | Ans | | Q | Ans | | Q | Ans |
|---|-----|-|---|-----|-|---|-----|-|---|-----|
| 1 | c | | 24 | d | | 47 | c | | 70 | c |
| 2 | c | | 25 | d | | 48 | d | | 71 | d |
| 3 | d | | 26 | d | | 49 | d | | 72 | d |
| 4 | c | | 27 | d | | 50 | d | | 73 | a |
| 5 | d | | 28 | c | | 51 | d | | 74 | d |
| 6 | c | | 29 | d | | 52 | d | | 75 | c |
| 7 | d | | 30 | d | | 53 | d | | 76 | d |
| 8 | d | | 31 | d | | 54 | c | | 77 | d |
| 9 | c | | 32 | c | | 55 | d | | 78 | d |
| 10 | d | | 33 | d | | 56 | d | | 79 | d |
| 11 | d | | 34 | c | | 57 | c | | 80 | d |
| 12 | c | | 35 | d | | 58 | d | | 81 | d |
| 13 | d | | 36 | c | | 59 | a | | 82 | c |
| 14 | c | | 37 | b | | 60 | d | | 83 | d |
| 15 | b | | 38 | d | | 61 | d | | 84 | d |
| 16 | c | | 39 | d | | 62 | d | | 85 | d |
| 17 | d | | 40 | c | | 63 | c | | 86 | d |
| 18 | d | | 41 | c | | 64 | d | | 87 | d |
| 19 | d | | 42 | d | | 65 | b | | 88 | c |
| 20 | d | | 43 | d | | 66 | d | | 89 | d |
| 21 | d | | 44 | c | | 67 | d | | 90 | d |
| 22 | c | | 45 | d | | 68 | d | | 91 | d |
| 23 | c | | 46 | d | | 69 | c | | 92 | d |

---

**All the best bhai! 💪🔥**
