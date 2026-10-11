# Module — Installing CUDA Toolkit and Verifying the GPU Environment

> Part IV · Course Module 21 — topic notes

## မြန်မာဘာသာ version — Installing CUDA Toolkit and Verifying the GPU Environment

## 1. သင်ယူရမည့် ရည်ရွယ်ချက်များ (Learning Objectives)

ဒီ module ပြီးဆုံးတဲ့အခါ အောက်ပါအချက်တွေကို လုပ်ဆောင်နိုင်ရပါမယ်။

- CUDA Toolkit ကို install မလုပ်မီ လိုအပ်ချက် (prerequisites) တွေကို ခွဲခြားဖော်ထုတ်ခြင်း။
- Support ပြုလုပ်ထားတဲ့ GPU hardware၊ operating system နဲ့ CPU architecture တွေကို နားလည်ခြင်း။
- Linux မှာ CUDA development အတွက် GCC ဘာကြောင့် လိုအပ်သလဲဆိုတာကို ရှင်းပြခြင်း။
- NVIDIA GPU နဲ့ driver ရှိ/မရှိ ရရှိနိုင်/မရရှိနိုင် စစ်ဆေးခြင်း။
- `nvidia-smi` က ပြသတဲ့ CUDA version နဲ့ install လုပ်ထားတဲ့ CUDA Toolkit version ကြားက ကွာခြားချက်ကို နားလည်ခြင်း။
- PyTorch သို့မဟုတ် TensorFlow ကို မသုံးစွဲမီ CUDA development environment ကို verify လုပ်ခြင်း။

## 2. CUDA Installation Requirements

CUDA မတိုင်ခင် install လုပ်မယ်ဆိုရင် ကိုယ် install လုပ်မယ့် CUDA Toolkit release အတွက် သတ်မှတ်ထားတဲ့ လိုအပ်ချက်တွေကို system က ဖြည့်ဆည်းထားခြင်း ရှိ/မရှိ အရင်စစ်ဆေးပါ။

အဓိက prerequisites တွေကတော့—

- **CUDA-capable NVIDIA GPU** — ရွေးချယ်ထားတဲ့ CUDA release က support ပြုလုပ်ထားတဲ့ GPU ဖြစ်ရပါမယ်။
- **Supported operating system** — အဲဒီ Linux distribution သို့မဟုတ် Windows version ကို အဲဒီ release က support ပြုလုပ်ထားရပါမယ်။
- **Compatible NVIDIA driver** — install လုပ်ထားတဲ့ driver က ရည်ရွယ်ထားတဲ့ CUDA applications နဲ့ toolkit requirements တွေကို support ပြုလုပ်နိုင်ရပါမယ်။
- **CUDA Toolkit** — CUDA applications တွေကို local မှာ develop သို့မဟုတ် compile လုပ်မှာဖြစ်ရင် လိုအပ်ပါတယ်။
- **Compatible development tools** — Linux မှာ CUDA C/C++ development အတွက် ပုံမှန်အားဖြင့် GCC ကဲ့သို့ supported host compiler တစ်ခု လိုအပ်ပါတယ်။

NVIDIA က supported operating systems၊ architectures၊ compiler versions နဲ့ installation methods တွေကို official documentation မှာ ထုတ်ပြန်ထားပါတယ်။ ဒီ requirements တွေဟာ CUDA release အလိုက် ပြောင်းလဲနိုင်တဲ့အတွက် ကိုယ် install လုပ်မယ့် version အတွက် စစ်ဆေးဖို့ လိုအပ်ပါတယ်။

## 3. Supported Operating Systems နဲ့ CPU Architectures

CUDA ကို supported Linux နဲ့ Windows environments တွေမှာ အသုံးပြုနိုင်ပါတယ်။ ဒါပေမဲ့ Linux distributions နဲ့ CPU architectures တွေဟာ အပြန်အလှန် အစားထိုးသုံးလို့ မရပါဘူး—support ဆိုတာက CUDA release နဲ့ သီးခြား hardware platform ပေါ် မူတည်ပါတယ်။

ပုံမှန်တွေ့ရလေ့ရှိတဲ့ platform categories တွေကတော့—

- **Linux x86_64** — GPU servers နဲ့ AI infrastructure တွေမှာ အများဆုံး အသုံးပြုပါတယ်။
- **Linux ARM64** — အချို့ platform နဲ့ distribution တွေအတွက် support ပြုလုပ်ထားပါတယ်။
- **Windows** — supported NVIDIA GPU development environments ရှိပါတယ်။
- **Specialized NVIDIA platforms** — Jetson ကဲ့သို့ platform အချို့မှာ ကိုယ်ပိုင် software နဲ့ compatibility requirements တွေ ရှိပါတယ်။

