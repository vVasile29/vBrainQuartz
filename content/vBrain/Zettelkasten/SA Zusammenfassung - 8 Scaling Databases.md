## 7. Scaling Databases

> [!warning] Distributed Applications are often still bbound by the scalability of the DB
> Services scale horizontal while DB scales vertical ⚡️
> → Vertical scaling (increasing size/power of server) is very limited + single point of failure

### 7.1 Database Scaling (horizontally)

#### Database Replication

> [!definition] Definition
> simple approach; Replicate Server with its associated data + load balancer to distribute requests

##### Master-Slave Replication

- Master has read-write priviliges
- Slaves only have read privileges
- Synchronize Slaves with Master afterwards (inconsistencies)
- if master node fails, system continues read-only until new master is created or slave is promotedy

![[Pasted image 20240722095442.png|400]]

##### Master-Master-Replication

- All Replicas can also do read-write
- nodes coordinate with each other on writes
- if one node goes down, system can continue with read-writes

![[Pasted image 20240722095643.png|400]]

##### Replication Disadvantages

> [!fail] Disadvantages
> - Dataloss if master fails
> - Drastically increases storage consumption
> - Synchronisation traffic/latency

#### Database Federation

- split up data by function/domain

![[Pasted image 20240722100039.png|300]]

> [!success] Advantages
> - Splits up load
> - higher performance (better cache-hits in smaller DB)

> [!fail] Disadvantages
> - Load might be concentrated on single table (e.g. USER)
> - Additional complexity in Application Logic (aggregate data, coordinate Queries)

#### Database Sharding

> [!info] Definition
> Distribute Entities among different DB servers (e.g User Table from A-M and N-Z in different DB servers)
> → Dynamically adapt shards during runtime

> [!success] Advantages
> - parallel writes (on different shards)
> - adaptible splitting of data

> [!fail] Disadvantages
> - Application knowledge needs to be updated to access the correct shard
> - good sharding function required
> - joining data amoung shards is complex

#### Data Partitioning

- Replication (reliability due to replicas) + Federation (Distribute by functionality) + Sharding (distribute large/high-traffic DBs)
→ availability + high throughput

#### Other Techniques

- denormalization (allow duplicates)
- caching
- SQL Tuning (Benchmark)
- Specialized DB Type

### 7.2 Database Characteristics

1. Ease of Learning
2. Scalability/Throughput
3. Consistency
4. Language Support/Maturity/Community
5. Availability/Partition Tolerance (ability to support higher availability)
6. Read/Write Priority (focus on ready or writes or neither)

#### CAP Theorem (pick 2!)

![[Pasted image 20240722164727.png]]

- Consistency: every read receives most recent write
- Availability: every request receives a response (without consistency guaranteed)
- **Partition Tolerance (must)**: Continuous operating despite arbitrary partitioning due to network failure
→ ==network not reliable in distributed systems, all systems must support partition tolerance==

##### Different Consistency modes

- strong: after a write, reads will see it, data replicated synchronously (Relational DBs, File Systems, ACID) → CP
- eventual: after a write, reads will eventually see it, data replicated asynchronously (Blockchain, DNS, Mail) → AP
- weak: after a write, reads may or may not see it (VOIP, Video chats, multiplayer games)

### 7.3 Database Types

#### Relational Databases

- RDBMSs (Management Systems for smaller non-distributed systems)
- de-facto standard
- strong consistency and SQL
- MySql, PostgreSql

![[Pasted image 20240722193753.png|400]]

#### NoSql Databases

- Schemaless, no relational tables
- not only sql
- driven by the need to run on clusters (for scalability)
- eventual consistency as opposed to strong consistency (ACID) assumed
- use aggregate-oriented data model (operate on data with more complex structure and populate it with data from Database)

##### Key-Value Stores

- stores value for a key, no schema
- Quorum: how many replicas have to approve a read/write request (one, all, > 50%, ...)

> [!success] Advantages
> - high throughput

> [!fail] Disadvantages
> - choosing right aggregate is tricky
> - only accessible with key → application must manages keys + modeling

![[Pasted image 20240722203629.png|400]]

##### Document Databases

- stores human-readable documents (json/xml)
- try to understand structure in document for efficient indexes/searches

![[Pasted image 20240722210829.png|400]]


> [!success] Advantages
> - not all docs need to follow same structure
> - more flexible when accessinng data (!= key/val)

> [!fail] Disadvantages
> - Indexing reduces scalability

##### Column-Family Database

- rows with varying number of columns; each being a key/value pair
- entry identifier row-key and column-key

> [!success] Advantages
> - good for sparse data (optional columns)
> - good for writes

> [!fail] Disadvantages
> - difficult to understand/model

![[Pasted image 20240722211443.png|400]]

##### NewSql Databases

- Combinationn of SQL and NoSQL → compromise between consistency & Scalability
- internally partitions and shards its data
- Clustrix, VOLT

![[Pasted image 20240722211727.png]]

##### Graph Databases

- store relations with Nodes & Edges → designed for traversing and querying relationship paths
- steep learning curve
- Neo4J

![[Pasted image 20240722211837.png]]

##### Time Series Databases

- storing sequences of data points during a timme window → automatically attaches timestamp
- good for IoT devices
- InfluxDB

![[Pasted image 20240722212026.png]]

#### Architectural Quanta

> [!info] Definition
> Independently deployable artifact with high functial cohesion, high static coupling and synchronous dynamic coupling
> → keep these components as a unit

![[Pasted image 20240722212221.png]]

→ split applications into different architectural quanta 

> [!success] Advantages
> - indepentent operations + scalability
> - increase fault tolerance
> - specialize DB for Usage in Application

> [!fail] Disadvantages
> - Increases complexity, data duplication
> - Inconsistencies



