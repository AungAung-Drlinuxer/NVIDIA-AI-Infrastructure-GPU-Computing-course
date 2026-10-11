# Module 27 — NVIDIA Base Command Manager (BCM)

> Part V · NVIDIA Monitoring, Cluster Management and Tool Selection — topic notes

## မြန်မာဘာသာ version

## 1. Learning Objectives

ဒီ Module မှာ NVIDIA Base Command Manager (BCM) က AI နဲ့ HPC cluster တွေအတွက် ဘယ်လို cluster-management platform အဖြစ် အလုပ်လုပ်သလဲဆိုတာ လေ့လာပါမယ်။

သင်ခန်းစာပြီးဆုံးတဲ့အခါ အောက်ပါအချက်တွေကို နားလည်နိုင်ရပါမယ်။

- BCM ရဲ့ အမည်အပြည့်အစုံနဲ့ အဓိကအခန်းကဏ္ဍ။
- BCM conceptual architecture — Administrator, Base View UI, cmsh, Head Node နဲ့ managed nodes။
- BCM Head Node နဲ့ compute node ရဲ့ ကွာခြားချက် (head node ≠ compute node)။
- Base View UI ရဲ့ features တွေနဲ့ UI graph တစ်ခုတည်းကနေ GPU performance ကို မဆုံးဖြတ်ရခြင်း။
- Cluster provisioning ဆိုတာ ဘာလဲ။
- BCM နဲ့ Slurm, Kubernetes, NVIDIA Run:ai တို့ရဲ့ ဆက်နွယ်မှု။
- BCM နဲ့ NVIDIA-SMI / DCGM တို့ရဲ့ ကွာခြားချက်။

## 2. BCM ဆိုတာဘာလဲ။

BCM = NVIDIA Base Command Manager — AI နဲ့ high-performance computing infrastructure အတွက် NVIDIA ရဲ့ cluster management software/platform ဖြစ်ပါတယ်။

- NVIDIA-SMI က GPU-device information ကို အဓိကထားပါတယ်။
- DCGM က GPU management နဲ့ telemetry ကို အဓိကထားပါတယ်။
- BCM က ပိုမိုကျယ်ပြန့်တဲ့ cluster lifecycle management ကို ကိုင်တွယ်ပါတယ် — cluster/node provisioning၊ infrastructure configuration နဲ့ administration၊ cluster health နဲ့ utilization monitoring၊ operational dashboards၊ Slurm ကဲ့သို့ workload managers တွေနဲ့ integration၊ heterogeneous clusters တွေကို စီမံခန့်ခွဲခြင်း။

BCM ဟာ monitoring dashboard တစ်ခုတည်း မဟုတ်သလို nvidia-smi နဲ့ တူညီတဲ့ command တစ်ခုလည်း မဟုတ်ပါဘူး။ Cluster management platform တစ်ခု ဖြစ်ပါတယ်။

## 3. BCM နဲ့ NVIDIA-SMI / DCGM ကွာခြားချက်

| Capability | NVIDIA-SMI | DCGM | Base Command Manager |
|---|---|---|---|
| Primary focus | GPU query and management | GPU telemetry, health, diagnostics, management | End-to-end cluster management |
| Server provisioning | မဟုတ်ပါဘူး | မဟုတ်ပါဘူး | ဟုတ်ပါတယ် |
| Cluster configuration | မဟုတ်ပါဘူး | GPU management scope အထိသာ | ဟုတ်ပါတယ် |
| Monitoring | Local GPU status | GPU telemetry/health/diagnostics | Cluster inventory နဲ့ configured monitoring data |
| Job scheduling | မဟုတ်ပါဘူး | မဟုတ်ပါဘူး | Scheduler integrations သာ (BCM ကိုယ်တိုင် scheduler မဟုတ်) |

Key takeaway — BCM ကို AI data center ရဲ့ cluster-management platform အဖြစ် နားလည်ပါ။ ၎င်းဟာ monitoring တစ်ခုတည်းမဟုတ်သလို GPU driver, DCGM, Prometheus, Grafana သို့မဟုတ် workload scheduler တို့ကို အားလုံးအစားထိုးပေးတဲ့ tool တစ်ခုလည်း မဟုတ်ပါဘူး။

## 4. BCM Conceptual Architecture

```
Administrator
                        |
             +----------+----------+
             |                     |
             v                     v
         Base View UI           cmsh CLI
             |                     |
             +----------+----------+
                        |
                        v
                 BCM Head Node
          +--------------------------+
          | Cluster Management      |
          | Configuration Database  |
          | Management Services     |
          | APIs / Web Interface    |
          +--------------------------+
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
      GPU Nodes     CPU Nodes    Managed Network /
      + DCGM        + Services   Storage Resources
          |
          v
     GPU / CPU / Storage / Network
          |
          v
    Slurm / Kubernetes / Run:ai
        Workload Ecosystem
```

ဒီပုံဟာ conceptual architecture ဖြစ်ပါတယ်။ အမှန်တကယ် deployment မှာ components တွေ၊ integrations တွေ၊ management interfaces တွေက BCM version နဲ့ system design ပေါ်မူတည်ပြီး ကွာခြားနိုင်ပါတယ်။

### 4.1 BCM Head Node

BCM Head Node ဟာ cluster management ရဲ့ အဓိကနေရာ ဖြစ်ပါတယ်။ ၎င်းမှာ deployment ပုံစံပေါ်မူတည်ပြီး အောက်ပါ components တွေ ပါဝင်နိုင်ပါတယ်။

- Cluster management services။
- Cluster configuration နဲ့ inventory data။
- Web-based management interface။
- Command-line management interface။
- APIs နဲ့ integration endpoints။
- Provisioning နဲ့ configuration-management functions။

**Head Node ဟာ workload တွေကို ကိုယ်တိုင် run ဖို့ ရည်ရွယ်ထားတဲ့ compute node နဲ့ မတူပါဘူး။** Cluster management ကို အဓိကလုပ်ဆောင်တဲ့ system ဖြစ်ပါတယ်။ Production environment မှာ Head Node ရဲ့ availability, backup, network security နဲ့ access control တို့ကို သေချာစီမံဖို့ လိုအပ်ပါတယ်။

### 4.2 Managed Nodes

Managed nodes တွေဟာ BCM က စီမံခန့်ခွဲတဲ့ compute systems တွေ ဖြစ်ပါတယ်။ ဥပမာ —

- NVIDIA DGX systems။
- GPU-accelerated servers။
- CPU-only compute nodes။
- အခြား supported heterogeneous compute nodes။

Node တစ်ခုချင်းစီမှာ hardware inventory, operating system, configuration နဲ့ operational status တို့ကို cluster management platform က စီမံခန့်ခွဲနိုင်ပါတယ်။

### 4.3 Management Interfaces

BCM မှာ အဓိကအသုံးပြုနိုင်တဲ့ interface တွေက —

