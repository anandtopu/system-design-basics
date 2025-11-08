# Quorums in Distributed Systems

## Summary
A concise reference describing quorum-based replication, guarantees, trade-offs, examples, and systems that use quorums. Use this note as a quick guide for design and tuning.

## Table of Contents
1. Overview
2. Motivation & trade-offs
3. Definitions
4. Core properties (math)
5. Worked examples
6. Quorums in Paxos/Raft
7. Quorums vs. eventual consistency
8. Practical systems & formulas
9. Implementation and tuning notes
10. Further reading
11. Conclusion

## 1. Overview
Quorum protocols require only a subset of replicas to participate in reads and writes to balance availability and consistency. They guarantee overlap between operations so newer writes are observed by subsequent reads.

## 2. Motivation \& trade-offs
- Full synchronous replication offers strong consistency but low availability.
- Single-replica writes increase availability but risk stale reads.
- Quorums aim for a middle ground: enough replicas participate to preserve ordering while tolerating failures.

## 3. Definitions
- `V`: total replicas.
- `Vw`: write quorum size (minimum acknowledgements for a write).
- `Vr`: read quorum size (replicas consulted for a read).

## 4. Core properties (math)
- `Vr + Vw > V` — ensures every read quorum intersects every write quorum (prevents stale reads).
- `Vw > V/2` — ensures any two write quorums intersect (prevents lost updates and enforces ordering).
- Availability: tolerates up to `floor((V-1)/2)` node failures (a majority must remain).

## 5. Worked examples
- `V = 3`, `Vw = 2`, `Vr = 2` — any read of size `2` intersects any write quorum of size `2`.
- General: for `V = 5`, using `Vw = 3`, `Vr = 3` preserves overlap and tolerates `2` failures.

## 6. Quorums in Paxos / Raft
- Consensus protocols use majority quorums to choose values safely.
- Paxos: prepare/accept phases rely on quorums; Multi\-Paxos optimizes repeated decisions.
- Raft: leader-based replication with majority commits; quorums ensure safety and liveness.

## 7. Quorums vs. eventual consistency
- Quorum-based consensus: strong/linearizable consistency when quorums overlap.
- Eventual systems (Dynamo/Cassandra): prioritize availability and converge asynchronously; quorums can be tuned to strengthen consistency.

## 8. Practical systems \& formulas
- Cassandra: `QUORUM = floor(replication_factor / 2) + 1`.
- ZooKeeper, etcd/Consul (Raft), MongoDB (`majority` write concern) use majority quorums.
- Dynamo-derived systems use tunable `R`/`W` values; strong if `R + W > N`.

## 9. Implementation and tuning notes
- Larger quorums increase latency (more ACKs) but improve consistency.
- For write-heavy workloads, `Vw = 1` improves throughput but risks stale reads unless `Vr` is increased.
- During partitions, apply CAP trade-off decisions explicitly: favor availability or consistency.

## 10. Further reading
- Lamport — "Paxos Made Simple" https://lamport.azurewebsites.net/pubs/paxos-simple.pdf 
- Ongaro & Ousterhout — "In Search of an Understandable Consensus Algorithm (Raft)" https://readings.shubheksha.com/papers/raft/
- Dynamo: amazon's highly available key-value store https://dl.acm.org/doi/abs/10.1145/1323293.1294281
- Gilbert & Lynch — "Brewer's Conjecture" https://users.ece.cmu.edu/~adrian/731-sp04/readings/GL-cap.pdf


## 11. Conclusion
Quorums are a simple, provable mechanism to balance consistency and availability in replicated systems. Properly chosen `Vr` and `Vw` values enable strong guarantees with tolerable availability costs. Tune quorum sizes according to failure models, latency budgets, and application semantics.

[See detailed quorum notes](./quorum.md)