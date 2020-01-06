# ScalableNoSql
All the jargones about NO-SQL


TOPIC 1 -> CAP theorem for distributed data store. (A "distributed data store" is a computer network where information is stored on more than one node, often in a replicated fashion. It is usually specifically used to refer to either a distributed database where users store information on a number of nodes, or a computer network in which users store information on a number of peer network nodes.) 
(A "distributed database" is a database in which not all storage devices are attached to a common processor.)

CAP : In the network partioned systems only two of three 1) Consitency, 2) Availability , 3)Partition tolerence can be acheived in case of network failure.

Consistency: Every read receives the most recent write or an error
Availability: Every request receives a (non-error) response, without the guarantee that it contains the most recent write
Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes

No distributed system is safe from network failures, thus network partitioning generally has to be tolerated.
In the presence of a partition, one is then left with two options: consistency or availability. When choosing consistency over availability, the system will return an error or a time out if particular information cannot be guaranteed to be up to date due to network partitioning. When choosing availability over consistency, the system will always process the query and try to return the most recent available version of the information, even if it cannot guarantee it is up to date due to network partitioning.
 
 
                                                   --------------------
         --------------                            |  SLAVE - I (read)|    
        |  MASTER      |                            -------------------
        | (Write/read) |              /
         --------------             /          ------------------------
                                   /           |  SLAVE - II (read)    |
                                 /            ------------------------
                              (n/w failure between Master and slave II)  

Initally data of write server is replicated across slave-I and slave-II. Everything is fine.
Assume that there's is network failure between Master & slave-II. i.e. All the updates at Master are not reaching slave-II. i.e. slave-II has stale data. In this case
(a) If CP model is chosen: In this case we always want consistent data. As soon as slave-II detects that it's not receiving updates from master, it should make itself unavailable since it can no more guarenty consistency of data. (i.e. in this case we compromised on availabilty at the cost of consistency)
(b) If AP model is chosen: In this case we want data to be present always. (Irrespective of it's staleness). In this example case slave-II would continue to serve the data even though it's not receving any updates from the write server. (i.e. in this case slave-II may serve the stale data.). Once n/w connection between Master and slave is established back again, slave-II should take all the updates and start serving consitent data.
(c) If CA model is chosen: I this case we dont want network partition the system i.e. in this case all the data should reside on the same machine. If the data resides on only one machine, then it's consitency and availability are not impacted by network failure.

Consistency here reference to same copy of data on multiple servers.
Consistency in ACID reference consistency across the subsystems. e.g. User A has 10 Rs in his account, and user B has 20. User A does transfer 5 Rs to B. Consistency here refers that systems state would either be User A : 10 Rs , User B : 20 Rs OR User A: 5 Rs , user B 25 Rs. (In-consistent system would have User A: 5 Rs but User B: 20) 

Consitency models: (a) Strong consistency : An update at Write server will result in update of same data across all the read servers before completing the update operation.  (b) Eventual consistency : An update at write server will complete as soon as data is at write server is updated. Update to read servers will be asynchronously.  (c) Bounded staleness consistency: In this model, data at read slaves cannot be stale beyond certain time. i.e. If a data is updated at write server, it's mandatory that all the read  servers should have updated copy within specified time.


Subtopic: PACELC:
The PACELC theorem builds on CAP by stating that even in the absence of partitioning (i.e. n/w failure), another trade-off between latency and consistency occurs.

The PACELC theorem is an extension to the CAP theorem. It states that in case of network partitioning (P) in a distributed computer system, one has to choose between availability (A) and consistency (C) (as per the CAP theorem), but else (E), even when the system is running normally in the absence of partitions, one has to choose between latency (L) and consistency (C).

PACELC builds on the CAP theorem. Both theorems describe how distributed databases have limitations and tradeoffs regarding consistency, availability, and partition tolerance. PACELC however goes further and states that another trade-off also exists: this time between latency and consistency, even in absence of partitions, thus providing a more complete portrayal of the potential consistency tradeoffs for distributed systems.

"A high availability requirement implies that the system must replicate data. As soon as a distributed system replicates data, a tradeoff between consistency and latency arises."
Data replication => (a) synchronous data replication -> latency is compromised. OR (b) asyncrhonous data replication -> consistency is compromised (i.e. low latency experianced by data update request to Master node as data replication to slave servers is done asynchrnously hence consistency is compromised at the cost of low latency.)

Examples of distobuted NOSQL data stores:
Apache Cassandra,
MongoDB,
dynamoDB,
CouchBase



Topic 2: Advantages/disadvantages of NO-SQL over SQL.

In almost all situations SQL databases are vertically scalable. This means that you can increase the load on a single server by increasing things like RAM, CPU or SSD. But on the other hand NoSQL databases are horizontally scalable. This means that you handle more traffic by sharding, or adding more servers in your NoSQL database. It is similar to adding more floors to the same building versus adding more buildings to the neighborhood. Thus NoSQL can ultimately become larger and more powerful, making these databases the preferred choice for large or ever-changing data sets.


(a) Advantages: 
- No fixed schema. (i.e. very easy to add new property to object already in the production without having to worry about existing objects/rows in the production DB. Similarly very easy to drop one of the existing property of object/row in the production) 
- Scales easily.
- No join operations required, all the data at single place.
- NoSQL databases can be document based, key-value pairs, graph databases whereas SQL databases are table based databases
- NOSQL is more suitable for solving data availability problem. Whereas SQL is more suitable for solving ACID.
Disadvantages:
- NOSQL is not an ideal choice for the complex query intensive environment.
- SQL follows ACID( Atomicity, Consistency, Isolation, and Durability) is a standard for RDBMS. Whereas NOSQL follows BASE ( Basically Available, Soft state, Eventually Consistent) is a model of many NoSQL systems



What is SQL:
Structured Query language (SQL) pronounced as "S-Q-L" or sometimes as "See-Quel" is the standard language for dealing with Relational Databases. A relational database defines relationships in the form of tables.
SQL programming can be effectively used to insert, search, update, delete database records.

What is NOSQL: Non relation database that does not require fixed schema, avoids joins, and is easy to scale. NoSQL database is used for distributed data stores with humongous data storage needs. NoSQL is used for Big data and real-time web apps. For example companies like Twitter, Facebook, Google that collect terabytes of user data every single day.


Topic 3: SQL data normalization/ de-normalization.
