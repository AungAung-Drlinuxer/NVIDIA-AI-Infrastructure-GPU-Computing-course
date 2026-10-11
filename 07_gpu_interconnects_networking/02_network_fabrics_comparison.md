# Module — Comparison of AI Data Center Network Fabrics

> Part II · GPU Interconnects, Networking and Data Movement — topic notes

## မြန်မာဘာသာ version

## 1. Network Fabric ၄ မျိုးကို ပြန်လည်ကြည့်မယ်

အခုဆိုရင် AI-centric Data Center ထဲမှာ အသုံးပြုတဲ့ network fabric ၄ မျိုးကို သိပြီးပါပြီ—

- Compute Network
- Storage Network
- In-Band Management Network
- Out-of-Band (OOB) Management Network

ဒီ section မှာတော့ ဒီ network fabric ၄ မျိုးကို comparison လုပ်ပြီး ပိုမိုနားလည်အောင် လေ့လာပါမယ်။

အဓိက compare လုပ်မယ့်အချက်တွေက—

- Purpose — ဘာအတွက်အသုံးပြုသလဲ?
- Implementation — Physical / Logical ဘယ်လိုတည်ဆောက်သလဲ?
- Key Design Features — ဘာတွေက အရေးကြီးသလဲ?
- Performance / Security / Scalability Considerations

Exam မှာ ဒီ topic ကနေ question ထွက်နိုင်ပါတယ်။

## 2. Overview Comparison

| Network Fabric | Main Purpose | Typical Implementation | Main Requirement |
|---|---|---|---|
| Compute | GPU ↔️ GPU / Node ↔️ Node communication | InfiniBand, RoCE, NVLink | Extremely high throughput + ultra-low latency |
| Storage | Compute nodes ↔️ Storage | InfiniBand, Ethernet/RoCE | High throughput + traffic isolation |
| In-Band Management | Cluster/OS management | Ethernet, VLAN/VXLAN/EVPN | Reliable, secure, isolated management |
| OOB Management | Remote hardware management/recovery | Dedicated physical management network | Always available + strong security |

## 3. Compute Network — Purpose

Compute network ရဲ့ အဓိကရည်ရွယ်ချက်က—**GPU-to-GPU communication within a node or across nodes** ဖြစ်ပါတယ်။

AI workload တွေမှာ GPU အများကြီး ပါဝင်နိုင်ပါတယ်။ GPU တွေဟာ—

- တစ်ခုနဲ့တစ်ခု
- Server တစ်လုံးနဲ့တစ်လုံး
- Compute node တစ်ခုနဲ့တစ်ခု

communication လုပ်ရပါတယ်။ ဒါကြောင့် compute network ဟာ AI workload ရဲ့ backbone တစ်ခုဖြစ်လာပါတယ်။

အထူးသဖြင့်—AI Training, AI Inference, Distributed AI workloads တွေအတွက် အရေးကြီးပါတယ်။

## 4. Compute Network Implementation

Source အရ compute fabric ကို အောက်ပါ technologies တွေနဲ့ implement လုပ်နိုင်ပါတယ်—

- InfiniBand
- RoCE
- NVLink

ဒီ technologies တွေကို နောက်ပိုင်း section တွေမှာ အသေးစိတ် လေ့လာပါမယ်။

အဓိက idea ကတော့—Compute nodes တွေကြားမှာ data ကို အမြန်ဆုံး ရွှေ့နိုင်တဲ့ high-bandwidth interconnect ဖြစ်ရမယ်။

## 5. Compute Network Design Requirements

**① Extremely High Throughput** — AI workload တွေမှာ data အများကြီးကို node တစ်ခုနဲ့တစ်ခု အမြန်ဆုံး ပို့ရပါတယ်။ ဒါကြောင့် throughput အလွန်မြင့်ရပါမယ်။

**② Ultra-Low Latency** — GPU တွေကြား communication မှာ delay နည်းရပါမယ်။ Lower latency = Faster communication

**③ Scalability** — GPU node အရေအတွက် တိုးလာတဲ့အခါ network performance က အလွန်အမင်း မကျသွားရပါဘူး။ ဥပမာ—

```
10 GPU Nodes
     │
     ▼
Good Performance

100 GPU Nodes
     │
     ▼
Should still scale reliably
```

