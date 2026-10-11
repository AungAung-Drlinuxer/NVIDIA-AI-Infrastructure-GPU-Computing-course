# Module 25 — NVIDIA-SMI: GPU Monitoring and Basic Management

> Part V · NVIDIA Monitoring, Cluster Management and Tool Selection — topic notes

## မြန်မာဘာသာ version

## 1. သင်ခန်းစာ၏ ရည်ရွယ်ချက် (Learning Objectives)

ဒီ Module မှာ NVIDIA GPU တစ်လုံးရဲ့ လက်ရှိအခြေအနေကို nvidia-smi command အသုံးပြုပြီး ဘယ်လိုစစ်ဆေးရမလဲဆိုတာ လေ့လာသွားပါမယ်။

သင်ခန်းစာပြီးဆုံးတဲ့အခါ အောက်ပါအချက်တွေကို နားလည်နိုင်ရပါမယ်။

- nvidia-smi ဆိုတာ ဘာလဲ၊ ဘာအတွက်အသုံးပြုသလဲ။
- GPU utilization, GPU memory, temperature, power consumption နဲ့ clock speed တွေကို ဘယ်လိုဖတ်ရမလဲ။
- GPU ကိုအသုံးပြုနေတဲ့ process နဲ့ PID ကို ဘယ်လိုရှာရမလဲ။
- nvidia-smi နဲ့ nvidia-smi -l, nvidia-smi dmon ကို ဘယ်လိုအသုံးပြုရမလဲ။
- GPU driver နဲ့ CUDA Toolkit version တို့ရဲ့ ကွာခြားချက်ကို ဘယ်လိုနားလည်ရမလဲ။
- nvidia-smi တစ်ခုတည်းနဲ့ Data Center တစ်ခုလုံးကို ဘာကြောင့် မစီမံခန့်ခွဲနိုင်သလဲ။

## 2. NVIDIA-SMI ဆိုတာဘာလဲ။

NVIDIA-SMI = NVIDIA System Management Interface

nvidia-smi ဟာ NVIDIA GPU တွေရဲ့ status, utilization, memory, temperature, power consumption နဲ့ GPU ကိုအသုံးပြုနေတဲ့ process တွေကို စစ်ဆေးဖို့ အသုံးပြုတဲ့ command-line utility ဖြစ်ပါတယ်။

ပုံမှန်အားဖြင့် NVIDIA GPU driver ကို install လုပ်တဲ့အခါ nvidia-smi utility ကိုလည်း ထည့်သွင်းပေးထားပါတယ်။ သို့သော် driver package နဲ့ installation method ပေါ်မူတည်ပြီး ရရှိနိုင်မှု ကွာခြားနိုင်ပါတယ်။

Official documentation: https://docs.nvidia.com/deploy/nvidia-smi/index.html

ဘယ်လိုအလုပ်လုပ်သလဲ။ Conceptual architecture ကို အောက်ပါအတိုင်း နားလည်နိုင်ပါတယ်။

```
Administrator / Engineer
          |
          v
      nvidia-smi
          |
          v
  NVIDIA Management Library
         (NVML)
          |
          v
    NVIDIA GPU Driver
          |
          v
      NVIDIA GPU
          |
          v
 Utilization / Memory /
 Temperature / Power /
 Processes / Clock Status
```

nvidia-smi ဟာ GPU နဲ့ပတ်သက်တဲ့ information တွေကို query လုပ်ဖို့ NVIDIA Management Library (NVML) ကို အသုံးပြုနိုင်ပါတယ်။ အချို့ management operations တွေကိုလည်း supported hardware နဲ့ driver ပေါ်မူတည်ပြီး လုပ်ဆောင်နိုင်ပါတယ်။

nvidia-smi ကို ဘယ်အချိန်မှာ အသုံးပြုသင့်သလဲ။

- GPU driver install လုပ်ပြီးနောက် GPU ကို detect ဖြစ်၊ မဖြစ် စစ်ဆေးခြင်း။
- Machine Learning training job တစ်ခု GPU ပေါ်မှာ အလုပ်လုပ်နေ၊ မလုပ်နေ စစ်ဆေးခြင်း။
- GPU memory မလုံလောက်တဲ့ ပြဿနာကို စုံစမ်းခြင်း။
- GPU temperature နဲ့ power usage ကို ကြည့်ခြင်း။
- GPU ကို အသုံးပြုနေတဲ့ process ကို ရှာဖွေခြင်း။
- GPU performance troubleshooting စတင်လုပ်ဆောင်ခြင်း။

အဓိကမှတ်ရန် — nvidia-smi ဟာ GPU troubleshooting စတင်ဖို့ အလွန်အသုံးဝင်ပေမဲ့ Enterprise Data Center monitoring platform တစ်ခုလုံးရဲ့ အစားထိုးပစ္စည်း မဟုတ်ပါဘူး။

## 3. ပထမဆုံး GPU Status ကို စစ်ဆေးခြင်း

Terminal မှာ အောက်ပါ command ကို run ပါ။

```
nvidia-smi
```

