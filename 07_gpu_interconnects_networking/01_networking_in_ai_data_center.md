# Module — Networking in an AI-Centric Data Center

> Part II · GPU Interconnects, Networking and Data Movement — topic notes

## မြန်မာဘာသာ version

## 1. AI-Centric Data Center မှာ Network ဘာကြောင့်လိုသလဲ?

အခုဆိုရင် AI-centric data center ရဲ့ Compute aspect ကို သိပြီးဖြစ်ပါတယ်။
အဓိက compute components တွေက—

- CPU — Central Processing Unit
- GPU — Graphics Processing Unit
- DPU — Data Processing Unit

တို့ဖြစ်ပါတယ်။

ဒါပေမယ့် မေးစရာတစ်ခုရှိပါတယ်။
ဒီ devices တွေတစ်ခုနဲ့တစ်ခု ဘယ်လို communication လုပ်ကြမလဲ?
ဒီနေရာမှာ Network လိုအပ်လာပါတယ်။

AI-centric data center တစ်ခုမှာ server တစ်လုံးတည်းရှိတာမဟုတ်ဘဲ server/node အများကြီးရှိနိုင်ပါတယ်။

```
       AI Data Center
            │
     ┌──────┼──────┐
     │      │      │
   Node 1  Node 2  Node 3
     │      │      │
     └──────┼──────┘
            │
         Network
```

ဒီ nodes တွေကြားမှာ—

- Application traffic
- Compute traffic
- Storage traffic
- Management traffic
- Monitoring traffic

စတာတွေကို ပို့ဆောင်ရပါတယ်။

## 2. ဘာကြောင့် Network တစ်ခုတည်း မသုံးတာလဲ?

ပထမဆုံး စဉ်းစားကြည့်ရအောင်။

"Traffic အားလုံးကို Network တစ်ခုတည်းထဲ ထည့်လိုက်ရင် မရဘူးလား?"

Technically အချို့ environment တွေမှာ လုပ်နိုင်ပါတယ်။
ဒါပေမယ့် AI-centric data center လို high-performance, large-scale environment မှာ traffic အမျိုးအစားအားလုံးကို network တစ်ခုတည်းပေါ်မှာ ထည့်ထားရင် ပြဿနာတွေ ဖြစ်လာနိုင်ပါတယ်။

အဓိကအကြောင်းရင်းက—**Traffic separation / Network isolation** ဖြစ်ပါတယ်။

AI data center မှာ traffic အမျိုးအစားတစ်ခုချင်းစီရဲ့ requirement မတူပါဘူး။ ဥပမာ—

```
Compute Traffic
    → Very High Bandwidth
    → Low Latency

Storage Traffic
    → High Bandwidth
    → High Throughput

Management Traffic
    → Lower Bandwidth
    → Operational / Control

OOB Management
    → Independent Management Path
    → Used for Recovery
```

ဒါကြောင့် traffic ကို logical သို့မဟုတ် physical အနေနဲ့ ခွဲထားနိုင်ပါတယ်။

## 3. Network Separation ရဲ့ အကျိုးကျေးဇူးများ

Network ကို ခွဲထားရတဲ့ အဓိကအကြောင်းရင်းတွေက—

- Performance Isolation
- Latency Isolation
- Failure Isolation
- Security
- Scalability

ဖြစ်ပါတယ်။

## 4. Performance Isolation

Compute network နဲ့ Storage network က bandwidth အများကြီး လိုနိုင်ပါတယ်။ ဥပမာ—

```
Compute
   │
   └── High Bandwidth

Storage
   │
   └── High Bandwidth

Management
   │
   └── Relatively Lower Bandwidth
```

Management traffic က compute/storage traffic လောက် bandwidth မလိုအပ်နိုင်ပါဘူး။

ဒါကြောင့် traffic အားလုံးကို network တစ်ခုတည်းထဲမှာထားရင် management traffic နဲ့ compute traffic တို့က resource ကို share လုပ်နေရပါမယ်။

