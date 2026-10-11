# Module — Storage in AI-Centric Data Centers

> Part I · Course Module 05 (AI Infrastructure: Compute, Storage and Cloud) — topic notes

## 1. After Compute and Network, Focus on Storage

When discussing an AI-centric data center, three major components are:

```
Compute
   +
Network
   +
Storage
```

- Compute performs the processing.
- Network moves data between systems.
- Storage provides the data required by AI workloads and makes that data available to compute resources.

## 2. Restaurant Analogy — Think of an AI Data Center as a Five-Star Kitchen

A useful analogy is a five-star restaurant.

```
Five-Star Restaurant

Chef       → GPU
Waiters    → Network
Pantry     → Storage
Customers  → Users / Applications
```

**Chef = GPU** — The chef performs the actual cooking and creates the dishes. Similarly, the GPU performs: AI computation, Model training, Inference, Tensor and matrix operations.

## 3. Waiter = Network

The waiter helps move the prepared food to the customers. This is similar to the role of the network.

```
Chef / GPU
     │
     │ Data
     ▼
 Network / Waiter
     │
     ▼
 Customer / Application
```

The network transports data between systems.

## 4. Pantry = Storage

A restaurant needs a well-stocked pantry. The pantry contains ingredients such as: Vegetables, Meat, Rice, Spices, Sauces, Other ingredients. The chef needs fast access to these ingredients.

In an AI data center: Pantry = Storage. Storage may contain: Training datasets, Model files, Checkpoints, Logs, Configuration data, Scripts, Historical data.

## 5. The Role of Storage

The fundamental requirement is: **Storage should provide fast access to the data required by GPUs.**

Think about the restaurant again. Even if the chef is extremely fast, the restaurant will not perform efficiently if the pantry takes a long time to provide ingredients. The same applies to AI:

```
Fast GPU
    │
    │ needs data
    ▼
Slow Storage
    │
    ▼
GPU waits
```

Therefore, storage performance is critical in an AI-centric data center.

## 6. AI Workload Storage Requirements

AI workloads typically require:

1. **High Throughput** — The storage system needs to move large amounts of data efficiently.
2. **Low Latency** — Data should be accessed with minimal delay.
3. **Scalability** — Storage must be able to scale as the amount of data and number of GPU nodes increase.

Therefore: `AI Storage = High Throughput + Low Latency + Scalability`

## 7. Storage Type 1 — NVMe SSD

One common option is NVMe SSD local storage. NVMe stands for: **Non-Volatile Memory Express**.

An AI server may contain:

```
                AI Server
        ┌─────────────────────┐
        │                     │
        │       CPU           │
        │        │            │
        │       GPU           │
        │                     │
        │     NVMe SSD        │
        │        │            │
        └────────┼────────────┘
                 │
            Local Storage
```

The NVMe SSDs are located inside the GPU server.

## 8. Why Use NVMe?

NVMe provides very fast local I/O. It can be useful for: Model training, Inference, Temporary or active data, High-speed local data access.

Restaurant analogy:

```
Chef / GPU
     │
     ▼
Nearby Pantry
     │
     ▼
NVMe SSD
```

The ingredients are very close to the chef, so they can be accessed quickly.

## 9. NVMe Limitation

Although NVMe SSDs are very fast, their capacity is limited by the physical server. You cannot keep adding unlimited numbers of SSDs to one server.

```
Server
┌──────────────────────┐
│ CPU                  │
│ GPU                  │
│ NIC                  │
│ NVMe │ NVMe │ NVMe   │
└──────────────────────┘

Physical Space = Limited
```

Therefore: `NVMe = Very Fast + Local + Limited Capacity`

## 10. Storage Type 2 — Parallel File System

What happens when we need much more capacity and many GPU servers need to access the same data? A common solution is a **Parallel File System**. It is a form of clustered storage.

```
GPU Node 1
                  │
             GPU Node 2
                  │
             GPU Node 3
                  │
             GPU Node 4
                  │
                  ▼
          Parallel File System
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
     Storage   Storage   Storage
      Node 1    Node 2    Node 3
```

Multiple GPU nodes can access multiple storage nodes behind the scenes.

## 11. Why Use Parallel File Systems?

An AI cluster may contain many GPU nodes. For example:

```
GPU Node 1 ───┐
GPU Node 2 ───┤
GPU Node 3 ───┼──► Shared High-Speed Storage
GPU Node 4 ───┤
GPU Node 5 ───┘
```

Many GPU nodes may need access to shared datasets at the same time. Therefore, a Parallel File System can provide shared high-speed access across many GPU nodes. This is especially useful for large-scale AI and HPC-style workloads.

## 12. Storage Type 3 — Network File System (NFS)

Another storage option is: **Network File System (NFS)**. NFS allows files to be shared across systems over a network. In the source's context, it can be used for distributed small datasets such as configuration files and scripts.

```
             NFS Server
                 │
       ┌─────────┼─────────┐
       │         │         │
       ▼         ▼         ▼
    Node 1     Node 2    Node 3
       │         │         │
     Config    Config    Config
     Scripts   Scripts   Scripts
```

## 13. What Is NFS Used For?

NFS can be useful for: Configuration files, Scripts, Shared small datasets, Common files.

The important point is: In this source's framing, NFS is primarily being discussed for configuration/script sharing and smaller distributed data, rather than as the highest-performance storage layer for large AI training datasets.

## 14. Storage Type 4 — Object Storage

Another important storage type is **Object Storage**. The source gives Amazon S3 as an example. Object Storage is useful for storing very large amounts of data, particularly for long-term storage.

## 15. Object Storage Use Cases