- **Base View** — Web-based management interface။
- **cmsh** — Cluster Management Shell။
- **REST API** — အခြား systems တွေနဲ့ programmatic integration လုပ်ရန်။

ဒီ interface တွေက တူညီတဲ့ cluster management environment ကို မတူညီတဲ့နည်းလမ်းတွေနဲ့ အသုံးပြုနိုင်စေပါတယ်။

## 5. Base View — Web-Based Management UI

Base View ဟာ BCM ရဲ့ web-based management interface ဖြစ်ပါတယ်။ ဒီ interface ကနေ cluster administrators တွေဟာ supported management functions တွေကို graphical interface နဲ့ အသုံးပြုနိုင်ပါတယ်။

UI ထဲမှာ version နဲ့ configuration ပေါ်မူတည်ပြီး အောက်ပါအမျိုးအစားတွေကို တွေ့နိုင်ပါတယ်။

- Cluster inventory။
- Compute nodes နဲ့ device status။
- Resource utilization။
- Node health နဲ့ monitoring information။
- Networking configuration။
- Provisioning နဲ့ node management။
- Device grouping။
- Workload-management information။
- Licensing information။
- Cluster configuration။

ဘာကြောင့် Web UI က အသုံးဝင်သလဲ — Server တစ်လုံးချင်းစီကို SSH ဝင်ပြီး command တွေ run လုပ်စရာမလိုဘဲ cluster inventory နဲ့ management information ကို အလွယ်တကူ ရှာဖွေနိုင်ပါတယ်။ ဥပမာ administrator တစ်ယောက်က GPU nodes တွေရဲ့ status ကို ကြည့်ပြီး ဘယ် node တွေမှာ troubleshooting လုပ်ဖို့ လိုအပ်သလဲဆိုတာ ဆုံးဖြတ်နိုင်ပါတယ်။

**သတိပြုရန် — UI ထဲက resource utilization graph တစ်ခုတည်းကို ကြည့်ပြီး GPU performance ကောင်း၊ မကောင်းကို အပြည့်အဝ မဆုံးဖြတ်သင့်ပါဘူး။** Workload metrics, hardware health, network performance နဲ့ application-level telemetry တို့ကိုလည်း ထည့်သွင်းစဉ်းစားရပါမယ်။

## 6. Cluster Provisioning

Provisioning ဆိုတာ compute nodes တွေကို အသုံးပြုနိုင်တဲ့ အခြေအနေသို့ ပြင်ဆင်ပေးခြင်းနဲ့ စီမံခန့်ခွဲခြင်းကို ဆိုလိုပါတယ်။ BCM ရဲ့ provisioning capabilities တွေက cluster ကို တည်ဆောက်ခြင်းနဲ့ node တွေကို စံသတ်မှတ်ထားတဲ့ configuration နဲ့ ပြင်ဆင်ခြင်းမှာ အထောက်အကူပြုနိုင်ပါတယ်။

ဥပမာ —

- Server hardware ကို cluster ထဲမှာ သတ်မှတ်ခြင်း။
- Network နဲ့ boot/provisioning configuration ကို ပြင်ဆင်ခြင်း။
- Supported operating system image ကို အသုံးပြုခြင်း။
- Node configuration ကို သတ်မှတ်ခြင်း။
- လိုအပ်တဲ့ software နဲ့ management services တွေကို ပြင်ဆင်ခြင်း။
- Node ကို cluster operation အတွက် အဆင်သင့်ဖြစ်အောင် ပြုလုပ်ခြင်း။

Provisioning က ဘာကြောင့်အရေးကြီးသလဲ — GPU server 20 လုံးကို manual configuration လုပ်မယ်ဆိုရင် node တစ်လုံးနဲ့တစ်လုံး configuration မတူညီနိုင်ပါဘူး (OS version, network interface configuration, driver version, shared storage configuration, required packages မတူခြင်း)။ Provisioning နဲ့ centralized configuration management ကို အသုံးပြုခြင်းက configuration drift ကို လျှော့ချဖို့ ကူညီပါတယ်။

သို့သော် provisioning ဆိုတာ server hardware ရဲ့ physical installation, cabling, power delivery နဲ့ data center cooling တို့ကို software တစ်ခုတည်းနဲ့ အလိုအလျောက် ဖြေရှင်းပေးတယ်လို့ မဆိုလိုပါဘူး။

## 7. Infrastructure Monitoring

BCM ရဲ့ အဓိကအခန်းကဏ္ဍတစ်ခုက cluster infrastructure ကို စောင့်ကြည့်နိုင်ခြင်း ဖြစ်ပါတယ်။ Monitoring scope က deployment, hardware support နဲ့ integration တွေပေါ်မူတည်ပြီး ကွာခြားနိုင်ပါတယ်။

### 7.1 CPU Monitoring

CPU-based compute nodes တွေအတွက် — CPU utilization၊ CPU နှင့် system-related health information၊ node status၊ memory utilization၊ OS နဲ့ hardware inventory တွေ ရရှိနိုင်ပါတယ်။ CPU temperature ကဲ့သို့ sensor metrics ရရှိနိုင်မှုက hardware sensor, driver နဲ့ platform integration ပေါ် မူတည်ပါတယ်။ BCM က CPU utilization ကို monitor လုပ်နိုင်တယ်ဆိုတာ CPU frequency, voltage နဲ့ hardware setting အားလုံးကို universal အနေနဲ့ တိုက်ရိုက်ပြောင်းလဲနိုင်တယ်လို့ မဆိုလိုပါဘူး။

### 7.2 GPU Monitoring

GPU-enabled nodes တွေအတွက် GPU monitoring ကို DCGM နဲ့ related monitoring integrations တွေနဲ့ ပေါင်းစပ်အသုံးပြုနိုင်ပါတယ်။ အသုံးဝင်တဲ့ metrics တွေက — GPU utilization၊ GPU memory utilization၊ GPU temperature၊ GPU power consumption၊ GPU health နဲ့ supported error information၊ GPU performance-related metrics။

BCM ရဲ့ monitoring environment က ဒီ data တွေကို cluster context နဲ့ ကြည့်ရှုစီမံနိုင်အောင် ကူညီပေးနိုင်ပါတယ်။ သို့သော် “BCM က DCGM ရဲ့ metrics အားလုံးကို အလိုအလျောက် စုဆောင်းပေးတယ်” လို့ အမြဲတမ်း မယူဆသင့်ပါဘူး။ ရရှိနိုင်တဲ့ metrics တွေဟာ configured exporters, monitoring forwarders, integrations နဲ့ platform version ပေါ်မူတည်ပါတယ်။

### 7.3 Network Monitoring

AI/HPC infrastructure မှာ network performance က အရေးကြီးပါတယ် — အထူးသဖြင့် InfiniBand၊ high-speed Ethernet၊ RoCE-based fabrics၊ supported NVIDIA networking hardware၊ BlueField DPU platforms။ Network monitoring မှာ link status, throughput, errors နဲ့ supported device telemetry တို့ကို အသုံးပြုနိုင်ပါတယ်။ ဒါပေမဲ့ BCM ရဲ့ network visibility ဟာ network switch, NIC နဲ့ DPU အားလုံးရဲ့ telemetry ကို အလိုအလျောက် တူညီတဲ့အတိုင်း ရရှိမယ်လို့ မဆိုလိုပါဘူး။

