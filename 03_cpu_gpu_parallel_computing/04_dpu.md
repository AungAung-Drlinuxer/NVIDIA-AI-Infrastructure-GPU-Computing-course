# Module — DPU (Data Processing Unit)

> Part I · Course Module 03 (CPU vs GPU and Parallel Computing) — topic notes

## 1. DPU — Data Processing Unit

After understanding the CPU and the GPU, the next important compute device is the DPU.

DPU = Data Processing Unit

In modern AI-centric data centers, infrastructure operations can consume significant CPU resources. A DPU helps offload selected infrastructure tasks from the CPU so the CPU can focus on application workloads.

## 2. What Is a DPU?

A DPU is a specialized processor designed to handle data movement and infrastructure tasks.

Conceptually:

```
                DATA CENTER
                      │
       ┌──────────────┼──────────────┐
       │              │              │
      CPU            GPU            DPU
       │              │              │
  Application     AI Compute    Data Movement
  General Tasks   Parallel      Infrastructure
                  Workloads     Offload
```

The idea is simple: the CPU and GPU should not waste resources on tasks that can be handled efficiently by dedicated infrastructure hardware.

## 3. Flight Analogy — CPU, GPU and DPU

Think of an AI data center as an airport system.

- **CPU = Flight operations manager** — coordinates decisions and general operations
- **GPU = Aircraft** — carries the heavy payload (AI computation)
- **DPU = Ground crew, baggage handling, refueling, catering, logistics** — keeps everything moving efficiently

The aircraft (GPU) is designed for flying (AI computation). But an aircraft cannot operate efficiently without ground services (DPU) that handle data movement and infrastructure work behind the scenes.

## 4. CPU vs GPU vs DPU

| Aspect | CPU | GPU | DPU |
|---|---|---|---|
| Primary role | General-purpose computing | Parallel AI/HPC compute | Data movement and infrastructure offload |
| Analogy | Flight operations manager | Aircraft | Ground crew and logistics |
| Typical work | Application logic, OS, control flow | Matrix/tensor operations, AI training, inference | Networking, storage access, security, data transfer |
| Strength | Complex sequential tasks | Massive parallelism | Efficient data movement |

## 5. What Tasks Can a DPU Offload?

DPU-based infrastructure offload can be useful for many data center operations. Examples include:

- Storage access
- Data movement
- Network traffic handling
- Storage protocol processing
- Data transfer acceleration
- Infrastructure processing

Storage-related operations can include:

- File access
- Storage I/O
- Data transfer between storage and compute
- Storage protocol processing

The idea is to move suitable data and infrastructure tasks from the CPU to specialized hardware so the CPU can focus on application workloads.

## 6. Networking Offload

Networking is one of the most important areas where DPU-based infrastructure offload can be useful.

Examples include:

- Packet processing
- Network protocol handling
- Routing-related tasks
- Load balancing
- Network traffic management

In a modern data center, network traffic can be very high. The CPU can handle network processing, but doing so consumes CPU resources.

```
Without DPU Offload

Application
     │
     ▼
    CPU ───► Network processing
     │
     ▼
  Infrastructure tasks compete
  with application workloads

With DPU Offload

Application
     │
     ▼
    CPU
     │
     ▼
 Application workloads

Infrastructure
     │
     ▼
    DPU ───► Network processing
```

## 7. Security Workloads

Security is another important area where DPU-based infrastructure offload can be useful. Examples include:

- Firewall processing
- Packet inspection
- IPsec offload
- TLS offload
- Multi-tenant isolation
- Zero-trust enforcement

The CPU can perform these operations, but doing so consumes CPU resources. The idea behind the DPU is to move suitable infrastructure/security processing onto specialized hardware.

The goal is: Free the CPU and GPU to focus on application and compute workloads.

## 8. DPU as the Control Tower and Ground Services

Think of the DPU as something similar to the control tower and ground services of a data center. Its job is not to replace the CPU or GPU. Instead, it helps make sure that data and infrastructure operations are handled:

- Securely
- Efficiently
- At high throughput
- Without unnecessarily overloading the CPU or GPU

A DPU may therefore provide hardware acceleration and offload for areas such as:

- Networking
- Storage
- Security
- Encryption
- Load balancing
- Data movement

## 9. Summary