Linux distribution တိုင်း၊ Windows release တိုင်း၊ CPU architecture တိုင်း၊ NVIDIA GPU တိုင်းက CUDA Toolkit version တိုင်းကို support ပြုလုပ်တယ်လို့ မယူဆသင့်ပါဘူး။

## 4. GCC ဘာကြောင့် လိုအပ်သလဲ။

GCC ဆိုတာ GNU Compiler Collection ဖြစ်ပါတယ်။ Linux development environments အများအပြားမှာ အသုံးပြုတဲ့ C နဲ့ C++ compilation tools တွေကို ပေးပါတယ်။

CUDA C/C++ code ကို compile လုပ်တဲ့အခါ NVIDIA ရဲ့ `nvcc` compiler ဟာ compatible host compiler တစ်ခုနဲ့ အတူလုပ်ဆောင်ပြီး program ရဲ့ CPU-side အပိုင်းတွေကို compile လုပ်ပါတယ်။ Support ပြုလုပ်ထားတဲ့ compiler version ကတော့ CUDA release ပေါ် မူတည်ပါတယ်။

Install မလုပ်ခင် system ကို စစ်ဆေးပါ—

```bash
gcc --version
g++ --version
```

GCC မရှိရင် သက်ဆိုင်ရာ Linux distribution အတွက် သင့်တော်တဲ့ development tools တွေကို install လုပ်ပါ။ Version က ရွေးချယ်ထားတဲ့ CUDA release နဲ့ မကိုက်ညီရင်တော့ version အသစ်ဆုံးက အလုပ်လုပ်မယ်လို့ မယူဆဘဲ supported compiler toolchain ကို အသုံးပြုပါ။

**အရေးကြီးတဲ့ ကွာခြားချက်** — GCC ဟာ CUDA development နဲ့ compilation အတွက် ပုံမှန်အားဖြင့် လိုအပ်ပေမဲ့ build ပြီးသား CUDA application တစ်ခုကို run ဖို့အတွက်တော့ ကိုယ်တိုင်ကိုယ်ကျ မလိုအပ်ပါဘူး။

## 5. CUDA Toolkit ကို Install လုပ်ခြင်း

NVIDIA က official installation instructions နဲ့ downloads တွေကို ပေးထားပါတယ်။

Official resources:

- CUDA downloads: https://developer.nvidia.com/cuda-downloads
- CUDA Linux Installation Guide: https://docs.nvidia.com/cuda/cuda-installation-guide-linux/
- CUDA Windows Installation Guide: https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/

Linux installation guide မှာ distribution package managers နဲ့ standalone installers အပါအဝင် နည်းလမ်းအမျိုးမျိုးကို ဖော်ပြထားပါတယ်။ ဘယ်နည်းလမ်းက မှန်ကန်သလဲဆိုတာက operating system၊ deployment requirements နဲ့ လက်ရှိ driver installation ပေါ် မူတည်ပါတယ်။

**အကြံပြုထားတဲ့ installation workflow**

1. NVIDIA GPU model ကို ဖော်ထုတ်ပါ။
2. Operating system နဲ့ CPU architecture ကို စစ်ဆေးပါ။
3. Install လုပ်ထားတဲ့ NVIDIA driver ကို စစ်ဆေးပါ။
4. Hardware, driver, operating system နဲ့ လိုအပ်တဲ့ framework တွေနဲ့ ကိုက်ညီတဲ့ CUDA Toolkit release ကို ရွေးချယ်ပါ။
5. ရွေးချယ်ထားတဲ့ installation method အတွက် NVIDIA ရဲ့ ညွှန်ကြားချက်အတိုင်း လိုက်နာပါ။
6. လိုအပ်ရင် environment ကို configure လုပ်ပါ။
7. Installation ကို verify လုပ်ပါ။
8. Supported CUDA sample သို့မဟုတ် အသေးစား GPU-enabled application တစ်ခုကို run လုပ်ပါ။

Production server ပေါ်မှာ driver သို့မဟုတ် toolkit အသစ်ကို မစဉ်းစားဘဲ တိုက်ရိုက် install မလုပ်ပါနဲ့။ အရင်ဆုံး လက်ရှိ configuration နဲ့ existing applications တွေ အသုံးပြုနေတဲ့ dependencies တွေကို စစ်ဆေးပါ။

## 6. Cloud Images တွေမှာ CUDA Components တွေ ကြိုတင်ပါဝင်နေနိုင်ခြင်း

Cloud providers အချို့က NVIDIA drivers, CUDA components သို့မဟုတ် AI frameworks တွေ ပါဝင်ပြီးသား GPU-enabled machine images တွေကို ပေးပါတယ်။

ဥပမာ—preconfigured image တစ်ခုမှာ အောက်ပါတွေထဲက အချို့သို့မဟုတ် အားလုံး ပါဝင်နေနိုင်ပါတယ်။