GPU node အရေအတွက် များလာတိုင်း network performance က လက်မခံနိုင်လောက်အောင် ကျသွားရမယ်လို့ မဆိုလိုပါဘူး။

**Compute Fabric Shortcut** — `Compute = High Bandwidth + Ultra-Low Latency + High Throughput + Scalability`

## 6. Storage Network — Purpose

Storage network ရဲ့ အဓိကရည်ရွယ်ချက်က—**Compute nodes တွေကို Storage systems တွေနဲ့ ချိတ်ဆက်ပေးရန်** ဖြစ်ပါတယ်။

ဥပမာ compute servers တွေက storage appliances တွေကို access လုပ်နေတယ်လို့ စဉ်းစားပါ။

```
Compute Nodes
 ┌──────┬──────┬──────┐
 │      │      │      │
 ▼      ▼      ▼      ▼
   ─── Storage Network ───
              │
              ▼
       Storage Systems
```

Storage system တွေမှာ—Storage arrays, File servers, HDFS systems, Parallel file systems စတာတွေ ပါဝင်နိုင်ပါတယ်။

## 7. Storage Network — What Traffic?

Storage network က အောက်ပါ traffic တွေကို support လုပ်နိုင်ပါတယ်—

- File system traffic
- Checkpoint traffic
- Storage I/O
- Data access

AI training မှာ checkpoint ရေးတာ၊ dataset ဖတ်တာတွေဟာ storage traffic အများကြီး ဖြစ်စေနိုင်ပါတယ်။ ဒါကြောင့် storage network ရဲ့ performance က AI workload အတွက် အရေးကြီးပါတယ်။

## 8. Storage Network Implementation

Storage network ကို design ပေါ်မူတည်ပြီး—InfiniBand, Ethernet, RoCE တို့နဲ့ implement လုပ်နိုင်ပါတယ်။

တချို့ environment တွေမှာ technology တစ်ခုတည်းမဟုတ်ဘဲ combination ဖြစ်နိုင်ပါတယ်။

အဓိကက—Storage traffic အတွက် လိုအပ်တဲ့ throughput နဲ့ isolation ရရှိအောင် design လုပ်ရမယ်။

## 9. Storage Network Design Requirements

**① High Throughput** — Compute node တစ်ခုချင်းစီအတွက် multi-GB-level throughput လိုအပ်နိုင်ပါတယ်။

**② Traffic Isolation** — Storage traffic ကို compute traffic ကနေ သီးခြားထားဖို့ အရေးကြီးပါတယ်။ ဥပမာ—

```
Compute Traffic
       │
       ▼
 Compute Fabric

Storage Traffic
       │
       ▼
 Storage Fabric
```

Compute traffic အများကြီး ဖြစ်လာလို့ storage network bottleneck ဖြစ်တာမျိုး မဖြစ်အောင် isolate လုပ်ထားနိုင်ပါတယ်။

**Storage Fabric Shortcut** — `Storage = Compute-to-Storage + High Throughput + I/O + Isolation`

## 10. In-Band Management Network — Purpose

In-band management network ရဲ့ အဓိကတာဝန်က—**Control-plane traffic ကို handle လုပ်ရန်** ဖြစ်ပါတယ်။ ဥပမာ—

- Cluster management
- SSH
- DNS
- Job scheduling
- Configuration
- Deployment
- Monitoring

တို့ကို ဒီ network မှာ သုံးနိုင်ပါတယ်။

## 11. External Repository Access

In-band management network ကို infrastructure operations တွေအတွက်လည်း အသုံးပြုနိုင်ပါတယ်။ ဥပမာ—

- Software dependencies install လုပ်ခြင်း
- OS packages download လုပ်ခြင်း
- Patches apply လုပ်ခြင်း
- External repositories access လုပ်ခြင်း

ဒါကြောင့် management network မှာ security က အရေးကြီးပါတယ်။

## 12. In-Band Management — Performance

Compute network နဲ့ Storage network လောက် throughput အလွန်မြင့်ဖို့ မလိုအပ်ပါဘူး။ ဒါပေမယ့်—**Reliable and reasonably high/moderate bandwidth** ရှိဖို့ လိုပါတယ်။

