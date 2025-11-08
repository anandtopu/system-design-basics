
In Apache ZooKeeper, **writes (such as leader election updates)** require a **quorum** of nodes to acknowledge the operation for it to be considered successful.

### 🧮 Quorum Calculation

ZooKeeper uses **majority consensus**, so the quorum size is:

$$
\text{Quorum} = \left\lfloor \frac{N}{2} \right\rfloor + 1
$$

Where **N** is the total number of ZooKeeper nodes.

***

### ✅ For 7 ZooKeeper Nodes:

$$
\left\lfloor \frac{7}{2} \right\rfloor + 1 = 3 + 1 = 4
$$

So, **at least 4 nodes** must acknowledge a write for it to be considered successful.

***

### 📌 Why Majority?

This ensures:

*   **Consistency**: No two partitions can both form a quorum.
*   **Fault tolerance**: The system can tolerate up to **N - Quorum = 3** node failures.

## How
Apache ZooKeeper reduces the need for continuous queries to determine the master node in a **Master-Slave architecture** by leveraging its **watch mechanism** and **ephemeral znodes**. Here's how it works:

***

### 🧠 Key Concepts

#### 1. **Ephemeral Znodes**

*   When a node becomes the master, it creates an **ephemeral znode** (e.g., `/master`) in ZooKeeper.
*   This znode **automatically disappears** if the master node crashes or disconnects.
*   Other nodes can check for the existence of this znode to know who the master is.

#### 2. **Watches**

*   Instead of polling ZooKeeper repeatedly, slave nodes **set a watch** on the `/master` znode.
*   A watch is a **one-time trigger** that notifies the client when the znode is created, deleted, or changed.
*   When the master fails and the znode is deleted, all watching nodes are notified immediately.

***

### 🔄 Leader Election Flow

1.  All nodes try to create an ephemeral znode (e.g., `/election/node_`) with a **sequential suffix**.
2.  ZooKeeper assigns each node a unique sequence number.
3.  The node with the **lowest sequence number** becomes the leader.
4.  Other nodes watch the znode with the **next lowest sequence number**.
5.  If the leader fails, the next node in line is notified and takes over.

***

### ✅ Benefits

*   **No polling**: Watches eliminate the need for continuous queries.
*   **Fast failover**: Ephemeral znodes and watches enable quick detection of master failure.
*   **Scalable**: Efficient even with many clients.

***