- NVIDIA GPU driver
- CUDA Toolkit
- cuDNN
- PyTorch သို့မဟုတ် TensorFlow
- GPU monitoring utilities

ဒါပေမဲ့ image နဲ့ ၎င်းရဲ့ version ပေါ်မူတည်ပြီး contents တွေက ကွာခြားနိုင်ပါတယ်။ VM ကို GPU ပါဝင်တဲ့အတွက် provision လုပ်ထားတာနဲ့ CUDA install ပြီးပြီလို့ မယူဆသင့်ပါဘူး။

ဘာမှ ထပ် install မလုပ်ခင် အမြဲတမ်း လက်ရှိ environment ကို အရင် verify လုပ်ပါ။

## 7. nvidia-smi နဲ့ CUDA ကို စစ်ဆေးခြင်း

`nvidia-smi` command ဟာ NVIDIA ရဲ့ System Management Interface utility ဖြစ်ပါတယ်။

Run လုပ်ပါ—

```bash
nvidia-smi
```

Output မှာ ပုံမှန်အားဖြင့် အောက်ပါတွေ ပြသပါတယ်—

- GPU model နဲ့ index
- Driver version
- Driver က ပြသတဲ့ CUDA compatibility version
- GPU utilization
- Memory usage
- Temperature
- Power usage
- အခြား supported GPU status information

GPU အများအပြားပါတဲ့ system တွေမှာတော့ ဒီ command က မြင်ရတဲ့ GPU တစ်ခုချင်းစီကို ပုံမှန်အားဖြင့် စာရင်းပြပါတယ်။

**အရေးကြီးဆုံး ကွာခြားချက်**

`nvidia-smi` က အောက်ပါအတိုင်း ပြသရင်—

```
Driver Version: 580.xx
CUDA Version: 13.x
```

`CUDA Version` field ဟာ install လုပ်ထားတဲ့ driver က support ပြုလုပ်တဲ့ CUDA version level ကို ဖော်ပြတာ ဖြစ်ပါတယ်။ CUDA Toolkit 13.x က local မှာ install ပြီးပြီလို့ သက်သေမပြပါဘူး။

အပေါ်က ဂဏန်းတွေဟာ ဥပမာသာ ဖြစ်ပြီး သီးခြား machine တစ်ခုအကြောင်း ပြောတာ မဟုတ်ပါဘူး။

CUDA Toolkit compiler ကို စစ်ဆေးချင်ရင်—

```bash
nvcc --version
```

`nvcc` install ပြီးပြီး shell ရဲ့ PATH ထဲမှာ ရရှိနိုင်ရင် compiler နဲ့ Toolkit version ကို ပြသပါတယ်။

`nvcc` က command not found ပြရင် GPU driver သို့မဟုတ် CUDA runtime ပျောက်နေပြီလို့ အလိုအလျောက် မဆိုလိုပါဘူး။ Full development Toolkit မ install ရသေးတာ ဖြစ်နိုင်သလို ၎င်းရဲ့ compiler က PATH ထဲမှာ မရှိတာလည်း ဖြစ်နိုင်ပါတယ်။

Toolkit version information ကို နောက်ထပ် နည်းလမ်းတစ်ခုအနေနဲ့ အောက်ပါ command နဲ့လည်း ကြည့်နိုင်ပါတယ်—

```bash
cat /usr/local/cuda/version.json
```

အဲဒီ file ရှိပြီး သက်ဆိုင်ရာ Toolkit ကို ကိုယ်စားပြုပါက Toolkit version information ကို ပြသပါတယ်။ သို့သော် CUDA Toolkit ကို မတူညီတဲ့ directory မှာ install လုပ်ထားနိုင်တဲ့အတွက် ဒီ command က system တိုင်းမှာ အလုပ်လုပ်မယ်လို့ မယူဆသင့်ပါဘူး။

## 8. လက်တွေ့ GPU Environment Verification Checklist

Linux GPU server တစ်ခုပေါ်မှာ အောက်ပါ commands တွေကို run လုပ်ပါ—

```bash
# Operating system
cat /etc/os-release

# CPU architecture
uname -m

# Linux kernel
uname -r

# NVIDIA GPU and driver status
nvidia-smi

# GPU inventory
nvidia-smi -L

# CUDA compiler / Toolkit version
nvcc --version

# Toolkit version file (if installed at the default location)
cat /usr/local/cuda/version.json

# Host compiler
gcc --version

# NVIDIA PCI devices
lspci | grep -i nvidia
```

သက်ဆိုင်ရာ utility သို့မဟုတ် development package မ install ရသေးရင် commands အချို့က မရရှိနိုင်ပါဘူး။

**Command တစ်ခုချင်းစီက ဘာကို ပြောပြသလဲ**