ဥပမာ patching သို့မဟုတ် package download လုပ်တဲ့အချိန်မှာ traffic တက်နိုင်ပါတယ်။

## 13. In-Band Management — Implementation

Source အရ ဒီ network က—**Ethernet-based** ဖြစ်ပါတယ်။

Network design မှာ Leaf-Spine architecture ကို အသုံးပြုနိုင်ပါတယ်။
Isolation အတွက်—VLAN, VXLAN, EVPN စတဲ့ technologies တွေကို အသုံးပြုနိုင်ပါတယ်။

```
                Spine
             /    |    \
            /     |     \
         Leaf    Leaf    Leaf
          │        │       │
        Servers  Servers  Servers
```

## 14. In-Band Management — Key Considerations

- **Reliability** — Management traffic က reliable ဖြစ်ရပါမယ်။
- **Security** — External repositories နဲ့ infrastructure access ပါဝင်နိုင်တဲ့အတွက် security က အရေးကြီးပါတယ်။
- **Traffic Isolation** — Application/compute traffic နဲ့ management traffic ကို သီးခြားထားနိုင်ရပါမယ်။
- **Adequate Bandwidth** — Compute network လောက် extreme bandwidth မလိုပေမယ့် management operations အတွက် လုံလောက်တဲ့ bandwidth ရှိရပါမယ်။

**In-Band Shortcut** — `In-Band = Control Plane + SSH/DNS + Cluster Management + Patching + Monitoring`

## 15. Out-of-Band Management Network — Purpose

Out-of-Band Management Network ရဲ့ အဓိကရည်ရွယ်ချက်က—**Server ကို remotely manage လုပ်ရန် — server OS မအလုပ်လုပ်တော့တဲ့အချိန်မှာတောင်** ဖြစ်ပါတယ်။

ဥပမာ—

```
Normal Situation

SSH
 │
 ▼
Server OS
```

ဒါပေမယ့်—

```
Server OS
    │
    X
  CRASH
```

ဖြစ်သွားရင် SSH မရတော့နိုင်ပါဘူး။ ဒီအချိန်မှာ—

```
Administrator
      │
      ▼
 OOB Network
      │
      ▼
     BMC
      │
      ▼
    Server
```

လိုမျိုး access လုပ်နိုင်ပါတယ်။

## 16. OOB Management Functions

OOB network က—Remote power control, Serial console, Hardware monitoring, Remote management, Recovery, Troubleshooting စတာတွေကို support လုပ်နိုင်ပါတယ်။

ဒါကြောင့် OOB network ဟာ—**Last-resort management path** လိုမျိုး အရေးကြီးပါတယ်။

## 17. OOB Network — Physical Implementation

OOB network က compute/storage network လောက် bandwidth မလိုအပ်ပါဘူး။ ဘာကြောင့်လဲဆိုတော့ သူ့ရဲ့အဓိကရည်ရွယ်ချက်က—Remote management ဖြစ်ပြီး massive data transfer မဟုတ်လို့ပါ။

ဒါကြောင့်—Dedicated physical management port, Low-speed management switches, Dedicated management network တွေကို အသုံးပြုနိုင်ပါတယ်။

```
Server 1 ──┐
Server 2 ──┤
Server 3 ──┼── OOB Switch
Server 4 ──┤
Server 5 ──┘
```

## 18. OOB Network — Availability

OOB network ရဲ့ အရေးကြီးဆုံး requirement တစ်ခုက—**It must be highly available / always available.**

ဘာကြောင့်လဲဆိုတော့ server failure ဖြစ်တဲ့အချိန်မှာပဲ ဒီ network ကို အများဆုံးလိုအပ်တာ ဖြစ်ပါတယ်။

Normal network ပျက်နေတဲ့အချိန်မှာ OOB network ပါ ပျက်သွားရင် recovery လုပ်ဖို့ အခက်အခဲရှိပါမယ်။

## 19. OOB Network — Security

OOB network မှာ strong security and access control အလွန်အရေးကြီးပါတယ်။ ဘာကြောင့်လဲဆိုတော့ OOB access ရရှိတဲ့သူက server hardware ကို control လုပ်နိုင်တဲ့ capability ရှိနိုင်ပါတယ်။ ဥပမာ—

