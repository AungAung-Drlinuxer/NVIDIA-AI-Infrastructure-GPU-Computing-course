# Module 16 — GPU Virtualization Fundamentals

> Part III · GPU Drivers, Virtualization and CUDA — Course Module 16 (GPU Virtualization Fundamentals) — topic notes

## မြန်မာဘာသာ version — GPU Virtualization Fundamentals

## 1. ပြဿနာ — GPU တစ်လုံးကို ဈေးကြီးကြီး ဝယ်ထားပြီး အပြည့်အဝ မသုံးနိုင်ခြင်း

Data center GPU တစ်လုံးဟာ AI infrastructure ထဲမှာ ဈေးအကြီးဆုံး resource တွေထဲက တစ်ခုဖြစ်ပါတယ်။ ဒါပေမဲ့ workload တစ်ခုတည်းကို GPU တစ်လုံးလုံး ပေးလိုက်ရင် အများအားဖြင့် GPU utilization နည်းနေတတ်ပြီး resource က waste ဖြစ်ပါတယ်။ တစ်ဖက်မှာလည်း—

- VM များစွာ
- User များစွာ
- Container workload များစွာ

တွေက GPU တစ်လုံးတည်းကို မျှဝေ အသုံးပြုချင်ကြပါတယ်။ တစ်ချို့ workload တွေက performance အပြည့်အဝ လိုချင်ပြီး၊ တစ်ချို့ကတော့ isolation သာ လုံလောက်ရင် ရပါပြီ။ ဒီလို လိုအပ်ချက် မတူညီတဲ့ tenant များစွာကြား GPU ကို ဘယ်လို ခွဲဝေမလဲ — ဒါက ဒီ module ရဲ့ ပြဿနာဖြစ်ပါတယ်။

GPU sharing နည်းလမ်းတွေ အမျိုးမျိုးရှိပြီး၊ တစ်ခုချင်းစီမှာ isolation, performance predictability, licensing နဲ့ management ပိုင်း ကွာခြားချက်တွေ ရှိပါတယ်။ ဒီ module က အဲဒီနည်းလမ်းတွေရဲ့ fundamentals တွေကို ရှင်းပြပါမယ်။ vGPU နဲ့ MIG ကို အသေးစိတ် နှိုင်းယှဉ်ချင်ရင်တော့ **Module 17 — NVIDIA vGPU vs. MIG** ကို ဆက်လေ့လာပါ။

## 2. အဓိက သဘောတရားများ (Core Concepts)

### 2.1 Temporal Partitioning နှင့် Spatial Partitioning

GPU sharing နည်းလမ်းတွေ အားလုံးကို သဘောတရား နှစ်ခုအောက်မှာ ခွဲခြားနားလည်နိုင်ပါတယ်။

- **Temporal partitioning (time slicing)** — GPU resource တွေကို tenant တွေ အားလုံး မျှဝေသုံးပြီး၊ အချိန်အလိုက် အလှည့်ကျ အသုံးပြုကြတာ ဖြစ်ပါတယ်။ Memory ကို hardware အဆင့်မှာ သီးခြား မခွဲဝေပါဘူး။
- **Spatial partitioning** — GPU chip ကို hardware အဆင့်ကနေ partition တွြ့အဖြစ် ခွဲပြီး၊ partition တစ်ခုချင်းစီကို သတ်မှတ်ထားတဲ့ SMs, memory နဲ့ memory bandwidth ပေးအပ်ပါတယ်။

```
Temporal partitioning (time slicing)          Spatial partitioning (MIG)
+------------------------------------+        +------------------+-----------------+
|         One GPU pipeline           |        | GPU Instance 1   | GPU Instance 2  |
|  VM1 -> VM2 -> VM3 -> VM1 -> ...   |        | dedicated SMs    | dedicated SMs   |
|  (အချိန်အလိုက် မျှဝေသုံး)          |        | dedicated memory | dedicated memory|
+------------------------------------+        +------------------+-----------------+
```

### 2.2 အသုံးအနှုန်းများ (Terminology)

- **GPU passthrough (PCI passthrough)** — Physical GPU တစ်လုံးလုံးကို VM တစ်ခုတည်းထံ သီးသန့် ပေးအပ်ခြင်း။
- **Time-slicing** — GPU pipeline ကို အချိန်အလိုက် အလှည့်ကျ ခွဲဝေပေးခြင်း (temporal partitioning)။
- **vGPU (virtual GPU)** — NVIDIA Virtual GPU Software နဲ့ driver များဖြင့် VM များထံ virtual GPU device များ ပေးအပ်ခြင်း။
- **MIG (Multi-Instance GPU)** — Physical GPU ကို hardware-supported GPU instances များအဖြစ် ခွဲခြားခြင်း (spatial partitioning)။
- **Oversubscription** — Resource တစ်ခုကို သူ့ရဲ့ physical capacity ထက် ပိုမို ခွဲဝေ ကတိပေးခြင်း (ဥပမာ — GPU တစ်လုံးကို replica ၄ ခုအဖြစ် ကြေညာခြင်း)။
- **QoS (Quality of Service)** — Workload တစ်ခုချင်းစီ ရရှိမယ့် performance ကို ခန့်မှန်းနိုင်မှု၊ အာမခံနိုင်မှု။
- **Multi-tenancy** — Tenant များစွာ (user/VM/team) က တူညီတဲ့ infrastructure ကို မျှဝေ အသုံးပြုပြီး၊ တစ်ခုနဲ့တစ်ခု သက်ရောက်မှု မရှိစေရန် isolation ပေးခြင်း။