| Command | ရည်ရွယ်ချက် |
|---|---|
| `cat /etc/os-release` | Linux distribution နဲ့ release ကို ဖော်ထုတ်ပေးပါတယ် |
| `uname -m` | x86_64 သို့မဟုတ် aarch64 ကဲ့သို့ system architecture ကို ဖော်ထုတ်ပေးပါတယ် |
| `uname -r` | Run နေတဲ့ Linux kernel ကို ပြပါတယ် |
| `nvidia-smi` | GPU status နဲ့ driver information ကို ပြပါတယ် |
| `nvidia-smi -L` | မြင်ရတဲ့ NVIDIA GPUs တွေကို စာရင်းပြပါတယ် |
| `nvcc --version` | ရရှိနိုင်ရင် install လုပ်ထားတဲ့ CUDA compiler version ကို ပြပါတယ် |
| `cat /usr/local/cuda/version.json` | Default location မှာ install ထားရင် Toolkit version information ကို ပြပါတယ် |
| `gcc --version` | Host compiler ကို စစ်ဆေးပေးပါတယ် |
| `lspci \| grep -i nvidia` | NVIDIA PCI devices တွေကို ဖော်ထုတ်ရာမှာ ကူညီပေးပါတယ် |

## 9. PyTorch သို့မဟုတ် TensorFlow နဲ့ CUDA ကို အသုံးပြုခြင်း

Environment ကို မှန်ကန်စွာ configure ပြီးရင် compatible GPU-enabled framework တစ်ခုကို အသုံးပြုနိုင်ပါပြီ။

ဥပမာ—PyTorch က ရိုးရှင်းတဲ့ check တစ်ခု ပေးပါတယ်—

