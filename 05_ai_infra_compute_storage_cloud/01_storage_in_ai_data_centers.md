# Module 05 — Storage in AI-Centric Data Centers

> Part I · AI Foundations and AI Infrastructure — Course Module 05 (AI Infrastructure: Compute, Storage and Cloud) — topic notes

## မြန်မာဘာသာ version

## 1. Compute နှင့် Network ပြီးနောက် Storage ကို အာရုံစိုက်ပါ

AI ကို ဗဟိုပြုသော data center တစ်ခုအကြောင်း ဆွေးနွေးသောအခါ အဓိက အစိတ်အပိုင်း သုံးခု ရှိပါသည် —

```
Compute
   +
Network
   +
Storage
```

- Compute က အချက်အလက်များကို စီမံဆောင်ရွက်ပေးပါသည် (processing)။
- Network က system များကြား data ကို ရွှေ့ပြောင်းပေးပါသည်။
- Storage က AI workloads များ လိုအပ်သော data ကို ပေးအပ်ပြီး ထို data ကို compute resources များ အသုံးပြုနိုင်စေပါသည်။

## 2. စားသောက်ဆိုင် ဥပမာ — AI Data Center ကို ငါးပွင့်မီးဖိုချောင်အဖြစ် တွေးကြည့်ပါ

အသုံးဝင်သော ဥပမာတစ်ခုမှာ ငါးပွင့်စားသောက်ဆိုင် တစ်ခု ဖြစ်ပါသည်။

```
Five-Star Restaurant

Chef       → GPU
Waiters    → Network
Pantry     → Storage
Customers  → Users / Applications
```

**Chef = GPU** — စားဖိုမှူးက တကယ်ချက်ပြုတ်ပြီး ဟင်းလျာများကို ဖန်တီးပါသည်။ အလားတူပင် GPU က AI computation၊ Model training၊ Inference၊ Tensor နှင့် matrix operations များကို လုပ်ဆောင်ပါသည်။

## 3. Waiter = Network

စားပွဲထိုးက ချက်ပြုတ်ပြီးသား အစားအစာများကို ဧည့်သည်ဆီ ရောက်အောင် ကူညီပေးပါသည်။ ဤအချက်သည် network ၏ အခန်းကဏ္ဍနှင့် ဆင်တူပါသည်။

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

Network က system များကြား data ကို သယ်ပို့ပေးပါသည်။

## 4. Pantry = Storage

စားသောက်ဆိုင်တစ်ခုအတွက် ပစ္စည်းအပြည့်ရှိသော pantry ကောင်းကောင်း လိုအပ်ပါသည်။ Pantry ထဲတွင် ဟင်းချက်ပစ္စည်းများဖြစ်သော ဟင်းသီးဟင်းရွက်၊ အသား၊ ဆန်၊ ဟင်းခတ်အမွှေးအကြိုင်၊ အရသာရည်များ၊ အခြားပါဝင်ပစ္စည်းများ ပါရှိပါသည်။ စားဖိုမှူးအနေဖြင့် ဤပစ္စည်းများကို အမြန်ဆုံး ရယူအသုံးပြုနိုင်ရန် လိုအပ်ပါသည်။

AI data center တွင် Pantry = Storage ဖြစ်ပါသည်။ Storage ထဲတွင် Training datasets၊ Model files၊ Checkpoints၊ Logs၊ Configuration data၊ Scripts၊ Historical data များ ပါရှိနိုင်ပါသည်။

## 5. Storage ၏ အခန်းကဏ္ဍ

အခြေခံလိုအပ်ချက်မှာ — **GPU များ လိုအပ်သော data ကို Storage က အမြန်ဆုံး ဝင်ရောက်အသုံးပြုနိုင်အောင် ပေးအပ်ရမည်** ဟူ၍ ဖြစ်ပါသည်။