GPU driver က မှန်ကန်စွာ အလုပ်လုပ်နေပြီး NVIDIA GPU ကို detect လုပ်ထားပါက GPU status table တစ်ခုကို ပြသပါလိမ့်မယ်။ ဥပမာ output ကို အောက်ပါအတိုင်း စိတ်ကူးကြည့်နိုင်ပါတယ်။

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 580.95.05    Driver Version: 580.95.05    CUDA Version: 13.0     |
+-----------------------------------------------------------------------------+
| GPU  Name       Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
|  0   Tesla T4   Off           | 00000000:00:1E.0 Off |                    0 |
+-----------------------------------------------------------------------------+
| Fan  Temp  Perf  Pwr:Usage/Cap | Memory-Usage | GPU-Util | Compute M.       |
| N/A   43C   P0    68W / 70W    | 471MiB / 15GiB | 97%   | Default          |
+-----------------------------------------------------------------------------+
| Processes:                                                                |
| GPU   PID      Type   Process name                         GPU Memory      |
|  0    465464   C      python3                                414MiB       |
+-----------------------------------------------------------------------------+
```

သတိပြုရန် — ဒီ table က သင်ခန်းစာထဲက ကိန်းဂဏန်းတွေကို ရှင်းပြဖို့ ပြန်လည်တည်ဆောက်ထားတဲ့ ဥပမာဖြစ်ပါတယ်။ အမှန်တကယ် output ဟာ GPU model, driver version, operating system နဲ့ workload ပေါ်မူတည်ပြီး ကွာခြားနိုင်ပါတယ်။

## 4. NVIDIA-SMI Output ကို အသေးစိတ်ဖတ်ခြင်း

### 4.1 NVIDIA-SMI Version

ဥပမာ — `NVIDIA-SMI 580.95.05`။ ဒီ version က nvidia-smi utility version ကို ဖော်ပြပါတယ်။ ပုံမှန်အားဖြင့် NVIDIA driver release နဲ့ ဆက်စပ်နေပါတယ်။

### 4.2 Driver Version

ဥပမာ — `Driver Version: 580.95.05`။ ဒီတန်ဖိုးက လက်ရှိ system မှာ အသုံးပြုနေတဲ့ NVIDIA driver version ကို ပြပါတယ်။ GPU ကို မတွေ့တာ၊ CUDA application မ run နိုင်တာ၊ driver နဲ့ library compatibility ပြဿနာရှိတာတွေကို စစ်ဆေးတဲ့အခါ ဒီ information က အသုံးဝင်ပါတယ်။

### 4.3 CUDA Version

ဥပမာ — `CUDA Version: 13.0`။ ဒီနေရာမှာ အရေးကြီးတဲ့ ကွာခြားချက်တစ်ခု ရှိပါတယ်။

nvidia-smi မှာ ဖော်ပြထားတဲ့ CUDA Version ဟာ လက်ရှိ NVIDIA driver က support လုပ်နိုင်တဲ့ CUDA compatibility level ကို ဖော်ပြတာဖြစ်ပါတယ်။ System မှာ CUDA Toolkit version အဲဒီတန်ဖိုးအတိုင်း install လုပ်ထားတယ်လို့ မဆိုလိုပါဘူး။

CUDA Toolkit compiler (nvcc) ရှိပြီး PATH ထဲမှာ ထည့်ထားတယ်ဆိုရင် `nvcc --version` ကို အသုံးပြုနိုင်ပါတယ်။

| Command | ဘာကိုစစ်ဆေးသလဲ |
|---|---|
| `nvidia-smi` | GPU status နဲ့ driver က support လုပ်တဲ့ CUDA compatibility |
| `nvcc --version` | အသုံးပြုနိုင်တဲ့ CUDA Toolkit compiler version |
| `cat /usr/local/cuda/version.json` | အဲဒီ file ရှိပြီး သက်ဆိုင်ရာ Toolkit ကို ကိုယ်စားပြုပါက Toolkit version information |

CUDA Toolkit ကို မတူညီတဲ့ directory မှာ install လုပ်ထားနိုင်တာကြောင့် နောက်ဆုံး command က system တိုင်းမှာ အလုပ်လုပ်မယ်လို့ မယူဆသင့်ပါဘူး။

### 4.4 GPU Index နဲ့ GPU Name

ဥပမာ — `GPU 0  Tesla T4`။ GPU 0 ဆိုတာ လက်ရှိ system ထဲမှာ driver က သတ်မှတ်ထားတဲ့ GPU index ဖြစ်ပြီး Tesla T4 ဆိုတာ GPU model ဖြစ်ပါတယ်။

GPU အများအပြားပါတဲ့ server မှာ `GPU 0/1/2/3  NVIDIA A100` လို တွေ့နိုင်ပါတယ်။ သို့သော် GPU index တွေဟာ reboot တစ်ခါလုပ်တိုင်း အမြဲတမ်းတူညီမယ်လို့ အာမခံမထားပါဘူး။ Production script တွေမှာ GPU ကို တည်ငြိမ်စွာ သတ်မှတ်ဖို့ UUID သို့မဟုတ် PCI Bus ID ကို အသုံးပြုစဉ်းစားသင့်ပါတယ်။

GPU စာရင်းကိုသာ ကြည့်ချင်ရင် —

```
nvidia-smi -L
```

### 4.5 Fan

ဥပမာ — `Fan: N/A`။ Fan speed information ကို GPU သို့မဟုတ် platform ပေါ်မူတည်ပြီး ပြသနိုင်ပါတယ်။ Data Center GPU တချို့မှာ fan ကို GPU card တစ်ခုချင်းစီက တိုက်ရိုက်မထိန်းချုပ်ဘဲ server chassis cooling system က စီမံထားနိုင်ပါတယ်။ ဒါကြောင့် N/A ဆိုတာ fan ပျက်နေတယ်လို့ တန်းပြီးမဆိုလိုပါဘူး။

### 4.6 Temperature

ဥပမာ — `Temp: 43C`။ ဒီတန်ဖိုးက GPU temperature ကို Celsius နဲ့ ပြပါတယ်။

Temperature ကို အောက်ပါအချက်တွေနဲ့ တွဲဖက်စစ်ဆေးသင့်ပါတယ် — GPU model ရဲ့ operating temperature limit၊ GPU utilization၊ Power consumption၊ Clock speed၊ Thermal throttling status၊ Server airflow နဲ့ cooling condition။

GPU temperature တစ်ခုတည်းကို ကြည့်ပြီး server cooling အခြေအနေကောင်း၊ မကောင်းကို အပြီးသတ်ဆုံးဖြတ်လို့ မရပါဘူး။

### 4.7 Performance State — Perf

ဥပမာ — `Perf: P0`။ P0, P2, P8 စတဲ့ တန်ဖိုးတွေဟာ GPU performance state (P-state) ကို ဖော်ပြပါတယ်။ ယေဘုယျအားဖြင့် P0 ဟာ high-performance state ကို ကိုယ်စားပြုလေ့ရှိပြီး P8 ဟာ low-power/idle state နဲ့ ဆက်စပ်နိုင်ပါတယ်။ သို့သော် အဓိပ္ပာယ်နဲ့ အပြုအမူကို GPU model နဲ့ workload ပေါ်မူတည်ပြီး နားလည်ရပါမယ်။ P0 ဖြစ်နေတယ်ဆိုတာ GPU က performance အကောင်းဆုံးရနေတယ်လို့ အလိုအလျောက် အာမခံတာ မဟုတ်ပါဘူး။

### 4.8 Power Usage

ဥပမာ — `Pwr: Usage/Cap  68W / 70W`။ 68W က လက်ရှိဖတ်ရှုရရှိတဲ့ GPU power consumption ဖြစ်ပြီး 70W က အဲဒီ GPU အတွက် လက်ရှိဖော်ပြထားတဲ့ power limit ဖြစ်ပါတယ်။ ဒီဥပမာမှာ GPU က power limit နားကပ်နေပါတယ်။

Power usage မြင့်တာက workload က compute-intensive ဖြစ်နေကြောင်း ပြနိုင်ပေမဲ့ power limit, temperature, clock speed နဲ့ application performance တို့ကိုပါ တွဲကြည့်သင့်ပါတယ်။ Power limit ကို ကိုယ်တိုင်မပြောင်းမီ workload owner, server policy နဲ့ hardware support ကို စစ်ဆေးရပါမယ်။ Power limit ပြောင်းလဲခြင်းက performance နဲ့ power/thermal behavior ကို သက်ရောက်စေနိုင်ပါတယ်။

### 4.9 Memory Usage

ဥပမာ — `Memory-Usage: 471MiB / 15GiB`။ ဒီတန်ဖိုးက GPU memory အသုံးပြုမှုကို ဖော်ပြပါတယ် (Used: 471MiB၊ Total: 15GiB ခန့်)။ Tesla T4 မှာ physical memory 16GB ရှိပေမဲ့ system မှာ ဖော်ပြတဲ့ usable memory ဟာ driver reservation နဲ့ အခြားအချက်တွေကြောင့် အနည်းငယ်နည်းနိုင်ပါတယ်။

Memory usage မြင့်နေတယ်ဆိုတာ GPU memory ထဲမှာ data နဲ့ model weights တွေ ရှိနေကြောင်း ပြနိုင်ပါတယ်။ သို့သော် memory အသုံးများခြင်းဟာ GPU compute utilization မြင့်ခြင်းနဲ့ မတူပါဘူး။

- GPU memory 90% အသုံးပြုပေမဲ့ GPU-Util 10% ဖြစ်နိုင်ပါတယ်။
- GPU memory 40% အသုံးပြုပေမဲ့ GPU-Util 95% ဖြစ်နိုင်ပါတယ်။

ဒါကြောင့် memory usage နဲ့ compute utilization ကို သီးခြားစီ နားလည်ရပါမယ်။

### 4.10 GPU Utilization

ဥပမာ — `GPU-Util: 97%`။ NVIDIA-SMI ရဲ့ GPU utilization က သတ်မှတ်ထားတဲ့ sampling period အတွင်း GPU kernel တစ်ခု သို့မဟုတ် တစ်ခုထက်ပိုပြီး execute လုပ်နေခဲ့တဲ့ အချိန်အချိုးကို ဖော်ပြပါတယ်။ 97% ဆိုတာ sample period အတွင်း GPU ဟာ အချိန်အများစုမှာ အလုပ်လုပ်နေခဲ့တယ်လို့ ဆိုလိုပါတယ်။

သို့သော် ဒီတန်ဖိုးတစ်ခုတည်းနဲ့ အောက်ပါအချက်တွေကို မဆုံးဖြတ်နိုင်ပါဘူး။

- GPU က အမြင့်ဆုံး throughput ရနေသလား။ (GPU-Util 97% ≠ best performance)
- Model training က မျှော်မှန်းထားတဲ့ speed ရနေသလား။
- GPU memory bandwidth bottleneck ရှိသလား။
- CPU က GPU ကို data ပေးဖို့ နှောင့်နှေးနေသလား။
- GPU က thermal သို့မဟုတ် power throttling ဖြစ်နေသလား။

GPU utilization ကို application throughput, latency, memory bandwidth နဲ့ CPU/network/storage metrics တွေနဲ့ တွဲဖက်ကြည့်ဖို့ လိုပါတယ်။

### 4.11 ECC Error

ဥပမာ — `Volatile Uncorr. ECC: 0`။ ECC ဆိုတာ Error-Correcting Code ဖြစ်ပါတယ်။ GPU model နဲ့ configuration က support လုပ်ပါက ECC-related status နဲ့ error information တွေကို စစ်ဆေးနိုင်ပါတယ်။ 0 ဆိုတာ ဒီနေရာမှာ ဖော်ပြထားတဲ့ volatile uncorrectable ECC error count က zero ဖြစ်တယ်လို့ ဆိုလိုပါတယ်။ ဒါပေမဲ့ 0 တစ်ခုတည်းနဲ့ GPU မှာ error အမျိုးအစားအားလုံး လုံးဝမရှိဘူးလို့ မဆိုလိုပါဘူး။ ECC support, error counters, Xid events နဲ့ diagnostic results တွေကို လိုအပ်သလို ထပ်မံစစ်ဆေးရပါမယ်။

### 4.12 Compute Mode

ဥပမာ — `Compute M.: Default`။ Default mode မှာ GPU က compute application အများအပြားရဲ့ contexts တွေကို ခွင့်ပြုနိုင်ပါတယ်။ အခြား mode တွေမှာ Exclusive Process နဲ့ Prohibited တို့ ပါဝင်နိုင်ပါတယ်။ အသုံးပြုနိုင်တဲ့ options တွေဟာ GPU နဲ့ driver support ပေါ်မူတည်ပါတယ်။ Compute mode က GPU memory ကို application အလိုက် ခွဲဝေပေးတဲ့ MIG နဲ့ မတူပါဘူး။

### 4.13 MIG

MIG ဆိုတာ Multi-Instance GPU ဖြစ်ပါတယ်။ MIG ကို support လုပ်တဲ့ GPU model တချို့မှာ GPU ကို hardware-backed instances တွေအဖြစ် ခွဲဝေနိုင်ပါတယ်။

Tesla T4 က MIG ကို support မလုပ်ပါဘူး။ ဒါကြောင့် T4 ပေါ်မှာ MIG configuration မရနိုင်တာက ပုံမှန်အခြေအနေပါ။ A100 ကဲ့သို့ MIG-supported GPU တွေမှာလည်း GPU model, driver နဲ့ configuration ကို စစ်ဆေးပြီးမှ MIG mode နဲ့ instance configuration ကို စီမံခန့်ခွဲရပါမယ်။

## 5. GPU Process တွေကို ဖတ်ခြင်း

nvidia-smi ရဲ့ အောက်ပိုင်းမှာ GPU ကို အသုံးပြုနေတဲ့ process တွေကို ပြသနိုင်ပါတယ်။ ဥပမာ —

```
Processes:
GPU   PID      Type   Process name   GPU Memory
0     465464   C      python3        414MiB
```

| Field | အဓိပ္ပာယ် |
|---|---|
| GPU | Process အသုံးပြုနေတဲ့ GPU index |
| PID | Process ID |
| Type | Process/context အမျိုးအစား |
| Process name | Process အမည် |
| GPU Memory | အဲဒီ process နဲ့ ဆက်စပ်တဲ့ GPU memory usage |

ဒီဥပမာမှာ python3 process က GPU 0 ကို အသုံးပြုနေပြီး PID က 465464 ဖြစ်ပါတယ်။ GPU ကိုအသုံးပြုနေတဲ့ process အများအပြားရှိရင် output ထဲမှာ process အများအပြားကို မြင်ရနိုင်ပါတယ်။ သို့သော် container, virtualization, permissions နဲ့ driver support ပေါ်မူတည်ပြီး process information အပြည့်အစုံ မပေါ်နိုင်တဲ့အခြေအနေတွေ ရှိနိုင်ပါတယ်။

PID ကို ရှာဖွေစစ်ဆေးခြင်း —

```
ps -fp 465464
```

ဒီ command က Linux operating system ဘက်မှာ အဲဒီ PID ရဲ့ process details ကို စစ်ဆေးပေးပါတယ်။

အရေးကြီးသော သတိပေးချက် — GPU ကို အသုံးပြုနေတဲ့ process ကို ရပ်တန့်မယ်ဆိုရင် nvidia-smi ကနေ အမြဲတမ်း တိုက်ရိုက် terminate လုပ်နိုင်တယ်လို့ မယူဆပါနဲ့။ ပုံမှန်အားဖြင့် process termination ကို OS process-management tools နဲ့ လုပ်ဆောင်ရပါတယ်။ Shared server မှာ process တစ်ခုကို မရပ်ခင် workload owner နဲ့ အရင်ညှိနှိုင်းသင့်ပါတယ်။

## 6. NVIDIA-SMI နဲ့ ဘာတွေကို စီမံခန့်ခွဲနိုင်သလဲ။

nvidia-smi ဟာ monitoring အတွက်သာမက supported GPU နဲ့ driver ပေါ်မူတည်ပြီး အချို့ configuration နဲ့ management operations တွေကိုလည်း လုပ်ဆောင်နိုင်ပါတယ်။

- GPU status နဲ့ configuration ကို query လုပ်ခြင်း။
- Supported GPU တွေမှာ power limit ကို ပြောင်းလဲခြင်း။
- Supported device တွေမှာ compute mode ကို ပြောင်းလဲခြင်း။
- MIG-supported GPU တွေမှာ MIG configuration ကို စီမံခန့်ခွဲခြင်း။
- Supported GPU တွေရဲ့ clock-related settings တွေကို စီမံခြင်း။

ဒါပေမဲ့ command တစ်ခုချင်းစီအတွက် hardware support, driver support နဲ့ administrative permissions လိုအပ်ချက်တွေ ကွာခြားနိုင်ပါတယ်။ Management command တွေကို production server ပေါ်မှာ မစမ်းသပ်ခင် command help နဲ့ official documentation ကို စစ်ဆေးပါ။

## 7. လက်တွေ့အသုံးဝင်တဲ့ NVIDIA-SMI Commands

### 7.1 GPU အားလုံးရဲ့ Status ကို ကြည့်ခြင်း

```
nvidia-smi
```

GPU status ကို တစ်ကြိမ်ဖတ်ရှုဖော်ပြပါတယ်။

### 7.2 GPU စာရင်းကို ကြည့်ခြင်း

```
nvidia-smi -L
```

GPU model နဲ့ GPU UUID တွေကို ဖော်ပြပေးပါတယ်။

### 7.3 တစ်စက္ကန့်ခြား Refresh လုပ်ခြင်း

```
watch -n 1 nvidia-smi
```

ဒီ command က nvidia-smi ကို တစ်စက္ကန့်ခြား ပြန်လည် run လုပ်ပြီး screen ကို update လုပ်ပေးပါတယ်။ ထွက်ချင်ရင် Ctrl+C ကို နှိပ်ပါ။

### 7.4 လိုအပ်တဲ့ Metrics တွေကိုသာ Query လုပ်ခြင်း

```
nvidia-smi \
  --query-gpu=name,utilization.gpu,memory.total,memory.used,memory.free,temperature.gpu,power.draw \
  --format=csv