Object Storage can hold: Massive raw datasets, Model checkpoints, Logs, Historical data, Archived models.

```
              Object Storage
                    │
       ┌────────────┼────────────┐
       │            │            │
       ▼            ▼            ▼
   Raw Dataset   Checkpoints    Logs
       │            │            │
       └────────────┼────────────┘
                    │
              Long-Term Data
```

Therefore: `Object Storage = Massive + Long-Term + Persistent Storage`

## 16. The Four Main Storage Types

| Storage | Main Purpose | Key Characteristic |
|---|---|---|
| NVMe SSD | Local active data / fast I/O | Very fast, local |
| Parallel File System | Shared data across many GPU nodes | High-speed shared access |
| NFS | Configuration and script sharing | Distributed file sharing |
| Object Storage | Long-term / massive data | Large-scale persistent storage |

## 17. Easy Memory Trick

```
NVMe
  ↓
Fast Local Storage

Parallel File System
  ↓
Fast Shared GPU Storage

NFS
  ↓
Config + Scripts

Object Storage
  ↓
Massive Long-Term Storage
```

Or simply:

- NVMe = Local Fast
- Parallel FS = Shared Fast
- NFS = Shared Files
- Object = Long-Term Massive

## 18. An AI Data Center Does Not Use Only One Storage Type

An important concept is: A data center normally does not need to use only one type of storage. Instead, it can use a **tiered storage** or **hybrid storage** approach.

Different data has different access patterns: Some data is actively being used. Some data is used occasionally. Some data is primarily kept for archive or historical purposes.

## 19. Tiered Storage

```
HOT DATA
   │
   ▼
NVMe / Fast Storage
   │
   │
   ▼
WARM DATA
   │
   ▼
Shared / Parallel Storage
   │
   │
   ▼
COLD DATA
   │
   ▼
Object Storage / Archive
```

The basic idea is to place data on the storage tier that matches its access requirements.

## 20. Hot, Warm, and Cold Data

**Hot Data** — Data that is actively being used. For example: Current training datasets, Active model data, Frequently accessed data. This data can be kept on:

```
Hot Data
   ↓
NVMe / Parallel File System
```

**Warm Data** — Data that is still useful but is not constantly accessed. Examples might include: Older model versions, Recently used datasets, Intermediate data.

**Cold Data** — Data that is rarely accessed. Examples: Historical datasets, Old model checkpoints, Archived logs, Old models. This type of data can be moved to Object Storage.

## 21. Data Lifecycle Policies

Data does not necessarily need to remain on the same storage tier forever. A lifecycle policy can move data based on its age or access pattern.

```
Active Training
      │
      ▼
    HOT
      │
      │ Lifecycle Policy
      ▼
    WARM
      │
      │ Lifecycle Policy
      ▼
    COLD
      │
      ▼
Object Storage
```

This can help: Avoid using expensive high-performance storage unnecessarily, Improve storage capacity utilization, Keep long-term data on more appropriate storage.

## 22. Complete Storage Architecture

A high-level AI-centric storage architecture can look like:

```
                         AI Workload
                             │
                             ▼
                           GPU
                             │
                 ┌───────────┼───────────┐
                 │           │           │
                 ▼           ▼           ▼
              NVMe      Parallel FS      NFS
              Local       Shared       Config/
              Fast        Fast         Scripts
                 │           │           │
                 └───────────┼───────────┘
                             │
                             ▼
                       Object Storage
                        Long-Term Data
```

## 23. Full Restaurant Analogy

```
Five-Star Kitchen
        │
        ├── Chef
        │     ↓
        │    GPU
        │
        ├── Waiter
        │     ↓
        │    Network
        │
        └── Pantry
              ↓
            Storage
```

The pantry itself can have different levels:

```
Nearby Shelf
    ↓
NVMe
Fast access

Large Shared Pantry
    ↓
Parallel File System
Shared high-speed access

Common File Cabinet
    ↓
NFS
Config + Scripts

Warehouse
    ↓
Object Storage
Massive long-term storage
```

This is a useful mental model for understanding why an AI data center can use multiple storage technologies.

## 24. NVIDIA and Storage

An important point from the source is: NVIDIA is not directly focused on storage hardware or storage software. However, storage is still essential to an AI data center. Therefore, NVIDIA works with storage ecosystem partners so that their storage solutions can integrate with NVIDIA's hardware and software environment.

```
                NVIDIA AI Platform
                       │
              ┌────────┴────────┐
              │                 │
            Compute          Networking
              │
              │
              ▼
       Storage Partners
              │
       ┌──────┼──────┐
       ▼      ▼      ▼
    NVMe   Parallel  Object
           File      Storage
           System
```

The goal is to create an optimized end-to-end solution: Compute + Network + Storage for AI workloads.

## 25. Final Exam Summary

**AI Storage Requirements** — High Throughput + Low Latency + Scalability

**Four Storage Types**
- NVMe → Fast Local Storage
- Parallel File System → High-Speed Shared Storage for GPU Nodes
- NFS → Configuration and Script Sharing
- Object Storage → Massive Long-Term Storage

**Storage Strategy**
- Hot → Fast Storage
- Warm → Shared / Intermediate Storage
- Cold → Object / Archive Storage

**Architecture** — AI data centers do not normally depend on only one storage type. Instead: Tiered / Hybrid Storage + Lifecycle Policies can be used to place data on the most appropriate storage tier according to its access pattern.

**One-Line Memory Trick** — GPU is the Chef, Network is the Waiter, and Storage is the Pantry. And remember: NVMe = Fast & Local · Parallel File System = Fast & Shared · NFS = Config & Scripts · Object Storage = Massive & Long-Term
