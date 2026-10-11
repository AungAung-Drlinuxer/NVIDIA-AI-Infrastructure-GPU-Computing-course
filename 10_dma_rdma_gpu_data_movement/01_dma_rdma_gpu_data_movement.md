# Module 10 — DMA, RDMA, and GPU Data Movement

> Part II · Course Module 10 (DMA, RDMA and GPU Data Movement) — topic notes

_Part 1: Understanding Direct Memory Access_

## မြန်မာဘာသာ version — DMA, RDMA, and GPU Data Movement

## 1. AI Infrastructure မှာ DMA ကို ဘာကြောင့် လေ့လာရသလဲ

Modern AI servers တွေမှာ စွမ်းအားမြင့် GPUs တွေ၊ CPUs တွေ၊ system memory၊ storage devices တွေနဲ့ network adapters တွေ ပါဝင်ပါတယ်။ ဒီ components တွေက data တွေကို အဆက်မပြတ် ဖလှယ်နေကြပါတယ်။

ဥပမာ—AI model training လုပ်နေစဉ်မှာ system က အောက်ပါတွေ လိုအပ်နိုင်ပါတယ်—

- Storage ကနေ training datasets တွေ ဖတ်ခြင်း။
- Data တွေကို GPU memory ထဲ လွှဲပြောင်းခြင်း။
- GPU တွေကြား intermediate results တွေ ဖလှယ်ခြင်း။
- GPU servers ကြား data ပို့ခြင်း။
- Checkpoints နဲ့ training results တွေကို storage ဆီ ပြန်ရေးခြင်း။

GPU က တွက်ချက်မှု ဘယ်လောက်ပဲ မြန်မြန် လိုအပ်တဲ့ data က နှေးနှေးရောက်ရင် ထိရောက်စွာ အလုပ်လုပ်နိုင်မှာ မဟုတ်ပါဘူး။

ဒါကြောင့် data movement က AI infrastructure ရဲ့ အရေးကြီးတဲ့ အစိတ်အပိုင်း ဖြစ်ပါတယ်။ DMA နဲ့ RDMA က data-transfer efficiency ကို တိုးတက်စေပြီး မလိုအပ်တဲ့ CPU အလုပ်တွေကို လျှော့ချပေးပါတယ်။

NVIDIA ရဲ့ GPUDirect family က ဒီ concepts တွေအပေါ် အခြေခံပြီး GPU memory ပါဝင်တဲ့ data movement ကို ပိုကောင်းမွန်စေပါတယ်။

## 2. DMA ဆိုတာ ဘာလဲ

DMA ဆိုတာ Direct Memory Access ရဲ့ အတိုကောက် ဖြစ်ပါတယ်။

Device တစ်ခုက CPU ကိုယ်တိုင် byte တိုင်းကို copy လုပ်စရာ မလိုဘဲ system memory ဆီ ဒါမှမဟုတ် system memory ကနေ data လွှဲပြောင်းခွင့် ပေးတဲ့ hardware-supported mechanism တစ်ခု ဖြစ်ပါတယ်။

DMA အသုံးပြုနိုင်တဲ့ devices တွေရဲ့ ဥပမာတွေက—

- Network Interface Cards (NICs)။
- Storage controllers နဲ့ NVMe devices။
- Audio နဲ့ video devices။
- GPUs နဲ့ တခြား PCIe devices။

CPU က operations တွေကို ပုံမှန် configure ဒါမှမဟုတ် initiate လုပ်ပေးပြီး memory mappings တွေ တည်ဆောက်ပြီး completion notifications တွေကို ကိုင်တွယ်ဆဲ ဖြစ်ပါတယ်။ အဓိက အကျိုးကျေးဇူးကတော့ CPU က data copy တိုင်းကို မလုပ်ဆောင်ရတော့ပါဘူး။

## 3. DMA မပါရင် — CPU က data-copying middleman ဖြစ်လာတယ်

Network adapter တစ်ခုက system memory ထဲ ထည့်ရမယ့် dataset ကြီးတစ်ခု လက်ခံရတယ်လို့ မှန်းကြည့်ပါ။