## 3. GPU Sharing Approaches များ

အောက်ပါ approach ငါးခုက လက်တွေ့မှာ တွေ့ရများတဲ့ နည်းလမ်းတွေ ဖြစ်ပါတယ်။

### 3.1 GPU Passthrough (PCI Passthrough)

Physical GPU တစ်လုံးလုံးကို VM တစ်ခုတည်းထံ သီးသန့် ပေးအပ်တာ ဖြစ်ပါတယ်။ အရိုးရှင်းဆုံး နည်းလမ်းဖြစ်ပြီး၊ အဲဒီ VM အတွက် performance နဲ့ isolation အကောင်းဆုံး ဖြစ်ပါတယ်။ GPU တစ်လုံးကို VM တစ်ခုတည်းကသာ ရရှိပါမယ် (1 VM per GPU)။

**သတိပြုရန်:** GPU ကို MIG mode ဖွင့်ပြီး passthrough လုပ်ရင် VM တစ်ခုတည်းက MIG-mode GPU တစ်လုံးလုံးကို ရရှိပြီး MIG slice အားလုံးကို မြင်နိုင်ပါတယ်။ Passthrough mode မှာ VM များစွာကြား slice တစ်ခုချင်းစီ ခွဲဝေပေးခြင်း မလုပ်နိုင်ပါဘူး။

### 3.2 Time-Sliced vGPU

NVIDIA Virtual GPU Manager က hypervisor အဆင့်မှာ တပ်ဆင်ပြီး၊ VM တစ်ခုချင်းစီကို GPU pipeline ကို အလှည့်ကျ သီးသန့် အသုံးပြုခွင့် ပေးပါတယ်။ VM များစွာက GPU တစ်လုံးကို မျှဝေ အသုံးပြုကြပါတယ်။ VM တစ်ခုချင်းစီမှာ guest driver များ သီးခြား လိုအပ်ပါတယ်။ NVIDIA vGPU license လိုအပ်ပါတယ်။

KB ဇယားအရ time-sliced vGPU ဖြင့် GPU တစ်လုံးကို VM အများဆုံး ၃၂ ခုအထိ မျှဝေနိုင်ပါတယ် (model/version-specific — သက်ဆိုင်ရာ GPU နဲ့ vGPU release notes မှာ စစ်ဆေးပါ)။

Scheduler time-slice length က latency နဲ့ throughput ကို ချိန်ညှိပေးပါတယ် — slice တိုရင် latency ပိုကောင်းပြီး၊ slice ရှည်ရင် context switch နည်းလို့ throughput ပိုကောင်းပါတယ် (needs verification — vGPU user guide ရဲ့ scheduler အခန်းတွင် စစ်ဆေးပါ)။

### 3.3 MIG (Multi-Instance GPU)

Hardware-level spatial partitioning ဖြစ်ပါတယ်။ Physical GPU chip ကို GPU instances များအဖြစ် ခွဲပြီး၊ instance တစ်ခုချင်းစီကို သတ်မှတ်ထားတဲ့ SMs, memory နဲ့ memory bandwidth ပေးအပ်ပါတယ်။ Instance ကြား memory, fault နဲ့ performance isolation ရှိပြီး performance ကို ကြိုတင် ခန့်မှန်းနိုင်ပါတယ်။

MIG ကို support လုပ်တာက data center GPU အချို့သာ ဖြစ်ပါတယ် (MIG User Guide ကို ကြည့်ပါ)။

**KB မှတ်ချက်:** MIG-enabled passthrough ကို "single-tenant environments အတွက် ဒီဇိုင်းပြုထားသည်" လို့ KB မှာ ဖော်ပြထားပါတယ်။

### 3.4 MIG-Backed vGPU

Spatial နဲ့ temporal partitioning ကို ပေါင်းစပ်တာ ဖြစ်ပါတယ်။ MIG slice တွေကနေ vGPU တွေ ဖန်တီးပါတယ်။

- **MIG-backed 1:1 vGPU** — Slice တစ်ခုချင်းစီကို vGPU တစ်ခုတည်း ပေးပြီး၊ VM တစ်ခုချင်းစီအတွက် full hardware isolation ရရှိပါတယ်။
- **MIG-backed time-sliced vGPU** — MIG slice တစ်ခုကို VM များစွာ မျှဝေပါတယ်။ KB ဥပမာအရ RTX PRO 6000 Blackwell မှာ MIG slice တစ်ခုလျှင် VM အများဆုံး ၁၂ ခု၊ GPU တစ်လုံးလျှင် အများဆုံး ၄၈ ခုအထိ (model/version-specific — KB ဥပမာသာဖြစ်သည်၊ GPU model အလိုက် စစ်ဆေးပါ)။