### 7.4 Storage Monitoring

AI training workloads တွေမှာ storage throughput နဲ့ latency က GPU utilization ကို သက်ရောက်စေနိုင်ပါတယ် (NFS shared storage, Lustre parallel file system, NVMe-based storage စသည်)။ Storage monitoring မှာ capacity, utilization, health နဲ့ performance-related data တွေ ပါဝင်နိုင်ပြီး ရရှိနိုင်တဲ့ metrics တွေက storage platform နဲ့ integration ပေါ် မူတည်ပါတယ်။ BCM ကို storage monitoring system အားလုံးအတွက် အလိုအလျောက် အစားထိုးတယ်လို့ မယူဆသင့်ပါဘူး။

## 8. Workload Management and Orchestration

BCM ရဲ့ အရေးကြီးတဲ့အချက်တစ်ခုက infrastructure resources တွေနဲ့ workload-management systems တွေကို ချိတ်ဆက်ပေးနိုင်ခြင်း ဖြစ်ပါတယ်။ GPU utilization ကိုသာ ကြည့်ရုံနဲ့ AI infrastructure ကို ထိရောက်စွာ စီမံခန့်ခွဲနိုင်မှာ မဟုတ်ပါဘူး။

### 8.1 Slurm

Slurm ဟာ HPC workload တွေအတွက် အသုံးများတဲ့ workload manager နဲ့ scheduler ဖြစ်ပြီး jobs တွေကို queue ထဲမှာ စီမံခန့်ခွဲခြင်း၊ available resources ပေါ်မူတည်ပြီး jobs schedule လုပ်ခြင်း၊ CPU နဲ့ GPU resources allocate လုပ်ခြင်း၊ job states နဲ့ execution status စီမံခြင်းတွေ လုပ်ဆောင်ပေးပါတယ်။ BCM က Slurm လို workload manager နဲ့ ပေါင်းစပ်ပြီး cluster provisioning နဲ့ infrastructure management ကို workload execution နဲ့ ချိတ်ဆက်ပေးနိုင်ပါတယ်။

### 8.2 Kubernetes

Kubernetes ဟာ containerized applications တွေကို deploy, schedule နဲ့ manage လုပ်ဖို့ အသုံးပြုတဲ့ orchestration platform ဖြစ်ပါတယ်။ BCM က Kubernetes cluster provisioning နဲ့ management workflows တွေကို support လုပ်နိုင်ပေမဲ့ BCM နဲ့ Kubernetes တို့ဟာ တူညီတဲ့ product မဟုတ်ပါဘူး။

### 8.3 NVIDIA Run:ai

NVIDIA Run:ai က AI workload orchestration နဲ့ GPU resource utilization ကို ပိုမိုထိရောက်စေဖို့ ရည်ရွယ်ထားတဲ့ platform ဖြစ်ပြီး BCM က Run:ai နဲ့ integration လုပ်နိုင်ပါတယ်။ သို့သော် Run:ai နဲ့ BCM ရဲ့ capabilities တွေဟာ အပြည့်အဝတူညီတာ မဟုတ်ပါဘူး။ Licensing, product version နဲ့ integration design ကို အခြေခံပြီး ဘယ် function ကို ဘယ် product က တာဝန်ယူသလဲ ခွဲခြားရပါတယ်။

အရေးကြီးသော ကွာခြားချက် — BCM ဟာ workload-management integrations တွေကို ပံ့ပိုးပေးနိုင်ပေမဲ့ Slurm, Kubernetes နဲ့ Run:ai တို့ရဲ့ function အားလုံးကို တစ်ခုတည်းအဖြစ် အစားထိုးပေးတယ်လို့ မယူဆသင့်ပါဘူး။

## 9. Resource Allocation, Job Status နဲ့ Performance

BCM နဲ့ ချိတ်ဆက်ထားတဲ့ workload-management system တွေပေါ်မူတည်ပြီး administrators တွေဟာ workload status နဲ့ resource usage ကို cluster context နဲ့ ကြည့်ရှုနိုင်ပါတယ် — job ID၊ job status၊ assigned nodes၊ allocated GPUs၊ CPU allocation၊ runtime၊ resource utilization၊ workload performance information။

GPU training job တစ်ခုမှာ GPU utilization နည်းနေရင် အောက်ပါအကြောင်းရင်းတွေ ဖြစ်နိုင်ပါတယ် — data loading နှေးနေခြင်း၊ CPU preprocessing bottleneck၊ storage throughput မလုံလောက်ခြင်း၊ network communication overhead၊ batch size သေးလွန်းခြင်း၊ job က အခြား operation တစ်ခုကို စောင့်ဆိုင်းနေရခြင်း။ ဒါကြောင့် utilization တစ်ခုတည်းကို ကြည့်ပြီး GPU resource ကို အလေအလွင့်ဖြစ်နေတယ်လို့ ချက်ချင်း မဆုံးဖြတ်သင့်ပါဘူး။

## 10. User Management နဲ့ Resource Quotas

Multi-user AI/HPC environment မှာ user တစ်ယောက်ချင်းစီက GPU resources တွေကို ဘယ်လိုအသုံးပြုခွင့်ရှိသလဲဆိုတာ စီမံဖို့ လိုအပ်ပါတယ် — user နဲ့ group management၊ authentication နဲ့ authorization၊ resource allocation၊ resource quotas၊ job access နဲ့ workload permissions။ သို့သော် quota enforcement ဟာ BCM configuration နဲ့ integration ပေါ်မူတည်ပြီး GPU quotas နဲ့ scheduling policies တွေမှာ Slurm, Kubernetes သို့မဟုတ် Run:ai က တိုက်ရိုက်တာဝန်ယူထားနိုင်ပါတယ်။

အဓိကမှတ်ရန် — Infrastructure management နဲ့ workload-level resource scheduling ဟာ ဆက်စပ်နေသော်လည်း တူညီတဲ့ function မဟုတ်ပါဘူး။

## 11. REST API နဲ့ Automation

BCM မှာ web interface နဲ့ CLI အပြင် programmatic integration အတွက် APIs တွေ ရှိပါတယ် — external monitoring platform နဲ့ integration၊ inventory data ကို automation scripts ကနေ အသုံးပြုခြင်း၊ approved configuration workflows automation၊ cluster-management workflows တွေကို အခြား enterprise systems နဲ့ ချိတ်ဆက်ခြင်း။