```
CPU  = Application and general-purpose computing
GPU  = AI and parallel compute acceleration
DPU  = Data movement and infrastructure offload
```

Remember: The CPU thinks and coordinates, the GPU computes, and the DPU moves and protects data — each device focuses on what it does best so the whole system performs better.

---

## မြန်မာဘာသာ — DPU (Data Processing Unit)

### 1. DPU — Data Processing Unit

CPU နဲ့ GPU ကို နားလည်ပြီးရင် နောက်အရေးကြီးတဲ့ compute device က DPU ဖြစ်ပါတယ်။

DPU = Data Processing Unit

Modern AI-centric data center တွေမှာ infrastructure operation တွေက CPU resource အများကြီး စားသုံးနိုင်ပါတယ်။ DPU က သင့်တော်တဲ့ infrastructure task တွေကို CPU ကနေ ခွဲပေးပြီး (offload) CPU က application workload ပေါ် အာရုံစိုက်နိုင်အောင် ကူညီပါတယ်။

### 2. DPU ဆိုတာ ဘာလဲ?

DPU က data movement နှင့် infrastructure task တွေကို ကိုင်တွယ်ဖို့ ဒီဇိုင်းထုတ်ထားတဲ့ specialized processor ဖြစ်ပါတယ်။

```
                DATA CENTER
                      │
       ┌──────────────┼──────────────┐
       │              │              │
      CPU            GPU            DPU
       │              │              │
  Application     AI Compute    Data Movement
  General Tasks   Parallel      Infrastructure
                  Workloads     Offload
```

အဓိက idea က ရိုးရှင်းပါတယ်— CPU နဲ့ GPU က specialized infrastructure hardware က ထိရောက်စွာ ကိုင်တွယ်နိုင်တဲ့ task တွေအတွက် resource မဖြုန်းသင့်ပါဘူး။

### 3. လေကြောင်း နှိုင်းယှဉ်ချက် — CPU, GPU နှင့် DPU

AI data center ကို လေဆိပ်စနစ်တစ်ခုလို စဉ်းစားကြည့်ပါ။

- **CPU = လေကြောင်း လုပ်ငန်း စီမံခန့်ခွဲသူ** — ဆုံးဖြတ်ချက်များနှင့် ယေဘုယျ လုပ်ငန်းများကို ညှိနှိုင်းပေးသည်
- **GPU = လေယာဉ်** — ဝန်လေးတဲ့ payload (AI computation) ကို သယ်ဆောင်သည်
- **DPU = မြေပြင်ဝန်ထမ်းများ၊ ခရီးဆောင်အိတ် ကိုင်တွယ်ခြင်း၊ ဆီဖြည့်ခြင်း၊ စားသောက်ဖွယ်ရာ၊ logistics** — အားလုံး ထိရောက်စွာ ရွေ့လျားနေစေသည်

လေယာဉ် (GPU) က ပျံသန်းဖို့ (AI computation) ဒီဇိုင်းထုတ်ထားပေမယ့် နောက်ခံမှာ data movement နှင့် infrastructure အလုပ်တွေ ကိုင်တွယ်ပေးတဲ့ ground services (DPU) မပါဘဲ ထိရောက်စွာ မလည်ပတ်နိုင်ပါဘူး။

### 4. CPU vs GPU vs DPU

| Aspect | CPU | GPU | DPU |
|---|---|---|---|
| အဓိက အခန်းကဏ္ဍ | General-purpose computing | Parallel AI/HPC compute | Data movement နှင့် infrastructure offload |
| နှိုင်းယှဉ်ချက် | လေကြောင်း လုပ်ငန်း စီမံခန့်ခွဲသူ | လေယာဉ် | မြေပြင်ဝန်ထမ်းနှင့် logistics |
| ပုံမှန်အလုပ် | Application logic, OS, control flow | Matrix/tensor operation, AI training, inference | Networking, storage access, security, data transfer |
| အားသာချက် | ရှုပ်ထွေးသော sequential task | Massive parallelism | ထိရောက်သော data movement |

### 5. DPU က ဘယ်လို task တွေကို offload လုပ်နိုင်သလဲ?

DPU-based infrastructure offload က data center operation များစွာအတွက် အသုံးဝင်ပါတယ်။ ဥပမာ—