```

ဒီ command က GPU name, utilization, memory, temperature နဲ့ power draw ကို CSV format နဲ့ ပြပေးပါတယ်။ Shell script, simple report နဲ့ data parsing အတွက် ပိုမိုအသုံးဝင်နိုင်ပါတယ်။ Query လုပ်နိုင်တဲ့ field အားလုံးက GPU နဲ့ driver version တိုင်းမှာ မရနိုင်တာကြောင့် လိုအပ်ရင် `nvidia-smi --help-query-gpu` ကို စစ်ဆေးပါ။

### 7.5 အချိန်အလိုက် GPU Metrics စောင့်ကြည့်ခြင်း

```
nvidia-smi -l 1
```

ဒီ command က တစ်စက္ကန့်ခြား GPU status ကို ပြန်လည်ဖော်ပြပေးပါတယ်။ ဒါကြောင့် nvidia-smi မှာ continuous monitoring option လုံးဝမရှိဘူးလို့ ပြောတာ မမှန်ပါဘူး။ သို့သော် ဒီနည်းလမ်းက long-term metrics storage, centralized dashboards နဲ့ alerting platform တစ်ခုလုံးကို အစားမထိုးနိုင်ပါဘူး။

### 7.6 Compact Monitoring — dmon

```
nvidia-smi dmon
nvidia-smi dmon -s pucm
```

dmon က support လုပ်တဲ့ GPU တွေအတွက် power, temperature, clock နဲ့ utilization ကဲ့သို့ metrics တွေကို အချိန်အလိုက် compact output နဲ့ ပြပေးနိုင်ပါတယ်။ `p, u, c, m` ဆိုတာ metric groups တွေကို ရွေးထားတာဖြစ်ပါတယ်။ ရရှိနိုင်တဲ့ groups နဲ့ metrics တွေက platform support ပေါ်မူတည်တာကြောင့် မိမိ driver ရဲ့ `nvidia-smi dmon -h` ကို စစ်ဆေးသင့်ပါတယ်။

## 8. NVIDIA-SMI ရဲ့ ကန့်သတ်ချက်များ

### 8.1 Local Node ကို အဓိကထားခြင်း

ပုံမှန် nvidia-smi command က မိမိ run နေတဲ့ system/node ထဲက GPU တွေကို စစ်ဆေးဖို့ ရည်ရွယ်ထားပါတယ်။ Native centralized remote fleet monitoring system တစ်ခု မဟုတ်ပါဘူး။ Remote server တွေကို စစ်ဆေးလိုရင် သီးခြား remote execution, exporters, monitoring agents သို့မဟုတ် centralized monitoring architecture လိုအပ်နိုင်ပါတယ်။

### 8.2 Historical Data

nvidia-smi ကို ပုံမှန် run တာက metrics database တစ်ခုကို အလိုအလျောက် တည်ဆောက်ပေးတာ မဟုတ်ပါဘူး။ ဒါပေမဲ့ `-l`, `dmon` နဲ့ support လုပ်တဲ့ driver version အချို့မှာ experimental daemon feature တွေကို အသုံးပြုပြီး metrics ကို အချိန်အလိုက် မှတ်တမ်းတင်နိုင်တဲ့ options တွေလည်း ရှိပါတယ်။ ထို့ကြောင့် “historical data ကို ဘယ်လိုမှ မသိမ်းနိုင်ဘူး” လို့ ပြောမယ့်အစား default one-shot command တစ်ခုတည်းက enterprise-grade historical monitoring ကို မပေးနိုင်ဘူးလို့ နားလည်သင့်ပါတယ်။

### 8.3 Native Enterprise Alerting မရှိခြင်း

nvidia-smi ဟာ enterprise monitoring platform တစ်ခုလို centralized alerting, notification routing, incident management နဲ့ dashboard management ကို အဓိကလုပ်ဆောင်ပေးတဲ့ tool မဟုတ်ပါဘူး။ ဥပမာ GPU temperature threshold ကျော်သွားရင် Slack, email သို့မဟုတ် incident-management system ကို အလိုအလျောက် alert ပို့ချင်တာမျိုးအတွက် သီးခြား monitoring solution လိုအပ်ပါတယ်။

### 8.4 GPU တစ်ခုတည်းကို ကြည့်ခြင်း

Data Center မှာ GPU performance ပြဿနာတစ်ခု ဖြစ်ပေါ်လာရင် GPU metrics တစ်ခုတည်းနဲ့ အကြောင်းရင်းကို အမြဲတမ်း မသိနိုင်ပါဘူး။ အောက်ပါ metrics တွေကိုလည်း စစ်ဆေးရနိုင်ပါတယ် — CPU utilization နဲ့ memory၊ Storage throughput နဲ့ latency၊ Network throughput/packet loss/congestion၊ GPU temperature နဲ့ power၊ Container/Kubernetes resource usage၊ Application throughput နဲ့ request latency၊ Node health နဲ့ system logs။

## 9. Enterprise GPU Monitoring Architecture

Data Center မှာ GPU အများအပြားကို စောင့်ကြည့်ဖို့ အောက်ပါ architecture ကို အသုံးပြုနိုင်ပါတယ်။

```
NVIDIA GPU Servers
        |
        v
 NVIDIA DCGM
        |
        v
 DCGM Exporter
        |
        v
   Prometheus
        |
        v
     Grafana
        |
        +----> Dashboards
        |
        +----> Alerting