စားသောက်ဆိုင်အကြောင်းကို ပြန်စဉ်းစားကြည့်ပါ။ စားဖိုမှူး မည်မျှပင် အမြန်ချက်ပြုတ်နိုင်စေကာမူ၊ pantry က ပစ္စည်းများ ပေးအပ်ရာတွင် အချိန်ကြာမြင့်ပါက ဆိုင်တစ်ခုလုံး ထိရောက်စွာ လည်ပတ်မည် မဟုတ်ပါ။ AI တွင်လည်း ထိုနည်းတူပင် ဖြစ်ပါသည် —

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

ထို့ကြောင့် AI-centric data center တစ်ခုတွင် storage performance သည် အလွန်အရေးကြီးလှပါသည်။

## 6. AI Workload များ၏ Storage လိုအပ်ချက်များ

AI workloads များသည် ပုံမှန်အားဖြင့် အောက်ပါအတိုင်း လိုအပ်ပါသည် —

1. **High Throughput** — Storage system က data အမြောက်အမြားကို ထိရောက်စွာ ရွှေ့ပြောင်းပေးနိုင်ရမည်။
2. **Low Latency** — Data ကို အနည်းဆုံး နှောင့်နှေးမှုဖြင့် ဝင်ရောက်အသုံးပြုနိုင်ရမည်။
3. **Scalability** — Data ပမာဏနှင့် GPU node အရေအတွက် တိုးများလာသည်နှင့်အမျှ storage ကို ချဲ့ထွင် (scale) နိုင်ရမည်။

ထို့ကြောင့် — `AI Storage = High Throughput + Low Latency + Scalability`

## 7. Storage အမျိုးအစား ၁ — NVMe SSD

အသုံးများသော ရွေးချယ်စရာတစ်ခုမှာ NVMe SSD local storage ဖြစ်ပါသည်။ NVMe ဆိုသည်မှာ — **Non-Volatile Memory Express** ကို ရည်ညွှန်းပါသည်။

AI server တစ်ခုတွင် အောက်ပါအရာများ ပါရှိနိုင်ပါသည် —

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

NVMe SSD များသည် GPU server အတွင်းတွင် တည်ရှိပါသည်။

## 8. NVMe ကို ဘာကြောင့် သုံးသနည်း

NVMe က local I/O ကို အလွန်အမြန် ပေးနိုင်ပါသည်။ အောက်ပါအတွက် အသုံးဝင်နိုင်ပါသည် — Model training၊ Inference၊ ယာယီ (temporary) သို့မဟုတ် အသုံးပြုနေဆဲ data၊ အမြန်နှုန်းမြင့် local data access။

စားသောက်ဆိုင် ဥပမာ —

```
Chef / GPU
     │
     ▼
Nearby Pantry
     │
     ▼
NVMe SSD
```

ပစ္စည်းများသည် စားဖိုမှူးနှင့် အလွန်နီးကပ်စွာ တည်ရှိသောကြောင့် အမြန်ဆုံး ရယူအသုံးပြုနိုင်ပါသည်။

## 9. NVMe ၏ ကန့်သတ်ချက်

NVMe SSD များသည် အလွန်အမြန်ဖြစ်သော်လည်း ၎င်းတို့၏ capacity ကို physical server က ကန့်သတ်ထားပါသည်။ Server တစ်လုံးတည်းတွင် SSD များကို အကန့်အသတ်မရှိ ဆက်လက်ထည့်သွင်း၍ မရပါ။

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

ထို့ကြောင့် — `NVMe = Very Fast + Local + Limited Capacity`

## 10. Storage အမျိုးအစား ၂ — Parallel File System

Capacity ပိုမိုများပြားစွာ လိုအပ်လာပြီး GPU server အများအပြားက data တူညီသည်ကို ဝင်ရောက်ကြည့်ရှုရန် လိုအပ်လာသောအခါ ဘာဖြစ်မည်နည်း။ အသုံးများသော ဖြေရှင်းနည်းမှာ **Parallel File System** ဖြစ်ပါသည်။ ၎င်းသည် clustered storage ၏ ပုံစံတစ်မျိုး ဖြစ်ပါသည်။

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