API endpoint, authentication, permissions နဲ့ available operations တွေဟာ BCM version နဲ့ configuration ပေါ်မူတည်ပါတယ်။ Production မှာ API automation လုပ်မယ်ဆိုရင် — API documentation ကို အရင်စစ်ဆေးပါ၊ least-privilege access သုံးပါ၊ credentials နဲ့ tokens တွေကို လုံခြုံစွာ သိမ်းဆည်းပါ၊ destructive operations အတွက် approval နဲ့ audit logging ထည့်သွင်းစဉ်းစားပါ၊ test environment မှာ စမ်းသပ်ပြီးမှ production မှာ အသုံးပြုပါ။

## 12. Licensing

BCM ဟာ cluster-management software ဖြစ်ပြီး license entitlement, activation နဲ့ support terms တွေကို စစ်ဆေးဖို့ လိုအပ်ပါတယ်။ သို့သော် BCM ကို သုံးဖို့ အမြဲတမ်း paid enterprise license ဝယ်ရမယ်ဆိုတာ မမှန်ပါဘူး။ NVIDIA ရဲ့ licensing documentation အရ အဓိကရွေးချယ်စရာတွေ —

| License / Offering | အဓိကအချက် |
|---|---|
| Free-to-use BCM | Eligible internal-use deployments အတွက် အခမဲ့ license option |
| BCM with standalone support | BCM license နဲ့ NVIDIA Enterprise Support |
| NVIDIA AI Enterprise | BCM နဲ့အတူ အခြား AI software capabilities နဲ့ support |
| NVIDIA Mission Control | BCM အပါအဝင် ပိုမိုကျယ်ပြန့်တဲ့ AI factory operations capabilities |

Official licensing page: https://docs.nvidia.com/base-command-manager/bcm-licensing-and-support/

**Free-to-use License** (NVIDIA documentation အရ) — system တစ်ခုလျှင် accelerator 8 ခုအထိ ခွင့်ပြုပါတယ်။ Cluster ထဲက server/node အရေအတွက်ကို သီးခြားအများဆုံးကန့်သတ်ချက် မထားပါဘူး။ GPU-accelerated နဲ့ CPU-based clusters နှစ်မျိုးစလုံးအတွက် အသုံးပြုနိုင်ပါတယ်။ NVIDIA Enterprise Support, SLA နဲ့ email/phone support မပါဝင်ပါဘူး။ Free license ရှိတယ်ဆိုတာ license မလိုဘူးလို့ မဆိုလိုပါဘူး — entitlement နဲ့ activation လုပ်ငန်းစဉ်ကို လိုက်နာဖို့ လိုအပ်နိုင်ပါတယ်။

Production အတွက် စီစဉ်ရင် ဆုံးဖြတ်စရာများ — cluster အရွယ်အစား၊ system တစ်ခုချင်းစီမှာ accelerator အရေအတွက်၊ Enterprise Support လိုအပ်မှု၊ AI Enterprise/Mission Control ရဲ့ အပို capabilities လိုအပ်မှု၊ organization ရဲ့ license eligibility နဲ့ renewal terms။ လက်ရှိ terms တွေ ပြောင်းလဲနိုင်တဲ့အတွက် purchase/deployment မလုပ်ခင် official licensing page ကို ပြန်စစ်ဆေးပါ။

## 13. BCM Installation နဲ့ Deployment

BCM ဟာ simple CLI utility တစ်ခုမဟုတ်ပါဘူး။ Cluster-management platform ဖြစ်တဲ့အတွက် deployment planning လိုအပ်ပါတယ်။ High-level deployment workflow —

```
1. Review Requirements
          |
          v
2. Select BCM Version and License
          |
          v
3. Prepare Management Node
          |
          v
4. Install BCM
          |
          v
5. Configure Network and Cluster
          |
          v
6. Add / Provision Managed Nodes
          |
          v
7. Configure Storage and Workload Integration
          |
          v
8. Validate Monitoring and Management
          |
          v
9. Operate and Maintain the Cluster
```

Deployment မစမီ စစ်ဆေးရမည့်အချက်များ — BCM version နဲ့ hardware compatibility၊ management network design၊ node provisioning network requirements၊ DNS/NTP နဲ့ required network services၊ management node sizing နဲ့ availability၊ license entitlement နဲ့ activation၊ OS နဲ့ driver compatibility၊ storage နဲ့ network integrations၊ Slurm/Kubernetes လို workload-management requirements၊ backup, access control နဲ့ upgrade plan။

Official documentation: https://docs.nvidia.com/base-command-manager/

ဒီသင်ခန်းစာမှာ deployment ကို conceptual level မှာ ရှင်းပြထားပါတယ်။ တကယ့် installation အတွက် BCM release နဲ့ ကိုက်ညီတဲ့ Installation Manual ကို အခြေခံရပါမယ်။

## 14. Monitoring Integration — Prometheus နဲ့ Grafana

BCM ကို Prometheus နဲ့ Grafana တို့နဲ့ ပေါင်းစပ်အသုံးပြုနိုင်ပါတယ်။ Integration အတိအကျက BCM version နဲ့ configured metrics forwarders/exporters တွေပေါ်မူတည်ပါတယ်။

```
 GPU Servers         CPU Nodes       Network / Storage
     |                   |                   |
     v                   v                   v
   DCGM              Node Metrics       Device Metrics
     |                   |                   |
     +-------------------+-------------------+
                         |
                         v
                Metrics Export / Forward
                         |
                         v
                    Prometheus
                         |
                         v
                      Grafana
                         |
                         v
                 Dashboards / Alerts

          BCM manages cluster infrastructure
          and related management workflows
```

ဒီ architecture မှာ — DCGM က GPU health နဲ့ telemetry ကို ပေးပါတယ်။ Exporters/metrics forwarders တွေက metrics ကို monitoring platform ဆီ ပို့ပေးပါတယ်။ Prometheus က scrape ပြီး time-series data အဖြစ် သိမ်းဆည်းပါတယ်။ Grafana က dashboards နဲ့ visualizations ကို ဖော်ပြပါတယ်။ BCM က cluster provisioning, inventory နဲ့ infrastructure management ကို ကိုင်တွယ်ပါတယ်။

သတိပြုရန် — BCM, Prometheus နဲ့ Grafana တို့ရဲ့ integration ကို အလိုအလျောက် အပြည့်အဝ configured ဖြစ်နေတယ်လို့ မယူဆသင့်ပါဘူး။ လိုအပ်တဲ့ exporters, endpoints, permissions နဲ့ scrape configuration တွေကို သက်ဆိုင်ရာ documentation အတိုင်း ပြင်ဆင်ရပါတယ်။

## 15. BCM ဘာတွေ လုပ်နိုင်၊ ဘာတွေ မလုပ်နိုင်ဘူး