Network ခွဲထားခြင်းက—Performance ကို တစ်ခုနဲ့တစ်ခု မလိုအပ်ဘဲ ထိခိုက်မှုနည်းစေပါတယ်။

## 5. Latency Isolation

AI workload တွေမှာ latency က အရေးကြီးပါတယ်။
အထူးသဖြင့် compute traffic တချို့ဟာ latency-sensitive ဖြစ်နိုင်ပါတယ်။

Compute traffic ကို management traffic နဲ့ storage traffic တို့နဲ့ အလွန်အကျွံ ရောနှောထားရင် congestion ဖြစ်နိုင်ပါတယ်။

ဒါကြောင့်—Latency-sensitive compute traffic ကို သီးခြား network/fabric ပေါ်မှာ isolate လုပ်ခြင်းက performance အတွက် အကျိုးရှိပါတယ်။

## 6. Failure Isolation and Robustness

နောက်ထပ်အရေးကြီးတာက Failure Isolation ဖြစ်ပါတယ်။

Network တစ်ခုတည်းကို traffic အားလုံးအတွက် အသုံးပြုထားတယ်ဆိုပါစို့။

```
              Single Network
                   │
        ┌──────────┼──────────┐
        │          │          │
     Compute    Storage   Management
        │          │          │
        └──────────┼──────────┘
                   │
                FAILURE
                   ↓
            Everything affected
```

Network တစ်ခုမှာ outage ဖြစ်သွားရင် traffic အမျိုးအစားအားလုံး ထိခိုက်နိုင်ပါတယ်။

Network ကို ခွဲထားရင်—

```
Compute Network    → OK
Storage Network    → OK
Management Network → FAILURE
OOB Network        → OK
```

လိုမျိုး fault isolation ပိုကောင်းလာနိုင်ပါတယ်။

အထူးသဖြင့် Out-of-Band Management Network ရှိထားရင် operating network တစ်ခု ပြဿနာဖြစ်နေတဲ့အချိန်မှာတောင် system ကို remotely access လုပ်နိုင်ပါတယ်။

## 7. Security

Network separation က security အတွက်လည်း အရေးကြီးပါတယ်။

External-facing traffic နဲ့ internal infrastructure traffic ကို တူညီတဲ့ network path ပေါ်မှာ အားလုံးရောထားတာထက်—Traffic အမျိုးအစားအလိုက် access control နဲ့ security policy တွေကို ခွဲပြီး သတ်မှတ်နိုင်ခြင်းက ပိုကောင်းပါတယ်။ ဥပမာ—

```
External / User Traffic
        │
        ▼
   Stronger Controls
        │
        ▼
   Application

Internal Infrastructure
        │
        ▼
   Internal Controls
```

ဒီလို separation ကြောင့် security policy ကို workload တစ်ခုချင်းစီအလိုက် ပိုပြီး တိကျစွာ သတ်မှတ်နိုင်ပါတယ်။

## 8. Scalability

နောက်ထပ်အကြောင်းရင်းက Scalability ဖြစ်ပါတယ်။

Compute workload တိုးလာရင် compute network capacity ပိုလိုလာနိုင်ပါတယ်။
Storage workload တိုးလာရင် storage network capacity ပိုလိုလာနိုင်ပါတယ်။

ဒါပေမယ့် compute နဲ့ storage တို့ရဲ့ growth rate တူချင်မှ တူပါမယ်။ ဥပမာ—

```
Compute Requirement
        │
        ├── 2× Growth

Storage Requirement
        │
        └── 5× Growth
```

Network separation ရှိထားရင် workload တစ်ခုချင်းစီရဲ့ requirement အလိုက် scale လုပ်နိုင်ပါတယ်။

## 9. What is Network Fabric?

AI-centric data center networking မှာ Network Fabric ဆိုတဲ့ term ကို ကြားရပါမယ်။