```

- NVIDIA-SMI — Individual node ရဲ့ GPU status ကို အမြန်စစ်ဆေးဖို့။
- DCGM — GPU telemetry, health monitoring နဲ့ diagnostics အတွက်။
- DCGM Exporter — Support လုပ်တဲ့ GPU metrics တွေကို Prometheus-compatible format နဲ့ ထုတ်ပေးဖို့။
- Prometheus — Metrics တွေကို collect လုပ်ပြီး time series အဖြစ် သိမ်းဆည်းဖို့။
- Grafana — Metrics တွေကို dashboards အဖြစ် မြင်သာအောင် ပြသဖို့။
- Alerting — သတ်မှတ်ထားတဲ့ thresholds နဲ့ rules ပေါ်မူတည်ပြီး notification ပို့ဖို့။

ဒီ architecture က example ဖြစ်ပြီး production deployment မှာ authentication, retention, availability, access control နဲ့ alert routing တို့ကိုလည်း စီမံရပါမယ်။

## 10. လက်တွေ့ Lab — GPU Health ကို စစ်ဆေးခြင်း

အောက်ပါအစီအစဉ်အတိုင်း စမ်းသပ်နိုင်ပါတယ်။

**Step 1 — GPU ရှိ၊ မရှိ စစ်ဆေးပါ**

```
nvidia-smi -L
```

**Step 2 — Driver နဲ့ GPU status ကို စစ်ဆေးပါ**

```
nvidia-smi
```

**Step 3 — အဓိက metrics တွေကို query လုပ်ပါ**

```
nvidia-smi \
  --query-gpu=name,driver_version,utilization.gpu,memory.total,memory.used,memory.free,temperature.gpu,power.draw \
  --format=csv
