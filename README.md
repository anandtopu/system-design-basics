# System Design Concepts & Research Papers

This repository collects concise system-design concepts, practical notes and canonical research papers for distributed systems, consensus, replication and related topics. Use this as a quick reference or a reading list for design reviews and research.

## Repository pointers
- Core concept notes: `core_concepts/`
  - Example: `core_concepts/quorum.md` (quorum math, examples).
- Additions: place new summaries or papers under `core_concepts/` or `papers/`.

## Key Concepts (short)
- Quorums  
  - Majority-based voting to ensure overlap between reads and writes. See `Vr`, `Vw` and `Vr + Vw > V` for the overlap invariant.
- Consensus (Paxos / Raft)  
  - Protocols that use quorums to reliably choose values despite failures.
- Consistency models  
  - Strong / linearizable, sequential, causal, eventual. Choose by application semantics.
- Replication strategies  
  - Primary-backup, multi-primary, chain replication — trade latency, availability and conflict complexity.
- Partitioning / Sharding  
  - Data distribution to scale; consider rebalancing and metadata management.
- CAP and trade-offs  
  - Availability vs. Consistency under partitions; design around expected failure modes.
- Fault tolerance & recovery  
  - Failure detection, leader election, log replay and membership changes determine system liveness.

## Practical systems (examples)
- Cassandra: tunable consistency with quorum reads/writes; replication factor and `QUORUM = floor(replication_factor / 2) + 1`.
- Dynamo / Riak: eventual consistency, vector clocks, hinted handoff and anti-entropy.
- ZooKeeper: strong consistency for coordination (majority quorum).
- etcd / Consul: Raft-based strongly-consistent key-value stores for configuration and service discovery.
- Spanner / Chubby: Google systems showing strong consistency and global distribution techniques.

## Canonical papers & resources
- Lamport — "Paxos Made Simple" (2001)  
  https://lamport.azurewebsites.net/pubs/paxos-simple.pdf
- Lamport — "The Part-Time Parliament" (Paxos original) (1998)  
  https://lamport.azurewebsites.net/pubs/part-time-parliament.pdf
- Ongaro & Ousterhout — "In Search of an Understandable Consensus Algorithm (Raft)" (2014)  
  https://raft.github.io/raft.pdf
- Lamport — "Time, Clocks, and the Ordering of Events in a Distributed System" (1978)  
  https://lamport.azurewebsites.net/pubs/time-clocks.pdf
- Gilbert & Lynch — "Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services" (2002)  
  https://cseweb.ucsd.edu/~venkatg/ps/brewer.pdf
- Fischer, Lynch, Paterson — "Impossibility of Distributed Consensus with One Faulty Process" (FLP, 1985)  
  https://netfiles.uiuc.edu/amir/www/FLP.pdf
- DeCandia et al. — "Dynamo: Amazon’s Highly Available Key-value Store" (2007)  
  https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf
- Corbett et al. — "Spanner: Google’s Globally-Distributed Database" (2012)  
  https://research.google/pubs/pub39966/
- Oki & Liskov — "Viewstamped Replication" (1988) — early replication approach
- Van Renesse & Schneider — "Chain Replication for Supporting High Throughput and Availability" (2004)

## How to use this repo
- Read topic notes in `core_concepts/` for concise explanations and examples.
- Add or update summaries as short Markdown notes; cite sources and include direct links.
- Use the papers list for deeper study and implementations.

## Contributing
- Add new concept notes or paper summaries as Markdown under `core_concepts/` or `papers/`.
- Keep entries short (one page) and include a short example or diagram when useful.

## License & attribution
- This repository is a curated reading and notes collection. Respect original paper licenses and attribute authors when quoting.