Device DMA မပါတဲ့ ရိုးရှင်းတဲ့ model မှာ CPU က device interface ကနေ data ကို ထပ်ခါထပ်ခါ ဖတ်ပြီး memory ထဲ copy လုပ်ရပါမယ်။ ဒါက CPU cycles တွေ စားသုံးပြီး throughput ကို ကန့်သတ်နိုင်ပါတယ်။

```
Network Device
      │
      ▼
     CPU  ──────► System Memory
   copies data
```

ဒါက conceptual illustration ဖြစ်ပြီး modern device တိုင်းရဲ့ တကယ့် data path ကို ဖော်ပြတာ မဟုတ်ပါဘူး။ Modern computer systems တွေက high-throughput I/O အတွက် ပုံမှန် DMA-capable devices တွေ အသုံးပြုပါတယ်။

ဒီနည်းလမ်းက ဘာကြောင့် မထိရောက်တာလဲ—

- CPU က application logic အစား data ရွှေ့ရာမှာ အချိန်ကုန်ပါတယ်။
- Data movement က CPU resources အများကြီး စားသုံးနိုင်ပါတယ်။
- Device ရဲ့ throughput က software overhead ကြောင့် ကန့်သတ်ခံရနိုင်ပါတယ်။
- I/O load များတဲ့အခါ CPU နဲ့ application workloads တွေက resources အတွက် ယှဉ်ပြိုင်ကြပါတယ်။

## 4. DMA ပါရင် — Device က data ကို တိုက်ရိုက် လွှဲပြောင်းတယ်

DMA ရှိရင် DMA-capable device တစ်ခုက ၎င်းနဲ့ ခွင့်ပြုထားတဲ့ system-memory region ကြား data ကို လွှဲပြောင်းနိုင်ပါတယ်။

```
                  CPU
                   │
                   │ Sets up the operation
                   ▼
            DMA-capable device
                   │
                   │ DMA transfer
                   ▼
              System Memory
```

System architecture ပေါ် မူတည်ပြီး DMA controller ဒါမှမဟုတ် DMA engine က transfer ကို စီမံပါတယ်။ အချို့ devices တွေမှာ သူတို့ကိုယ်ပိုင် DMA engines တွေ ပါဝင်ပါတယ်။

Typical operation တစ်ခုက အောက်ပါအတိုင်း အလုပ်လုပ်ပါတယ်—

1. CPU ဒါမှမဟုတ် driver က data transfer တစ်ခုကို request လုပ်ပါတယ်။
2. Operating system နဲ့ driver က သင့်တော်တဲ့ memory region ကို ပြင်ဆင်ပြီး transfer ကို configure လုပ်ပါတယ်။
3. Device ရဲ့ DMA engine က data ကို လွှဲပြောင်းပါတယ်။
4. Device ဒါမှမဟုတ် controller က interrupt ဒါမှမဟုတ် completion queue မှတစ်ဆင့် ပြီးစီးကြောင်း အချက်ပေးပါတယ်။
5. CPU က result ပေါ် မှီခိုတဲ့ အလုပ်တွေကို ပြန်လည် ဆောင်ရွက်ပါတယ်။

CPU က control path ရဲ့ အစိတ်အပိုင်းအဖြစ် ကျန်ရှိနေဆဲ ဖြစ်ပေမယ့် CPU instructions တွေဖြင့် byte တိုင်းကို copy လုပ်စရာ မလိုတော့ပါဘူး။

**အရေးကြီးသည်** — DMA က transfer တိုင်း ပိုမြန်မယ်လို့ အာမခံတာ မဟုတ်ပါဘူး။ Performance က transfer size၊ hardware၊ memory layout၊ driver behavior နဲ့ တခြား system bottlenecks တွေပေါ် မူတည်ပါတယ်။

## 5. AI Training အတွက် DMA က ဘာကြောင့် အရေးကြီးသလဲ

Training data batches တွေကို ထပ်ခါထပ်ခါ load လုပ်တဲ့ GPU training workload တစ်ခုကို စဉ်းစားကြည့်ပါ။