```

**Step 4 — GPU အသုံးပြုနေတဲ့ process တွေကို ကြည့်ပါ**

```
nvidia-smi
```

Output ရဲ့ Processes section ကို ကြည့်ပါ။

**Step 5 — GPU status ကို အချိန်အလိုက် စောင့်ကြည့်ပါ**

```
nvidia-smi -l 1
```

Ctrl+C နှိပ်ပြီး ထွက်နိုင်ပါတယ်။

**Step 6 — CUDA Toolkit ကို သီးခြားစစ်ဆေးပါ**

```
nvcc --version
```

`nvcc: command not found` လို့ ပြရင် CUDA Toolkit compiler မရှိတာ၊ သို့မဟုတ် PATH ထဲမှာ မရှိတာ ဖြစ်နိုင်ပါတယ်။ ဒါက GPU driver ပျက်နေတယ်လို့ တန်းပြီးမဆိုလိုပါဘူး။

## 11. Common Misconceptions

| မှားယွင်းနိုင်တဲ့ အယူအဆ | မှန်ကန်တဲ့ နားလည်မှု |
|---|---|
| nvidia-smi မှာ CUDA 13.0 ပြရင် CUDA Toolkit 13.0 install လုပ်ထားတယ်။ | Driver-supported CUDA compatibility ကို ဖော်ပြတာဖြစ်ပြီး Toolkit installation version နဲ့ မတူနိုင်ပါဘူး။ |
| GPU utilization 97% ဆိုရင် workload က အကောင်းဆုံး performance ရနေတယ်။ | GPU busy ဖြစ်နေကြောင်း ပြပေမဲ့ throughput နဲ့ bottleneck တွေကို သီးခြားစစ်ဆေးရပါမယ်။ |
| Memory usage မြင့်ရင် GPU utilization လည်း မြင့်ရမယ်။ | GPU memory usage နဲ့ GPU compute utilization တို့ဟာ မတူညီတဲ့ metrics တွေပါ။ |
| nvidia-smi က continuous monitoring လုံးဝမလုပ်နိုင်ဘူး။ | `-l`, `dmon` နဲ့ support လုပ်တဲ့အခြေအနေတွေမှာ daemon option တွေ ရှိပါတယ်။ |
| nvidia-smi တစ်ခုတည်းနဲ့ Data Center GPU အားလုံးကို centralized monitoring လုပ်နိုင်တယ်။ | ပုံမှန်အားဖြင့် local node ကို အဓိကထားပါတယ်။ Fleet monitoring အတွက် centralized tools တွေ လိုအပ်ပါတယ်။ |
| Process တစ်ခုကို nvidia-smi နဲ့ အမြဲတမ်း terminate လုပ်နိုင်တယ်။ | Process termination ကို ပုံမှန်အားဖြင့် OS tools နဲ့ လုပ်ရပါတယ်။ Shared workload ကို မရပ်ခင် ခွင့်ပြုချက်ယူသင့်ပါတယ်။ |

## 12. Review Questions

1. nvidia-smi ရဲ့ အမည်အပြည့်အစုံက ဘာလဲ။
2. nvidia-smi နဲ့ `nvcc --version` တို့ရဲ့ ကွာခြားချက်က ဘာလဲ။
3. `GPU-Util: 97%` ဆိုတာ ဘာကို ဖော်ပြတာလဲ။
4. `Memory-Usage: 471MiB / 15GiB` ကို ဘယ်လိုဖတ်ရမလဲ။
5. GPU process ရဲ့ PID ကို ဘာကြောင့် သိဖို့လိုသလဲ။
6. `nvidia-smi -l 1` က ဘာလုပ်ပေးသလဲ။
7. nvidia-smi တစ်ခုတည်းနဲ့ Data Center monitoring ကို ဘာကြောင့် မလုံလောက်သလဲ။
8. GPU utilization မြင့်နေပေမဲ့ application performance မကောင်းနိုင်တဲ့ အကြောင်းရင်းတစ်ခုကို ပြောပါ။
9. GPU memory usage နဲ့ GPU utilization တို့ ဘာကြောင့် မတူညီသလဲ။
10. DCGM Exporter, Prometheus နဲ့ Grafana တို့ကို ဘယ်လိုချိတ်ဆက်အသုံးပြုနိုင်သလဲ။

## 13. Module Summary

- nvidia-smi ဟာ NVIDIA GPU infrastructure ကို စီမံခန့်ခွဲရာမှာ အခြေခံအကျဆုံးနဲ့ အရေးအကြီးဆုံး command-line tools တွေထဲက တစ်ခုဖြစ်ပါတယ်။
- ၎င်းကို အသုံးပြုပြီး GPU model, driver version, utilization, memory, temperature, power consumption နဲ့ running processes တွေကို စစ်ဆေးနိုင်ပါတယ်။ Supported hardware နဲ့ driver ပေါ်မူတည်ပြီး configuration operations အချို့ကိုလည်း လုပ်ဆောင်နိုင်ပါတယ်။
- သို့သော် Enterprise Data Center မှာ historical metrics, centralized dashboards, alerting နဲ့ fleet-wide visibility လိုအပ်လာတဲ့အခါ nvidia-smi ကို DCGM, DCGM Exporter, Prometheus, Grafana နဲ့ အခြား monitoring tools တွေနဲ့ တွဲဖက်အသုံးပြုသင့်ပါတယ်။

## 14. Self-check Questions and Answers

1. **မေး** — `nvidia-smi -L` နဲ့ `nvidia-smi` ရဲ့ ကွာခြားချက်က ဘာလဲ။  
   **ဖြေ** — `-L` က GPU စာရင်း (model နဲ့ UUID) ကိုသာ ပြပြီး `nvidia-smi` က status table အပြည့် (utilization, memory, temperature, power, processes) ကို ပြပါတယ်။
2. **မေး** — nvidia-smi မှာ `CUDA Version: 13.0` ပြရင် CUDA Toolkit 13.0 ထည့်ထားတယ်လို့ ဆိုလိုသလား။  
   **ဖြေ** — မဆိုလိုပါဘူး။ Driver က support လုပ်တဲ့ CUDA compatibility level သာ ဖြစ်ပါတယ်။ Toolkit version ကို `nvcc --version` နဲ့ သီးခြားစစ်ပါ။
3. **မေး** — ရေရှည် metrics သိမ်းချင်ရင် ဘာလုပ်မလဲ။  
   **ဖြေ** — DCGM → DCGM Exporter → Prometheus → Grafana ကဲ့သို့ monitoring pipeline ကို တည်ဆောက်ပါ။
4. **မေး** — Processes section မှာ PID မပေါ်ရင် ဘာကြောင့် ဖြစ်နိုင်သလဲ။  
   **ဖြေ** — Container, virtualization, permissions နဲ့ driver support ပေါ်မူတည်ပြီး process information မပြည့်စုံနိုင်ပါဘူး။
5. **မေး** — GPU-Util 97% က ဘာကို ဆိုလိုသလဲ။  
   **ဖြေ** — Sampling period အတွင်း GPU က အချိန်အများစု အလုပ်လုပ်နေကြောင်းသာ ဖြစ်ပြီး performance အကောင်းဆုံး ရနေတယ်လို့ မဆိုလိုပါဘူး။

## English Version

## 1. Learning Objectives

In this module you will learn how to inspect the current state of an NVIDIA GPU using the nvidia-smi command.

By the end of this module you should understand:

- What nvidia-smi is and what it is used for.
- How to read GPU utilization, GPU memory, temperature, power consumption, and clock speed.
- How to find the process and PID using a GPU.
- How to use nvidia-smi, nvidia-smi -l, and nvidia-smi dmon.
- The difference between the GPU driver and the CUDA Toolkit version.
- Why nvidia-smi alone cannot manage an entire data center.

## 2. What Is NVIDIA-SMI?

NVIDIA-SMI = NVIDIA System Management Interface.

nvidia-smi is a command-line utility used to check the status, utilization, memory, temperature, power consumption, and processes using NVIDIA GPUs.

Normally, installing the NVIDIA GPU driver also installs the nvidia-smi utility. However, availability can vary with the driver package and installation method.

Official documentation: https://docs.nvidia.com/deploy/nvidia-smi/index.html

How does it work? Understand the conceptual architecture as follows:

```
Administrator / Engineer
          |
          v
      nvidia-smi
          |
          v
  NVIDIA Management Library
         (NVML)
          |
          v
    NVIDIA GPU Driver
          |
          v
      NVIDIA GPU
          |
          v
 Utilization / Memory /
 Temperature / Power /
 Processes / Clock Status