အလွယ်ပြောရရင်—Network Fabric ဆိုတာ traffic အမျိုးအစားအလိုက် communication ကို handle လုပ်ပေးတဲ့ logical/physical networking infrastructure ဖြစ်ပါတယ်။

AI-centric data center မှာ network fabric အမျိုးမျိုး ရှိနိုင်ပါတယ်။
ဒီ lesson ရဲ့ framework အရ အဓိက ၄ မျိုးကို ပြောထားပါတယ်။

- Compute Fabric
- Storage Fabric
- In-Band Management Fabric
- Out-of-Band Management Fabric

## 10. Compute Network / Compute Fabric

ပထမဆုံးက—Compute Network ဖြစ်ပါတယ်။

AI data center ထဲမှာ node အများကြီးရှိပါတယ်။ ဒီ nodes တွေဟာ တစ်ခုနဲ့တစ်ခု communication လုပ်ရပါတယ်။ ဥပမာ—

```
Node 1 ─────┐
            │
Node 2 ─────┼── Compute Network
            │
Node 3 ─────┤
            │
Node 4 ─────┘
```

ဒီ network ပေါ်မှာ—

- Application traffic
- Inter-node communication
- Distributed computation
- Data exchange between nodes

စတာတွေ ဖြစ်နိုင်ပါတယ်။

AI workload တွေမှာ GPU nodes အများကြီးက workload တစ်ခုကို distributed အနေနဲ့ လုပ်နိုင်တဲ့အတွက် node-to-node communication က အရေးကြီးပါတယ်။

ဒါကြောင့် compute network ဟာ high bandwidth and low latency လိုအပ်နိုင်ပါတယ်။

## 11. Storage Network / Storage Fabric

AI workloads တွေမှာ data အများကြီးလိုအပ်ပါတယ်။ ဥပမာ—

- Training datasets
- Model files
- Checkpoints
- Logs
- Generated data

Compute nodes တွေက storage system ထဲက data တွေကို access လုပ်ရပါတယ်။

```
       Compute Nodes
      ┌────┬────┬────┐
      │    │    │    │
      ▼    ▼    ▼    ▼
     ────────────────
       Storage Network
     ────────────────
              │
              ▼
         Storage System
```

Storage network ရဲ့ အဓိကရည်ရွယ်ချက်က—Compute nodes တွေလိုအပ်တဲ့ data ကို performance bottleneck မဖြစ်အောင် ပို့ပေးနိုင်ရန် ဖြစ်ပါတယ်။

Storage network မှာ—High throughput, High bandwidth, Reliable data access တို့က အရေးကြီးပါတယ်။

## 12. In-Band Management Network

နောက်ထပ် network တစ်ခုက—In-Band Management Network ဖြစ်ပါတယ်။

ဒီ network ကို management activities အတွက် အသုံးပြုပါတယ်။ ဥပမာ—

- Configuration
- Deployment
- OS updates
- Monitoring
- Operational management
- Collecting monitoring data

Server ရဲ့ Operating System running ဖြစ်နေတဲ့အချိန်မှာ OS-level network path ကနေ management လုပ်နိုင်ပါတယ်။

## 13. ဘာကြောင့် In-Band Management Network ကို သီးခြားထားတာလဲ?

Application traffic နဲ့ management traffic ကို အတူတူ network ပေါ်မှာ အားလုံးပေါင်းထားရင် traffic congestion ဖြစ်နိုင်ပါတယ်။

ဒါကြောင့် management traffic ကို သီးခြား network/fabric ပေါ်မှာထားနိုင်ပါတယ်။

```
Application Traffic
       │
       ▼
 Compute Network

Management Traffic
       │
       ▼
 In-Band Management Network
```

ဒီလိုခွဲထားခြင်းကြောင့် management activities က application traffic ကို မလိုအပ်ဘဲ အနှောင့်အယှက်မဖြစ်စေဖို့ ကူညီနိုင်ပါတယ်။