- Power off
- Power on
- Reboot
- Console access
- Hardware configuration
- Recovery operations

ဒါကြောင့်—**OOB access must be strongly protected.**

အရေးကြီးတဲ့ security controls တွေက—Strong authentication, Role-based access control, Network isolation, Restricted access, Audit logging တို့ ဖြစ်နိုင်ပါတယ်။

## 20. OOB Implementation

OOB management ကို server hardware ပေါ်မှာ ပါလာတဲ့ management controller ကနေ အသုံးပြုနိုင်ပါတယ်။ ဥပမာ—BMC (Baseboard Management Controller) သို့မဟုတ် server vendor ရဲ့ hardware management controller technology ဖြစ်နိုင်ပါတယ်။

Server မှာ dedicated management port ရှိနိုင်ပါတယ်။

```
             Server
        ┌───────────────┐
        │               │
        │     CPU       │
        │      │        │
        │      ▼        │
        │     OS        │
        │               │
        │     BMC       │──── Dedicated Port
        │               │          │
        └───────────────┘          ▼
                              OOB Network
```

## 21. Four Network Fabrics — Full Comparison

| Feature | Compute | Storage | In-Band Management | OOB Management |
|---|---|---|---|---|
| Purpose | GPU/node communication | Compute ↔️ Storage | Control/management | Remote hardware management |
| Typical Traffic | AI compute traffic | I/O, filesystem, checkpoints | SSH, DNS, monitoring, scheduling | Console, power, hardware management |
| Bandwidth | Extremely high | High / multi-GB per node | Moderate | Low |
| Latency | Ultra-low | Performance-sensitive | Moderate | Not primary requirement |
| Typical Technology | InfiniBand, RoCE, NVLink | InfiniBand, Ethernet/RoCE | Ethernet | Dedicated Ethernet/management network |
| Isolation | High | High | High | Very high |
| OS Required | Usually for application traffic | Usually for normal data access | Yes for OS-level management | No host OS dependency for BMC access |
| Failure Recovery | Not primary | Not primary | Normal management | Primary purpose |
| Security | Important | Important | Important | Extremely important |
| Scalability | Critical | Critical | Important | Important |

## 22. The Most Important Difference

ဒီ four fabrics ကို တစ်ကြောင်းတည်းနဲ့ ခွဲမယ်ဆိုရင်—

- **Compute** — "Compute nodes တွေ ဘယ်လောက်မြန်မြန် ဆက်သွယ်နိုင်သလဲ?"
- **Storage** — "Compute nodes တွေ data ကို ဘယ်လောက်မြန်မြန် access လုပ်နိုင်သလဲ?"
- **In-Band Management** — "Server တွေနဲ့ cluster ကို ပုံမှန်အတိုင်း ဘယ်လို manage လုပ်သလဲ?"
- **OOB Management** — "ပုံမှန် path ပျက်သွားရင် server ကို ဘယ်လို manage/recover လုပ်သလဲ?"

## 23. Exam Shortcut

```
COMPUTE
    ↓
GPU ↔️ GPU
Node ↔️ Node
    ↓
InfiniBand / RoCE / NVLink
    ↓
Ultra-Low Latency
Very High Throughput

STORAGE
    ↓
Compute ↔️ Storage
    ↓
InfiniBand / Ethernet / RoCE
    ↓
High Throughput
Traffic Isolation

IN-BAND
    ↓
Cluster Management
SSH / DNS / Scheduling
Patching / Repositories
    ↓
Ethernet
VLAN / VXLAN / EVPN
    ↓
Security + Isolation + Reliability

OOB
    ↓
Hardware Management
Power / Console / Recovery
    ↓
BMC + Dedicated Management Network
    ↓
Low Bandwidth
High Availability
Strong Security
```

## 24. Final Memory Trick

- Compute → Compute traffic ကို အမြန်ဆုံး လုပ်ဆောင်ပါ
- Storage → Data ကို အမြန်ဆုံး ရယူပါ
- In-Band → ပုံမှန်အတိုင်း manage လုပ်ပါ
- OOB → ပုံမှန် management ပျက်သွားရင် recover လုပ်ပါ

အတိုချုပ် version ကတော့—