MIG-backed vGPU အတွက် vGPU license enforcement ကျင့်သုံးပါတယ်။

### 3.5 Kubernetes Time-Slicing (GPU Operator)

GPU Operator က device plugin ရဲ့ extended options များဖြင့် GPU oversubscription ကို ခွင့်ပြုပါတယ်။ GPU resource တစ်ခုကို replica များအဖြစ် သတ်မှတ်ပြီး၊ replica တစ်ခုချင်းစီကို pod တစ်ခုချင်းစီထံ သီးခြား ပေးအပ်နိုင်ပါတယ်။

**MIG နဲ့ မတူတဲ့အချက်:** Replica တွေကြား memory သို့မဟုတ် fault isolation မရှိပါဘူး။ GPU process အားလုံးကို အချိန် ညီမျှစွာ မျှဝေပေးပါတယ်။ Replica အရေအတွက်ကို ConfigMap နဲ့ cluster-wide သို့မဟုတ် node-specific ချိန်ညှိနိုင်ပြီး၊ MIG နဲ့လည်း ပေါင်းစပ် အသုံးပြုနိုင်ပါတယ်။ ဒီနည်းလမ်းက MIG မရှိတဲ့ GPU အဟောင်းတွေမှာလည်း sharing လုပ်နိုင်စေပါတယ်။

### 3.6 နှိုင်းယှဉ်ချက် (Comparison)

အောက်ပါဇယားထဲက ဂဏန်းအရေအတွက်များဟာ NVIDIA documentation က ဥပမာပေးထားတဲ့ ဂဏန်းတွေဖြစ်ပြီး **GPU model နဲ့ software version ပေါ် မူတည်ပါတယ် — အမြဲတမ်း သက်ဆိုင်ရာ release notes နဲ့ support matrix မှာ စစ်ဆေးပါ**။

| Approach | Isolation type | Sharing model | Licensing | Best for |
|---|---|---|---|---|
| GPU passthrough | Exclusive — VM တစ်ခုက GPU တစ်လုံးလုံး ပိုင်ဆိုင်၊ isolation အခိုင်မာဆုံး | GPU တစ်လုံးလျှင် VM ၁ ခု (MIG-mode passthrough မှာလည်း VM ၁ ခုက GPU တစ်လုံးလုံး ရရှိ) — model/version-specific, verify | vGPU software license မလို (vGPU software မသုံး) | Single-tenant HPC၊ full-GPU performance လိုအပ်သော workload |
| Time-sliced vGPU | VM ကြား hardware-based memory နဲ့ fault isolation (KB)၊ compute pipeline ကို round-robin အလှည့်ကျ မျှဝေ | VM များစွာ မျှဝေ (KB: GPU တစ်လုံးလျှင် အများဆုံး VM ၃၂ ခု — model/version-specific, verify) | NVIDIA vGPU license လိုအပ် | VDI, virtual workstations, VM အတွင်း AI workload |
| MIG | Instance ကြား memory + fault + performance isolation (hardware — dedicated SMs/memory) | Bare-metal, container သို့မဟုတ် passthrough ဖြင့် VM ၁ ခု (KB: single-tenant အတွက် ဒီဇိုင်း) | MIG feature အတွက် platform/software requirements စစ်ဆေးပါ | Predictable performance လိုအပ်သော multi-tenant inference |
| MIG-backed vGPU | Hardware-based memory နဲ့ fault isolation; 1:1 mode မှာ VM တစ်ခုချင်းစီ full hardware isolation | MIG slice များကနေ vGPU — 1:1 (slice လျှင် vGPU ၁ ခု) သို့မဟုတ် time-sliced (KB ဥပမာ: MIG slice လျှင် VM အများဆုံး ၁၂ ခု၊ GPU တစ်လုံးလျှင် ၄၈ ခု — model/version-specific, verify) | vGPU license လိုအပ် (MIG-backed vGPU တွင် license enforcement ရှိ) | Secure multi-tenant VM environment — spatial + temporal နှစ်မျိုးလုံး လိုအပ်သောနေရာ |
| Kubernetes time-slicing (GPU Operator) | Replica ကြား isolation မရှိ — memory သို့မဟုတ် fault isolation မပါ | Pod များက `nvidia.com/gpu` replica များဖြင့် GPU တစ်လုံး မျှဝေ (replica အရေအတွက် configurable) | vGPU software license မလို (driver/device-plugin အဆင့် sharing) | Strict isolation မလိုသော container batch/dev inference |

## 4. ဘယ်လို အလုပ်လုပ်သလဲ (How It Works)

### 4.1 Virtual GPU Manager + Guest Drivers

