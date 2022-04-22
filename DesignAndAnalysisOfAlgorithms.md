## 1. Course Overview, Interval Scheduling

### Theme of Lecture:
- similar problems can have different complexity
- P: class of problems solvable in polynomial time O(n^k) for some constant k. 
    - Shortest-path: O(v^2), for v = verticies in graph
- NP: Non-deterministic polynomial time. Class of probs. solution is verifiable in polynomial time.
    - Hamiltonian cycle: given a directed graph, find a simple cycle that contain each vertex in V
- NP-complete: problem is in NP and is as hard as any problem in NP. 
- Given requests 1, 2, ..., i, j, ..., n, with s(i) being start time, f(i) finishing time, s(i) < f(i)
- Greedy Algorithm: a myopic algorithm that processes inputs one piece at a time. 
    - Greedy Interval Scheduling: use a simple rule to request i, reject incompatible to i, and repeat until process is done
        - min(s(i))? longest early segment... bad
        - min(f(i) - s(i))? smallest segment.. bad