- Compute = GPU communication
- Storage = Data access
- In-Band = Normal management
- OOB = Emergency management

## 25. Key Exam Points

**Compute Network**
- Purpose: GPU-to-GPU and node-to-node communication
- Technology: InfiniBand, RoCE, NVLink
- Requirement: Extremely high throughput + ultra-low latency + scalability

**Storage Network**
- Purpose: Compute nodes ↔️ storage systems
- Technology: InfiniBand, Ethernet, RoCE
- Requirement: High throughput + I/O performance + isolation

**In-Band Management**
- Purpose: Cluster and OS-level management
- Technology: Ethernet, VLAN, VXLAN, EVPN
- Requirement: Reliability + security + traffic isolation + adequate bandwidth

**Out-of-Band Management**
- Purpose: Remote hardware management and recovery
- Technology: BMC + dedicated management network
- Requirement: Availability + strong security + independent access

**Final One-Line Summary**
Compute fabric က compute traffic ကို အမြန်ဆုံး ရွှေ့ပေးပြီး၊ Storage fabric က compute နဲ့ storage ကြားက data ကို ရွှေ့ပေးပါတယ်။ In-Band fabric က အလုပ်လုပ်နေတဲ့ infrastructure ကို manage လုပ်ပြီး၊ OOB fabric က ပျက်သွားတဲ့ server တွေကို manage/recover လုပ်ဖို့ သီးခြားလမ်းကြောင်း ပေးပါတယ်။

## Self-Check Questions (အဖြေများပါဝင်သည်)

1. **Network fabric ၄ မျိုးကို ဘယ်အချက်တွေနဲ့ compare လုပ်သလဲ?** — Purpose, Implementation (physical/logical), Key Design Features နဲ့ Performance / Security / Scalability အချက်တွေနဲ့ compare လုပ်ပါတယ်။
2. **Compute fabric ရဲ့ design requirement အဓိက ၃ ခုက ဘာတွေလဲ?** — Extremely high throughput, ultra-low latency နဲ့ scalability တို့ ဖြစ်ပြီး InfiniBand, RoCE, NVLink တို့နဲ့ implement လုပ်နိုင်ပါတယ်။
3. **Management traffic ကို ဘာကြောင့် သီးခြားထားသင့်သလဲ?** — Application/compute traffic နဲ့ ရောနှောရင် congestion ဖြစ်နိုင်လို့ ဖြစ်ပါတယ်။ Ethernet ပေါ်မှာ VLAN, VXLAN, EVPN နဲ့ isolate လုပ်ပြီး reliable, secure, adequate bandwidth ရှိရပါမယ်။
4. **OOB network ကို ဘာကြောင့် highly available ဖြစ်အောင် ထားရပြီး strong security လိုအပ်သလဲ?** — Server failure ဖြစ်တဲ့အချိန်မှာ ဒီ network ကို အများဆုံး လိုအပ်လို့ဖြစ်ပြီး၊ OOB access ရတဲ့သူက power, console, hardware control စွမ်းရည် ရနိုင်လို့ strong authentication, RBAC, isolation, audit logging တွေ လိုအပ်ပါတယ်။
5. **Fabric ၄ မျိုးကို တစ်ကြောင်းတည်းနဲ့ ဘယ်လို ခွဲမလဲ?** — Compute က node/GPU ကြား အမြန်ဆုံး ဆက်သွယ်ရေး၊ Storage က data အမြန်ဆုံး access ရရေး၊ In-Band က ပုံမှန် management၊ OOB က ပုံမှန် path ပျက်တဲ့အခါ manage/recover လုပ်ရေး ဖြစ်ပါတယ်။

## English Version

## 1. Reviewing the Four Network Fabrics

By now you already know the four types of network fabric used in an AI-centric data center—

- Compute Network
- Storage Network
- In-Band Management Network
- Out-of-Band (OOB) Management Network

In this section we compare these four network fabrics so that you understand them more deeply.

The main comparison points are—

- Purpose — what is it used for?
- Implementation — how is it built, physically and logically?
- Key Design Features — what matters most?
- Performance / Security / Scalability Considerations

Exam questions can come from this topic.

## 2. Overview Comparison