Time-sliced vGPU မှာ အဓိက component နှစ်ခု ပါဝင်ပါတယ်။

1. **NVIDIA Virtual GPU Manager** — Hypervisor (vSphere, Citrix Hypervisor, KVM စသည် — သက်ဆိုင်ရာ release notes အလိုက် စစ်ဆေးပါ) အဆင့်မှာ တပ်ဆင်ပြီး၊ physical GPU ကို VM များစွာကြား ခွဲဝေ စီမံပါတယ်။ VM တစ်ခုချင်းစီကို GPU pipeline အသုံးပြုခွင့်ကို အချိန်အလိုက် အလှည့်ကျ ပေးပါတယ် (vGPU scheduler က round-robin ပုံစံ အလှည့်ကျ စီမံပါတယ် — KB မှာ best-effort က default scheduler ဖြစ်ကြောင်း ဖော်ပြထားပါတယ်)။
2. **Guest VM drivers** — VM တစ်ခုချင်းစီအတွင်းမှာ NVIDIA guest driver များ တပ်ဆင်ရပါတယ်။ VM အတွက် vGPU device က normal GPU device တစ်ခုလို မြင်ရပြီး၊ application များက ပုံမှန်အတိုင်း CUDA/driver stack ကို အသုံးပြုနိုင်ပါတယ်။

vGPU profile များကို graphics (A/B/Q series) နဲ့ compute အလိုက် ရွေးချယ်နိုင်ပြီး၊ သက်ဆိုင်ရာ vGPU license လိုအပ်ပါတယ်။

### 4.2 MIG Slices

MIG mode ဖွင့်ထားတဲ့ GPU မှာ chip ကို GPU instances များအဖြစ် ခွဲပါတယ်။ Instance တစ်ခုချင်းစီမှာ—

- သတ်မှတ်ထားတဲ့ **SMs** (Streaming Multiprocessors)
- သတ်မှတ်ထားတဲ့ **memory** နဲ့ **memory bandwidth**

ရရှိပါတယ်။ ဒါကြောင့် instance တစ်ခုရဲ့ workload က နောက် instance ကို memory, fault သို့မဟုတ် performance ပိုင်း သက်ရောက်မှု မရှိစေပါဘူး။ Instance တစ်ခုချင်းစီက mini-GPU တစ်လုံးလို မြင်ရပါတယ်။

MIG instance အရေအတွက်နဲ့ profile ပေါင်းစပ်မှုက GPU model နဲ့ supported configuration ပေါ် မူတည်ပါတယ်။

### 4.3 Kubernetes Time-Slicing ConfigMap

GPU Operator မှာ time-slicing ကို ConfigMap တစ်ခုနဲ့ ချိန်ညှိပါတယ်။ အောက်ပါဟာ documentation က ပေးထားတဲ့ sample ဖြစ်ပြီး (illustrative)၊ replica ၄ ခု သတ်မှတ်ထားတဲ့အတွက် GPU တစ်လုံးရှိတဲ့ node က GPU resource ၄ ခုအဖြစ် ကြေညာပါမယ်။

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-slicing-config
data:
  any: |-
    version: v1
    flags:
      migStrategy: none
    sharing:
      timeSlicing:
        renameByDefault: false
        failRequestsGreaterThanOne: false
        resources:
          - name: nvidia.com/gpu
            replicas: 4