| Area | BCM ရဲ့ အခန်းကဏ္ဍ |
|---|---|
| Cluster provisioning | Managed cluster nodes တွေကို deploy/provision လုပ်ခြင်း |
| Configuration management | Supported node configuration နဲ့ software management |
| Infrastructure monitoring | Cluster inventory နဲ့ configured monitoring data ကို ကြည့်ရှုစီမံခြင်း |
| GPU management | GPU infrastructure ကို support လုပ်တဲ့ management workflows တွေနဲ့ ပေါင်းစပ်ခြင်း |
| Network management | Supported network hardware နဲ့ configured integrations တွေကို စီမံခြင်း |
| Storage | Supported storage systems နဲ့ integrations တွေကို စီမံခြင်း |
| Workload integration | Slurm, Kubernetes, Run:ai လို ecosystem တွေနဲ့ ချိတ်ဆက်ခြင်း |
| Job scheduling | Scheduler integrations ကို အသုံးပြုခြင်း; BCM ကို universal scheduler အဖြစ် မယူဆရ |
| Historical metrics | Monitoring backend နဲ့ configuration ပေါ်မူတည် |
| Hardware management | Supported hardware နဲ့ firmware/configuration capabilities ပေါ်မူတည် |

## 16. Practical Scenario — AI Training Cluster တစ်ခုကို စီမံခန့်ခွဲခြင်း

အောက်ပါ environment ကို စဉ်းစားကြည့်ပါ — GPU server 8 လုံး၊ server တစ်လုံးစီမှာ NVIDIA GPU 8 လုံး၊ high-speed InfiniBand fabric၊ shared Lustre storage၊ Slurm scheduler၊ DCGM Exporter/Prometheus/Grafana monitoring stack။

**BCM မရှိဘဲ** — Administrator က provisioning, node configuration, inventory, workload integration နဲ့ monitoring configuration တွေကို သီးခြား tools တွေနဲ့ စီမံရနိုင်ပါတယ်။

**BCM ကို အသုံးပြုတဲ့အခါ** — managed nodes တွေကို inventory နဲ့ provisioning workflow ထဲမှာ စီမံနိုင်ပါတယ်။ Cluster configuration ကို ပိုမိုစနစ်တကျ ထိန်းသိမ်းနိုင်ပါတယ်။ Supported infrastructure monitoring information ကို cluster context နဲ့ ကြည့်နိုင်ပါတယ်။ Slurm နဲ့ ချိတ်ဆက်ပြီး workload-management environment ကို စီမံနိုင်ပါတယ်။ DCGM Exporter နဲ့ Prometheus/Grafana ကို GPU metrics အတွက် ဆက်လက်အသုံးပြုနိုင်ပါတယ်။

ဒီလိုနဲ့ BCM က infrastructure management ကို စုစည်းပေးပြီး workload scheduler နဲ့ monitoring stack တွေက သက်ဆိုင်ရာ function တွေကို ဆက်လက်လုပ်ဆောင်ပါတယ်။

## 17. Common Misconceptions

| မှားယွင်းနိုင်တဲ့ အယူအဆ | မှန်ကန်တဲ့ နားလည်မှု |
|---|---|
| BCM က monitoring dashboard တစ်ခုပဲ။ | BCM မှာ provisioning, configuration management နဲ့ workload-management integrations တွေလည်း ပါဝင်ပါတယ်။ |
| BCM က DCGM ကို အစားထိုးတယ်။ | DCGM က GPU-specific telemetry, health နဲ့ diagnostics အတွက် အရေးကြီးပြီး BCM က cluster-management scope ကို ပိုမိုကျယ်ပြန့်စေပါတယ်။ |
| BCM က အမြဲတမ်း paid enterprise license လိုတယ်။ | Eligible internal-use deployments အတွက် free-to-use license ရှိပါတယ် — ဒါပေမဲ့ license terms, accelerator limits နဲ့ support exclusions တွေကို လိုက်နာရပါတယ်။ |
| BCM က Slurm သို့မဟုတ် Kubernetes နဲ့ တူတယ်။ | BCM က Slurm, Kubernetes နဲ့ Run:ai တွေနဲ့ ပေါင်းစပ်အသုံးပြုနိုင်ပေမဲ့ product တစ်ခုချင်းစီရဲ့ role နဲ့ scope မတူညီပါဘူး။ |
| BCM က device တိုင်းက metrics အားလုံးကို အလိုအလျောက် စုဆောင်းတယ်။ | အမြဲတမ်း မမှန်ပါဘူး — metrics coverage က hardware support, configured exporters, integrations နဲ့ software versions ပေါ် မူတည်ပါတယ်။ |

## 18. Review Questions

1. BCM ရဲ့ အမည်အပြည့်အစုံက ဘာလဲ။
2. BCM နဲ့ DCGM တို့ရဲ့ အဓိကကွာခြားချက်က ဘာလဲ။
3. BCM Head Node ရဲ့ အခန်းကဏ္ဍက ဘာလဲ။
4. Base View UI နဲ့ cmsh တို့က ဘာအတွက် အသုံးပြုသလဲ။
5. Cluster provisioning ဆိုတာ ဘာလဲ။
6. BCM က Slurm နဲ့ Kubernetes တို့နဲ့ ဘာကြောင့် integration လုပ်သလဲ။
7. BCM မှာ GPU monitoring data ရရှိနိုင်မှုက ဘာတွေပေါ်မူတည်သလဲ။
8. BCM နဲ့ Prometheus/Grafana တို့ရဲ့ အခန်းကဏ္ဍတွေ ဘယ်လိုကွာခြားသလဲ။
9. BCM free-to-use license မှာ accelerator limit နဲ့ support terms ဘယ်လိုရှိသလဲ။
10. BCM က workload scheduling ကို အပြည့်အဝ အစားထိုးပေးတယ်လို့ ပြောနိုင်သလား။
11. GPU utilization နည်းနေတယ်ဆိုရင် ဘယ် bottlenecks တွေကို ထပ်မံစစ်ဆေးသင့်သလဲ။
12. Production deployment မလုပ်မီ ဘယ် licensing နဲ့ infrastructure requirements တွေကို စစ်ဆေးသင့်သလဲ။

## 19. Module Summary

- NVIDIA Base Command Manager ဟာ AI နဲ့ HPC infrastructure တွေအတွက် cluster provisioning, configuration management, infrastructure monitoring နဲ့ workload-management integrations တွေကို ပံ့ပိုးပေးတဲ့ platform ဖြစ်ပါတယ်။
- NVIDIA-SMI က GPU ကို အမြန်စစ်ဆေးဖို့ အသုံးဝင်ပြီး DCGM က GPU telemetry, health နဲ့ diagnostics ကို ကိုင်တွယ်ပါတယ်။ BCM ကတော့ ဒီ GPU-level operations တွေကို cluster provisioning, managed nodes နဲ့ workload-management ecosystem တို့နဲ့ ပေါင်းစပ်အသုံးပြုနိုင်အောင် ကူညီပေးပါတယ်။
- BCM ကို Slurm, Kubernetes, NVIDIA Run:ai, DCGM Exporter, Prometheus နဲ့ Grafana တို့နဲ့ architecture လိုအပ်ချက်အလိုက် တွဲဖက်အသုံးပြုနိုင်ပါတယ်။