ထိရောက်တဲ့ data path မရှိရင် data ရွှေ့ခြင်းက CPU resources တွေ စားသုံးပြီး GPU ရဲ့ အလုပ်ကို နှောင့်နှေးစေနိုင်ပါတယ်။ GPU က batch နောက်တစ်ခုကို စောင့်ရင်း idle ဖြစ်နေနိုင်ပါတယ်။

DMA-capable devices တွေနဲ့ဆိုရင် data-copying overhead နည်းနည်းဖြင့် transfers တွေ ဆက်လက် လုပ်ဆောင်နိုင်ပါတယ်။ အဲဒီအခါ CPU က data preparation၊ scheduling နဲ့ တခြားအလုပ်တွေကို ပိုအာရုံစိုက်နိုင်ပြီး hardware က support လုပ်တဲ့ transfers တွေကို ကိုင်တွယ်ပါတယ်။

မျှော်လင့်နိုင်တဲ့ အကျိုးကျေးဇူးတွေက—

- Data movement အတွက် CPU overhead လျှော့ချခြင်း။
- သင့်တော်တဲ့ workloads တွေမှာ I/O throughput တိုးတက်ခြင်း။
- Data transfer နဲ့ computation ကြား ပိုထိရောက်တဲ့ overlap။
- System resources တွေ ပိုကောင်းကောင်း အသုံးချနိုင်ခြင်း။

DMA တစ်ခုတည်းနဲ့ workload က 10 ဆ ဒါမှမဟုတ် 100 ဆ ပိုမြန်သွားမယ်လို့ အာမခံတာ မဟုတ်ပါဘူး။ တကယ့် တိုးတက်မှုက အရင်က bottleneck နဲ့ transfer path ပေါ် မူတည်ပါတယ်။

## 6. Local DMA vs. Remote DMA

ပုံမှန် DMA ဆိုတာ local system ရဲ့ hardware architecture အတွင်းမှာ ဝင်ရောက်နိုင်တဲ့ device တစ်ခုနဲ့ memory region တစ်ခု ပါဝင်တဲ့ transfers တွေကို ရည်ညွှန်းလေ့ရှိပါတယ်။

ဒါပေမယ့် GPU servers နှစ်ခုက data ဖလှယ်ဖို့ လိုအပ်ရင် ဘယ်လိုလုပ်မလဲ—

```
Host 1                                Host 2
┌────────────────────┐              ┌────────────────────┐
│ CPU                │              │ CPU                │
│ System Memory      │              │ System Memory      │
│ GPU                │              │ GPU                │
│ Network Adapter    │◄────────────►│ Network Adapter    │
└────────────────────┘   Network    └────────────────────┘
```

Local DMA က host တစ်ခုအတွင်းမှာ devices တွေ data လွှဲပြောင်းရာမှာ ကူညီပေမယ့် host တစ်ခုက နောက် host တစ်ခုရဲ့ memory ကို ဝင်ရောက်ခွင့် ပေးတဲ့ mechanism ကို ၎င်းတစ်ခုတည်းနဲ့ မပေးပါဘူး။

အဲဒီနေရာမှာ RDMA ရောက်လာပါတယ်။

## 7. RDMA ဆိုတာ ဘာလဲ

RDMA ဆိုတာ Remote Direct Memory Access ရဲ့ အတိုကောက် ဖြစ်ပါတယ်။

RDMA က support လုပ်တဲ့ network adapters တွေကို data path ထဲမှာ CPU ပါဝင်မှု လျှော့ချပြီး hosts အမျိုးမျိုးပေါ်က registered memory regions ကြား data လွှဲပြောင်းခွင့် ပေးပါတယ်။

ဥပမာ—Host 1 ပေါ်က RDMA-capable adapter တစ်ခုက support လုပ်တဲ့ network တစ်ခုမှတစ်ဆင့် Host 2 ပေါ်က registered memory region တစ်ခုနဲ့ data ဖလှယ်နိုင်ပါတယ်။

CPU နဲ့ software က communication ကို တည်ဆောက်ပေးပြီး memory ကို register ဒါမှမဟုတ် map လုပ်ပေးပြီး access ကို ခွင့်ပြုပြီး completion information ကို လုပ်ဆောင်ဆဲ ဖြစ်ပါတယ်။ RDMA က data copy တိုင်းကို CPU ကိုင်တွယ်ရန် လိုအပ်ချက်ကို လျှော့ချပေးတာ ဖြစ်ပါတယ်။