| Network Fabric | Main Purpose | Typical Implementation | Main Requirement |
|---|---|---|---|
| Compute | GPU ↔️ GPU / Node ↔️ Node communication | InfiniBand, RoCE, NVLink | Extremely high throughput + ultra-low latency |
| Storage | Compute nodes ↔️ Storage | InfiniBand, Ethernet/RoCE | High throughput + traffic isolation |
| In-Band Management | Cluster/OS management | Ethernet, VLAN/VXLAN/EVPN | Reliable, secure, isolated management |
| OOB Management | Remote hardware management/recovery | Dedicated physical management network | Always available + strong security |

## 3. Compute Network — Purpose

The main purpose of the Compute network is **GPU-to-GPU communication within a node or across nodes**.

AI workloads can involve a large number of GPUs. The GPUs have to communicate—

- with one another
- between servers
- between compute nodes

That is why the Compute network becomes a backbone of the AI workload.

It is especially important for AI Training, AI Inference, and Distributed AI workloads.

## 4. Compute Network Implementation

According to the source, a compute fabric can be implemented with the following technologies—

- InfiniBand
- RoCE
- NVLink

We will study these technologies in detail in later sections.

The key idea is that there must be a high-bandwidth interconnect that can move data between compute nodes as fast as possible.

## 5. Compute Network Design Requirements

**① Extremely High Throughput** — In AI workloads, very large amounts of data must be moved from one node to another as fast as possible. Therefore throughput must be extremely high.

**② Ultra-Low Latency** — Communication between GPUs must have minimal delay. Lower latency = Faster communication

**③ Scalability** — When the number of GPU nodes grows, network performance must not degrade dramatically. For example—

```
10 GPU Nodes
     │
     ▼
Good Performance

100 GPU Nodes
     │
     ▼
Should still scale reliably
```

More GPU nodes should not automatically mean unacceptable network slowdown.

**Compute Fabric Shortcut** — `Compute = High Bandwidth + Ultra-Low Latency + High Throughput + Scalability`

## 6. Storage Network — Purpose

The main purpose of the Storage network is **to connect Compute nodes with Storage systems**.

Imagine compute servers accessing storage appliances.

```
Compute Nodes
 ┌──────┬──────┬──────┐
 │      │      │      │
 ▼      ▼      ▼      ▼
   ─── Storage Network ───
              │
              ▼
       Storage Systems
```

Storage systems can include—Storage arrays, File servers, HDFS systems, Parallel file systems, and more.

## 7. Storage Network — What Traffic?

The Storage network can support the following traffic—

- File system traffic
- Checkpoint traffic
- Storage I/O
- Data access

In AI training, writing checkpoints and reading datasets can generate a lot of storage traffic. That is why the performance of the storage network matters for AI workloads.

## 8. Storage Network Implementation

Depending on the design, the Storage network can be implemented with—InfiniBand, Ethernet, or RoCE.

In some environments it may be a combination rather than a single technology.

The key is to design so that the throughput and isolation storage traffic needs are actually delivered.

## 9. Storage Network Design Requirements

**① High Throughput** — Each compute node may need multi-GB-level throughput.

**② Traffic Isolation** — It is important to keep Storage traffic separate from compute traffic. For example—

```
Compute Traffic
       │
       ▼
 Compute Fabric

Storage Traffic
       │
       ▼
 Storage Fabric
```

Isolating the traffic prevents heavy compute traffic from turning the storage network into a bottleneck.

**Storage Fabric Shortcut** — `Storage = Compute-to-Storage + High Throughput + I/O + Isolation`

## 10. In-Band Management Network — Purpose

The main responsibility of the in-band management network is **to handle control-plane traffic**. For example—

- Cluster management
- SSH
- DNS
- Job scheduling
- Configuration
- Deployment
- Monitoring

All of these can be used on this network.

## 11. External Repository Access

The in-band management network can also be used for infrastructure operations. For example—

- Installing software dependencies
- Downloading OS packages
- Applying patches
- Accessing external repositories

That is why security on the management network is important.

## 12. In-Band Management — Performance

It does not need throughput as extremely high as the Compute network or the Storage network. But it does need **Reliable and reasonably high/moderate bandwidth**.

For example, traffic can spike during patching or package downloads.

## 13. In-Band Management — Implementation