Key takeaway — BCM ကို AI data center ရဲ့ cluster-management platform အဖြစ် နားလည်ပါ။ ၎င်းဟာ monitoring တစ်ခုတည်းမဟုတ်သလို GPU driver, DCGM, Prometheus, Grafana သို့မဟုတ် workload scheduler တို့ကို အားလုံးအစားထိုးပေးတဲ့ tool တစ်ခုလည်း မဟုတ်ပါဘူး။

## 20. Self-check Questions and Answers

1. **မေး** — BCM Head Node နဲ့ compute node ဘာကွာသလဲ။  
   **ဖြေ** — Head Node က cluster management (services, configuration database, APIs, provisioning) ကို လုပ်ဆောင်ပြီး workload တွေကို ကိုယ်တိုင် run ဖို့ ရည်ရွယ်ထားတဲ့ compute node မဟုတ်ပါဘူး။
2. **မေး** — Base View နဲ့ cmsh က ဘာတွေလဲ။  
   **ဖြေ** — Base View က BCM ရဲ့ web-based management UI ဖြစ်ပြီး cmsh က Cluster Management Shell (CLI) ဖြစ်ပါတယ်။
3. **မေး** — Base View UI ရဲ့ utilization graph တစ်ခုတည်းကို ကြည့်ပြီး GPU performance ကောင်းမကောင်း ဆုံးဖြတ်လို့ရသလား။  
   **ဖြေ** — မရပါဘူး။ Workload metrics, hardware health, network performance နဲ့ application-level telemetry တွေကိုပါ ထည့်သွင်းစဉ်းစားရပါမယ်။
4. **မေး** — BCM က Slurm ကို အစားထိုးနိုင်သလား။  
   **ဖြေ** — မထိုးနိုင်ပါဘူး။ BCM က workload-management systems တွေနဲ့ integrate လုပ်ပေးပြီး job scheduling က Slurm ရဲ့ တာဝန်ဖြစ်ပါတယ်။
5. **မေး** — BCM က DCGM metrics အားလုံးကို အလိုအလျောက် စုဆောင်းပေးသလား။  
   **ဖြေ** — အမြဲတမ်း မဟုတ်ပါဘူး။ ရရှိနိုင်တဲ့ metrics တွေက configured exporters, monitoring forwarders, integrations နဲ့ platform version ပေါ် မူတည်ပါတယ်။

## English Version

## 1. Learning Objectives

This module explains how NVIDIA Base Command Manager (BCM) works as a cluster-management platform for AI and HPC clusters.

By the end of this module you should be able to:

- State BCM's full name and main role.
- Describe the BCM conceptual architecture — Administrator, Base View UI, cmsh, Head Node, and managed nodes.
- Explain the difference between the BCM Head Node and a compute node (head node ≠ compute node).
- List Base View UI features and explain why one UI graph alone does not fully assess GPU performance.
- Explain what cluster provisioning is.
- Describe how BCM relates to Slurm, Kubernetes, and NVIDIA Run:ai.
- Distinguish BCM from NVIDIA-SMI and DCGM.

## 2. What Is BCM?

BCM = NVIDIA Base Command Manager — NVIDIA's cluster management software/platform for AI and high-performance computing infrastructure.

- NVIDIA-SMI focuses on GPU-device information.
- DCGM focuses on GPU management and telemetry.
- BCM addresses broader cluster lifecycle management: cluster/node provisioning, infrastructure configuration and administration, cluster health and utilization monitoring, operational dashboards, integration with workload managers such as Slurm, and management of heterogeneous clusters.

BCM is not just a monitoring dashboard, nor a command equivalent to nvidia-smi. It is a cluster management platform.

## 3. BCM vs NVIDIA-SMI / DCGM

| Capability | NVIDIA-SMI | DCGM | Base Command Manager |
|---|---|---|---|
| Primary focus | GPU query and management | GPU telemetry, health, diagnostics, management | End-to-end cluster management |
| Server provisioning | No | No | Yes |
| Cluster configuration | No | Limited to GPU management scope | Yes |
| Monitoring | Local GPU status | GPU telemetry/health/diagnostics | Cluster inventory and configured monitoring data |
| Job scheduling | No | No | Scheduler integrations only (BCM itself is not a scheduler) |

Key takeaway: understand BCM as the AI data center's cluster-management platform. It is not just monitoring, and it does not replace the GPU driver, DCGM, Prometheus, Grafana, or the workload scheduler.

## 4. BCM Conceptual Architecture

```
Administrator
                        |
             +----------+----------+
             |                     |
             v                     v
         Base View UI           cmsh CLI
             |                     |
             +----------+----------+
                        |
                        v
                 BCM Head Node
          +--------------------------+
          | Cluster Management      |
          | Configuration Database  |
          | Management Services     |
          | APIs / Web Interface    |
          +--------------------------+
                        |
          +-------------+-------------+
          |             |             |
          v             v             v
      GPU Nodes     CPU Nodes    Managed Network /
      + DCGM        + Services   Storage Resources
          |
          v
     GPU / CPU / Storage / Network
          |
          v
    Slurm / Kubernetes / Run:ai
        Workload Ecosystem
```

This diagram is a conceptual architecture. In real deployments, components, integrations, and management interfaces can differ depending on the BCM version and system design.

### 4.1 BCM Head Node

The BCM Head Node is the center of cluster management. Depending on the deployment it can contain:

- Cluster management services.
- Cluster configuration and inventory data.
- Web-based management interface.
- Command-line management interface.
- APIs and integration endpoints.
- Provisioning and configuration-management functions.

**The Head Node is not the same as a compute node intended to run workloads itself.** It is the system that mainly performs cluster management. In production, the Head Node's availability, backup, network security, and access control must be managed carefully.

### 4.2 Managed Nodes

Managed nodes are the compute systems BCM manages. Examples:

- NVIDIA DGX systems.
- GPU-accelerated servers.
- CPU-only compute nodes.
- Other supported heterogeneous compute nodes.

For each node, hardware inventory, operating system, configuration, and operational status can be managed by the cluster management platform.

### 4.3 Management Interfaces

BCM's main interfaces are:

- **Base View** — the web-based management interface.
- **cmsh** — the Cluster Management Shell.
- **REST API** — for programmatic integration with other systems.

These interfaces let you use the same cluster management environment in different ways.

## 5. Base View — the Web-Based Management UI

Base View is BCM's web-based management interface. Through it, cluster administrators can use supported management functions with a graphical interface.

Depending on version and configuration, the UI can show categories such as:

- Cluster inventory.
- Compute nodes and device status.
- Resource utilization.
- Node health and monitoring information.
- Networking configuration.
- Provisioning and node management.
- Device grouping.
- Workload-management information.
- Licensing information.
- Cluster configuration.

Why a web UI is useful: you can find cluster inventory and management information easily without SSH-ing into every server to run commands. For example, an administrator can look at GPU node statuses and decide which nodes need troubleshooting.

**Caution: do not fully judge GPU performance from a single resource-utilization graph in the UI.** Workload metrics, hardware health, network performance, and application-level telemetry must also be taken into account.

## 6. Cluster Provisioning

