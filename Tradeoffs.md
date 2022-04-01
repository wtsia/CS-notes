# Performance vs Scalability
- Scalable: if performance is proportional to resources added
- can be serving more units or larger units
- Performance problem: slow for 1 user
- Scalability problem: fast for 1 user but slow under load
- Achieving good scalability is possible if we architect and engineer our systems to take scalability into account, account for redundancy, and how one should handle heterogeneity.

### Scalability Patterns
- Scale up vs Scale out (for load?)
    - immutability as default
    - Referential Transparency
    - Laziness
    - Different data needs different guarantees
    - tradeoffs:
        - no free lunch

### Latency vs Thoroughput
- Latency is time to perform an action, or result
- Throughput is number of actions/results per unit of time
- aim: max throughput, acceptable latency
    - memory bandwidth sometimes used to specify throughput of memory systems

### Availability vs consistency
- CAP Theorem: Consistency, Availability, Partition Tolerance. Theorem states in a distributed computer system, you can only support two of the guarantees from CAP.
    - Consistency: every read receives the most recent write or an error
    - availability: every req receives a response, without guarantee that it contains the most recent version of information
    - Partition Tolerance: The system continues to operate despite arbitrary partitioning due to network failures
    - Networks arent reliable, so support partition tolerance, and tradeoff between consistency and availability 
        - CP - consistency partition tolerance
            - good for buisiness requiring atomic reads and writes
        - AP - availability and partition tolerance
            - responds with most readily available version of data available on any node (may not be latest)
            - goof for business needs eventual consistency or continued function despite external errors

### Consistency Patterns
Now we are faced with options on how to synchronize them so clients have a consistent view of data.

- Weak Consistency: reads may or may not see a write. best effort approach is taken i.e. in systems like memcached. 
    - use cases in VoIP, Video Chat, Realtime multiplayer games.
- Eventual consistency: reads will eventually see a write (ms), and data is replicated asynchronously.
    - useful for DNS, email, high available systems
- Strong consistency: after write, read will see it, data is replicated synchronously
    - used in file systems like RDBMSes, or systems that need transactions