```

အဓိက အပြုအမူများ —

- Pod တစ်ခုက GPU resource တောင်းရင် shared GPU တစ်ခုကို ရရှိပါမယ်။
- Time-sliced GPU တစ်ခုထက် ပိုတောင်းရင်လည်း proportional compute ရမယ်လို့ အာမခံချက် မရှိပါဘူး။
- Cluster-wide default သတ်မှတ်နိုင်သလို node-specific configuration လည်း သတ်မှတ်နိုင်ပြီး နှစ်ခုလုံး ပေါင်းစပ် အသုံးပြုနိုင်ပါတယ်။

## 5. Workload အလိုက် သင့်တော်သော ရွေးချယ်မှု

| Workload | သင့်တော်သော approach | အကြောင်းပြချက် |
|---|---|---|
| Single-tenant HPC / full-GPU performance | GPU passthrough | GPU တစ်လုံးလုံး သီးသန့်ရ၊ isolation အခိုင်မာဆုံး |
| Graphics workstation / VDI | Time-sliced vGPU (graphics profiles) | VM တစ်ခုချင်းစီကို vGPU device ပေးပြီး OS ကွဲပြားနိုင် |
| Multi-tenant inference | MIG သို့မဟုတ် MIG-backed vGPU | Instance ကြား memory/fault/performance isolation နဲ့ predictable QoS |
| Container batch compute (strict isolation မလို) | Kubernetes time-slicing | MIG မရှိတဲ့ GPU တွေမှာပါ sharing လုပ်နိုင်၊ ရိုးရှင်း |
| Secure multi-tenant VM environment | MIG-backed vGPU | Spatial + temporal ပေါင်းစပ်၊ VM တစ်ခုချင်းစီအတွက် hardware isolation |

ရွေးချယ်ရာမှာ workload ရဲ့ isolation လိုအပ်ချက်၊ performance predictability, GPU model ရဲ့ support ပုံစံ၊ management ပိုင်းနဲ့ license ကုန်ကျစရိတ်တွေကို ပေါင်းစပ် စဉ်းစားပါ။

## 6. သတိပြုရန် အချက်များ (Cautions)

- **VM အရေအတွက်များက GPU model/version-specific ဖြစ်ပါတယ်** — KB ဂဏန်းတွေ (time-sliced vGPU: GPU တစ်လုံးလျှင် VM အများဆုံး ၃၂ ခု; MIG-backed: GPU တစ်လုံးလျှင် ၄၈ ခု၊ MIG slice လျှင် ၁၂ ခု — RTX PRO 6000 Blackwell ဥပမာ) ကို universal maximum အဖြစ် မယူဆပါနဲ့။ vGPU/MIG release notes နဲ့ support matrix မှာ အမြဲ စစ်ဆေးပါ။
- **vGPU licensing** — Time-sliced vGPU နဲ့ MIG-backed vGPU နှစ်ခုလုံးအတွက် NVIDIA vGPU software license လိုအပ်ပါတယ်။
- **MIG တစ်ခုတည်းက multi-tenancy မဟုတ်ပါဘူး** — vGPU KB အရ "MIG alone does not support multi-tenancy. To achieve true multi-tenancy, vGPU is required." ဖြစ်ပါတယ်။ MIG-enabled passthrough ကို single-tenant အတွက်သာ ဒီဇိုင်းပြုထားပါတယ်။
- **Time-slicing replicas ကြား isolation မရှိပါဘူး** — Kubernetes time-slicing မှာ replica တွေကြား memory သို့မဟုတ် fault isolation မပါပါဘူး။ တစ်ခုချင်းစီ workload ရဲ့ resource request က proportional GPU compute ကို အာမခံချက် မပေးပါဘူး။
- **MIG support က data center GPU အချို့မှာသာ** — GPU memory များရုံနဲ့ MIG ရှိတာ မဟုတ်ပါဘူး။ အတိအကျ GPU model ရဲ့ MIG support ကို MIG User Guide မှာ စစ်ဆေးပါ။
- **MIG mode + passthrough** — VM တစ်ခုတည်းက GPU တစ်လုံးလုံး (slice အားလုံး) ရရှိပြီး၊ passthrough mode မှာ slice ခွဲဝေပေးခြင်း မလုပ်နိုင်ပါဘူး။

## 7. စစ်ဆေးရန် မေးခွန်းများနှင့် အဖြေများ

**မေးခွန်း ၁။** Temporal partitioning နဲ့ spatial partitioning ရဲ့ ကွာခြားချက်က ဘာလဲ?

**အဖြေ။** Temporal partitioning (time slicing) မှာ tenant တွေက GPU resource တွေကို အချိန်အလိုက် အလှည့်ကျ မျှဝေသုံးပြီး memory ကို hardware အဆင့် မခွဲဝေပါဘူး။ Spatial partitioning (MIG) မှာ GPU ကို hardware အဆင့်ကနေ instances အဖြစ် ခွဲပြီး instance တစ်ခုချင်းစီကို dedicated SMs, memory နဲ့ memory bandwidth ပေးပါတယ်။

**မေးခွန်း ၂။** ဘယ် approach မှာ replica/pod ကြား memory နဲ့ fault isolation မရှိဘူးလဲ?

**အဖြေ။** Kubernetes time-slicing (GPU Operator) မှာ ဖြစ်ပါတယ်။ MIG နဲ့ မတူဘဲ replica တွေကြား memory သို့မဟုတ် fault isolation မရှိပါဘူး။ GPU process အားလုံးကို အချိန် ညီမျှစွာသာ မျှဝေပေးပါတယ်။

**မေးခွန်း ၃။** MIG တစ်ခုတည်းနဲ့ true multi-tenancy ရနိုင်ပြီလား?

**အဖြေ။** မရနိုင်သေးပါဘူး။ vGPU KB အရ MIG တစ်ခုတည်းက multi-tenancy ကို support မလုပ်ပါဘူး။ True multi-tenancy အတွက် vGPU (MIG-backed vGPU အပါအဝင်) လိုအပ်ပါတယ်။

**မေးခွန်း ၄။** Time-sliced vGPU ဖြင့် GPU တစ်လုံးမှာ VM အမြဲတမ်း ၃၂ ခုအထိ ရနိုင်သလား?

**အဖြေ။** မဟုတ်ပါဘူး။ ဒီဂဏန်းက KB ဇယားက ပေးထားတဲ့ maximum ဖြစ်ပြီး GPU model, vGPU profile နဲ့ software version ပေါ် မူတည်ပါတယ်။ သက်ဆိုင်ရာ release notes နဲ့ support matrix မှာ စစ်ဆေးရပါမယ်။

**မေးခွန်း ၅။** vGPU အလုပ်လုပ်ဖို့ component ဘာတွေ လိုအပ်သလဲ?

**အဖြေ။** Hypervisor အဆင့်မှာ NVIDIA Virtual GPU Manager၊ VM တစ်ခုချင်းစီအတွင်းမှာ NVIDIA guest driver များ၊ နဲ့ သက်ဆိုင်ရာ NVIDIA vGPU software license တို့ လိုအပ်ပါတယ်။

---

## English Version — GPU Virtualization Fundamentals

## 1. The Problem — Expensive GPUs That Sit Underused

A data-center GPU is one of the most expensive components in an AI infrastructure, yet a single workload rarely keeps it busy. At the same time, many consumers want a share of that GPU:

- many VMs
- many users
- many container workloads

Each consumer has different needs — some want bare-metal-like performance, others only need a fair share of capacity and basic isolation. This module covers the fundamentals of how one physical GPU can be shared: the partitioning models, the main approaches, and how to choose between them. For an in-depth comparison of vGPU and MIG specifically, continue with **Module 17 — NVIDIA vGPU vs. MIG**.

## 2. Core Concepts

### 2.1 Temporal Partitioning vs. Spatial Partitioning

Every GPU sharing approach is built on one of two partitioning ideas:

- **Temporal partitioning (time slicing)** — all tenants use the same GPU resources, taking turns over time. Memory is not physically divided between tenants.
- **Spatial partitioning** — the GPU is split at the hardware level into partitions, each with its own dedicated SMs, memory, and memory bandwidth.

```
Temporal partitioning (time slicing)          Spatial partitioning (MIG)
+------------------------------------+        +------------------+-----------------+
|         One GPU pipeline           |        | GPU Instance 1   | GPU Instance 2  |
|  VM1 -> VM2 -> VM3 -> VM1 -> ...   |        | dedicated SMs    | dedicated SMs   |
|  (shared over time)                |        | dedicated memory | dedicated memory|
+------------------------------------+        +------------------+-----------------+
```

### 2.2 Terminology

- **GPU passthrough (PCI passthrough)** — assigning a whole physical GPU exclusively to one VM.
- **Time-slicing** — dividing GPU pipeline access into turns over time (temporal partitioning).
- **vGPU (virtual GPU)** — presenting virtual GPU devices to VMs using NVIDIA Virtual GPU Software and drivers.
- **MIG (Multi-Instance GPU)** — splitting a physical GPU into hardware-supported GPU instances (spatial partitioning).
- **Oversubscription** — promising a resource to more consumers than its physical capacity (for example, advertising one GPU as four replicas).
- **QoS (Quality of Service)** — how predictable and guaranteed a workload's performance is.
- **Multi-tenancy** — many tenants (users, VMs, teams) sharing the same infrastructure with isolation so tenants do not affect each other.

## 3. Sharing Approaches

Five approaches appear most often in practice.

### 3.1 GPU Passthrough (PCI Passthrough)

A whole physical GPU is assigned exclusively to one VM. This is the simplest approach and gives that VM the best performance and strongest isolation. The trade-off: one VM per GPU.

**Note:** With the GPU in MIG mode, passthrough still gives one VM the entire MIG-mode GPU, and that VM can see all MIG slices. Per-slice assignment across multiple VMs is not available in passthrough mode.

### 3.2 Time-Sliced vGPU

The NVIDIA Virtual GPU Manager runs at the hypervisor level and schedules each VM exclusive turns on the GPU pipeline, so many VMs share one GPU. Each VM needs its own guest drivers, and an NVIDIA vGPU license is required.

Per the vGPU knowledge base table, a GPU can be shared by up to 32 VMs with time-sliced vGPU (model/version-specific — verify against the relevant GPU and vGPU release notes).

The scheduler's time-slice length trades latency against throughput: shorter slices lower latency, while longer slices mean fewer context switches and better throughput (needs verification — check the scheduler chapter of the vGPU user guide).

### 3.3 MIG (Multi-Instance GPU)

MIG is hardware-level spatial partitioning. The physical GPU chip is split into GPU instances, each with its own dedicated SMs, memory, and memory bandwidth. Instances have memory, fault, and performance isolation from each other, which makes performance predictable.

MIG is supported only on certain data center GPUs (see the MIG User Guide).

**KB note:** MIG-enabled passthrough is described in the KB as "designed for single-tenant environments".

### 3.4 MIG-Backed vGPU

This combines spatial and temporal partitioning: vGPUs are created from MIG slices.

- **MIG-backed 1:1 vGPU** — one vGPU per slice, giving full hardware isolation per VM.
- **MIG-backed time-sliced vGPU** — one MIG slice is shared by many VMs. KB example: on the RTX PRO 6000 Blackwell, up to 12 VMs per MIG slice and up to 48 VMs per GPU (model/version-specific — this is a KB example; verify per GPU model).

vGPU license enforcement applies to MIG-backed vGPU.

### 3.5 Kubernetes Time-Slicing (GPU Operator)

The GPU Operator enables GPU oversubscription through extended options for the NVIDIA Kubernetes Device Plugin. A GPU resource is defined as a set of replicas, each handed out independently to a pod.

**Unlike MIG,** there is no memory or fault isolation between replicas — the GPU simply gives an equal share of time to all GPU processes. The replica count is configured with a ConfigMap, cluster-wide or per node, and time-slicing can be combined with MIG. It also enables sharing on older GPUs that do not support MIG.

### 3.6 Comparison

The counts in the table below are example figures from NVIDIA documentation. They are **model- and version-specific — always verify against the relevant release notes and support matrix**.

| Approach | Isolation type | Sharing model | Licensing | Best for |
|---|---|---|---|---|
| GPU passthrough | Exclusive — one VM owns the whole GPU; strongest isolation | 1 VM per GPU (MIG-mode passthrough also gives one VM the whole GPU) — model/version-specific, verify | No vGPU software license (no vGPU software involved) | Single-tenant HPC, full-GPU performance |
| Time-sliced vGPU | Hardware-based memory and fault isolation between VMs (KB); GPU pipeline shared via round-robin scheduling | Many VMs per GPU (KB: up to 32 VMs per GPU — model/version-specific, verify) | NVIDIA vGPU license required | VDI, virtual workstations, AI workloads in VMs |
| MIG | Memory + fault + performance isolation between instances (hardware — dedicated SMs/memory) | Bare-metal, containers, or 1 VM via passthrough (KB: designed for single-tenant) | Check platform/software requirements for the MIG feature | Multi-tenant inference needing predictable performance |
| MIG-backed vGPU | Hardware-based memory and fault isolation; 1:1 mode gives full hardware isolation per VM | vGPUs from MIG slices — 1:1 (one vGPU per slice) or time-sliced (KB example: up to 12 VMs per MIG slice, 48 VMs per GPU — model/version-specific, verify) | vGPU license required (license enforcement applies to MIG-backed vGPU) | Secure multi-tenant VM environments needing both spatial and temporal sharing |
| Kubernetes time-slicing (GPU Operator) | None between replicas — no memory or fault isolation | Pods share one GPU through `nvidia.com/gpu` replicas (replica count configurable) | No vGPU software license (driver/device-plugin level sharing) | Container batch/dev inference without strict isolation |

## 4. How It Works

### 4.1 Virtual GPU Manager + Guest Drivers

Time-sliced vGPU has two key components.

1. **NVIDIA Virtual GPU Manager** — installed at the hypervisor level (vSphere, Citrix Hypervisor, KVM, etc. — check the relevant release notes), it divides and manages the physical GPU across many VMs, granting each VM exclusive turns on the GPU pipeline over time (the vGPU scheduler allocates turns in a round-robin fashion — the KB notes best-effort is the default scheduler).
2. **Guest VM drivers** — NVIDIA guest drivers are installed inside each VM. The vGPU device looks like a normal GPU device to the VM, so applications use the usual CUDA/driver stack unchanged.

vGPU profiles come in graphics (A/B/Q series) and compute variants, and the corresponding vGPU license is required.

### 4.2 MIG Slices

On a GPU with MIG mode enabled, the chip is split into GPU instances. Each instance receives:

- dedicated **SMs** (Streaming Multiprocessors)
- dedicated **memory** and **memory bandwidth**

A workload in one instance therefore cannot affect the memory, faults, or performance of another instance. Each instance looks like a mini-GPU.

The number of MIG instances and the usable profile combinations depend on the GPU model and supported configurations.

### 4.3 Kubernetes Time-Slicing ConfigMap

In the GPU Operator, time-slicing is configured with a ConfigMap. The following is the sample from the documentation (illustrative): with 4 replicas configured, a node with one GPU advertises four GPU resources.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-slicing-config
data:
  any: |-
    version: v1
    flags:
      migStrategy: none
    sharing:
      timeSlicing:
        renameByDefault: false
        failRequestsGreaterThanOne: false
        resources:
          - name: nvidia.com/gpu
            replicas: 4
```