- Storage access
- Data movement
- Network traffic handling
- Storage protocol processing
- Data transfer acceleration
- Infrastructure processing

Storage နှင့် ဆိုင်သော operation များ—

- File access
- Storage I/O
- Storage နှင့် compute ကြား data transfer
- Storage protocol processing

အဓိက idea က သင့်တော်တဲ့ data နှင့် infrastructure task တွေကို CPU ကနေ specialized hardware ဆီ ရွှေ့ပြီး CPU က application workload ပေါ် အာရုံစိုက်နိုင်စေခြင်း ဖြစ်ပါတယ်။

### 6. Networking Offload

Networking က DPU-based infrastructure offload အတွက် အရေးကြီးဆုံးနေရာတွေထဲက တစ်ခုဖြစ်ပါတယ်။ ဥပမာ—

- Packet processing
- Network protocol handling
- Routing-related task များ
- Load balancing
- Network traffic management

Modern data center မှာ network traffic အလွန်များနိုင်ပါတယ်။ CPU က network processing လုပ်နိုင်ပေမယ့် CPU resource စားသုံးပါတယ်။

```
DPU Offload မပါဘဲ

Application
     │
     ▼
    CPU ───► Network processing
     │
     ▼
  Infrastructure task များက
  application workload နှင့် ယှဉ်ပြိုင်နေ

DPU Offload ပါရင်

Application
     │
     ▼
    CPU
     │
     ▼
 Application workload

Infrastructure
     │
     ▼
    DPU ───► Network processing
```

### 7. Security Workloads

Security က DPU-based infrastructure offload အသုံးဝင်နိုင်တဲ့ နောက်ထပ်နေရာတစ်ခုဖြစ်ပါတယ်။ ဥပမာ—

- Firewall processing
- Packet inspection
- IPsec offload
- TLS offload
- Multi-tenant isolation
- Zero-trust enforcement

CPU က ဒီ operation တွေ လုပ်နိုင်ပေမယ့် CPU resource စားသုံးပါတယ်။ DPU ရဲ့ ရည်ရွယ်ချက်က သင့်တော်တဲ့ infrastructure/security processing ကို specialized hardware ဆီ ရွှေ့ခြင်းဖြစ်ပါတယ်။

ရည်မှန်းချက်က— CPU နှင့် GPU ကို application နှင့် compute workload များပေါ် အာရုံစိုက်နိုင်စေရန် ဖြစ်ပါတယ်။

### 8. Data Center ရဲ့ Control Tower နှင့် Ground Services အဖြစ် DPU

DPU ကို data center ရဲ့ control tower နှင့် ground services နှင့် ဆင်တူသည်ဟု စဉ်းစားနိုင်ပါတယ်။ ၎င်းရဲ့ အလုပ်က CPU သို့မဟုတ် GPU ကို အစားထိုးခြင်း မဟုတ်ပါဘူး။ ထိုအစား data နှင့် infrastructure operation တွေကို—

- လုံခြုံစွာ
- ထိရောက်စွာ
- Throughput မြင့်မြင့်
- CPU သို့မဟုတ် GPU ကို မလိုအပ်ဘဲ ဝန်မတင်ဘဲ

ကိုင်တွယ်ပေးနိုင်ပါတယ်။

DPU က အောက်ပါနေရာများအတွက် hardware acceleration နှင့် offload ပေးနိုင်ပါတယ်—

- Networking
- Storage
- Security
- Encryption
- Load balancing
- Data movement

### 9. အကျဉ်းချုပ်

```
CPU  = Application နှင့် general-purpose computing
GPU  = AI နှင့် parallel compute acceleration
DPU  = Data movement နှင့် infrastructure offload
```

မှတ်ရန်— CPU က စဉ်းစားပြီး ညှိနှိုင်းပေးသည်၊ GPU က တွက်ချက်ပေးသည်၊ DPU က data ကို ရွှေ့ပြီး ကာကွယ်ပေးသည် — စက်တစ်ခုချင်းစီက မိမိအကောင်းဆုံးလုပ်နိုင်သော အလုပ်ပေါ် အာရုံစိုက်ခြင်းကြောင့် စနစ်တစ်ခုလုံး ပိုကောင်းမွန်စွာ လုပ်ဆောင်နိုင်ပါတယ်။