GPU node အများအပြားသည် နောက်ကွယ်တွင်ရှိသော storage node အများအပြားကို တစ်ပြိုင်နက် ဝင်ရောက်အသုံးပြုနိုင်ပါသည်။

## 11. Parallel File System ကို ဘာကြောင့် သုံးသနည်း

AI cluster တစ်ခုတွင် GPU node အများအပြား ပါရှိနိုင်ပါသည်။ ဥပမာ —

```
GPU Node 1 ───┐
GPU Node 2 ───┤
GPU Node 3 ───┼──► Shared High-Speed Storage
GPU Node 4 ───┤
GPU Node 5 ───┘
```

GPU node အများအပြားသည် shared datasets များကို တစ်ပြိုင်နက် ဝင်ရောက်အသုံးပြုရန် လိုအပ်နိုင်ပါသည်။ ထို့ကြောင့် Parallel File System သည် GPU node အများအပြား တစ်ခုလုံးအတွက် shared high-speed access ကို ပေးနိုင်ပါသည်။ ဤအချက်သည် အထူးသဖြင့် large-scale AI နှင့် HPC-style workloads များအတွက် အလွန်အသုံးဝင်ပါသည်။

## 12. Storage အမျိုးအစား ၃ — Network File System (NFS)

နောက်ထပ် storage ရွေးချယ်စရာတစ်ခုမှာ — **Network File System (NFS)** ဖြစ်ပါသည်။ NFS က network ပေါ်မှတစ်ဆင့် system အမျိုးမျိုးကြား files များကို မျှဝေအသုံးပြုနိုင်စေပါသည်။ မူရင်းရှင်းလင်းချက်အရ configuration files နှင့် scripts ကဲ့သို့သော distributed small datasets များအတွက် အသုံးပြုနိုင်ပါသည်။

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

## 13. NFS ကို ဘာအတွက် သုံးသနည်း

NFS က အောက်ပါအတွက် အသုံးဝင်နိုင်ပါသည် — Configuration files၊ Scripts၊ Shared small datasets၊ ဘုံ (common) files များ။

အရေးကြီးသော အချက်မှာ — ဤရှင်းလင်းချက်အရ NFS ကို အဓိကအားဖြင့် configuration/script မျှဝေခြင်းနှင့် အသေးစား distributed data အတွက် ဆွေးနွေးထားခြင်းဖြစ်ပြီး AI training datasets ကြီးများအတွက် စွမ်းဆောင်ရည်အမြင့်ဆုံး storage layer အဖြစ် ရည်ညွှန်းထားခြင်း မဟုတ်ပါ။

## 14. Storage အမျိုးအစား ၄ — Object Storage

နောက်ထပ် အရေးကြီးသော storage အမျိုးအစားမှာ **Object Storage** ဖြစ်ပါသည်။ မူရင်းစာတွင် Amazon S3 ကို ဥပမာအဖြစ် ပေးထားပါသည်။ Object Storage သည် data အလွန်အမြောက်အမြားကို သိမ်းဆည်းရာတွင် အသုံးဝင်ပြီး အထူးသဖြင့် ရေရှည်သိမ်းဆည်းမှုအတွက် သင့်လျော်ပါသည်။

## 15. Object Storage အသုံးပြုမှု နည်းလမ်းများ

Object Storage တွင် အောက်ပါအရာများကို ထားရှိနိုင်ပါသည် — ပမာဏကြီးမားသော raw datasets များ၊ Model checkpoints များ၊ Logs များ၊ Historical data များ၊ Archived models များ။

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

ထို့ကြောင့် — `Object Storage = Massive + Long-Term + Persistent Storage`

## 16. အဓိက Storage အမျိုးအစား လေးမျိုး

| Storage | အဓိကရည်ရွယ်ချက် | အဓိကလက္ခဏာ |
|---|---|---|
| NVMe SSD | Local active data / fast I/O | အလွန်အမြန်၊ local |
| Parallel File System | GPU node အများအပြားကြား shared data | အမြန်နှုန်းမြင့် shared access |
| NFS | Configuration နှင့် script မျှဝေခြင်း | Distributed file sharing |
| Object Storage | Long-term / ပမာဏကြီး data | အကြီးစား persistent storage |