```python
import torch

print("PyTorch version:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

ဒါက install လုပ်ထားတဲ့ PyTorch environment ဟာ CUDA-enabled GPU ကို access လုပ်နိုင်/မနိုင် စစ်ဆေးပေးပါတယ်။ ၎င်းဟာ framework-level check ဖြစ်ပြီး driver နဲ့ Toolkit ကို သီးခြား verify လုပ်တာကို အစားမထိုးပါဘူး။

Compatible framework package တွေမှာ သီးခြား CUDA runtime libraries တွေ ပါဝင်နိုင်သလို ၎င်းတို့ပေါ် မူတည်နိုင်ပါတယ်။ ဒါကြောင့် prebuilt PyTorch application တစ်ခုကို run ဖို့အတွက် system-wide CUDA Toolkit အပြည့်အစုံ install လုပ်ဖို့ အမြဲတမ်း မလိုအပ်ပါဘူး။ CUDA code compile လုပ်တာမျိုး၊ Toolkit လိုအပ်တဲ့ extensions တွေ develop လုပ်တာမျိုးဆိုရင်တော့ ပုံမှန်အားဖြင့် လိုအပ်လာပါတယ်။

## 10. အဖြစ်များတဲ့ Troubleshooting Scenarios

**Scenario A — nvidia-smi အလုပ်လုပ်ပေမဲ့ nvcc မရှိဘူး**

ဖြစ်နိုင်တဲ့ ရှင်းလင်းချက်—driver install ပြီးပေမဲ့ CUDA development Toolkit မ install ရသေးတာ သို့မဟုတ် `nvcc` က PATH ထဲမှာ မရှိတာ ဖြစ်နိုင်ပါတယ်။

နောက်တစ်ဆင့်—Toolkit installation နဲ့ environment configuration ကို စစ်ဆေးပါ။

**Scenario B — nvidia-smi အလုပ်မလုပ်ဘူး**

ဖြစ်နိုင်တဲ့ အကြောင်းရင်းတွေ—

- GPU ကို operating system ဆီ ဖော်ပြထားခြင်း (expose) မရှိတာ။
- NVIDIA driver ပျောက်နေတာ သို့မဟုတ် load မလုပ်ရသေးတာ။
- Driver မှာ kernel-module သို့မဟုတ် compatibility ပြဿနာ ရှိတာ။
- လိုအပ်တဲ့ GPU access မပါဝင်တဲ့ container သို့မဟုတ် VM ထဲမှာ system run နေတာ။

နောက်တစ်ဆင့်—Toolkit မ install ခင် driver installation နဲ့ GPU visibility ကို စစ်ဆေးပါ။

**Scenario C — PyTorch က CUDA unavailable လို့ ပြတယ်**

ဖြစ်နိုင်တဲ့ အကြောင်းရင်းတွေ—

- CPU-only PyTorch package install လုပ်ထားတာ။
- Driver က package က မျှော်လင့်ထားတဲ့ runtime နဲ့ မကိုက်ညီတာ။
- လက်ရှိ process သို့မဟုတ် container အတွက် GPU access မရရှိနိုင်တာ။
- Environment သို့မဟုတ် library configuration ပြဿနာ ရှိတာ။

နောက်တစ်ဆင့်—`nvidia-smi`၊ PyTorch installation နဲ့ framework ရဲ့ compatibility requirements တွေကို verify လုပ်ပါ။

**Scenario D — Compiler compatibility errors**

ဖြစ်နိုင်တဲ့ ရှင်းလင်းချက်—install လုပ်ထားတဲ့ host compiler က ရွေးချယ်ထားတဲ့ CUDA Toolkit အတွက် supported range အပြင်ကို ရောက်နေတာ ဖြစ်နိုင်ပါတယ်။

နောက်တစ်ဆင့်—သက်ဆိုင်ရာ CUDA release ရဲ့ official compiler compatibility table ကို စစ်ဆေးပြီး supported compiler တစ်ခုကို configure လုပ်ပါ။

## 11. Review Questions

1. CUDA Toolkit install လုပ်ဖို့ အဓိက prerequisites တွေက ဘာတွေလဲ။
2. Operating system နဲ့ CPU architecture ကို ဘာကြောင့် စစ်ဆေးရမလဲ။
3. CUDA development မှာ GCC ရဲ့ အခန်းကဏ္ဍက ဘာလဲ။
4. `nvidia-smi` က full CUDA Toolkit install ပြီးပြီဆိုတာကို သက်သေပြသလား။
5. `nvcc --version` က ဘာကို ပြောပြသလဲ။
6. Cloud GPU image တစ်ခုမှာ CUDA components တွေ ကြိုတင်ပါဝင်နေနိုင်တာ ဘာကြောင့်လဲ။
7. `nvidia-smi` အလုပ်လုပ်ပေမဲ့ `nvcc` မရရှိရင် ဘာတွေ စစ်ဆေးသင့်သလဲ။
8. Prebuilt PyTorch application တစ်ခုကို run ဖို့ full CUDA Toolkit install လုပ်ဖို့ အမြဲတမ်း လိုအပ်သလား။
9. Production servers တွေမှာ driver သို့မဟုတ် CUDA versions တွေ မပြောင်းခင် ဘာကြောင့် အရင်စစ်ဆေးသင့်သလဲ။
10. PyTorch က GPU ကို access လုပ်နိုင်ကြောင်း ဘယ်လို verify လုပ်မလဲ။

## Module Summary

CUDA installation အတွက် compatible NVIDIA hardware၊ supported operating system နဲ့ သင့်တော်တဲ့ software components တွေ လိုအပ်ပါတယ်။ Linux မှာ CUDA development အတွက် compatible host compiler လည်း လိုအပ်ပါတယ်။

`nvidia-smi` ဟာ GPU visibility၊ driver information နဲ့ runtime status တွေကို စစ်ဆေးရာမှာ အသုံးဝင်ပါတယ်။ ဒါပေမဲ့ ၎င်းပြတဲ့ CUDA compatibility version ဟာ install လုပ်ထားတဲ့ Toolkit version နဲ့ မတူပါဘူး။ `nvcc` ရရှိနိုင်ရင် `nvcc --version` နဲ့ CUDA compiler ကို စစ်ဆေးပြီး applications တွေက GPU access လုပ်နိုင်ခြင်းကိုတော့ framework-specific checks တွေနဲ့ verify လုပ်ပါ။

အကြံပြုလိုတဲ့ practice—အရင်ဆုံး GPU၊ driver၊ OS၊ architecture နဲ့ compiler တွေကို inventory လုပ်ပါ။ ပြီးရင် workload အတွက် လိုအပ်တဲ့ components တွေကိုသာ install လုပ်ပြီး AI applications တွေ run မချခင် result ကို verify လုပ်ပါ။

## Self-Check Questions (အဖြေများပါဝင်သည်)

1. **`nvidia-smi` မှာ "CUDA Version: 13.x" ပြရင် CUDA Toolkit 13.x install ပြီးပြီလို့ ဆိုလိုသလား။** — မဆိုလိုပါဘူး။ အဲဒီ field က driver က support ပြုလုပ်တဲ့ CUDA version level ကို ဖော်ပြတာဖြစ်ပါတယ်။ Toolkit version ကို `nvcc --version` (သို့မဟုတ် default location ဆိုရင် `cat /usr/local/cuda/version.json`) နဲ့ စစ်ဆေးရပါမယ်။
2. **`nvcc` command not found ပြရင် GPU driver ပျက်သွားပြီလို့ ဆိုလိုသလား။** — မဆိုလိုပါဘူး။ Driver အလုပ်လုပ်နေဆဲ ဖြစ်နိုင်ပြီး development Toolkit မ install ရသေးတာ သို့မဟုတ် compiler က PATH ထဲမပါတာ ဖြစ်နိုင်ပါတယ်။
3. **Prebuilt PyTorch application run ဖို့ full CUDA Toolkit လိုသလား။** — ပုံမှန်အားဖြင့် မလိုပါဘူး။ Framework package တွေမှာ လိုအပ်တဲ့ CUDA runtime libraries ပါဝင်/မူတည်နိုင်ပါတယ်။ CUDA code compile မှသာ Toolkit လိုအပ်လေ့ရှိပါတယ်။
4. **Linux မှာ CUDA C/C++ compile လုပ်ဖို့ GCC ဘာကြောင့် လိုသလဲ။** — `nvcc` က program ရဲ့ CPU-side အပိုင်းတွေကို compile ဖို့ compatible host compiler လိုအပ်လို့ ဖြစ်ပါတယ်။ GCC version က CUDA release နဲ့ ကိုက်ညီရပါမယ်။
5. **Cloud GPU VM အသစ်တစ်ခုမှာ CUDA install ပြီးပြီလို့ ယူဆလို့ရသလား။** — မရပါဘူး။ Image ပေါ်မူတည်ပြီး driver, Toolkit, cuDNN, frameworks တွေ အပြည့်/တစ်ပိုင်းတစ်စ ပါဝင်နိုင်သလို လုံးဝမပါဝင်ဘဲလည်း ရှိနိုင်ပါတယ်။ အမြဲတမ်း environment ကို အရင် verify လုပ်ပါ။

## English Version — Installing CUDA Toolkit and Verifying the GPU Environment

## 1. Learning Objectives

By the end of this module, you should be able to:

- Identify the prerequisites for installing the CUDA Toolkit.
- Understand supported GPU hardware, operating systems, and CPU architectures.
- Explain why GCC is required for CUDA development on Linux.
- Check whether an NVIDIA GPU and driver are available.
- Understand the difference between the CUDA version reported by `nvidia-smi` and the installed CUDA Toolkit version.
- Verify a CUDA development environment before using PyTorch or TensorFlow.

## 2. CUDA Installation Requirements

Before installing CUDA, check that your system meets the requirements for the particular CUDA Toolkit release you intend to use.

The main prerequisites are:

- **CUDA-capable NVIDIA GPU** — The GPU must be supported by the selected CUDA release.
- **Supported operating system** — The Linux distribution or Windows version must be supported by that release.
- **Compatible NVIDIA driver** — The installed driver must support the intended CUDA applications and toolkit requirements.
- **CUDA Toolkit** — Required when developing or compiling CUDA applications locally.
- **Compatible development tools** — On Linux, CUDA C/C++ development generally requires a supported host compiler such as GCC.

NVIDIA publishes the supported operating systems, architectures, compiler versions, and installation methods in its official documentation. These requirements change across CUDA releases, so they must be checked for the version being installed.

## 3. Supported Operating Systems and CPU Architectures

CUDA is available for supported Linux and Windows environments. Linux distributions and CPU architectures are not interchangeable, however: support depends on the CUDA release and the specific hardware platform.

Common platform categories include:

- **Linux x86_64** — Commonly used for GPU servers and AI infrastructure.
- **Linux ARM64** — Supported for particular platforms and distributions.
- **Windows** — Supported NVIDIA GPU development environments are available.
- **Specialized NVIDIA platforms** — Some platforms, such as Jetson, have their own software and compatibility requirements.

Do not assume that every Linux distribution, Windows release, CPU architecture, or NVIDIA GPU supports every CUDA Toolkit version.

## 4. Why Is GCC Needed?

GCC is the GNU Compiler Collection. It provides C and C++ compilation tools used by many Linux development environments.

When compiling CUDA C/C++ code, NVIDIA's `nvcc` compiler works with a compatible host compiler to compile the CPU-side portions of the program. The supported compiler version depends on the CUDA release.

Check the system before installation:

```bash
gcc --version
g++ --version
```

If GCC is missing, install the appropriate development tools for your Linux distribution. If the version is incompatible with the chosen CUDA release, use a supported compiler toolchain rather than assuming the newest version will work.

**Important distinction** — GCC is generally required for CUDA development and compilation, but it is not inherently required just to run an already-built CUDA application.

## 5. Installing CUDA Toolkit

NVIDIA provides official installation instructions and downloads.

Official resources:

- CUDA downloads: https://developer.nvidia.com/cuda-downloads
- CUDA Linux Installation Guide: https://docs.nvidia.com/cuda/cuda-installation-guide-linux/
- CUDA Windows Installation Guide: https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/

The Linux installation guide documents several approaches, including distribution package managers and standalone installers. The correct approach depends on the operating system, deployment requirements, and existing driver installation.

**Recommended installation workflow**

1. Identify the NVIDIA GPU model.
2. Check the operating system and CPU architecture.
3. Check the installed NVIDIA driver.
4. Choose a CUDA Toolkit release compatible with the hardware, driver, operating system, and required framework.
5. Follow NVIDIA's instructions for the selected installation method.
6. Configure the environment if required.
7. Verify the installation.
8. Run a supported CUDA sample or a small GPU-enabled application.

Avoid installing a new driver or toolkit blindly on a production server. First check the current configuration and any dependencies used by existing applications.

## 6. Cloud Images May Already Include CUDA Components

Some cloud providers offer GPU-enabled machine images that include NVIDIA drivers, CUDA components, or AI frameworks.

For example, a preconfigured image may contain some or all of the following:

- NVIDIA GPU driver
- CUDA Toolkit
- cuDNN
- PyTorch or TensorFlow
- GPU monitoring utilities

However, the contents depend on the image and its version. Do not assume CUDA is installed simply because the virtual machine has been provisioned with a GPU.

Always verify the actual environment before installing anything else.

## 7. Checking CUDA with nvidia-smi

The `nvidia-smi` command is NVIDIA's System Management Interface utility.

Run:

```bash
nvidia-smi
```

The output commonly displays:

- GPU model and index
- Driver version
- CUDA compatibility version reported by the driver
- GPU utilization
- Memory usage
- Temperature
- Power usage
- Other supported GPU status information

For systems with multiple GPUs, the command normally lists each visible GPU.

**The most important distinction**

If `nvidia-smi` displays:

```
Driver Version: 580.xx
CUDA Version: 13.x
```

the `CUDA Version` field indicates the CUDA version level supported by the installed driver. It does not prove that CUDA Toolkit 13.x is installed locally.

The exact numbers above are illustrative, not a statement about a particular machine.

To check the CUDA Toolkit compiler, run:

```bash
nvcc --version
```

If `nvcc` is installed and available in the shell's PATH, it reports the compiler and Toolkit version.

If `nvcc` returns *command not found*, that does not automatically mean the GPU driver or CUDA runtime is missing. The full development Toolkit may not be installed, or its compiler may not be in the PATH.

Another way to inspect Toolkit version information is:

```bash
cat /usr/local/cuda/version.json
```

If that file exists and represents the relevant Toolkit, it reports Toolkit version information. Because the CUDA Toolkit can be installed in a different directory, however, do not assume this command works on every system.

## 8. A Practical GPU Environment Verification Checklist

Run the following commands on a Linux GPU server:

```bash
# Operating system
cat /etc/os-release