## 14. Out-of-Band Management Network

နောက်ဆုံးက—Out-of-Band Management Network အတိုကောက် OOB ဖြစ်ပါတယ်။

ဒီ network ရဲ့ အဓိကရည်ရွယ်ချက်က—Server ရဲ့ Operating System မအလုပ်လုပ်တော့တဲ့အချိန်မှာတောင် server ကို remotely manage လုပ်နိုင်ရန် ဖြစ်ပါတယ်။

ဒါက In-Band Management နဲ့ အဓိကကွာခြားချက်ပါ။

## 15. In-Band vs Out-of-Band

ဥပမာ server တစ်လုံးမှာ OS crash ဖြစ်သွားတယ်ဆိုပါစို့။

```
Server
   │
   └── Operating System
             │
             X
           CRASH
```

အဲဒီအချိန်မှာ ပုံမှန် network access မရနိုင်တော့ပါဘူး။ ဥပမာ—SSH, RDP, OS-level management tools တွေကို အသုံးပြုဖို့ ခက်ခဲသွားနိုင်ပါတယ်။

ဒီအချိန်မှာ OOB Management က အသုံးဝင်ပါတယ်။

## 16. BMC — Baseboard Management Controller

Server hardware မှာ BMC — Baseboard Management Controller ဆိုတဲ့ management controller တစ်ခု ရှိနိုင်ပါတယ်။

BMC = Baseboard Management Controller

BMC ဟာ host Operating System နဲ့ သီးခြား management capability ပေးနိုင်ပါတယ်။ Conceptually—

```
                Server
        ┌──────────────────┐
        │                  │
        │       CPU        │
        │        │         │
        │        ▼         │
        │       OS         │
        │                  │
        │       BMC        │
        │        │         │
        └────────┼─────────┘
                 │
                 ▼
          OOB Management
             Network
```

ဒါကြောင့် host OS က crash ဖြစ်နေရင်တောင် BMC-based management path ကနေ hardware ကို remotely manage လုပ်နိုင်တဲ့ capability ရှိနိုင်ပါတယ်။

## 17. OOB Management နဲ့ ဘာတွေ လုပ်နိုင်သလဲ?

OOB management ကို အသုံးပြုပြီး server infrastructure ကို remotely manage/monitor လုပ်နိုင်ပါတယ်။ ဥပမာ—

- Remote control
- Hardware monitoring
- System health monitoring
- Remote power control
- Hardware-level troubleshooting
- Logging

အရေးကြီးဆုံး concept က—OS မအလုပ်လုပ်တော့တဲ့အချိန်မှာတောင် management path တစ်ခုရှိနေခြင်း ဖြစ်ပါတယ်။

## 18. In-Band vs OOB Management

| Feature | In-Band Management | Out-of-Band Management |
|---|---|---|
| Main purpose | Normal system management | Recovery / independent management |
| OS required | Generally yes | No host OS dependency for core BMC access |
| Network path | Production/OS network path or dedicated management fabric | Independent management path |
| SSH/RDP | Can be used | Not dependent on them |
| OS crash | Usually inaccessible | Still potentially accessible |
| Hardware-level access | Limited | Stronger |
| Remote power control | Usually not | Common BMC capability |
| Recovery | Normal operations | Failure/recovery scenarios |

## 19. The Four Network Fabrics

အခုဆိုရင် AI-centric data center မှာ network fabric ၄ မျိုးကို ဒီလို မှတ်နိုင်ပါတယ်။

```
                 AI DATA CENTER NETWORK
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
       ▼                  ▼                  ▼
    COMPUTE            STORAGE          MANAGEMENT
       │                  │                  │
       │                  │          ┌───────┴───────┐
       │                  │          │               │
       ▼                  ▼          ▼               ▼
   Compute Data       Storage     In-Band           OOB
    / Traffic         Traffic    Management      Management
```