## 17. မှတ်ရလွယ်သော နည်းလမ်း

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

သို့မဟုတ် ရိုးရိုးရှင်းရှင်းပြောရလျှင် —

- NVMe = Local Fast
- Parallel FS = Shared Fast
- NFS = Shared Files
- Object = Long-Term Massive

## 18. AI Data Center တွင် Storage အမျိုးအစား တစ်မျိုးတည်းကိုသာ မသုံးပါ

အရေးကြီးသော အယူအဆတစ်ခုမှာ — data center တစ်ခုသည် storage အမျိုးအစားတစ်မျိုးတည်းကိုသာ အသုံးပြုရန် လိုအပ်သည် မဟုတ်ပါ။ ထိုအစား **tiered storage** သို့မဟုတ် **hybrid storage** ချဉ်းကပ်မှုကို အသုံးပြုနိုင်ပါသည်။

Data အမျိုးအစားအလိုက် access pattern များ မတူညီပါ — အချို့ data များမှာ လက်ရှိ အသုံးပြုနေဆဲဖြစ်သည်။ အချို့ data များမှာ ရံဖန်သာ အသုံးပြုပါသည်။ အချို့ data များမှာမူ archive သို့မဟုတ် သမိုင်းမှတ်တမ်း ရည်ရွယ်ချက်အတွက်သာ သိမ်းဆည်းထားခြင်း ဖြစ်ပါသည်။

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

အခြေခံအကြံမှာ data ကို ၎င်း၏ access လိုအပ်ချက်နှင့် ကိုက်ညီသော storage tier ပေါ်တွင် ထားရှိခြင်း ဖြစ်ပါသည်။

## 20. Hot, Warm နှင့် Cold Data

**Hot Data** — လက်ရှိ အသုံးပြုနေဆဲ data ဖြစ်ပါသည်။ ဥပမာ — လက်ရှိ training datasets များ၊ active model data များ၊ အကြိမ်ကြိမ် ဝင်ရောက်အသုံးပြုသော data များ။ ဤ data ကို အောက်ပါတွင် ထားရှိနိုင်ပါသည် —

```
Hot Data
   ↓
NVMe / Parallel File System
```

**Warm Data** — အသုံးဝင်သေးသော်လည်း အမြဲတမ်း ဝင်ရောက်အသုံးမပြုသော data ဖြစ်ပါသည်။ ဥပမာများမှာ — အဟောင်း model versions များ၊ မကြာသေးမီက အသုံးပြုခဲ့သော datasets များ၊ intermediate data များ ဖြစ်ပါသည်။

**Cold Data** — ရှားပါးစွာ ဝင်ရောက်အသုံးပြုသော data ဖြစ်ပါသည်။ ဥပမာများ — historical datasets များ၊ အဟောင်း model checkpoints များ၊ archived logs များ၊ အဟောင်း models များ။ ဤ data အမျိုးအစားကို Object Storage သို့ ရွှေ့ပြောင်းနိုင်ပါသည်။

## 21. Data Lifecycle Policies

Data သည် storage tier တစ်ခုတည်းတွင် ထာဝရ ကျန်ရှိနေရန် မလိုအပ်ပါ။ Lifecycle policy က data ၏ သက်တမ်း (age) သို့မဟုတ် access pattern ကို အခြေခံ၍ data ကို ရွှေ့ပြောင်းပေးနိုင်ပါသည်။

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

ဤနည်းလမ်းက အောက်ပါအတွက် ကူညီပေးနိုင်ပါသည် — မလိုအပ်ဘဲ ဈေးကြီးသော high-performance storage ကို အသုံးပြုမိခြင်းကို ရှောင်ရှားခြင်း၊ storage capacity အသုံးပြုမှုကို တိုးတက်ကောင်းမွန်စေခြင်း၊ ရေရှည် data များကို ပိုမိုသင့်လျော်သော storage တွင် ထားရှိခြင်း။