According to the source, this network is **Ethernet-based**.

The network design can use a Leaf-Spine architecture.
For isolation—technologies such as VLAN, VXLAN, and EVPN can be used.

```
                Spine
             /    |    \
            /     |     \
         Leaf    Leaf    Leaf
          │        │       │
        Servers  Servers  Servers
```

## 14. In-Band Management — Key Considerations

- **Reliability** — Management traffic must be reliable.
- **Security** — Since it can involve external repositories and infrastructure access, security is critical.
- **Traffic Isolation** — Application/compute traffic and management traffic must be kept separate.
- **Adequate Bandwidth** — It does not need the extreme bandwidth of the Compute network, but it must have enough bandwidth for management operations.

**In-Band Shortcut** — `In-Band = Control Plane + SSH/DNS + Cluster Management + Patching + Monitoring`

## 15. Out-of-Band Management Network — Purpose

The main purpose of the Out-of-Band Management Network is **to manage the server remotely — even when the server OS is no longer working**.

For example—

```
Normal Situation

SSH
 │
 ▼
Server OS
```

But if—

```
Server OS
    │
    X
  CRASH
```

happens, SSH may no longer be available. At that point you can access the server like this—

```
Administrator
      │
      ▼
 OOB Network
      │
      ▼
     BMC
      │
      ▼
    Server
```

## 16. OOB Management Functions

The OOB network can support—Remote power control, Serial console, Hardware monitoring, Remote management, Recovery, Troubleshooting, and more.

That is why the OOB network matters as a **Last-resort management path**.

## 17. OOB Network — Physical Implementation

The OOB network does not need as much bandwidth as the compute/storage networks. Why? Because its main purpose is remote management, not massive data transfer.

Therefore—Dedicated physical management ports, low-speed management switches, and a dedicated management network can be used.

```
Server 1 ──┐
Server 2 ──┤
Server 3 ──┼── OOB Switch
Server 4 ──┤
Server 5 ──┘
```

## 18. OOB Network — Availability

One of the most important requirements for the OOB network is—**It must be highly available / always available.**

Why? Because this network is needed most exactly when a server failure occurs.

If the OOB network also goes down while the normal networks are down, recovery becomes very difficult.

## 19. OOB Network — Security

Strong security and access control are extremely important on the OOB network. Why? Because anyone with OOB access can gain the capability to control server hardware. For example—

- Power off
- Power on
- Reboot
- Console access
- Hardware configuration
- Recovery operations

Therefore—**OOB access must be strongly protected.**

Important security controls include—Strong authentication, Role-based access control, Network isolation, Restricted access, and Audit logging.

## 20. OOB Implementation

OOB management can be provided by a management controller built into the server hardware. For example—a BMC (Baseboard Management Controller) or the server vendor's hardware management controller technology.

The server may have a dedicated management port.

```
             Server
        ┌───────────────┐
        │               │
        │     CPU       │
        │      │        │
        │      ▼        │
        │     OS        │
        │               │
        │     BMC       │──── Dedicated Port
        │               │          │
        └───────────────┘          ▼
                              OOB Network
```

## 21. Four Network Fabrics — Full Comparison

| Feature | Compute | Storage | In-Band Management | OOB Management |
|---|---|---|---|---|
| Purpose | GPU/node communication | Compute ↔️ Storage | Control/management | Remote hardware management |
| Typical Traffic | AI compute traffic | I/O, filesystem, checkpoints | SSH, DNS, monitoring, scheduling | Console, power, hardware management |
| Bandwidth | Extremely high | High / multi-GB per node | Moderate | Low |
| Latency | Ultra-low | Performance-sensitive | Moderate | Not primary requirement |
| Typical Technology | InfiniBand, RoCE, NVLink | InfiniBand, Ethernet/RoCE | Ethernet | Dedicated Ethernet/management network |
| Isolation | High | High | High | Very high |
| OS Required | Usually for application traffic | Usually for normal data access | Yes for OS-level management | No host OS dependency for BMC access |
| Failure Recovery | Not primary | Not primary | Normal management | Primary purpose |
| Security | Important | Important | Important | Extremely important |
| Scalability | Critical | Critical | Important | Important |

## 22. The Most Important Difference

If you had to separate these four fabrics in one line each—