Key behaviors:

- A pod requesting a GPU resource gets a shared GPU.
- Requesting more than one time-sliced GPU does not guarantee a proportional amount of GPU compute.
- You can set a cluster-wide default, node-specific configurations, or combine both.

## 5. Workload Fit Guidance

| Workload | Suitable approach | Why |
|---|---|---|
| Single-tenant HPC / full-GPU performance | GPU passthrough | Whole GPU exclusive to one VM; strongest isolation |
| Graphics workstation / VDI | Time-sliced vGPU (graphics profiles) | Each VM gets its own vGPU device and can run its own OS |
| Multi-tenant inference | MIG or MIG-backed vGPU | Memory/fault/performance isolation between instances and predictable QoS |
| Container batch compute (no strict isolation needed) | Kubernetes time-slicing | Works even on GPUs without MIG; simple to operate |
| Secure multi-tenant VM environment | MIG-backed vGPU | Spatial + temporal combined; hardware isolation per VM |

When choosing, combine the workload's isolation needs, performance predictability, the GPU model's support, operational management, and license cost.

## 6. Cautions

- **VM counts are GPU model/version-specific** — do not treat the KB figures (time-sliced vGPU: up to 32 VMs per GPU; MIG-backed: up to 48 VMs per GPU and 12 per MIG slice — RTX PRO 6000 Blackwell example) as universal maxima. Always verify against the vGPU/MIG release notes and support matrix.
- **vGPU licensing** — both time-sliced vGPU and MIG-backed vGPU require an NVIDIA vGPU software license.
- **MIG alone is not multi-tenancy** — per the vGPU KB: "MIG alone does not support multi-tenancy. To achieve true multi-tenancy, vGPU is required." MIG-enabled passthrough is designed for single-tenant environments only.
- **No isolation between time-slicing replicas** — Kubernetes time-slicing replicas share without memory or fault isolation, and a workload's resource request does not guarantee a proportional amount of GPU compute.
- **MIG support is limited to certain data center GPUs** — a lot of GPU memory does not imply MIG support. Check the exact GPU model in the MIG User Guide.
- **MIG mode + passthrough** — a single VM gets the whole GPU (all slices); per-slice assignment is not available in passthrough mode.