Provisioning means preparing compute nodes into a usable state and managing them. BCM's provisioning capabilities help build a cluster and prepare nodes with standardized configuration:

- Registering server hardware into the cluster.
- Preparing network and boot/provisioning configuration.
- Applying supported operating system images.
- Defining node configuration.
- Preparing required software and management services.
- Making nodes ready for cluster operation.

Why provisioning matters: manually configuring 20 GPU servers can leave every node different (OS version, network interface configuration, driver version, shared storage configuration, required packages). Provisioning and centralized configuration management help reduce configuration drift.

However, provisioning does not mean one piece of software automatically solves physical server installation, cabling, power delivery, and data center cooling.

## 7. Infrastructure Monitoring

One of BCM's key roles is monitoring cluster infrastructure. The monitoring scope varies with deployment, hardware support, and integrations.

### 7.1 CPU Monitoring

For CPU-based compute nodes: CPU utilization, CPU/system-related health information, node status, memory utilization, and OS/hardware inventory can be available. Availability of sensor metrics such as CPU temperature depends on hardware sensors, drivers, and platform integration. BCM being able to monitor CPU utilization does not mean it can universally change every CPU frequency, voltage, or hardware setting.

### 7.2 GPU Monitoring

For GPU-enabled nodes, GPU monitoring can be combined with DCGM and related monitoring integrations. Useful metrics include GPU utilization, GPU memory utilization, GPU temperature, GPU power consumption, GPU health and supported error information, and GPU performance-related metrics.

BCM's monitoring environment can help view and manage this data in a cluster context. However, do not always assume "BCM automatically collects every DCGM metric" — available metrics depend on configured exporters, monitoring forwarders, integrations, and platform version.

### 7.3 Network Monitoring

Network performance matters in AI/HPC infrastructure — especially InfiniBand, high-speed Ethernet, RoCE-based fabrics, supported NVIDIA networking hardware, and BlueField DPU platforms. Network monitoring can use link status, throughput, errors, and supported device telemetry. But BCM's network visibility does not automatically mean identical telemetry from every network switch, NIC, and DPU.

### 7.4 Storage Monitoring

In AI training workloads, storage throughput and latency can affect GPU utilization (NFS shared storage, Lustre parallel file systems, NVMe-based storage, and more). Storage monitoring can include capacity, utilization, health, and performance-related data; available metrics depend on the storage platform and integration. Do not treat BCM as an automatic replacement for every storage monitoring system.

## 8. Workload Management and Orchestration

One important BCM capability is connecting infrastructure resources with workload-management systems. GPU utilization alone is not enough to manage AI infrastructure efficiently.

### 8.1 Slurm

Slurm is a widely used workload manager and scheduler for HPC workloads — job queue management, scheduling on available resources, CPU/GPU resource allocation, and job state/execution management. BCM can integrate with a workload manager like Slurm to connect cluster provisioning and infrastructure management with workload execution.

### 8.2 Kubernetes

Kubernetes is an orchestration platform for deploying, scheduling, and managing containerized applications. BCM can support Kubernetes cluster provisioning and management workflows, but BCM and Kubernetes are not the same product.

### 8.3 NVIDIA Run:ai

NVIDIA Run:ai is a platform aimed at AI workload orchestration and more efficient GPU resource utilization; BCM can integrate with Run:ai. However, Run:ai and BCM capabilities are not fully identical — licensing, product version, and integration design determine which product owns which function.

Important distinction: BCM supports workload-management integrations but does not replace all functions of Slurm, Kubernetes, and Run:ai.

## 9. Resource Allocation, Job Status, and Performance

Depending on the workload-management systems connected to BCM, administrators can view workload status and resource usage in cluster context: job ID, job status, assigned nodes, allocated GPUs, CPU allocation, runtime, resource utilization, and workload performance information.

When GPU utilization of a training job is low, possible causes include slow data loading, CPU preprocessing bottlenecks, insufficient storage throughput, network communication overhead, a batch size that is too small, or the job waiting on another operation. So do not immediately conclude a GPU resource is wasted from utilization alone.

## 10. User Management and Resource Quotas

In multi-user AI/HPC environments you must manage how each user may use GPU resources: user and group management, authentication and authorization, resource allocation, resource quotas, and job access/workload permissions. However, quota enforcement varies with BCM configuration and integration; Slurm, Kubernetes, or Run:ai may directly own GPU quotas and scheduling policies.

Key point: infrastructure management and workload-level resource scheduling are related but not the same function.

## 11. REST API and Automation

Beyond the web interface and CLI, BCM has APIs for programmatic integration: integration with external monitoring platforms, using inventory data from automation scripts, automating approved configuration workflows, and connecting cluster-management workflows to other enterprise systems.

API endpoints, authentication, permissions, and available operations depend on BCM version and configuration. For API automation in production: check the API documentation first, use least-privilege access, store credentials and tokens securely, consider approval and audit logging for destructive operations, and test in a test environment before production use.

## 12. Licensing

BCM is cluster-management software and license entitlement, activation, and support terms must be checked. However, it is not true that BCM always requires a paid enterprise license. Per NVIDIA's licensing documentation, main options include:

| License / Offering | Key point |
|---|---|
| Free-to-use BCM | Free license option for eligible internal-use deployments |
| BCM with standalone support | BCM license plus NVIDIA Enterprise Support |
| NVIDIA AI Enterprise | BCM together with additional AI software capabilities and support |
| NVIDIA Mission Control | Broader AI factory operations capabilities including BCM |

Official licensing page: https://docs.nvidia.com/base-command-manager/bcm-licensing-and-support/

**Free-to-use license** (per NVIDIA documentation): allows up to 8 accelerators per system; there is no separate maximum limit on the number of servers/nodes in the cluster; it can be used for both GPU-accelerated and CPU-based clusters; it does not include NVIDIA Enterprise Support, SLAs, or email/phone support. A free license existing does not mean no license is needed — entitlement and activation procedures may still apply.

Decisions for production: cluster size, accelerators per system, need for Enterprise Support, need for AI Enterprise/Mission Control extras, and your organization's license eligibility and renewal terms. Terms can change — re-check the official licensing page before purchasing or deploying.

## 13. BCM Installation and Deployment

BCM is not a simple CLI utility. Being a cluster-management platform, it needs deployment planning. High-level deployment workflow:

```
1. Review Requirements
          |
          v
2. Select BCM Version and License
          |
          v
3. Prepare Management Node
          |
          v
4. Install BCM
          |
          v
5. Configure Network and Cluster
          |
          v
6. Add / Provision Managed Nodes
          |
          v
7. Configure Storage and Workload Integration
          |
          v
8. Validate Monitoring and Management
          |
          v
9. Operate and Maintain the Cluster
```

Check before deployment: BCM version and hardware compatibility; management network design; node provisioning network requirements; DNS, NTP, and required network services; management node sizing and availability; license entitlement and activation; OS and driver compatibility; storage and network integrations; workload-management requirements such as Slurm/Kubernetes; backup, access control, and upgrade plans.