## 22. ပြည့်စုံသော Storage Architecture

အဆင့်မြင့် AI-centric storage architecture တစ်ခုသည် အောက်ပါအတိုင်း ဖြစ်နိုင်ပါသည် —

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

## 23. စားသောက်ဆိုင် ဥပမာ အပြည့်အစုံ

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

Pantry ကိုယ်တိုင်တွင်လည်း အဆင့်အမျိုးမျိုး ရှိနိုင်ပါသည် —

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

AI data center တစ်ခုက ဘာကြောင့် storage နည်းပညာ အမျိုးမျိုးကို တစ်ပြိုင်နက် အသုံးပြုနိုင်သနည်း ဆိုသည်ကို နားလည်ရန် ဤအရာသည် အသုံးဝင်သော mental model တစ်ခု ဖြစ်ပါသည်။

## 24. NVIDIA နှင့် Storage

မူရင်းစာမှ အရေးကြီးသော အချက်တစ်ခုမှာ — NVIDIA သည် storage hardware သို့မဟုတ် storage software ကို တိုက်ရိုက် အာရုံမစိုက်ပါ။ သို့သော် AI data center အတွက် storage သည် မဖြစ်မနေ လိုအပ်ဆဲ ဖြစ်ပါသည်။ ထို့ကြောင့် NVIDIA သည် storage ecosystem partners များနှင့် ပူးပေါင်းဆောင်ရွက်ပြီး ၎င်းတို့၏ storage solutions များကို NVIDIA ၏ hardware နှင့် software environment နှင့် ပေါင်းစပ်အလုပ်လုပ်နိုင်အောင် ဆောင်ရွက်ပါသည်။

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

ရည်မှန်းချက်မှာ AI workloads များအတွက် စွမ်းဆောင်ရည်မြင့် end-to-end solution — Compute + Network + Storage — ကို ဖန်တီးခြင်း ဖြစ်ပါသည်။

## 25. နောက်ဆုံး စာမေးပွဲ အကျဉ်းချုပ်

**AI Storage လိုအပ်ချက်များ** — High Throughput + Low Latency + Scalability

**Storage အမျိုးအစား လေးမျိုး**
- NVMe → Fast Local Storage
- Parallel File System → GPU Nodes များအတွက် High-Speed Shared Storage
- NFS → Configuration နှင့် Script မျှဝေခြင်း
- Object Storage → Massive Long-Term Storage

**Storage မဟာဗျူဟာ**
- Hot → Fast Storage
- Warm → Shared / Intermediate Storage
- Cold → Object / Archive Storage

**Architecture** — AI data centers များသည် storage အမျိုးအစား တစ်မျိုးတည်းကိုသာ အားကိုးနေလေ့ မရှိပါ။ ထိုအစား — Tiered / Hybrid Storage + Lifecycle Policies များကို အသုံးပြု၍ data ၏ access pattern အလိုက် အသင့်လျော်ဆုံး storage tier တွင် data ကို ထားရှိနိုင်ပါသည်။

**တစ်ကြောင်းတည်း မှတ်ရနည်း** — GPU သည် စားဖိုမှူး၊ Network သည် စားပွဲထိုး၊ Storage သည် Pantry ဖြစ်သည်။ ထို့ပြင် ဤအရာကို မှတ်ထားပါ — NVMe = Fast & Local · Parallel File System = Fast & Shared · NFS = Config & Scripts · Object Storage = Massive & Long-Term

## 26. စစ်ဆေးရန် မေးခွန်းများနှင့် အဖြေများ (Self-Check Questions and Answers)

**မေးခွန်း ၁။** AI workload များအတွက် storage လိုအပ်ချက် သုံးချက်က ဘာတွေလဲ။

**အဖြေ။** High Throughput၊ Low Latency နှင့် Scalability တို့ ဖြစ်ပါသည် (`AI Storage = High Throughput + Low Latency + Scalability`)။

**မေးခွန်း ၂။** အဓိက storage အမျိုးအစား လေးမျိုးနှင့် ၎င်းတို့၏ အဓိက အသုံးပြုမှုကို ပြောပြပါ။

