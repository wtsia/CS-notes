# Design and Analysis of Algorithms, MIT 6.046J 2015 
## Notes Summary
##### 1. Course Overview, Interval Scheduling
` P vs NP: class of problems solvable in polynomial time O(n^k) vs non-deterministic polynomial time, which is verifiable in poly. time.
` Claims:
    1. Greedy algorithm produces a list of intervals s.t. starts and finishes are chronological
    2. Given a list of Intervals L, greedy algorithmn with earliest finish time produces k* intervals where k* is maximum
    - Conclusion: earliest finish time is mathematically the most optimal algorithm

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
- Claim 1: Greedy algo outputs a list of intervals <s(i_1), f(i_1)>, ... s.t. s(i_1) < f(i_1) <= s(i_2) ...
    - Proof: by contradiction. if finish of 'j' is greater than start if 'j+1', it contradicts step 2 of algo.
- Claim 2: Given list of intervals L, greedy algorithm with earliest finish time produces k* intervals where k* is optimal
    - Proof: Induction on k*. k*=1, then inductive step for k* + 1 intervals. By construction we know f(i_1) <= f(j_1) since algo picks earliest finish time. Construct schedule S** = <s(i_1), f(i_1)>, ...

### Weighted Interval Scheduling
- Each request i has weight w(i). Schedule subset of requests that are non-overlapping
with maximum weight.
    - A key observation here is that the greedy algorithm no longer works.