```

nvidia-smi can use the NVIDIA Management Library (NVML) to query GPU-related information. Some management operations can also be performed depending on the supported hardware and driver.

When should you use nvidia-smi?

- After installing the GPU driver, checking whether the GPU is detected.
- Checking whether a machine learning training job is running on the GPU.
- Investigating GPU memory exhaustion problems.
- Viewing GPU temperature and power usage.
- Finding the process using the GPU.
- Starting GPU performance troubleshooting.

Key point: nvidia-smi is extremely useful for starting GPU troubleshooting, but it is not a replacement for an enterprise data center monitoring platform.

## 3. Checking GPU Status for the First Time

Run this command in a terminal:

```
nvidia-smi
```

If the GPU driver is working correctly and an NVIDIA GPU is detected, a GPU status table will be displayed. You can imagine the example output like this:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 580.95.05    Driver Version: 580.95.05    CUDA Version: 13.0     |
+-----------------------------------------------------------------------------+
| GPU  Name       Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
|  0   Tesla T4   Off           | 00000000:00:1E.0 Off |                    0 |
+-----------------------------------------------------------------------------+
| Fan  Temp  Perf  Pwr:Usage/Cap | Memory-Usage | GPU-Util | Compute M.       |
| N/A   43C   P0    68W / 70W    | 471MiB / 15GiB | 97%   | Default          |
+-----------------------------------------------------------------------------+
| Processes:                                                                |
| GPU   PID      Type   Process name                         GPU Memory      |
|  0    465464   C      python3                                414MiB       |
+-----------------------------------------------------------------------------+
```

Note: this table is a reconstructed example used to explain the numbers in the lesson. Actual output can differ depending on GPU model, driver version, operating system, and workload.

## 4. Reading the NVIDIA-SMI Output in Detail

### 4.1 NVIDIA-SMI Version

Example — `NVIDIA-SMI 580.95.05`. This version shows the nvidia-smi utility version and is normally related to the NVIDIA driver release.

### 4.2 Driver Version

Example — `Driver Version: 580.95.05`. This value shows the NVIDIA driver version in use on the current system. It is useful when checking GPU-not-found issues, CUDA applications that will not run, and driver/library compatibility problems.

### 4.3 CUDA Version

Example — `CUDA Version: 13.0`. There is one important distinction here.

The CUDA Version shown in nvidia-smi indicates the CUDA compatibility level supported by the current NVIDIA driver. It does not mean that the same CUDA Toolkit version is installed on the system.

If the CUDA Toolkit compiler (nvcc) exists and is on your PATH, you can use `nvcc --version`.

| Command | What it checks |
|---|---|
| `nvidia-smi` | GPU status and the driver-supported CUDA compatibility |
| `nvcc --version` | The usable CUDA Toolkit compiler version |
| `cat /usr/local/cuda/version.json` | Toolkit version information, if that file exists and represents the toolkit in question |

The CUDA Toolkit may be installed in a different directory, so do not assume the last command works on every system.