ရိုးရှင်းတဲ့ မြင်ကွင်း—

```
Host 1                                      Host 2
┌────────────────────┐                  ┌────────────────────┐
│ GPU / CPU          │                  │ GPU / CPU          │
│ Registered Memory  │                  │ Registered Memory  │
│ RDMA Adapter       │                  │ RDMA Adapter       │
└─────────┬──────────┘                  └─────────┬──────────┘
          │                                       │
          └──────── Supported RDMA Network ───────┘
                   InfiniBand or RoCE
```

RDMA ကို ပံ့ပိုးတဲ့ technologies တွေက—

- InfiniBand — HPC နဲ့ AI အတွက် တွင်ကျယ်စွာ အသုံးပြုတဲ့ high-performance interconnect။
- RoCE — RDMA over Converged Ethernet။
- iWARP — TCP/IP ပေါ်မှာ အလုပ်လုပ်တဲ့ RDMA technology တစ်ခု။

RDMA က InfiniBand အတွက်သာ သီးသန့် မဟုတ်ပါဘူး။

## 8. RDMA က GPU တိုင်းကို remote memory ကို ဖတ်ခွင့် ပေးသလား

မပေးပါဘူး။ ဒါက အရေးကြီးတဲ့ ကွာခြားချက် ဖြစ်ပါတယ်။

RDMA က တခြား server တစ်ခုရဲ့ memory ကို ကန့်သတ်ချက်မရှိ ဝင်ရောက်ခွင့် မပေးပါဘူး။ Application၊ operating system၊ network adapter၊ drivers နဲ့ RDMA stack တို့က operation ကို တည်ဆောက်ပြီး ခွင့်ပြုရပါမယ်။

Memory regions တွေကို ပုံမှန်အားဖြင့် register လုပ်ရမယ် ဒါမှမဟုတ် support လုပ်တဲ့ RDMA mechanism မှတစ်ဆင့် ရရှိနိုင်အောင် ပြုလုပ်ရပါမယ်။ Access permissions နဲ့ keys တွေက ဘယ် operations တွေ လုပ်ဆောင်နိုင်မလဲ ထိန်းချုပ်ရာမှာ ကူညီပါတယ်။

ထို့အပြင် ပုံမှန် RDMA နဲ့ GPUDirect RDMA ဆိုတာ တူညီတာ မဟုတ်ပါဘူး—

- ပုံမှန် RDMA က registered host-memory regions ကြား data လွှဲပြောင်းနိုင်ပါတယ်။
- GPUDirect RDMA က support လုပ်တဲ့ peer device (ဥပမာ network adapter) တစ်ခုက compatible platform နဲ့ software stack မှတစ်ဆင့် support လုပ်တဲ့ GPU memory ကို တိုက်ရိုက် ဝင်ရောက်ခွင့် ပေးပါတယ်။

ဒီ capabilities တွေအတွက် compatible hardware၊ drivers နဲ့ application support လိုအပ်ပါတယ်။

## 9. GPUDirect RDMA က ဘယ်လို ပါဝင်သလဲ

NVIDIA GPUDirect RDMA က support လုပ်တဲ့ third-party peer device တစ်ခု—ဥပမာ ConnectX network adapter ဒါမှမဟုတ် BlueField DPU—ကို support လုပ်တဲ့ PCIe peer-to-peer path တစ်ခုမှတစ်ဆင့် GPU memory နဲ့ တိုက်ရိုက် data ဖလှယ်ခွင့် ပေးပါတယ်။

ဒါက data ကို host-memory bounce buffer မှတစ်ဆင့် staging လုပ်ခြင်းကို ရှောင်ရှားနိုင်ပြီး မလိုအပ်တဲ့ copying နဲ့ CPU overhead တွေကို လျှော့ချပေးပါတယ်။ GPUDirect RDMA က support လုပ်တဲ့ InfiniBand နှင့် RoCE configurations တွေနဲ့ အလုပ်လုပ်နိုင်ပါတယ်။