## 7. Self-Check Questions and Answers

**Q1.** What is the difference between temporal and spatial partitioning?

**A1.** In temporal partitioning (time slicing), tenants share the GPU resources by taking turns over time, and memory is not divided at the hardware level. In spatial partitioning (MIG), the GPU is split at the hardware level into instances, each with dedicated SMs, memory, and memory bandwidth.

**Q2.** Which approach has no memory or fault isolation between replicas/pods?

**A2.** Kubernetes time-slicing (GPU Operator). Unlike MIG, replicas share without memory or fault isolation; the GPU gives all GPU processes an equal share of time.

**Q3.** Does MIG alone deliver true multi-tenancy?

**A3.** Not yet. Per the vGPU KB, MIG alone does not support multi-tenancy; true multi-tenancy requires vGPU (including MIG-backed vGPU).

**Q4.** Can you always get 32 VMs on one GPU with time-sliced vGPU?

**A4.** No. That figure is the maximum shown in the KB table and depends on the GPU model, vGPU profile, and software version. Verify against the relevant release notes and support matrix.

**Q5.** What components are needed for vGPU to work?

**A5.** The NVIDIA Virtual GPU Manager at the hypervisor level, NVIDIA guest drivers inside each VM, and the corresponding NVIDIA vGPU software license.