### 4.4 GPU Index and GPU Name

Example — `GPU 0  Tesla T4`. GPU 0 is the GPU index assigned by the driver in the current system; Tesla T4 is the GPU model.

On a server with many GPUs you may see `GPU 0/1/2/3  NVIDIA A100`. However, GPU indices are not guaranteed to stay the same across reboots. In production scripts, consider using the UUID or PCI Bus ID to identify GPUs stably.

To list GPUs only:

```
nvidia-smi -L
```

### 4.5 Fan

Example — `Fan: N/A`. Fan speed information may be shown depending on the GPU or platform. On some data center GPUs the fan is not controlled by each GPU card directly but managed by the server chassis cooling system. So N/A does not automatically mean a broken fan.

### 4.6 Temperature

Example — `Temp: 43C`. This value shows the GPU temperature in Celsius.

Temperature should be checked together with: the GPU model's operating temperature limit, GPU utilization, power consumption, clock speed, thermal throttling status, and server airflow/cooling conditions.

A single GPU temperature reading cannot conclusively determine whether server cooling is healthy.

### 4.7 Performance State — Perf

Example — `Perf: P0`. Values such as P0, P2, P8 indicate the GPU performance state (P-state). Generally P0 represents a high-performance state and P8 can relate to a low-power/idle state. However, meaning and behavior must be understood per GPU model and workload. P0 does not automatically guarantee the GPU is delivering its best performance.

### 4.8 Power Usage

Example — `Pwr: Usage/Cap  68W / 70W`. 68W is the currently measured GPU power consumption; 70W is the currently reported power limit for that GPU. In this example the GPU is close to its power limit.

High power usage can indicate a compute-intensive workload, but you should also look at power limit, temperature, clock speed, and application performance. Before changing a power limit yourself, check the workload owner, server policy, and hardware support. Changing a power limit can affect performance and power/thermal behavior.

### 4.9 Memory Usage

Example — `Memory-Usage: 471MiB / 15GiB`. This value shows GPU memory usage (Used: 471MiB, Total: about 15GiB). A Tesla T4 has 16GB of physical memory, but usable memory reported by the system can be slightly lower due to driver reservation and other factors.

High memory usage can mean data and model weights reside in GPU memory. However, high memory usage is not the same as high GPU compute utilization:

- GPU memory can be 90% used while GPU-Util is 10%.
- GPU memory can be 40% used while GPU-Util is 95%.

So memory usage and compute utilization must be understood separately.

### 4.10 GPU Utilization

Example — `GPU-Util: 97%`. NVIDIA-SMI's GPU utilization shows the fraction of time during the defined sampling period that one or more GPU kernels were executing. 97% means the GPU was working for most of the sample period.

However, this single value cannot determine:

- Whether the GPU is achieving maximum throughput. (GPU-Util 97% ≠ best performance.)
- Whether model training is running at the expected speed.
- Whether there is a GPU memory bandwidth bottleneck.
- Whether the CPU is slow to feed data to the GPU.
- Whether the GPU is thermal or power throttling.

GPU utilization must be viewed together with application throughput, latency, memory bandwidth, and CPU/network/storage metrics.

### 4.11 ECC Error

Example — `Volatile Uncorr. ECC: 0`. ECC stands for Error-Correcting Code. If the GPU model and configuration support it, ECC-related status and error information can be checked. 0 means the volatile uncorrectable ECC error count shown here is zero. But a single 0 does not mean the GPU has no errors of any kind. ECC support, error counters, Xid events, and diagnostic results should be checked as needed.

### 4.12 Compute Mode

Example — `Compute M.: Default`. In Default mode the GPU can allow contexts from multiple compute applications. Other modes can include Exclusive Process and Prohibited. Available options depend on GPU and driver support. Compute mode is not the same as MIG, which partitions GPU memory into instances per application.

### 4.13 MIG

MIG stands for Multi-Instance GPU. On some MIG-supported GPU models the GPU can be partitioned into hardware-backed instances.

Tesla T4 does not support MIG, so being unable to configure MIG on a T4 is normal. On MIG-supported GPUs such as the A100, the GPU model, driver, and configuration must be checked before managing MIG mode and instance configuration.

## 5. Reading GPU Processes

The bottom section of nvidia-smi can show the processes using the GPU. Example:

```
Processes:
GPU   PID      Type   Process name   GPU Memory
0     465464   C      python3        414MiB
```

| Field | Meaning |
|---|---|
| GPU | The GPU index used by the process |
| PID | Process ID |
| Type | Process/context type |
| Process name | Process name |
| GPU Memory | GPU memory usage associated with that process |

In this example a python3 process uses GPU 0 and its PID is 465464. If multiple processes use the GPU, multiple entries can appear in the output. However, depending on container, virtualization, permissions, and driver support, process information may not be fully visible.

Checking a PID:

```
ps -fp 465464
```

This command inspects process details for that PID on the Linux operating system side.

Important warning: do not assume a GPU-using process can always be terminated directly from nvidia-smi. Process termination is normally done with OS process-management tools. On a shared server, coordinate with the workload owner before stopping a process.

## 6. What Can You Manage with NVIDIA-SMI?

Beyond monitoring, nvidia-smi can perform some configuration and management operations depending on the supported GPU and driver:

- Querying GPU status and configuration.
- Changing the power limit on supported GPUs.
- Changing compute mode on supported devices.
- Managing MIG configuration on MIG-supported GPUs.
- Managing clock-related settings on supported GPUs.

However, per-command hardware support, driver support, and administrative permission requirements can differ. Check the command help and official documentation before trying management commands on a production server.

## 7. Practical NVIDIA-SMI Commands

### 7.1 View the status of all GPUs

```
nvidia-smi
```

Reads and displays GPU status once.

### 7.2 List GPUs

```
nvidia-smi -L
```

Shows GPU models and GPU UUIDs.

### 7.3 Refresh every second

```
watch -n 1 nvidia-smi
```

Reruns nvidia-smi about every second and updates the screen. Press Ctrl+C to exit.

### 7.4 Query only the metrics you need

```
nvidia-smi \
  --query-gpu=name,utilization.gpu,memory.total,memory.used,memory.free,temperature.gpu,power.draw \
  --format=csv
```

Shows GPU name, utilization, memory, temperature, and power draw in CSV format — useful for shell scripts, simple reports, and data parsing. Not every queryable field exists on every GPU/driver version; check `nvidia-smi --help-query-gpu` when needed.

### 7.5 Monitor GPU metrics over time

```
nvidia-smi -l 1
```

Redisplays GPU status every second. So it is wrong to say nvidia-smi has no continuous monitoring option at all. However, this method does not replace long-term metrics storage, centralized dashboards, or an alerting platform.

### 7.6 Compact Monitoring — dmon

```
nvidia-smi dmon
nvidia-smi dmon -s pucm
```

dmon can show metrics such as power, temperature, clock, and utilization over time in compact output for supported GPUs. Here `p, u, c, m` select metric groups. Available groups and metrics depend on platform support, so check your driver's `nvidia-smi dmon -h`.