Concept အရ—

GPUDirect RDMA မပါဘဲ—

```
GPU Memory
    │
    ▼
Host Memory (staging / bounce buffer)
    │
    ▼
Network Adapter
    │
    ▼
Network
```

GPUDirect RDMA ပါရင်—

```
GPU Memory
    │
    ▼
Supported Network Adapter
    │
    ▼
Network
```

ဒီ diagrams တွေက data path ကို ရိုးရှင်းစွာ ဖော်ပြတာ ဖြစ်ပါတယ်။ တကယ့် implementation က hardware topology၊ drivers၊ memory registration နဲ့ platform support ပေါ် မူတည်ပါတယ်။ Setup နဲ့ control operations တွေမှာ CPU က ပါဝင်နေဆဲ ဖြစ်နိုင်ပါတယ်။

## 10. GPUDirect Storage က ဘယ်လို ပါဝင်သလဲ

GPUDirect Storage (GDS) က support လုပ်တဲ့ local ဒါမှမဟုတ် remote storage နဲ့ GPU memory ကြား တိုက်ရိုက် data path တစ်ခု တည်ဆောက်ပေးဖို့ ဒီဇိုင်းလုပ်ထားပြီး host memory မှတစ်ဆင့် မလိုအပ်တဲ့ intermediate copies တွေကို လျှော့ချပေးပါတယ်။

ဥပမာ—AI application တစ်ခုက NVMe storage ဒါမှမဟုတ် support လုပ်တဲ့ remote storage system တစ်ခုကနေ training dataset ကြီးတစ်ခု ဖတ်နိုင်ပါတယ်။ GDS က support လုပ်ရင် ပုံမှန် host-memory bounce-buffer path မလိုဘဲ data ကို GPU memory ဆီ လွှဲပြောင်းရာမှာ ကူညီနိုင်ပါတယ်။

```
Storage
   │
   ▼
Supported Storage / Network I/O Path
   │
   ▼
GPU Memory
```

GDS နဲ့ GPUDirect RDMA က ဆက်စပ်ပေမယ့် ရည်ရွယ်ချက် မတူပါဘူး—

| Technology | အဓိက ရည်ရွယ်ချက် |
|---|---|
| DMA | CPU က byte တိုင်း copy မလုပ်ဘဲ device က accessible memory ဆီ/ကနေ data လွှဲပြောင်းခြင်း |
| RDMA | Hosts ကြား registered memory regions ကြား data လွှဲပြောင်းခြင်း |
| GPUDirect RDMA | Support လုပ်တဲ့ peer devices တွေက GPU memory နဲ့ တိုက်ရိုက် data ဖလှယ်ခွင့်ပေးခြင်း |
| GPUDirect Storage | Storage နဲ့ GPU memory ကြား support လုပ်တဲ့ တိုက်ရိုက် data path ပေးခြင်း |

## 11. အဓိက အကျဉ်းချုပ်ချက်များ

- DMA က device I/O အတွင်း CPU က byte တိုင်း copy လုပ်ရန် လိုအပ်ချက်ကို လျှော့ချပေးပါတယ်။
- RDMA က support လုပ်တဲ့ network နဲ့ registered memory တွေကို အသုံးပြုပြီး hosts ကြား ထိရောက်တဲ့ memory transfer ကို တိုးချဲ့ပေးပါတယ်။
- InfiniBand နဲ့ RoCE နှစ်ခုလုံးက RDMA ကို ပံ့ပိုးပါတယ်။
- GPUDirect RDMA က support လုပ်ရင် host memory မှတစ်ဆင့် မလိုအပ်တဲ့ copies တွေကို ရှောင်ရှားပေးနိုင်ပါတယ်။
- GPUDirect Storage က storage နဲ့ GPU memory ကြား data movement ကို ရည်ရွယ်ပါတယ်။
- CPU က setup၊ authorization ဒါမှမဟုတ် completion handling ကနေ ဖယ်ရှားခံရတာ မဟုတ်ပါဘူး။
- Performance တိုးတက်မှုက hardware၊ software၊ topology နဲ့ workload ပေါ် မူတည်ပါတယ်။