- **Compute** — "How fast can my compute nodes communicate?"
- **Storage** — "How fast can my compute nodes access data?"
- **In-Band Management** — "How do I normally manage my servers and cluster?"
- **OOB Management** — "How do I manage/recover a server when the normal path is broken?"

## 23. Exam Shortcut

```
COMPUTE
    ↓
GPU ↔️ GPU
Node ↔️ Node
    ↓
InfiniBand / RoCE / NVLink
    ↓
Ultra-Low Latency
Very High Throughput

STORAGE
    ↓
Compute ↔️ Storage
    ↓
InfiniBand / Ethernet / RoCE
    ↓
High Throughput
Traffic Isolation

IN-BAND
    ↓
Cluster Management
SSH / DNS / Scheduling
Patching / Repositories
    ↓
Ethernet
VLAN / VXLAN / EVPN
    ↓
Security + Isolation + Reliability

OOB
    ↓
Hardware Management
Power / Console / Recovery
    ↓
BMC + Dedicated Management Network
    ↓
Low Bandwidth
High Availability
Strong Security
```

## 24. Final Memory Trick

- Compute → Compute Fast
- Storage → Get Data Fast
- In-Band → Manage Normally
- OOB → Recover When Normal Management Fails

Or the shortest version:

- Compute = GPU communication
- Storage = Data access
- In-Band = Normal management
- OOB = Emergency management

## 25. Key Exam Points

**Compute Network**
- Purpose: GPU-to-GPU and node-to-node communication
- Technology: InfiniBand, RoCE, NVLink
- Requirement: Extremely high throughput + ultra-low latency + scalability

**Storage Network**
- Purpose: Compute nodes ↔️ storage systems
- Technology: InfiniBand, Ethernet, RoCE
- Requirement: High throughput + I/O performance + isolation

**In-Band Management**
- Purpose: Cluster and OS-level management
- Technology: Ethernet, VLAN, VXLAN, EVPN
- Requirement: Reliability + security + traffic isolation + adequate bandwidth

**Out-of-Band Management**
- Purpose: Remote hardware management and recovery
- Technology: BMC + dedicated management network
- Requirement: Availability + strong security + independent access

**Final One-Line Summary**
Compute fabric moves compute traffic fast; Storage fabric moves data between compute and storage; In-Band fabric manages the running infrastructure; OOB fabric provides an independent path to manage and recover failed servers.

## Self-Check Questions (with answers)

1. **Which criteria are used to compare the four network fabrics?** — Purpose, implementation (physical/logical), key design features, and performance / security / scalability considerations.
2. **What are the three main design requirements of the compute fabric?** — Extremely high throughput, ultra-low latency, and scalability; it can be implemented with InfiniBand, RoCE, or NVLink.
3. **Why should management traffic be isolated?** — Mixing it with application/compute traffic can cause congestion. It is isolated on Ethernet with technologies such as VLAN, VXLAN, and EVPN and needs reliable, secure operation with adequate bandwidth.
4. **Why must the OOB network be highly available and strongly secured?** — It is needed most exactly during server failures, and anyone with OOB access can control power, console, and hardware; strong authentication, role-based access control, isolation, and audit logging are therefore required.
5. **How do the four fabrics differ in one line each?** — Compute: fast GPU/node communication; Storage: fast data access; In-Band: normal management; OOB: manage/recover when the normal path is broken.

## Technical review notes

- Structure normalized to the bilingual convention: blockquote updated to the full Part II title ("GPU Interconnects, Networking and Data Movement"), `## မြန်မာဘာသာ version` and `## English Version` section headers added, and self-check questions with concise answers appended to each language section.
- English sentences inside the Burmese body (section 5 scalability note, section 22 one-line questions, section 24 memory trick, section 25 key exam points) were translated into Burmese; the unchanged English originals remain in the English section. Tables and ASCII diagrams are technical/term-level content and are preserved verbatim in both sections.
- No content removed; section numbering 1–25 matches across both language sections.
- Needs verification: "multi-GB-level throughput" per compute node (section 9) is a design guideline, not a specification — actual requirements depend on the storage platform and workload. The comparison tables give qualitative levels (e.g., "Extremely high", "Ultra-low") without absolute figures.