## 8. Limitations of NVIDIA-SMI

### 8.1 Local-node focus

The normal nvidia-smi command is intended to inspect the GPUs in the system/node it runs on. It is not a native centralized remote fleet monitoring system. Inspecting remote servers may require separate remote execution, exporters, monitoring agents, or a centralized monitoring architecture.

### 8.2 Historical data

Running nvidia-smi normally does not automatically build a metrics database. However, with `-l`, `dmon`, and some supported driver versions there are options, including experimental daemon features, that can record metrics over time. Rather than saying "historical data can never be saved", understand that the default one-shot command alone does not provide enterprise-grade historical monitoring.

### 8.3 No native enterprise alerting

nvidia-smi is not a tool that primarily provides centralized alerting, notification routing, incident management, and dashboard management like an enterprise monitoring platform. For example, automatically alerting Slack, email, or an incident-management system when a GPU temperature threshold is exceeded requires a separate monitoring solution.

### 8.4 Looking at a single GPU

When a GPU performance problem occurs in a data center, the cause cannot always be determined from GPU metrics alone. Other metrics may also need checking: CPU utilization and memory, storage throughput and latency, network throughput/packet loss/congestion, GPU temperature and power, container/Kubernetes resource usage, application throughput and request latency, node health and system logs.

## 9. Enterprise GPU Monitoring Architecture

To monitor many GPUs in a data center, the following architecture can be used:

```
NVIDIA GPU Servers
        |
        v
 NVIDIA DCGM
        |
        v
 DCGM Exporter
        |
        v
   Prometheus
        |
        v
     Grafana
        |
        +----> Dashboards
        |
        +----> Alerting
```

- NVIDIA-SMI — quick inspection of an individual node's GPU status.
- DCGM — GPU telemetry, health monitoring, and diagnostics.
- DCGM Exporter — exposes supported GPU metrics in Prometheus-compatible format.
- Prometheus — collects metrics and stores them as time series.
- Grafana — displays metrics as dashboards.
- Alerting — sends notifications based on defined thresholds and rules.

This architecture is an example; production deployments must also handle authentication, retention, availability, access control, and alert routing.

## 10. Practical Lab — Checking GPU Health

You can try the following sequence.

**Step 1 — Check whether GPUs exist**

```
nvidia-smi -L
```

**Step 2 — Check the driver and GPU status**

```
nvidia-smi
```

**Step 3 — Query the key metrics**

```
nvidia-smi \
  --query-gpu=name,driver_version,utilization.gpu,memory.total,memory.used,memory.free,temperature.gpu,power.draw \
  --format=csv
```

**Step 4 — View processes using the GPU**

```
nvidia-smi
```

Look at the Processes section of the output.

**Step 5 — Monitor GPU status over time**

```
nvidia-smi -l 1
```

Press Ctrl+C to exit.

**Step 6 — Check the CUDA Toolkit separately**

```
nvcc --version
```

If it shows `nvcc: command not found`, the CUDA Toolkit compiler may be missing or not on PATH. That does not immediately mean the GPU driver is broken.

## 11. Common Misconceptions

| Misconception | Correct understanding |
|---|---|
| If nvidia-smi shows CUDA 13.0, CUDA Toolkit 13.0 is installed. | It shows driver-supported CUDA compatibility, which may differ from the installed Toolkit version. |
| GPU utilization 97% means the workload performs at its best. | It shows the GPU is busy; throughput and bottlenecks must be checked separately. |
| If memory usage is high, GPU utilization must also be high. | GPU memory usage and GPU compute utilization are different metrics. |
| nvidia-smi cannot do continuous monitoring at all. | With `-l`, `dmon`, and supported setups, daemon options exist. |
| nvidia-smi alone can centrally monitor all data center GPUs. | It normally focuses on the local node; fleet monitoring needs centralized tools. |
| A process can always be terminated with nvidia-smi. | Process termination is normally done with OS tools; get permission before stopping shared workloads. |

## 12. Review Questions

1. What does nvidia-smi stand for?
2. What is the difference between nvidia-smi and `nvcc --version`?
3. What does `GPU-Util: 97%` indicate?
4. How do you read `Memory-Usage: 471MiB / 15GiB`?
5. Why do you need to know a GPU process's PID?
6. What does `nvidia-smi -l 1` do?
7. Why is nvidia-smi alone insufficient for data center monitoring?
8. Give one reason application performance can be poor even with high GPU utilization.
9. Why are GPU memory usage and GPU utilization different?
10. How do DCGM Exporter, Prometheus, and Grafana work together?

## 13. Module Summary

- nvidia-smi is one of the most basic and important command-line tools for managing NVIDIA GPU infrastructure.
- It can inspect GPU model, driver version, utilization, memory, temperature, power consumption, and running processes. Depending on supported hardware and driver it can also perform some configuration operations.
- However, when a data center needs historical metrics, centralized dashboards, alerting, and fleet-wide visibility, nvidia-smi should be used together with DCGM, DCGM Exporter, Prometheus, Grafana, and other monitoring tools.

## 14. Self-check Questions and Answers

1. **Q** — What is the difference between `nvidia-smi -L` and `nvidia-smi`?  
   **A** — `-L` lists GPUs (model and UUID) only, while `nvidia-smi` shows the full status table (utilization, memory, temperature, power, processes).
2. **Q** — Does `CUDA Version: 13.0` in nvidia-smi mean CUDA Toolkit 13.0 is installed?  
   **A** — No. It is the driver-supported CUDA compatibility level. Check the Toolkit version separately with `nvcc --version`.
3. **Q** — What should you build for long-term metrics storage?  
   **A** — A monitoring pipeline such as DCGM → DCGM Exporter → Prometheus → Grafana.
4. **Q** — Why might no PID appear in the Processes section?  
   **A** — Depending on container, virtualization, permissions, and driver support, process information may not be fully visible.
5. **Q** — What does GPU-Util 97% mean?  
   **A** — The GPU was busy for most of the sampling period; it does not mean best performance.

## Technical review notes

- Source material arrived as Burmese study notes split across messages (learning objectives through the lab steps in one message; the misconceptions table, review questions, and summary in a later message). The English version is a faithful rendering of the same content; no technical claims were added.
- The sample nvidia-smi table is reconstructed for teaching (source caveat kept): driver 580.95.05 / CUDA 13.0 / Tesla T4 numbers are illustrative and vary by system.
- Source caveats preserved: nvidia-smi's CUDA Version is driver CUDA compatibility, not the installed CUDA Toolkit; `GPU-Util 97% ≠ best performance`; memory usage ≠ compute utilization; `Fan: N/A` does not mean a broken fan; GPU indices may change across reboots (use UUID/PCI Bus ID in scripts); process termination is an OS operation and shared workloads need coordination; monitoring ≠ scheduling.
- The `nvcc --version` vs `cat /usr/local/cuda/version.json` comparison and the `nvidia-smi dmon -s pucm` example are source-provided; field availability is driver/platform-dependent — needs verification on the target system.