## 12. Review Questions (မေးခွန်းနှင့် အဖြေများ)

**Q1. DMA ဆိုတာ ဘာကို ဆိုလိုသလဲ**

Direct Memory Access။

**Q2. DMA ရဲ့ အဓိက အားသာချက်က ဘာလဲ**

Support လုပ်တဲ့ devices တွေကို CPU က byte တိုင်း copy လုပ်စရာ မလိုဘဲ memory ဆီ/ကနေ data လွှဲပြောင်းခွင့် ပေးပါတယ်။

**Q3. RDMA က ဘာကို ထပ်မံ ပေးသလဲ**

Hosts အမျိုးမျိုးပေါ်က registered memory regions ကြား support လုပ်တဲ့ data transfers တွေကို ဖြစ်စေပါတယ်။

**Q4. RDMA က InfiniBand အတွက်သာ သီးသန့်လား**

မဟုတ်ပါဘူး။ RoCE မှတစ်ဆင့် Ethernet ပေါ်မှာလည်း ကောင်း iWARP မှတစ်ဆင့် TCP/IP ပေါ်မှာပါ ပံ့ပိုးပါတယ်။

**Q5. GPUDirect RDMA က ဘာလုပ်သလဲ**

Support လုပ်တဲ့ peer devices တွေ (ဥပမာ network adapters) ကို မလိုအပ်တဲ့ host-memory copies တွေကို ရှောင်ရှားပြီး GPU memory နဲ့ တိုက်ရိုက် data ဖလှယ်ခွင့် ပေးပါတယ်။

**Q6. GPUDirect Storage က ဘာအတွက် ဒီဇိုင်းလုပ်ထားသလဲ**

Support လုပ်တဲ့ storage နဲ့ GPU memory ကြား တိုက်ရိုက် data path အတွက် ဖြစ်ပါတယ်။

**Q7. DMA ဒါမှမဟုတ် RDMA က CPU ပါဝင်မှု အားလုံးကို ဖယ်ရှားပေးသလား**

မပေးပါဘူး။ CPU က transfers တွေကို configure လုပ်ခြင်း၊ memory access တည်ဆောက်ခြင်း၊ operations တွေ ခွင့်ပြုခြင်းနဲ့ completion notifications တွေ လုပ်ဆောင်ခြင်းတွေ ဆက်လက် လုပ်ဆောင်နိုင်ပါတယ်။

## English Version — DMA, RDMA, and GPU Data Movement

_Part 1: Understanding Direct Memory Access_

## 1. Why Are We Studying DMA in AI Infrastructure?

Modern AI servers contain powerful GPUs, CPUs, system memory, storage devices, and network adapters. These components constantly exchange data.

During AI model training, for example, a system may need to:

- Read training datasets from storage.
- Transfer data into GPU memory.
- Exchange intermediate results between GPUs.
- Send data between GPU servers.
- Write checkpoints and training results back to storage.

Even if a GPU can perform calculations extremely quickly, it cannot work efficiently when the data it needs arrives too slowly.

This is why data movement is a critical part of AI infrastructure. DMA and RDMA help improve data-transfer efficiency and reduce unnecessary CPU work.

NVIDIA's GPUDirect family builds on these concepts to improve data movement involving GPU memory.

## 2. What Is DMA?

DMA stands for Direct Memory Access.

It is a hardware-supported mechanism that allows a device to transfer data to or from system memory without requiring the CPU to copy every byte itself.

Examples of devices that can use DMA include:

- Network Interface Cards (NICs).
- Storage controllers and NVMe devices.
- Audio and video devices.
- GPUs and other PCIe devices.

The CPU still commonly configures or initiates operations, sets up memory mappings, and handles completion notifications. The key benefit is that the CPU does not need to perform every individual data copy.

## 3. Without DMA: The CPU Becomes a Data-Copying Middleman

Imagine a network adapter receives a large dataset that must be placed in system memory.

In a simplified model without device DMA, the CPU must repeatedly read data from the device interface and copy it into memory. This consumes CPU cycles and can limit throughput.

```
Network Device
      │
      ▼
     CPU  ──────► System Memory
   copies data
```

