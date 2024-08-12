## System design fundamental Concepts

12 Key Concepts - [doc](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#how-to-approach-a-system-design-interview-question):

#### [1. Scalability](#q1)

#### [2. Performance](#q2)

#### [3. Availability](#q3)

#### [4. Reliability](#q4)

#### [5. Consistency](#q5)

#### [6. CAP theorem](#q6)

#### [7. Data Storage and Retrieval](#q7)

#### [8. ACID Transactions in DB](#q8)

#### [9. Consistent Hashing](#q8)

#### [10. Rate Limiting](#q10)

#### [11. Networking and Communication](#q11)

#### [12. Security and Privacy](#q12)

<div id="q1" />

#### 1 Scalability

Scalability([article](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#step-2-review-the-scalability-article)): How well a system can **handle more users or data without slowing down.**

- **Vertical Scaling**: getting more resouces **on a single machine**, eg: more hard-drive, or memory.
- **Horizontal Scaling**: add more machines to the system to spread the load.

<div id="q2" />

#### 2 Performance

[Performance](https://github.com/binhnguyennus/awesome-scalability?tab=readme-ov-file#performance): **How fast the system works?**

**How to measure?**

- **Latency**: how long time does it take for a single task?
- **ThroughPut**: how many tasks your system can handle in a certain time?

Articles:

- [Performance vs scalability](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#performance-vs-scalability)
- [Latency vs throughput](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#latency-vs-throughput)

<div id="q3" />

#### 3 Availability

[Availability](https://github.com/binhnguyennus/awesome-scalability?tab=readme-ov-file#availability): the system is up and running when users need it **without any significant downtime**.

#### 3.1 [Availability in Numbers](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-in-numbers): eg: `P90`

#### 3.2 Article

- [Availability vs consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-vs-consistency)

<div id="q4" />

#### 4 Reliability

Reliability: **the system is doing what it's supposed to do even if things go wrong.**

Helpful patterns:

- [Replication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#replication)
- Redundancy
- [Failover](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#fail-over) mechanisms

<div id="q5" />

#### 5 Consistency

[Consistency](): All users see the same data at the same time no matter which part of the system they interact with.

**Limitations:**
but having a strong **consistency has a cost**, which might **slow down performance**.
Then many systems adpot **"Eventual Consistency": the data may not be upto date immediately but it will be after a specific time.**.

**[Consistency patterns](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#consistency-patterns):**

- [Weak consistency](-%20Weak%20consistency%20Eventual%20consistency%20Strong%20consistency)
- [Eventual consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#eventual-consistency)
- [Strong consistency](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#strong-consistency)

<div id="q6" />

### 6 CAP theorem

[CAP theorem](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cap-theorem): In a distributed system, you can only have 2/3 of the below items:

- Consistency
- Availability
- Partition tolerance

![image](../assets/cap-theorem-design-chart.png)

**Article:**

- [CP-consistency and partition tolerance](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#cp---consistency-and-partition-tolerance)
- [AP - availability and partition tolerance](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#ap---availability-and-partition-tolerance)

<div id="q7" />

### 7 Data Storage and Retrieval

**[Database](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#database):**

1. choosing the right database:

   - [RDBMS(relational db management system)](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#relational-database-management-system-rdbms)
   - [NoSQL](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#nosql)
     - [key-value store](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#key-value-store)
     - [document store](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#document-store)
     - [graph database](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#graph-database)
   - [SQL or NoSQL](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sql-or-nosql)

2. design database schema
3. How to optimize the DB storage and retrieval?

   - partitioning
   - [sharding](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sharding)
   - replication:

     - [Master-slave replication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#master-slave-replication)
     - [Master-master replication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#master-master-replication)

   - [SQL tuning](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#sql-tuning)

<div id="q8" />

### 8 ACID Transactions in DB

A way to make sure **everything we do in DB is done right and reliably**:

- `A`: Atomicity
- `C`: Consistency
- `I`: Isolation
- `D`: Durability

<div id="q9" />

### 9 Consistent Hashing

[Consistent Hashing](https://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html): **a method to spread data across a group of servers. **

**Benifit:**

- It makes the system easy to **remove/add a server without causing too many distrubances**.
- It also helps improve: **Scalability & Load balancing**.

<div id="q10" />

### 10 Rate Limiting

[Rate limiting](https://www.keycdn.com/support/rate-limiting): Control the rate at which clients can make requests to a system.

**Benefits:**

- Prevent Abuse
- Protect Against DDOS Attacks
- Ensures fair usage of resources

Examples:

- Control the rate at which clients can make requests to a system.
- Rate Limiting for Scaling to Millions of Domains at Cloudflare
- Cloud Bouncer: Distributed Rate Limiting at Yahoo
- [Scaling API with Rate Limiters at Stripe](https://stripe.com/blog/rate-limiters)
- Distributed Rate Limiting at Allegro
- [Ratequeue: Core Queueing-And-Rate-Limiting System at Twilio](https://www.twilio.com/blog/2017/11/chaos-engineering-ratequeue-ha.html)

<div id="q11" />

#### 11 Networking and Communication

[Communication](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#communication): How different parts of a system communicate?

- Network Protocals: TCP, UDP, [RPC](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#remote-procedure-call-rpc), [REST](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#representational-state-transfer-rest)
- APIs
- [Message Queues](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#message-queues)
- Event Driven Architectures

<div id="q12" />

### 12 Security and Privacy

[Security](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#security): Putting in place methods to keep important data safe and stop unwanted access.

- authentication
- authorization
- encryption
