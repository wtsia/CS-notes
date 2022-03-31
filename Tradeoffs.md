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