1. **Compute** — Node-to-node communication
2. **Storage** — Data access between compute nodes and storage
3. **In-Band Management** — Configuration, deployment, monitoring, OS-level management
4. **Out-of-Band Management** — Independent remote management and recovery

## 20. Simple Memory Trick

ဒီ ၄ ခုကို Exam အတွက် ဒီလိုမှတ်ပါ။

- Compute → Compute
- Storage → Data
- In-Band → Manage the OS
- OOB → Manage even when the OS is down

ဒါမှမဟုတ်—

- Compute = Communicate
- Storage = Retrieve Data
- In-Band = Manage Normally
- OOB = Recover Remotely

## 21. Final Exam Summary

AI-centric data center တစ်ခုမှာ Network ကို traffic အားလုံးအတွက် တစ်ခုတည်းသုံးမယ့်အစား separation လုပ်နိုင်ပါတယ်။ အဓိကအကြောင်းရင်းတွေက—

- Performance Isolation
- Latency Isolation
- Failure Isolation
- Security
- Scalability

ဖြစ်ပါတယ်။

ပြီးတော့ အဓိက network fabrics ၄ မျိုးက—

- **Compute Fabric** — Node-to-node communication နဲ့ application/compute traffic အတွက်။
- **Storage Fabric** — Compute nodes တွေက storage data ကို high-performance နဲ့ access လုပ်နိုင်ဖို့။
- **In-Band Management Fabric** — Configuration, deployment, monitoring နဲ့ normal OS-level management အတွက်။
- **Out-of-Band Management Fabric** — OS မအလုပ်လုပ်တော့တဲ့အချိန်မှာတောင် BMC ကတစ်ဆင့် server ကို remotely manage/recover လုပ်နိုင်ဖို့။

## 22. One-Line Memory

`AI Data Center Network = Compute + Storage + In-Band Management + Out-of-Band Management`

ပြီးတော့—`Separate the traffic to isolate performance, latency, failures, security, and scalability requirements.`

## Self-Check Questions (အဖြေများပါဝင်သည်)

1. **AI-centric data center မှာ network ကို ဘာကြောင့် traffic အမျိုးအစားအလိုက် ခွဲသင့်သလဲ?** — Traffic တစ်မျိုးချင်းစီရဲ့ bandwidth, latency, security requirement တွေ မတူညီလို့ ဖြစ်ပါတယ်။ ခွဲထားခြင်းက performance, latency, failure isolation, security, scalability အတွက် အကျိုးရှိပါတယ်။
2. **အဓိက network fabric ၄ မျိုးက ဘာတွေလဲ?** — Compute Fabric, Storage Fabric, In-Band Management Fabric, Out-of-Band Management Fabric တို့ ဖြစ်ပါတယ်။
3. **In-Band Management နဲ့ Out-of-Band Management ရဲ့ အဓိကကွာခြားချက်က ဘာလဲ?** — In-Band က host OS အလုပ်လုပ်နေချိန်မှာ OS-level network path ကနေ manage လုပ်တာဖြစ်ပြီး၊ OOB က OS မအလုပ်လုပ်တော့ချိန်မှာတောင် BMC ကတစ်ဆင့် server ကို remotely manage/recover လုပ်နိုင်တာ ဖြစ်ပါတယ်။
4. **BMC ဆိုတာ ဘာလဲ?** — Baseboard Management Controller ဖြစ်ပြီး host Operating System နဲ့ သီးခြား management capability ပေးတဲ့ server hardware ပေါ်က management controller ဖြစ်ပါတယ်။
5. **Storage Fabric ရဲ့ ရည်ရွယ်ချက်က ဘာလဲ?** — Compute nodes တွေကို storage system နဲ့ ချိတ်ဆက်ပေးပြီး data ကို performance bottleneck မဖြစ်အောင် high throughput, high bandwidth, reliable data access တွေနဲ့ ပို့ပေးရန် ဖြစ်ပါတယ်။

## English Version

## 1. Why Do We Need a Network in an AI-Centric Data Center?