This is a conceptual illustration, not a description of every modern device's actual data path. Modern computer systems normally use DMA-capable devices for high-throughput I/O.

Why this approach is inefficient:

- The CPU spends time moving data instead of executing application logic.
- Data movement can consume significant CPU resources.
- The device's throughput may be constrained by software overhead.
- Under heavy I/O load, CPU and application workloads compete for resources.

## 4. With DMA: The Device Transfers Data Directly

With DMA, a DMA-capable device can transfer data between itself and an approved system-memory region.

```
                  CPU
                   │
                   │ Sets up the operation
                   ▼
            DMA-capable device
                   │
                   │ DMA transfer
                   ▼
              System Memory
```

A DMA controller or DMA engine manages the transfer, depending on the system architecture. Some devices contain their own DMA engines.

A typical operation works like this:

1. The CPU or driver requests a data transfer.
2. The operating system and driver prepare an appropriate memory region and configure the transfer.
3. The device's DMA engine transfers the data.
4. The device or controller signals completion, often through an interrupt or completion queue.
5. The CPU resumes any work that depends on the result.

The CPU remains part of the control path, but it does not have to copy each byte through CPU instructions.

Important: DMA does not guarantee that every transfer is faster. Performance depends on transfer size, hardware, memory layout, driver behavior, and other system bottlenecks.

## 5. Why DMA Matters for AI Training

Consider a GPU training workload that repeatedly loads batches of training data.

Without an efficient data path, moving data can consume CPU resources and delay the GPU's work. The GPU may sit idle while waiting for the next batch.

With DMA-capable devices, transfers can proceed with less CPU copying overhead. This allows the CPU to focus more on data preparation, scheduling, and other work while the hardware handles supported transfers.

The expected benefits include:

- Reduced CPU overhead for data movement.
- Improved I/O throughput in suitable workloads.
- More efficient overlap between data transfer and computation.
- Better use of system resources.

DMA alone does not guarantee that a workload becomes 10 or 100 times faster. The actual improvement depends on the previous bottleneck and the transfer path.

## 6. Local DMA vs. Remote DMA

Ordinary DMA usually refers to transfers involving a device and a memory region accessible within the local system's hardware architecture.

But what if two GPU servers need to exchange data?

```
Host 1                                Host 2
┌────────────────────┐              ┌────────────────────┐
│ CPU                │              │ CPU                │
│ System Memory      │              │ System Memory      │
│ GPU                │              │ GPU                │
│ Network Adapter    │◄────────────►│ Network Adapter    │
└────────────────────┘   Network    └────────────────────┘
```

Local DMA helps devices transfer data within a host. It does not, by itself, provide a mechanism for one host to access another host's memory.

That is where RDMA comes in.

## 7. What Is RDMA?

RDMA stands for Remote Direct Memory Access.

RDMA allows supported network adapters to transfer data between registered memory regions on different hosts while reducing CPU involvement in the data path.

For example, an RDMA-capable adapter on Host 1 can exchange data with a registered memory region on Host 2 over a supported network.

The CPU and software still establish the communication, register or map memory, authorize access, and process completion information. RDMA reduces the need for the CPU to handle each data copy.

A simplified view:

```
Host 1                                      Host 2
┌────────────────────┐                  ┌────────────────────┐
│ GPU / CPU          │                  │ GPU / CPU          │
│ Registered Memory  │                  │ Registered Memory  │
│ RDMA Adapter       │                  │ RDMA Adapter       │
└─────────┬──────────┘                  └─────────┬──────────┘
          │                                       │
          └──────── Supported RDMA Network ───────┘
                   InfiniBand or RoCE
```

RDMA is supported over technologies including:

- InfiniBand — a high-performance interconnect widely used for HPC and AI.
- RoCE — RDMA over Converged Ethernet.
- iWARP — an RDMA technology that operates over TCP/IP.

RDMA is not exclusive to InfiniBand.

## 8. Does RDMA Let Any GPU Read Any Remote Memory?

No. This is an important distinction.

RDMA does not provide unrestricted access to another server's memory. The application, operating system, network adapter, drivers, and RDMA stack must establish and authorize the operation.

