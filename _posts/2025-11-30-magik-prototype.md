# **From Vision to Reality - The Digital Sovereignty Fabric**

In 2020, I published a short article on my blog titled simply [**Beemesh**](https://flavio.aiello.ch/beemesh/).

It outlined a vision: a global, decentralized, zero-trust compute fabric that could run anywhere - cloud, edge, on-prem, or IoT - without the brittleness and bureaucracy of centralized control planes and their implicit risk. I described how such a system could use decentralized discovery, identity separation, and self-healing workloads to scale far beyond what Kubernetes- or cluster-centric designs allowed.

Five years later, that vision is no longer a sketch on a napkin.
**A prototype is now on the horizon, and it’s called Magik.run.**

The idea is still the same: a fully decentralized, cryptographically isolated, identity-powered scale-out fabric designed for **tens of thousands of nodes**, network partitions, and unavoidable real-world chaos. This post revisits that original vision and explores why the system works the way it does, what problems it eliminates, and what it means for the future of distributed computing.

---

# **The New Paradigm: Computing Without a Control Plane**

For the past decade, infrastructure engineering has revolved around one gravity well: **the control plane**. From Kubernetes to Nomad and every orchestrator in between, the industry has optimized the *cluster* - but rarely questioned whether a cluster should exist at all.

Kubernetes and similar platforms have served us well, but they all share the same architectural weight:

**A centralized control plane.**

As clusters grow, this becomes a bottleneck:

* Consensus (etcd/Raft) struggles under load
* Scheduler throughput becomes a ceiling
* Control-plane outages can paralyze entire environments
* Edge and IoT environments simply can’t host such infrastructure

Magik.run takes the opposite stance:

> **If the control plane is the bottleneck, remove the control plane.**

No masters.
No etcd.
No Raft quorum.
No global API server dependency.

Instead, Magik.run is a:

### **Decentralized scale-out choreography fabric**

Nodes collaborate through secure peer-to-peer protocols, schedule work ephemerally, and recover from churn without relying on global state.

### **Infrastructure without state**

Machines are disposable. Their identity is ephemeral. They store no global truth.

### **Workloads as sovereign units**

Each workload carries its own state management, identity, and consistency boundaries.

This separation is the heart of Magik.run.

---

# **Why Magik.run Exists**

Legacy orchestrators struggle with:

| Problem                                | Magik.run Solution                                          |
| -------------------------------------- | --------------------------------------------------------- |
| Control-plane scaling limits           | No control plane - **fully decentralized choreography**   |
| Infrastructure failures ripple outward | Machines are disposable; **state lives inside workloads** |
| Complexity (etcd, Raft, API servers)   | A single lightweight daemon (~50–80 MiB RAM)              |
| Weak identity boundaries               | Separate identities for **machines and workloads**        |
| Vendor lock-in                         | Runs anywhere: cloud, edge, IoT, air-gapped               |
| Day-2 maintenance toil                 | Rebuild > repair; no persistent control-plane state       |

This isn’t an incremental improvement on Kubernetes.
It’s a departure.

---

# **The Core Architectural Idea: Two Planes, Two Concerns**

## **1. The Machineplane - A/P (Availability + Partition Tolerance)**

Manages node discovery, scheduling, and resource negotiation.
Stores no persistent state.
100% disposable.

## **2. The Workplane - C/P (Consistency + Partition Tolerance)**

Implements service discovery, workload connectivity, replica tracking, and optional Raft-based consistency for stateful sets.

**Infrastructure failures cannot corrupt workloads.**
Each plane is optimized for its purpose, independent of the other.

This was implicit in the 2020 blog post; now it is formalized in code and protocol.

---

# **Security: Zero Trust, Native and Mandatory**

In 2020, I wrote about a mesh where **identity travels with the workload** and trust originates *inside* the application boundary.

Magik.run fulfills that vision:

### **Machine identities**

Each machine generates its own Ed25519 keypair for machine-to-machine communication.

### **Workload identities**

Every workload has its own independent cryptographic identity.

### **Mutually authenticated, encrypted streams by default**

All communication - both between machines and between workloads - uses mTLS over QUIC.

Even if a machine is compromised, workloads remain isolated.
This is true Zero Trust, not the marketing variety.

---

# **Ephemeral Scheduling: The Anti-Scheduler**

Traditional schedulers maintain global shared state and require consensus.
Magik.run does neither.

Scheduling is ephemeral:

1. A machine announces a **tender** (request to place a workload).
2. Nodes matching criteria **bid**.
3. One node receives the **award**.
4. The tender vanishes - no state persists.

This yields:

* No single point of failure
* No coordination bottlenecks
* Natural horizontal scalability
* Partition resilience
* Scheduling throughput that improves as the network grows

The “global cluster view” required by Kubernetes simply isn’t needed here.

---

# **The Workplane: A Decentralized Service Mesh Without Sidecars**

Each pod runs a lightweight Workplane agent that provides:

* Service discovery (via a per-workload DHT)
* Replica health and self-healing
* Optional built-in Raft for stateful sets
* Secure workload-to-workload connectivity

This establishes **per-workload trust domains**, not a cluster-wide identity layer.
It also eliminates the need for sidecars, kube-proxy, mesh controllers, and API-server-centric networking.

For stateful workloads, the Workplane’s Raft implementation provides leader election and consistency gating.

---

# **How Magik.run Compares**

| Feature          | Kubernetes       | Nomad        | **Magik.run**                |
| ---------------- | ---------------- | ------------ | -------------------------- |
| Control plane    | Required         | Required     | **None**                   |
| Scheduling model | Centralized      | Centralized  | **Ephemeral, distributed** |
| State location   | etcd             | Raft cluster | **Inside the workload**    |
| Scalability      | ~5k nodes        | ~10k nodes   | **Tens of thousands+**     |
| Multicloud       | Complex          | Possible     | **Native**                 |
| Edge/IoT         | Limited          | Moderate     | **Excellent**              |
| Security         | Add-ons for mTLS | Add-ons      | **Default**                |

Magik.run doesn’t compete with Kubernetes or Nomad; it occupies a different architectural space - one suited to dynamic, unreliable, heterogeneous environments.

---

# **Where Magik.run Truly Shines**

### **Edge & IoT**

Expect churn. Expect partitions. Expect devices to vanish.
Magik.run does not mind.

### **Multicloud**

Run workloads across providers without control-plane coupling.

### **Air-gapped environments**

Magik.run operates fully offline - no registry, cluster, or external identity provider required.

### **Global analytics & batch computing**

Ephemeral scheduling naturally absorbs bursts.

### **Stateful workloads**

State belongs to the workload, not to the cluster.

### **Smart cities, telco, logistics**

Millions of devices, unpredictable topology - ideal conditions for a decentralized fabric.

---

# **From Prototype Promise to Reality - The Loop Closes**

When I wrote the initial Magik.run article in 2020, the technology required to realize the vision existed only in fragments.
QUIC was still maturing; libp2p’s QUIC support was experimental; Rust’s async networking ecosystem was young.

Today, the ecosystem has caught up.
And the promise - *“a prototype will be available soon”* - is no longer speculative.

**Magik.run is that prototype, and more:
a complete, working architecture for global-scale decentralized computing.**

The vision of 2020 was the seed.
The emerging 2024 implementation is the tree.

---

# **Getting Started with Magik.run**

```bash
git clone https://github.com/Magik.run/Magik.run.git
cd Magik.run
cargo build --release

./target/release/machineplane
```

Then run the Workplane agent inside workloads and deploy manifests via the kubectl-compatible API.

**Documentation:** [https://docs.Magik.run.io](https://docs.Magik.run.io)
**Source:** [https://github.com/Magik.run/Magik.run](https://github.com/Magik.run/Magik.run)

---

# **Research Grounding & Related Work**

Magik.run's architecture isn't theoretical speculation - it builds on decades of distributed systems research. Here's how the core design decisions map to established academic foundations.

## **Decentralized Scheduling & Resource Markets**

The ephemeral tender → bid → award scheduling model draws from:

* **Sparrow** (SOSP 2013) - demonstrates that distributed, low-latency schedulers outperform centralized ones at scale using randomized, stateless task placement. [Paper](https://people.eecs.berkeley.edu/~matei/papers/2013/sosp_sparrow.pdf)
* **Hopper** (SIGCOMM 2015) - extends decentralized scheduling to speculation-aware analytics clusters. [Paper](https://conferences.sigcomm.org/sigcomm/2015/pdf/papers/p379.pdf)
* **Auction Protocols for Decentralized Scheduling** (Wellman et al.) - models resource allocation as auctions where jobs and machines exchange bids. [Paper](https://www.sciencedirect.com/science/article/pii/S0899825600908224)
* **Tycoon** - distributed market-based resource allocation using bidding for fair, low-latency allocation. [Paper](https://arxiv.org/abs/cs/0404013)

Kubernetes documents that production clusters are [practically limited to ~5,000 nodes](https://kubernetes.io/docs/setup/best-practices/cluster-large/) due to control-plane scalability. Magik's premise - "scale is limited by the control plane, so remove it" - is a logical extension.

## **DHTs, Pub/Sub Overlays & Gossip**

The Machine DHT and Workload DHT for decentralized discovery build on:

* **Chord** (SIGCOMM 2001) - scalable peer-to-peer lookup with O(log N) routing. [Paper](https://sites.cs.ucsb.edu/~rich/class/cs293b-cloud/papers/chord.pdf)
* **Kademlia** (IPTPS 2002) - DHT based on XOR metric for distributed key-value storage. [Paper](https://www.researchgate.net/publication/2492563_Kademlia_A_Peer-to-peer_Information_System_Based_on_the_XOR_Metric)
* **Scribe** - large-scale multicast built on DHT overlays. [Paper](https://people.mpi-sws.org/~druschel/publications/Scribe-jsac.pdf)
* **SWIM** - scalable, weakly-consistent infection-style membership protocol. [Paper](https://www.cs.cornell.edu/projects/Quicksilver/public_pdfs/SWIM.pdf)
* **Gossip-Style Failure Detection** (Van Renesse et al.) - epidemic dissemination for membership and failure detection. [Paper](https://www.cs.cornell.edu/home/rvr/papers/GossipFD.pdf)

## **CAP-Aware Design & Per-Workload State**

The A/P Machineplane + C/P Workplane separation is grounded in:

* **Gilbert & Lynch's CAP Proof** - formal proof that partitioned systems must trade consistency for availability. [Paper](https://www.cs.princeton.edu/courses/archive/spr22/cos418/papers/cap.pdf)
* **Dynamo** (SOSP 2007) - Amazon's highly available key-value store with application-assisted conflict resolution. [Paper](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* **Bayou** (USENIX 1995) - weakly consistent replicated storage pushing conflict resolution to applications. [Paper](https://www.cs.utexas.edu/~lorenzo/corsi/cs380d/papers/p172-terry.pdf)
* **Coda** - disconnected operation in distributed file systems. [Paper](https://people.eecs.berkeley.edu/~brewer/cs262b/Coda-TOCS.pdf)
* **Raft** (USENIX ATC 2014) - understandable consensus for replicated logs. [Paper](https://raft.github.io/raft.pdf)

Magik generalizes this pattern: every stateful workload is its own Dynamo/Bayou/Coda-style system; the fabric never pretends to maintain a globally consistent view.

## **Zero Trust & Workload Identity**

The separate machine/workload identity model implements:

* **NIST SP 800-207** - Zero Trust Architecture principles for continuous identity-centric verification. [Paper](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-207.pdf)
* **SPIFFE/SPIRE** - workload-centric, cryptographically verifiable identities decoupled from infrastructure. [Site](https://spiffe.io/)
* **Istio Security Model** - per-workload certificates and mutual TLS for service-to-service authentication. [Docs](https://istio.io/latest/docs/concepts/security/)

Magik bakes SPIFFE-like workload identity into the fabric while distinguishing machine IDs from workload IDs and making mTLS mandatory, not optional.

## **Edge, IoT & Fog Computing**

The stateless, transient machine model addresses:

* **Fog Computing** (Bonomi et al., MCC 2012) - extending cloud to the edge with low latency, geographic distribution, and frequent churn. [Paper](https://conferences.sigcomm.org/sigcomm/2012/paper/mcc/p13.pdf)
* **Resource Management in Fog/Edge Computing** (ACM CSUR 2019) - surveys emphasizing heterogeneity, resource constraints, and decentralized management. [Paper](https://pureadmin.qub.ac.uk/ws/files/168704756/Fog_EdgeResourceManagement_Survey_CSUR_2019.pdf)
* **Dependability in Fog Computing** - challenges of failures, partitions, and placement strategies for distributed IoT. [Paper](https://www.science-gate.com/IJAAS/Articles/2021/2021-8-4/1021833ijaas202104010.pdf)

## **Self-Healing & Autonomic Systems**

The Workplane's autonomous remediation implements:

* **The Vision of Autonomic Computing** (Kephart & Chess, IEEE 2003) - defines self-configuration, self-optimization, self-protection, and self-healing as core system goals. [Paper](https://www.researchgate.net/publication/2955831_The_Vision_Of_Autonomic_Computing)
* **Autonomic Computing Surveys** - runtime monitoring, local fault detection, and automated recovery with minimal operator involvement. [Paper](https://www.cs.colostate.edu/~france/CS614/Readings/Readings2008/AdaptiveSoftware/a7-huebscher-autonomicSysSurvey.pdf)

## **What's Novel**

Every piece of Magik has strong backing in prior work. What appears novel is the *combination*:

1. **Two fully separate planes** (Machineplane A/P, Workplane C/P) with their own DHTs and trust domains
2. **Stateless tender → bid → award scheduling** where correctness is guaranteed by workload-level protocols
3. **All infrastructure as disposable**, pushing durability entirely to workloads
4. **Mutually authenticated, encrypted streams by default** at both infra and workload layers

These choices don't contradict the literature; they combine its building blocks in a more extreme, mesh-native architecture.

---

# **Closing Thoughts: Beyond Clusters**

Magik.run is not an orchestrator.
It is not a cluster manager.
It is not an incremental improvement on existing systems.

It is a **departure** - a realization that:

* the world is too distributed,
* networks too unreliable,
* devices too transient,
* and global workloads too complex

…to continue relying on centralized control planes and fragile global consensus.

Magik.run points toward a future where:

* infrastructure is disposable
* workloads are sovereign
* security is guaranteed by design
* availability emerges from decentralization
* scale has no predefined ceiling

It is the logical implementation to the idea published on my blog in 2020 - now refined, expanded, and becoming real.

**Magik.run isn’t the future of clusters.
It’s the future beyond clusters.**