---

## Official references

- https://docs.nvidia.com/vgpu/knowledge-base/latest/vgpu-features.html — vGPU features KB (schedulers, MIG, MIG-backed vGPU, Secure Multitenancy comparison table)
- https://docs.nvidia.com/grid/latest/grid-vgpu-user-guide/ — Virtual GPU Software User Guide
- https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-sharing.html — Time-Slicing GPUs in Kubernetes (GPU Operator)
- https://docs.nvidia.com/datacenter/tesla/mig-user-guide/index.html — MIG User Guide

## Technical review notes

- VM counts in the comparison table come from the vGPU KB "Secure Multitenancy" table (Passthrough: 1 VM per GPU; time-sliced vGPU: 32 VMs per GPU; MIG-backed vGPU: 48 VMs per GPU, 12 VMs per MIG instance) and were re-verified against the live KB page. They are model/version-specific; the 12-VMs-per-MIG-slice figure is documented for the RTX PRO 6000 Blackwell and must be verified per GPU model.
- The quote "MIG alone does not support multi-tenancy. To achieve true multi-tenancy, vGPU is required." is taken from the research notes attributed to the vGPU KB. It is consistent with the KB's verified statement that MIG-enabled passthrough is "designed for single-tenant environments"; the exact wording should be re-checked against the live KB page (needs verification).
- The scheduler time-slice length trade-off (short slices → lower latency; long slices → higher throughput, fewer context switches) is from the research notes and needs verification against the vGPU user guide's scheduler documentation. The statement that best-effort is the default vGPU scheduler was verified on the live KB page.
- The sample Kubernetes time-slicing ConfigMap is reproduced from the GPU Operator "Time-Slicing GPUs in Kubernetes" page and was verified against the live page; it is a documentation example, not a universal configuration.
- Licensing: vGPU paths require an NVIDIA vGPU license (the KB confirms license enforcement applies to MIG-backed vGPU). Passthrough and Kubernetes time-slicing do not use vGPU software, so no vGPU license is claimed for them; confirm licensing for specific deployments against the NVIDIA License System documentation.
- MIG support is limited to specific data center GPUs; the module points to the MIG User Guide instead of an exhaustive model list, per the research notes' caution. Hypervisor support for vGPU features is version-dependent and is marked "per release notes" throughout.