**အဖြေ။** NVMe SSD = local active data နှင့် fast I/O၊ Parallel File System = GPU node အများအပြားကြား shared high-speed access၊ NFS = configuration နှင့် script မျှဝေခြင်း၊ Object Storage = massive long-term / persistent data တို့ ဖြစ်ပါသည်။

**မေးခွန်း ၃။** NVMe SSD ရဲ့ အဓိက ကန့်သတ်ချက်က ဘာလဲ။

**အဖြေ။** Capacity ကို physical server က ကန့်သတ်ထားပါသည်။ Server တစ်လုံးတည်းတွင် SSD များကို အကန့်အသတ်မရှိ ဆက်ထည့်၍ မရပါ (`NVMe = Very Fast + Local + Limited Capacity`)။

**မေးခွန်း ၄။** Hot, Warm နှင့် Cold data တို့ကို မည်သည့် storage tier များတွင် ထားရှိသနည်း။

**အဖြေ။** Hot data ကို NVMe / Parallel File System ကဲ့သို့ fast storage တွင်လည်းကောင်း၊ Warm data ကို shared / intermediate storage တွင်လည်းကောင်း၊ Cold data ကို Object Storage / archive တွင်လည်းကောင်း ထားရှိပါသည်။

**မေးခွန်း ၅။** AI data center တစ်ခုက storage အမျိုးအစား တစ်မျိုးတည်းကိုသာ ဘာကြောင့် အားကိုးရသနည်း။

**အဖြေ။** Data အမျိုးအစားအလိုက် access pattern မတူညီသောကြောင့် ဖြစ်ပါသည်။ Tiered / hybrid storage နှင့် lifecycle policies များဖြင့် data ကို ၎င်း၏ access pattern အလိုက် အသင့်လျော်ဆုံး storage tier တွင် ထားရှိနိုင်ပါသည်။

---

## English Version

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

## 26. Self-Check Questions and Answers

**Q1.** What are the three storage requirements for AI workloads?

**A1.** High Throughput, Low Latency, and Scalability (`AI Storage = High Throughput + Low Latency + Scalability`).

**Q2.** Name the four main storage types and their primary uses.

**A2.** NVMe SSD = local active data and fast I/O; Parallel File System = shared high-speed access across many GPU nodes; NFS = configuration and script sharing; Object Storage = massive long-term / persistent data.

**Q3.** What is the main limitation of NVMe SSD storage?

**A3.** Capacity is limited by the physical server — you cannot keep adding unlimited SSDs to one server (`NVMe = Very Fast + Local + Limited Capacity`).

**Q4.** Where are Hot, Warm, and Cold data typically placed?

**A4.** Hot data on fast storage such as NVMe / Parallel File System, Warm data on shared / intermediate storage, and Cold data on Object Storage / archive.

**Q5.** Why can't an AI data center rely on only one storage type?

**A5.** Different data has different access patterns. With tiered / hybrid storage and lifecycle policies, data can be placed on the storage tier that matches its access pattern.

---

## Technical review notes

- Structure normalized to the course convention: a single top-of-file header and blockquote (the duplicate blockquote that previously sat inside the Burmese section was removed), an explicit `## English Version` heading added above the existing English content, `## မြန်မာဘာသာ version` placed first, and matching `## 1.`–`## 26.` numbering in both language sections. All existing paragraphs, ASCII diagrams, lists, and tables are preserved unchanged in meaning.
- The five self-check questions per language were written strictly from this module's own content; no storage products, performance numbers, commands, or URLs were added beyond the source's own example (Amazon S3 as an Object Storage example).
- Claims needing verification: the source's framing caveats about NFS usage (discussed mainly for configuration/script sharing and smaller distributed data, not as the highest-performance layer for large AI training datasets) are preserved as given. The qualitative statements that NVMe is "very fast" with capacity limited by the physical server, and that Object Storage suits massive long-term data, are kept as general characterizations rather than specifications.