# CPU architecture
uname -m

# Linux kernel
uname -r

# NVIDIA GPU and driver status
nvidia-smi

# GPU inventory
nvidia-smi -L

# CUDA compiler / Toolkit version
nvcc --version

# Toolkit version file (if installed at the default location)
cat /usr/local/cuda/version.json

# Host compiler
gcc --version

# NVIDIA PCI devices
lspci | grep -i nvidia
```

Some commands may be unavailable if the corresponding utility or development package has not been installed.

**What each command tells you**

| Command | Purpose |
|---|---|
| `cat /etc/os-release` | Identifies the Linux distribution and release |
| `uname -m` | Identifies the system architecture, such as x86_64 or aarch64 |
| `uname -r` | Shows the running Linux kernel |
| `nvidia-smi` | Reports GPU status and driver information |
| `nvidia-smi -L` | Lists visible NVIDIA GPUs |
| `nvcc --version` | Reports the installed CUDA compiler version when available |
| `cat /usr/local/cuda/version.json` | Reports Toolkit version information when installed at the default location |
| `gcc --version` | Checks the host compiler |
| `lspci \| grep -i nvidia` | Helps identify NVIDIA PCI devices |

## 9. Using CUDA with PyTorch or TensorFlow

Once the environment is configured correctly, you can use a compatible GPU-enabled framework.

For example, PyTorch provides a simple check:

```python
import torch