Memory regions generally need to be registered or otherwise made available through the supported RDMA mechanism. Access permissions and keys help control which operations can be performed.

Also, ordinary RDMA and GPUDirect RDMA are not identical:

- Ordinary RDMA can transfer data between registered host-memory regions.
- GPUDirect RDMA enables a supported peer device, such as a network adapter, to access supported GPU memory directly through a compatible platform and software stack.

These capabilities require compatible hardware, drivers, and application support.

## 9. How GPUDirect RDMA Fits In

NVIDIA GPUDirect RDMA enables a supported third-party peer device—such as a ConnectX network adapter or BlueField DPU—to exchange data directly with GPU memory over a supported PCIe peer-to-peer path.

This can avoid staging the data through a host-memory bounce buffer, reducing unnecessary copying and CPU overhead. GPUDirect RDMA can work with supported InfiniBand and RoCE configurations.

Conceptually:

Without GPUDirect RDMA:

```
GPU Memory
    │
    ▼
Host Memory (staging / bounce buffer)
    │
    ▼
Network Adapter
    │
    ▼
Network
```

With GPUDirect RDMA:

```
GPU Memory
    │
    ▼
Supported Network Adapter
    │
    ▼
Network
```

These diagrams simplify the data path. The actual implementation depends on hardware topology, drivers, memory registration, and platform support. Setup and control operations may still involve the CPU.

## 10. How GPUDirect Storage Fits In

GPUDirect Storage (GDS) is designed to establish a direct data path between supported local or remote storage and GPU memory, reducing unnecessary intermediate copies through host memory.

For example, an AI application may read a large training dataset from NVMe storage or a supported remote storage system. GDS can help transfer the data to GPU memory without the usual host-memory bounce-buffer path where supported.

```
Storage
   │
   ▼
Supported Storage / Network I/O Path
   │
   ▼
GPU Memory
```

GDS and GPUDirect RDMA are related but serve different purposes:

| Technology | Main purpose |
|---|---|
| DMA | Device transfers data to or from accessible memory without CPU copying every byte |
| RDMA | Transfers data between registered memory regions across hosts |
| GPUDirect RDMA | Enables supported peer devices to exchange data directly with GPU memory |
| GPUDirect Storage | Enables a supported direct data path between storage and GPU memory |

## 11. Key Takeaways

- DMA reduces the need for the CPU to copy every byte during device I/O.
- RDMA extends efficient memory transfer across hosts using a supported network and registered memory.
- InfiniBand and RoCE both support RDMA.
- GPUDirect RDMA can avoid unnecessary copies through host memory when supported.
- GPUDirect Storage targets data movement between storage and GPU memory.
- The CPU is not necessarily removed from setup, authorization, or completion handling.
- Performance gains depend on the hardware, software, topology, and workload.

## 12. Review Questions (Self-Check Questions and Answers)

**Q1. What does DMA stand for?**

Direct Memory Access.

**Q2. What is the main advantage of DMA?**

It allows supported devices to transfer data to or from memory without requiring the CPU to copy every byte.

**Q3. What does RDMA add to the picture?**

It enables supported data transfers between registered memory regions on different hosts.

**Q4. Is RDMA exclusive to InfiniBand?**

No. It is also supported over Ethernet through RoCE and over TCP/IP through iWARP.

**Q5. What does GPUDirect RDMA do?**

It enables supported peer devices, such as network adapters, to exchange data directly with GPU memory, avoiding unnecessary host-memory copies.

**Q6. What is GPUDirect Storage designed for?**

A direct data path between supported storage and GPU memory.

**Q7. Does DMA or RDMA eliminate all CPU involvement?**

No. The CPU may still configure transfers, set up memory access, authorize operations, and process completion notifications.

## Technical review notes

- `<Cite refs={...}/>` markers appearing in the source transcript were reply-UI artifacts and were dropped; no content was attached to them.
- All diagrams and the DMA/RDMA/GPUDirect comparison table are preserved from the source transcript; no benchmark numbers are claimed anywhere in this module.
- The source transcript for Module 10 is complete (sections 1–12 ending at Review Question Q7) and required no cross-message merging.
