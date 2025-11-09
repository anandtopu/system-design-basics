# Mastering System Design: SQL vs. NoSQL — An In-Depth Guide for Scalable Architectures in 2025

## Summary
A concise, structured reference contrasting SQL and NoSQL choices, core features, trade-offs, sharding strategies, practical use-cases, and 2025 trends. Use this note as a quick guide for architecture decisions and trade-off analysis.

## Table of Contents
1. Overview
2. Motivation \& trade-offs
3. Definitions
4. SQL: Features and trade-offs
5. NoSQL: Features and trade-offs
6. Sharding and data distribution
7. NoSQL families and examples
8. Manual sharding design
9. Practical picks and use-cases
10. 2025 trends
11. Further reading
12. Conclusion

## 1. Overview
Databases shape performance, reliability, and scalability. This note contrasts SQL (relational, ACID) and NoSQL (distributed, BASE) and presents patterns for combining them in modern systems.

## 2. Motivation \& trade-offs
- Data shape, volume, and access patterns should drive the choice.
- CAP theorem: must trade among Consistency, Availability, Partition tolerance.
- Polyglot persistence is common: different stores for different needs.

## 3. Definitions
- `ACID`: Atomicity, Consistency, Isolation, Durability.
- `BASE`: Basically Available, Soft state, Eventual consistency.
- `Sharding`: Horizontal partitioning of data across nodes.
- `Replication`: Copies of data for durability and read scale.

## 4. SQL: Features and trade-offs
### Core features
- Strong schema, relational model, declarative queries.
- ACID transactions, joins, mature optimizers.
- Good for financial systems, strong consistency needs.

### Strengths
- Predictable semantics, easy reasoning, powerful joins and analytics.
- Tooling and ecosystem (backups, migrations, ORMs).

### Limitations
- Fixed schema complexity for variable data.
- Horizontal scaling (sharding) adds complexity; cross-shard joins are expensive.

## 5. NoSQL: Features and trade-offs
### Core features
- Schema flexibility, denormalization, horizontal scaling.
- Tunable consistency (e.g., `R`/`W`/`N`), design for availability.

### Strengths
- Scales horizontally for high throughput and large datasets.
- Fits semi-structured or evolving schemas; low-latency local reads with denormalization.

### Limitations
- Eventual consistency can complicate correctness for some apps.
- Application-side complexity: conflict resolution, versioning.

## 6. Sharding and data distribution
### Sharding principles
- Choose a shard key that balances load and minimizes cross-shard operations.
- Use consistent hashing or range partitioning depending on access patterns.

### Shard key guidance
- Uniform distribution across keys.
- Minimize scatter for common queries.
- Avoid hotspotting keys with unbounded growth.

### Migration and rebalancing
- Virtual nodes or consistent-hash ring reduce data movement.
- Online rebalancing strategies: streaming, backfill, and throttling.

## 7. NoSQL families and examples
- Key-Value (Redis, DynamoDB): simple, ultra-low latency for session or cache.
- Document (MongoDB): JSON documents for flexible schemas and queryability.
- Column-Family (Cassandra, HBase): time-series and wide-row workloads.
- Graph (Neo4j): relationship-first queries for social/graph analytics.

## 8. Manual sharding design
Brief design sketch (pseudocode outlines in prose):
- Hash ring with virtual nodes for even distribution.
- Replication factor (commonly 3) for durability and failover.
- Node add/remove: compute affected token range and asynchronously migrate.
- Failure handling: promote replica, use gossip for membership.

## 9. Practical picks and use-cases
- SQL: transactional systems, accounting, OLTP with complex joins.
- NoSQL: social feeds, telemetry, high-write time-series, caches.
- Hybrid: SQL for transactions + NoSQL for large-scale analytics or feature stores.

## 10. 2025 trends
- Polyglot persistence is standard; choose per workload.
- AI-driven indexing and query optimization (vector indexes, auto-index).
- Distributed SQL / NewSQL (e.g., Yugabyte, CockroachDB) blur boundaries.
- Managed cloud offerings and open-source innovations (Scylla, Vitess).

## 11. Further reading
- Cassandra, Dynamo, Redis, PostgreSQL and selected papers.
- Include direct links as appropriate: vendor docs and canonical research papers.

## 12. Conclusion
Design for data: prefer SQL when semantics and transactions are primary; prefer NoSQL when scale and flexible schemas dominate. Combine stores where appropriate and iterate with real workload metrics.

[See also: SQLvsNoSQL_notes](./SQLvsNoSQL.md)