By now, we already understand the compute aspect of an AI-centric data center.
The main compute components are:

- CPU — Central Processing Unit
- GPU — Graphics Processing Unit
- DPU — Data Processing Unit

But one question remains.
How will these devices communicate with each other?
This is where the Network becomes necessary.

An AI-centric data center does not have just a single server — it can have many servers/nodes.

```
       AI Data Center
            │
     ┌──────┼──────┐
     │      │      │
   Node 1  Node 2  Node 3
     │      │      │
     └──────┼──────┘
            │
         Network
```

Between these nodes, we have to carry:

- Application traffic
- Compute traffic
- Storage traffic
- Management traffic
- Monitoring traffic

## 2. Why Not Use a Single Network?

Let's think about this first.

"Can't we just put all the traffic into a single network?"

Technically, in some environments, you can.
But in a high-performance, large-scale environment like an AI-centric data center, putting every traffic type on one network can cause problems.

The main reason is **traffic separation / network isolation**.

In an AI data center, each traffic type has different requirements. For example:

```
Compute Traffic
    → Very High Bandwidth
    → Low Latency

Storage Traffic
    → High Bandwidth
    → High Throughput

Management Traffic
    → Lower Bandwidth
    → Operational / Control

OOB Management
    → Independent Management Path
    → Used for Recovery
```

That is why traffic can be separated logically or physically.

## 3. Benefits of Network Separation

The main reasons for separating the network are:

- Performance Isolation
- Latency Isolation
- Failure Isolation
- Security
- Scalability

## 4. Performance Isolation

The compute network and the storage network can need a lot of bandwidth. For example:

```
Compute
   │
   └── High Bandwidth

Storage
   │
   └── High Bandwidth

Management
   │
   └── Relatively Lower Bandwidth
```

Management traffic may not need as much bandwidth as compute/storage traffic.

If all traffic shares a single network, management traffic and compute traffic have to share resources.

Separating the network means that one kind of traffic does not unnecessarily degrade the performance of another.

## 5. Latency Isolation

Latency matters in AI workloads.
In particular, some compute traffic can be latency-sensitive.

Mixing compute traffic too heavily with management traffic and storage traffic can cause congestion.

That is why isolating latency-sensitive compute traffic on its own network/fabric benefits performance.

## 6. Failure Isolation and Robustness

Another important point is failure isolation.

Suppose a single network carries all traffic.

```
              Single Network
                   │
        ┌──────────┼──────────┐
        │          │          │
     Compute    Storage   Management
        │          │          │
        └──────────┼──────────┘
                   │
                FAILURE
                   ↓
            Everything affected
```

If that one network has an outage, every traffic type can be affected.

If the network is separated, you can get better fault isolation like this:

```
Compute Network    → OK
Storage Network    → OK
Management Network → FAILURE
OOB Network        → OK
```

In particular, if you have an Out-of-Band Management Network, you can still access the system remotely even while an operating network is having problems.

## 7. Security

Network separation is also important for security.

Rather than mixing external-facing traffic and internal infrastructure traffic on the same network path, it is better to define access control and security policies separately for each traffic type. For example:

```
External / User Traffic
        │
        ▼
   Stronger Controls
        │
        ▼
   Application

Internal Infrastructure
        │
        ▼
   Internal Controls
```

This kind of separation lets you define security policy more precisely, per workload.

## 8. Scalability

Another reason is scalability.

As compute workloads grow, you may need more compute network capacity.
As storage workloads grow, you may need more storage network capacity.

But compute and storage do not necessarily grow at the same rate. For example:

```
Compute Requirement
        │
        ├── 2× Growth

Storage Requirement
        │
        └── 5× Growth
```

With network separation, you can scale each workload according to its own requirements.

## 9. What is Network Fabric?

In AI-centric data center networking, you will hear the term Network Fabric.

Simply put, a Network Fabric is the logical/physical networking infrastructure that handles communication for each traffic type.