Official documentation: https://docs.nvidia.com/base-command-manager/

This lesson explains deployment at a conceptual level. Actual installation must follow the Installation Manual matching your BCM release.

## 14. Monitoring Integration — Prometheus and Grafana

BCM can be combined with Prometheus and Grafana. The exact integration depends on the BCM version and configured metrics forwarders/exporters.

```
 GPU Servers         CPU Nodes       Network / Storage
     |                   |                   |
     v                   v                   v
   DCGM              Node Metrics       Device Metrics
     |                   |                   |
     +-------------------+-------------------+
                         |
                         v
                Metrics Export / Forward
                         |
                         v
                    Prometheus
                         |
                         v
                      Grafana
                         |
                         v
                 Dashboards / Alerts

          BCM manages cluster infrastructure
          and related management workflows
```

In this architecture: DCGM provides GPU health and telemetry; exporters/forwarders send metrics to the monitoring platform; Prometheus scrapes and stores time-series data; Grafana shows dashboards and visualizations; BCM handles cluster provisioning, inventory, and infrastructure management.

Note: do not assume the BCM/Prometheus/Grafana integration is automatically and fully configured. Required exporters, endpoints, permissions, and scrape configuration must be set up per the relevant documentation.

## 15. What BCM Can and Cannot Do

| Area | BCM's role |
|---|---|
| Cluster provisioning | Deploy/provision managed cluster nodes |
| Configuration management | Supported node configuration and software management |
| Infrastructure monitoring | View and manage cluster inventory and configured monitoring data |
| GPU management | Integrate with GPU-infrastructure management workflows |
| Network management | Manage supported network hardware and configured integrations |
| Storage | Manage supported storage systems and integrations |
| Workload integration | Connect with ecosystems like Slurm, Kubernetes, Run:ai |
| Job scheduling | Uses scheduler integrations; do not treat BCM as a universal scheduler |
| Historical metrics | Depends on the monitoring backend and configuration |
| Hardware management | Depends on supported hardware and firmware/configuration capabilities |

## 16. Practical Scenario — Managing an AI Training Cluster

Consider this environment: 8 GPU servers, each with 8 NVIDIA GPUs, a high-speed InfiniBand fabric, shared Lustre storage, a Slurm scheduler, and a DCGM Exporter/Prometheus/Grafana monitoring stack.

**Without BCM** — the administrator can manage provisioning, node configuration, inventory, workload integration, and monitoring configuration with separate tools.

**With BCM** — managed nodes can be handled in inventory and provisioning workflows; cluster configuration can be kept more systematically; supported infrastructure monitoring information can be viewed in cluster context; the workload-management environment can be managed with the Slurm integration; and DCGM Exporter with Prometheus/Grafana continues to serve GPU metrics.

This way BCM consolidates infrastructure management while the workload scheduler and monitoring stack keep their own functions.

## 17. Common Misconceptions

| Misconception | Correct understanding |
|---|---|
| BCM is just a monitoring dashboard. | BCM also includes provisioning, configuration management, and workload-management integrations. |
| BCM replaces DCGM. | DCGM is essential for GPU-specific telemetry, health, and diagnostics; BCM broadens the cluster-management scope. |
| BCM always requires a paid enterprise license. | A free-to-use license exists for eligible internal-use deployments — but license terms, accelerator limits, and support exclusions apply. |
| BCM is the same as Slurm or Kubernetes. | BCM can integrate with Slurm, Kubernetes, and Run:ai, but each product has a different role and scope. |
| BCM automatically collects every metric from every device. | Not always — metric coverage depends on hardware support, configured exporters, integrations, and software versions. |

## 18. Review Questions

1. What is BCM's full name?
2. What is the main difference between BCM and DCGM?
3. What is the role of the BCM Head Node?
4. What are Base View UI and cmsh used for?
5. What is cluster provisioning?
6. Why does BCM integrate with Slurm and Kubernetes?
7. What does GPU monitoring data availability in BCM depend on?
8. How do the roles of BCM and Prometheus/Grafana differ?
9. What accelerator limit and support terms apply to the BCM free-to-use license?
10. Can BCM fully replace workload scheduling?
11. If GPU utilization is low, what other bottlenecks should be checked?
12. What licensing and infrastructure requirements should be checked before production deployment?

## 19. Module Summary

- NVIDIA Base Command Manager is a platform supporting cluster provisioning, configuration management, infrastructure monitoring, and workload-management integrations for AI and HPC infrastructure.
- NVIDIA-SMI is useful for quick GPU checks and DCGM handles GPU telemetry, health, and diagnostics. BCM helps combine these GPU-level operations with cluster provisioning, managed nodes, and the workload-management ecosystem.
- BCM can be used together with Slurm, Kubernetes, NVIDIA Run:ai, DCGM Exporter, Prometheus, and Grafana as the architecture requires.

Key takeaway: understand BCM as the AI data center's cluster-management platform. It is not just monitoring, and it does not replace the GPU driver, DCGM, Prometheus, Grafana, or the workload scheduler.

## 20. Self-check Questions and Answers

1. **Q** — How does the BCM Head Node differ from a compute node?  
   **A** — The Head Node performs cluster management (services, configuration database, APIs, provisioning) and is not meant to run workloads itself like a compute node.
2. **Q** — What are Base View and cmsh?  
   **A** — Base View is BCM's web-based management UI; cmsh is the Cluster Management Shell (CLI).
3. **Q** — Can you fully judge GPU performance from one Base View utilization graph?  
   **A** — No. Workload metrics, hardware health, network performance, and application-level telemetry must also be considered.
4. **Q** — Can BCM replace Slurm?  
   **A** — No. BCM integrates with workload-management systems; job scheduling is Slurm's responsibility.
5. **Q** — Does BCM automatically collect every DCGM metric?  
   **A** — Not always. Available metrics depend on configured exporters, monitoring forwarders, integrations, and platform version.

## Technical review notes

- The supplied source (Burmese study notes) begins mid-module at the conceptual-architecture section; sections 1–3 of this file were framed strictly from the same author's supplied three-tool comparison and module summary so that the file stands alone. The English version is a faithful rendering of the Burmese content.
- Source caveats preserved: head node ≠ compute node; a Base View utilization graph alone does not fully assess GPU performance; provisioning does not automate physical installation/cabling/power/cooling; BCM does not automatically collect every DCGM/device metric; BCM is not a universal scheduler (monitoring ≠ scheduling); BCM does not replace Slurm/Kubernetes/Run:ai functions.
- Licensing figures (free-to-use: up to 8 accelerators per system, no separate node-count cap, no enterprise support) are quoted from the supplied notes and NVIDIA's licensing page — needs verification against https://docs.nvidia.com/base-command-manager/bcm-licensing-and-support/ before any purchase decision, since terms change.
- Deployment workflow, monitoring-integration diagram, and the 8-server × 8-GPU scenario are conceptual/illustrative, per the source.
