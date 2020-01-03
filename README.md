# ScalableNoSql
All the jargones about NO-SQL


CAP : In the network partioned systems only two of three Consitency, Availability and Partition tolerence can be acheived in case of network failure.
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