An AI-centric data center can have several kinds of network fabric.
According to this lesson's framework, the main four are:

- Compute Fabric
- Storage Fabric
- In-Band Management Fabric
- Out-of-Band Management Fabric

## 10. Compute Network / Compute Fabric

The first one is the Compute Network.

There are many nodes inside an AI data center, and these nodes must communicate with each other. For example:

```
Node 1 ─────┐
            │
Node 2 ─────┼── Compute Network
            │
Node 3 ─────┤
            │
Node 4 ─────┘
```

On this network, you can have:

- Application traffic
- Inter-node communication
- Distributed computation
- Data exchange between nodes

In AI workloads, many GPU nodes work on a single workload in a distributed way, so node-to-node communication is critical.

That is why the compute network may require high bandwidth and low latency.

## 11. Storage Network / Storage Fabric

AI workloads need a lot of data. For example:

- Training datasets
- Model files
- Checkpoints
- Logs
- Generated data

Compute nodes must access the data in the storage system.

```
       Compute Nodes
      ┌────┬────┬────┐
      │    │    │    │
      ▼    ▼    ▼    ▼
     ────────────────
       Storage Network
     ────────────────
              │
              ▼
         Storage System
```

The main purpose of the storage network is to deliver the data the compute nodes need without becoming a performance bottleneck.

For a storage network, high throughput, high bandwidth, and reliable data access are what matter most.

## 12. In-Band Management Network

Another network is the In-Band Management Network.

This network is used for management activities. For example:

- Configuration
- Deployment
- OS updates
- Monitoring
- Operational management
- Collecting monitoring data

While the server's Operating System is running, you can manage it over the OS-level network path.

## 13. Why Keep the In-Band Management Network Separate?

If application traffic and management traffic are combined on the same network, traffic congestion can occur.

That is why management traffic can be placed on its own network/fabric.

```
Application Traffic
       │
       ▼
 Compute Network

Management Traffic
       │
       ▼
 In-Band Management Network
```

This separation helps management activities avoid unnecessarily disrupting application traffic.

## 14. Out-of-Band Management Network

The last one is the Out-of-Band Management Network, abbreviated OOB.

The main purpose of this network is to let you manage the server remotely even when the server's Operating System is no longer working.

This is the key difference from In-Band Management.

## 15. In-Band vs Out-of-Band

Suppose a server's OS crashes.

```
Server
   │
   └── Operating System
             │
             X
           CRASH
```

At that point, normal network access is no longer available. For example, SSH, RDP, and OS-level management tools become hard or impossible to use.

This is exactly when OOB Management becomes useful.

## 16. BMC — Baseboard Management Controller

Server hardware can include a management controller called the BMC — Baseboard Management Controller.

BMC = Baseboard Management Controller

The BMC provides a management capability separate from the host Operating System. Conceptually:

```
                Server
        ┌──────────────────┐
        │                  │
        │       CPU        │
        │        │         │
        │        ▼         │
        │       OS         │
        │                  │
        │       BMC        │
        │        │         │
        └────────┼─────────┘
                 │
                 ▼
          OOB Management
             Network
```

So even if the host OS has crashed, a BMC-based management path can still let you manage the hardware remotely.

## 17. What Can You Do with OOB Management?

Using OOB management, you can remotely manage and monitor server infrastructure. For example:

- Remote control
- Hardware monitoring
- System health monitoring
- Remote power control
- Hardware-level troubleshooting
- Logging

The most important concept is that a management path still exists even when the OS is no longer working.

## 18. In-Band vs OOB Management

| Feature | In-Band Management | Out-of-Band Management |
|---|---|---|
| Main purpose | Normal system management | Recovery / independent management |
| OS required | Generally yes | No host OS dependency for core BMC access |
| Network path | Production/OS network path or dedicated management fabric | Independent management path |
| SSH/RDP | Can be used | Not dependent on them |
| OS crash | Usually inaccessible | Still potentially accessible |
| Hardware-level access | Limited | Stronger |
| Remote power control | Usually not | Common BMC capability |
| Recovery | Normal operations | Failure/recovery scenarios |