print("PyTorch version:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

This checks whether the installed PyTorch environment can access a CUDA-enabled GPU. It is a framework-level check, not a replacement for verifying the driver and Toolkit separately.

A compatible framework package may include or depend on specific CUDA runtime libraries. Consequently, installing the full CUDA Toolkit system-wide is not always necessary merely to run a prebuilt PyTorch application. It is commonly needed when compiling CUDA code or developing extensions that require the Toolkit.

## 10. Common Troubleshooting Scenarios

**Scenario A — nvidia-smi works, but nvcc is missing**

Possible explanation: the driver is installed, but the CUDA development Toolkit is not installed or `nvcc` is not in the PATH.

Next step: check the Toolkit installation and environment configuration.

**Scenario B — nvidia-smi fails**

Possible causes include:

- The GPU is not exposed to the operating system.
- The NVIDIA driver is missing or not loaded.
- The driver has a kernel-module or compatibility problem.
- The system is running in a container or VM without the required GPU access.

Next step: inspect the driver installation and GPU visibility before installing the Toolkit.

**Scenario C — PyTorch reports CUDA unavailable**

Possible causes include:

- A CPU-only PyTorch package is installed.
- The driver is incompatible with the runtime expected by the package.
- GPU access is unavailable to the current process or container.
- An environment or library configuration problem exists.

Next step: verify `nvidia-smi`, the PyTorch installation, and the framework's compatibility requirements.

**Scenario D — Compiler compatibility errors**

Possible explanation: the installed host compiler is outside the supported range for the selected CUDA Toolkit.

Next step: check the CUDA release's official compiler compatibility table and configure a supported compiler.

## 11. Review Questions

1. What are the main prerequisites for installing CUDA Toolkit?
2. Why must the operating system and CPU architecture be checked?
3. What is the role of GCC in CUDA development?
4. Does `nvidia-smi` prove that the full CUDA Toolkit is installed?
5. What does `nvcc --version` tell you?
6. Why might a cloud GPU image already contain CUDA components?
7. What should you check if `nvidia-smi` works but `nvcc` is unavailable?
8. Is installing the full CUDA Toolkit always necessary to run a prebuilt PyTorch application?
9. Why should production servers be checked before changing drivers or CUDA versions?
10. How would you verify that PyTorch can access a GPU?

## Module Summary

CUDA installation requires compatible NVIDIA hardware, a supported operating system, and the appropriate software components. CUDA development on Linux also requires a compatible host compiler.

`nvidia-smi` is useful for checking GPU visibility, driver information, and runtime status. However, its reported CUDA compatibility version is not the same as the installed Toolkit version. Use `nvcc --version` to check the CUDA compiler when it is available, and use framework-specific checks to verify GPU access from applications.

Recommended practice: Start by inventorying the GPU, driver, OS, architecture, and compiler. Then install only the components required by your workload and verify the result before running AI applications.

## Self-Check Questions (with answers)

1. **Does "CUDA Version: 13.x" in `nvidia-smi` mean CUDA Toolkit 13.x is installed?** — No. That field shows the CUDA version level supported by the driver. Check the Toolkit with `nvcc --version` (or `cat /usr/local/cuda/version.json` at the default location).
2. **Does `nvcc: command not found` mean the GPU driver is broken?** — No. The driver can be working fine; the development Toolkit may simply be uninstalled, or the compiler may not be in the PATH.
3. **Is the full CUDA Toolkit required to run a prebuilt PyTorch application?** — Usually not. Framework packages may include or depend on the required CUDA runtime libraries. The Toolkit is commonly needed when compiling CUDA code.
4. **Why is GCC needed to compile CUDA C/C++ on Linux?** — `nvcc` needs a compatible host compiler for the CPU-side portions of the program, and the supported GCC version depends on the CUDA release.
5. **Can you assume CUDA is ready on a fresh cloud GPU VM?** — No. Depending on the image, the driver, Toolkit, cuDNN, and frameworks may be fully preloaded, partially present, or absent. Always verify the environment first.

---

## Technical review notes

- Source transcript (msg_2587) မှ Module 21 content အားလုံးကို ပုံတူကူးယူပြီး နှစ်ဘာသာဖြင့် ရေးသားထားသည်။ Section numbering 1–11 နှင့် Module Summary ကို source အတိုင်း ထိန်းထားသည်။
- `nvidia-smi` ၏ "CUDA Version" field သည် driver-supported CUDA compatibility level သာဖြစ်ကြောင်းနှင့် "nvcc missing ≠ driver broken" ဆိုသော distinction ကို source အတိုင်း ထိန်းထားသည်။
- `Driver Version: 580.xx` / `CUDA Version: 13.x` သည် source တွင် illustrative သာဖြစ်ကြောင်း caveat ကို ထိန်းထားသည်။
- `cat /usr/local/cuda/version.json` fragment ကို msg_2593 (nvidia-smi deep-dive module) မှ ဖြည့်စွက်ထားပြီး "Toolkit ကို အခြား directory တွင် install ထားနိုင်၍ system တိုင်းတွင် အလုပ်မလုပ်နိုင်" ဆိုသော source caveat ကို ထိန်းထားသည်။
- GCC/CUDA compiler compatibility သည် CUDA release အလိုက် ပြောင်းလဲနိုင်သောကြောင့် specific version range များကို မဖော်ပြဘဲ official compatibility table တွင် စစ်ဆေးရန် ဖော်ပြထားသည် (needs verification မှ လွဲ၍ source အတိုင်း)။
- Commands များအားလုံး (nvidia-smi, nvidia-smi -L, nvcc --version, gcc --version, uname, lspci, torch.cuda checks) သည် source တွင်ပါရှိသော commands များသာ ဖြစ်သည်။