## 19. The Four Network Fabrics

Now you can remember the four network fabrics in an AI-centric data center like this.

```
                 AI DATA CENTER NETWORK
                          │
       ┌──────────────────┼──────────────────┐
       │                  │                  │
       ▼                  ▼                  ▼
    COMPUTE            STORAGE          MANAGEMENT
       │                  │                  │
       │                  │          ┌───────┴───────┐
       │                  │          │               │
       ▼                  ▼          ▼               ▼
   Compute Data       Storage     In-Band           OOB
    / Traffic         Traffic    Management      Management
```

1. **Compute** — Node-to-node communication
2. **Storage** — Data access between compute nodes and storage
3. **In-Band Management** — Configuration, deployment, monitoring, OS-level management
4. **Out-of-Band Management** — Independent remote management and recovery

## 20. Simple Memory Trick

Remember these four for the exam like this:

- Compute → Compute
- Storage → Data
- In-Band → Manage the OS
- OOB → Manage even when the OS is down

Or:

- Compute = Communicate
- Storage = Retrieve Data
- In-Band = Manage Normally
- OOB = Recover Remotely

## 21. Final Exam Summary

In an AI-centric data center, instead of using a single network for all traffic, you can separate it. The main reasons are:

- Performance Isolation
- Latency Isolation
- Failure Isolation
- Security
- Scalability

And the four main network fabrics are:

- **Compute Fabric** — For node-to-node communication and application/compute traffic.
- **Storage Fabric** — So compute nodes can access storage data with high performance.
- **In-Band Management Fabric** — For configuration, deployment, monitoring, and normal OS-level management.
- **Out-of-Band Management Fabric** — So you can remotely manage and recover the server through the BMC even when the OS is no longer working.

## 22. One-Line Memory

`AI Data Center Network = Compute + Storage + In-Band Management + Out-of-Band Management`

And: `Separate the traffic to isolate performance, latency, failures, security, and scalability requirements.`

## Self-Check Questions (with answers)

1. **Why separate traffic types onto different networks in an AI-centric data center?** — Each traffic type has different bandwidth, latency, and security requirements; separation provides performance, latency, and failure isolation plus better security and scalability.
2. **What are the four main network fabrics?** — Compute Fabric, Storage Fabric, In-Band Management Fabric, and Out-of-Band Management Fabric.
3. **What is the key difference between In-Band and Out-of-Band management?** — In-Band management uses the OS-level network path while the host OS is running; OOB uses the BMC over an independent path to manage and recover the server even when the OS is down.
4. **What is a BMC?** — A Baseboard Management Controller: a management controller on the server hardware that provides management capability independent of the host Operating System.
5. **What is the purpose of the Storage Fabric?** — To connect compute nodes to the storage system and deliver data with high throughput, high bandwidth, and reliable access without becoming a performance bottleneck.

## Technical review notes

- Structure normalized to the bilingual convention: blockquote updated to the full Part II title ("GPU Interconnects, Networking and Data Movement"), `## မြန်မာဘာသာ version` and `## English Version` section headers added, the redundant `---` separator removed, and the English section's subsection levels (`### N.`) aligned with the Burmese section (`## N.`) so numbering matches across both language sections.
- Self-check questions with concise answers added at the end of each language section.
- No content removed: all paragraphs, lists, ASCII diagrams, and the In-Band vs OOB comparison table are preserved in both language sections.
- Needs verification: the In-Band vs OOB table rows "OS required — No host OS dependency for core BMC access" and "Remote power control — Common BMC capability" are broadly true for BMC-equipped servers but are vendor/implementation-dependent (verify per server vendor and BMC firmware).
- The note is conceptual; it asserts no commands, benchmark numbers, or model-specific specifications.
