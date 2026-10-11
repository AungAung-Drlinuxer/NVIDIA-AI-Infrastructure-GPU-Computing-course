# Module 18 — NVIDIA vGPU vs. MIG: MIG Profiles and Operations

> Part III · GPU Virtualization — topic notes

## မြန်မာဘာသာ version — MIG Profiles နှင့် Operations

## 1. လေ့လာမည့် ရည်ရွယ်ချက်များ

ဒီ Module မှာ အောက်ပါအချက်တွေကို လေ့လာရပါမယ်။

- vGPU နဲ့ MIG ရဲ့ Isolation Model ကို နှိုင်းယှဉ်ခြင်း
- GPU Model အလိုက် MIG Support ကို စစ်ဆေးခြင်း
- GPU Memory နဲ့ Streaming Multiprocessors (SMs) ကို MIG Profile အလိုက် ခွဲဝေခြင်း
- nvidia-smi နဲ့ MIG Mode ကို စစ်ဆေးခြင်း၊ Enable လုပ်ခြင်းနဲ့ Instance ဖန်တီးခြင်း
- VM-based GPU Virtualization နဲ့ Container-based GPU Partitioning ကို ရွေးချယ်ခြင်း

## 2. MIG Profile ဆိုတာဘာလဲ?

MIG Profile ဆိုတာ Physical GPU ရဲ့ Compute နဲ့ Memory Resource တွေကို ဘယ်လိုအချိုးအစားနဲ့ Instance တစ်ခုအဖြစ် ခွဲဝေမလဲဆိုတာ သတ်မှတ်ပေးတဲ့ Configuration ဖြစ်ပါတယ်။

MIG Profile အမည်ကို အောက်ပါအတိုင်း ဖတ်နိုင်ပါတယ်။ ဥပမာ — `3g.20gb`

- `3g` — သတ်မှတ်ထားတဲ့ GPU Compute Slice အချိုးအစားကို ဖော်ပြတယ်။
- `20gb` — Instance အတွက် သတ်မှတ်ထားတဲ့ GPU Memory အရွယ်အစားကို ဖော်ပြတယ်။

ဒီနေရာမှာ `3g` ကို GPU Core 3 ခုလို့ မဖတ်ရပါဘူး။ ၎င်းဟာ သတ်မှတ်ထားတဲ့ MIG GPU Slice အရွယ်အစားကို ဖော်ပြတာဖြစ်ပါတယ်။ အတွင်းပိုင်းမှာ Streaming Multiprocessors (SMs) နဲ့ အခြား Hardware Resource များကို Profile အလိုက် ခွဲဝေပေးထားပါတယ်။

**A100 40GB MIG Profile ဥပမာများ** — NVIDIA A100-SXM4-40GB မှာ အောက်ပါ Profile များကို တွေ့နိုင်ပါတယ်။

| MIG Profile | GPU Memory | SM Allocation | Maximum Instances |
|---|---|---|---|
| 1g.5gb | 5 GB အတန်းအစား | 1/7 | 7 |
| 2g.10gb | 10 GB အတန်းအစား | 2/7 | 3 |
| 3g.20gb | 20 GB အတန်းအစား | 3/7 | 2 |
| 4g.20gb | 20 GB အတန်းအစား | 4/7 | 1 |
| 7g.40gb | 40 GB အတန်းအစား | 7/7 | 1 |

Profile အမည်ထဲက Memory ပမာဏဟာ သတ်မှတ်ထားတဲ့ အတန်းအစားကို ဖော်ပြတာဖြစ်ပြီး nvidia-smi မှာ မြင်ရတဲ့ Usable Memory ပမာဏဟာ အနည်းငယ်ကွာနိုင်ပါတယ်။ Profile တွေဟာလည်း GPU Model အားလုံးမှာ တူညီခြင်း မရှိပါဘူး။

Source: NVIDIA Supported MIG Profiles (https://docs.nvidia.com/datacenter/tesla/mig-user-guide/latest/supported-mig-profiles.html)

## 3. nvidia-smi ဖြင့် MIG Support ကို စစ်ဆေးခြင်း

ပထမဆုံး GPU Model နဲ့ Driver ကို စစ်ဆေးပါ။

```bash
nvidia-smi
```

GPU 0 ရဲ့ MIG Mode ကို စစ်ဆေးရန် —

```bash
nvidia-smi -i 0 --query-gpu=name,mig.mode.current --format=csv
```

MIG ဆိုင်ရာ အသေးစိတ်အချက်အလက်ကို ကြည့်ရန် —

```bash
nvidia-smi -q | grep -A 8 -i MIG
```

GPU က MIG ကို Support လုပ်သလားဆိုတာကို NVIDIA ရဲ့ Supported GPU List နဲ့ တွဲစစ်ဆေးသင့်ပါတယ်။

**သတိပြုရန်:** nvidia-smi ကို အသုံးပြုနိုင်ခြင်းတစ်ခုတည်းနဲ့ GPU က MIG ကို Support လုပ်တယ်လို့ မဆိုနိုင်ပါဘူး။ GPU Model ကို အရင်စစ်ဆေးရပါမယ်။ nvidia-smi မှာ MIG ဆိုင်ရာ field တစ်ခု N/A ပြနေရင် feature မရှိခြင်း သို့မဟုတ် မသက်ဆိုင်ခြင်း ဖြစ်နိုင်ပြီး error လို့ အလိုအလျောက် မယူဆသင့်ပါဘူး။

## 4. MIG Mode ကို Enable လုပ်ခြင်း

MIG ကို Support လုပ်တဲ့ GPU ဖြစ်ကြောင်း အတည်ပြုပြီးနောက် GPU 0 မှာ MIG Mode ကို Enable လုပ်နိုင်ပါတယ်။

```bash
sudo nvidia-smi -i 0 -mig 1
```

ပြန်စစ်ဆေးရန် —

```bash
nvidia-smi -i 0 --query-gpu=name,mig.mode.current --format=csv
```

**သတိပြုရန် —**

- GPU Model နဲ့ Architecture ပေါ်မူတည်ပြီး GPU Reset သို့မဟုတ် Reboot လိုအပ်နိုင်ပါတယ်။
- လက်ရှိ GPU ကို အသုံးပြုနေတဲ့ Application သို့မဟုတ် Monitoring Process တွေကြောင့် Enable လုပ်တာ မအောင်မြင်နိုင်ပါဘူး။
- MIG Mode ကို Enable လုပ်ထားရုံနဲ့ MIG Instance တွေ အလိုအလျောက် ဖန်တီးပြီးသား မဖြစ်ပါဘူး။
- Production GPU ပေါ်မှာ MIG Configuration ပြောင်းလဲခြင်းမလုပ်ခင် Workload ကို ရပ်တန့်ဖို့နဲ့ Maintenance Window စီစဉ်ဖို့ လိုအပ်နိုင်ပါတယ်။

## 5. MIG Profile များကို စာရင်းကြည့်ခြင်း

MIG ကို Support လုပ်ပြီး Enable လုပ်ထားတဲ့ GPU မှာ အသုံးပြုနိုင်တဲ့ GPU Instance Profile များကို ကြည့်ရန် —

```bash
sudo nvidia-smi mig -lgip
```

GPU Instance များကို ဖန်တီးထားနိုင်တဲ့ Placement ကို စစ်ဆေးရန် —

```bash
sudo nvidia-smi mig -lgipp
```

ဒီ Commands တွေက GPU က ဘယ် Profile တွေကို Support လုပ်သလဲ၊ ဘယ် Profile ပေါင်းစပ်မှုတွေ ဖန်တီးနိုင်သလဲဆိုတာ နားလည်ဖို့ အထောက်အကူဖြစ်ပါတယ်။

## 6. MIG Instance ဖန်တီးခြင်း

ဥပမာ A100 40GB ပေါ်မှာ `3g.20gb` Profile ကို အသုံးပြုပြီး MIG Instance နှစ်ခု ဖန်တီးလိုပါက —

```bash
sudo nvidia-smi mig -cgi 3g.20gb,3g.20gb -C
```

ဒီ Command မှာ —

- `mig` — MIG Management Command
- `-cgi` — GPU Instance Profile များကို ဖန်တီးရန်
- `3g.20gb,3g.20gb` — ဖန်တီးလိုတဲ့ GPU Instance Profile နှစ်ခု
- `-C` — သက်ဆိုင်ရာ Compute Instances များကိုလည်း ဖန်တီးရန်

Command ရဲ့ အောင်မြင်မှုဟာ GPU Model, Driver Version, လက်ရှိ MIG Configuration နဲ့ Available Placement ပေါ်မူတည်ပါတယ်။

ဖန်တီးထားတဲ့ GPU Instance များကို ကြည့်ရန် —

```bash
sudo nvidia-smi mig -lgi
```

MIG Device များကို စာရင်းကြည့်ရန် —

```bash
nvidia-smi -L
```

**သတိပြုရန်** — အထက်ပါ Profile ဥပမာကို MIG ကို Support လုပ်တဲ့ သင့်လျော်သော GPU ပေါ်မှာသာ အသုံးပြုပါ။ Tesla T4 သို့မဟုတ် A10G ပေါ်မှာ ဒီ MIG Configuration ကို ဖန်တီးလို့ မရပါဘူး။

Source: NVIDIA MIG Getting Started Guide (https://docs.nvidia.com/datacenter/tesla/mig-user-guide/getting-started-with-mig.html)

## 7. GPU အများအပြားရှိတဲ့ Server မှာ MIG

Server တစ်လုံးမှာ MIG ကို Support လုပ်တဲ့ GPU 4 လုံး ရှိတယ်ဆိုပါစို့။ GPU တစ်လုံးစီမှာ MIG Instance 7 ခုအထိ ဖန်တီးနိုင်တဲ့ Supported Configuration ဖြစ်တယ်ဆိုရင် သီအိုရီအရ —

```
4 GPUs × 7 instances = 28 instances
```

ဒါပေမဲ့ ဒီတွက်ချက်မှုဟာ GPU တစ်လုံးစီမှာ 7 Instances အပြည့် ဖန်တီးနိုင်တယ်လို့ ယူဆထားတဲ့ အခြေအနေဖြစ်ပါတယ်။ အမှန်တကယ် Instance အရေအတွက်က GPU Model, Profile Size, Placement နဲ့ Resource Availability ပေါ်မူတည်ပါတယ်။

## 8. Operational သတိပြုရန် အချက်များ

- **Maintenance window** — Production GPU ပေါ်မှာ MIG Mode ပြောင်းခြင်း၊ Profile ဖန်တီးခြင်း ပြုလုပ်မယ်ဆိုရင် workload ကို အရင်ရပ်ပြီး maintenance window စီစဉ်ပါ။
- **Mode ≠ Instances** — MIG Mode enable ထားရုံနဲ့ instance တွေ ဖန်တီးပြီးမဟုတ်ပါဘူး။ Profile တွေကို သီးခြားဖန်တီးရပါမယ်။
- **Blocking processes** — GPU ကို အသုံးပြုနေတဲ့ application သို့မဟုတ် monitoring process တွေက enable/change ကို တားဆီးနိုင်ပါတယ်။
- **Placement rules** — ဖန်တီးချင်တဲ့ profile ပေါင်းစပ်မှုဟာ GPU ရဲ့ supported placement နဲ့ ကိုက်ညီမှ အောင်မြင်ပါမယ်။ `-lgipp` နဲ့ ကြိုစစ်ဆေးနိုင်ပါတယ်။
- **Licensing** — vGPU အတွက် NVIDIA vGPU Software License လိုအပ်နိုင်ပါတယ်။ MIG feature အတွက်လည်း သက်ဆိုင်ရာ platform နဲ့ software requirements ကို စစ်ဆေးပါ။
- **Support matrix** — vGPU/MIG ပေါင်းစပ် configuration များအတွက် NVIDIA vGPU Product Support Matrix (https://docs.nvidia.com/vgpu/) ကို စစ်ဆေးပါ။

## 9. VM-based နဲ့ Container-based Partitioning ကို ရွေးချယ်ခြင်း

**vGPU ကို ရွေးချယ်သင့်တဲ့အခြေအနေများ**

- Virtual Desktop Infrastructure (VDI)
- Virtual Workstation
- VM အတွင်း AI/ML Workload များ
- Hypervisor ကနေ VM များအတွက် GPU Device များကို စီမံခန့်ခွဲလိုတဲ့အခါ

**MIG ကို ရွေးချယ်သင့်တဲ့အခြေအနေများ**

- Bare-metal Linux ပေါ်က Containerized AI Workload များ
- HPC နဲ့ AI Inference Workload များ
- Workload တစ်ခုချင်းစီအတွက် သတ်မှတ်ထားတဲ့ GPU Compute နဲ့ Memory Resource လိုအပ်တဲ့အခါ
- Hardware-backed Resource Partitioning ကို ဦးစားပေးတဲ့အခါ

အချို့ Supported Platform များမှာ MIG-backed vGPU ကို အသုံးပြုနိုင်တာကြောင့် ဒီနည်းပညာနှစ်ခုဟာ အမြဲတမ်း အပြန်အလှန်ဖယ်ထုတ်ရမယ့် ရွေးချယ်စရာတွေ မဟုတ်ပါဘူး။

## 10. Module Summary

- vGPU က VM များထံ Virtual GPU Device များ တင်ပြပေးတယ်။
- MIG က Supported Physical GPU ကို Hardware GPU Instances များအဖြစ် ခွဲခြားတယ်။
- MIG Profile က Compute Slice နဲ့ GPU Memory Resource ကို သတ်မှတ်ပေးတယ်။
- MIG Instance အများဆုံး အရေအတွက်ဟာ GPU Model နဲ့ Profile ပေါ်မူတည်တယ်။
- nvidia-smi ကို အသုံးပြုပြီး GPU Model, MIG Support, MIG Mode နဲ့ Instance များကို စစ်ဆေးစီမံနိုင်တယ်။
- Production Environment မှာ MIG Configuration ပြောင်းလဲမယ်ဆိုရင် Workload Impact နဲ့ Maintenance Requirements ကို ကြိုတင်စစ်ဆေးရမယ်။

**အဓိကမှတ်သားရန်:** GPU Memory ပမာဏကို သိရုံနဲ့ MIG Profile ကို ခန့်မှန်းရွေးချယ်လို့ မရပါဘူး။ GPU Model က Support လုပ်ထားတဲ့ MIG Profile နဲ့ Placement ကို အခြေခံပြီး ရွေးချယ်ရပါတယ်။

## 11. စစ်ဆေးရန် မေးခွန်းများနှင့် အဖြေများ

**မေးခွန်း ၁။** MIG Profile အမည် `3g.20gb` မှာ `3g` က ဘာကို ဆိုလိုသလဲ?

**အဖြေ။** GPU Core 3 ခု မဟုတ်ပါဘူး။ သတ်မှတ်ထားတဲ့ MIG GPU Compute Slice အချိုးအစားကို ဖော်ပြတာဖြစ်ပြီး အတွင်းမှာ SMs နဲ့ အခြား hardware resource များ ပါဝင်ပါတယ်။

**မေးခွန်း ၂။** MIG Mode ကို enable လုပ်ထားရုံနဲ့ MIG instance တွေ ရလာပြီလား?

**အဖြေ။** မရသေးပါဘူး။ MIG Mode enable ပြီးမှ `nvidia-smi mig -cgi ... -C` လို command တွေနဲ့ instance တွေကို သီးခြားဖန်တီးရပါတယ်။

**မေးခွန်း ၃။** A100 40GB ပေါ်မှာ `3g.20gb` နှစ်ခု ဖန်တီးမယ်ဆိုရင် ဘယ် command ကို သုံးမလဲ?

**အဖြေ။** `sudo nvidia-smi mig -cgi 3g.20gb,3g.20gb -C` — `-C` က သက်ဆိုင်ရာ compute instance တွေကိုပါ ဖန်တီးပေးပါတယ်။

**မေးခွန်း ၄။** Profile ဥပမာတွေကို Tesla T4 ပေါ်မှာ အသုံးပြုလို့ရမလား?

**အဖြေ။** မရပါဘူး။ T4 နဲ့ A10G မှာ MIG support မရှိပါဘူး။ MIG-capable GPU ပေါ်မှာသာ အသုံးပြုရပါမယ်။

**မေးခွန်း ၅။** GPU 4 လုံး × 7 instances = 28 instances ဆိုတဲ့ တွက်ချက်မှုက အမြဲမှန်သလား?

**အဖြေ။** မမှန်ပါဘူး။ အဲဒါက သီအိုရီအရ တွက်ချက်မှုသာဖြစ်ပြီး အမှန်တကယ် instance အရေအတွက်က GPU Model, profile size, placement နဲ့ resource availability ပေါ် မူတည်ပါတယ်။

---

## English Version — MIG Profiles and Operations

## 1. Learning Objectives

In this module you will learn to:

- Compare the isolation models of vGPU and MIG
- Check MIG support per GPU model
- Understand how GPU memory and Streaming Multiprocessors (SMs) are divided per MIG profile
- Check, enable, and configure MIG mode and instances with nvidia-smi
- Choose between VM-based GPU virtualization and container-based GPU partitioning

## 2. What Is a MIG Profile?

A MIG profile defines how a physical GPU's compute and memory resources are divided into an instance, and in what ratio.

A MIG profile name is read as follows. For example — `3g.20gb`:

- `3g` — indicates the allocated GPU compute slice ratio.
- `20gb` — indicates the GPU memory size allocated to the instance.

Here `3g` does not mean 3 GPU cores. It indicates the size of the allocated MIG GPU slice. Internally, Streaming Multiprocessors (SMs) and other hardware resources are divided according to the profile.

**A100 40GB MIG profile examples** — On an NVIDIA A100-SXM4-40GB you can find the following profiles:

| MIG Profile | GPU Memory | SM Allocation | Maximum Instances |
|---|---|---|---|
| 1g.5gb | 5 GB class | 1/7 | 7 |
| 2g.10gb | 10 GB class | 2/7 | 3 |
| 3g.20gb | 20 GB class | 3/7 | 2 |
| 4g.20gb | 20 GB class | 4/7 | 1 |
| 7g.40gb | 40 GB class | 7/7 | 1 |

The memory amount in the profile name indicates the designated class; the usable memory shown in nvidia-smi may differ slightly. Profiles are also not identical across all GPU models.

Source: NVIDIA Supported MIG Profiles (https://docs.nvidia.com/datacenter/tesla/mig-user-guide/latest/supported-mig-profiles.html)

## 3. Checking MIG Support with nvidia-smi

First check the GPU model and driver:

```bash
nvidia-smi
```

To check GPU 0's MIG mode:

```bash
nvidia-smi -i 0 --query-gpu=name,mig.mode.current --format=csv
```

To view MIG-related details:

```bash
nvidia-smi -q | grep -A 8 -i MIG
```

Whether a GPU supports MIG should be checked against NVIDIA's supported GPU list.

**Caution:** The mere availability of nvidia-smi does not mean the GPU supports MIG. Check the GPU model first. If a MIG-related field in nvidia-smi shows N/A, it may simply mean the feature is unsupported or not applicable — do not automatically treat it as an error.

## 4. Enabling MIG Mode

After confirming the GPU supports MIG, you can enable MIG mode on GPU 0:

```bash
sudo nvidia-smi -i 0 -mig 1
```

To verify:

```bash
nvidia-smi -i 0 --query-gpu=name,mig.mode.current --format=csv
```

**Caution —**

- Depending on the GPU model and architecture, a GPU reset or reboot may be required.
- Enabling may fail because of applications or monitoring processes currently using the GPU.
- Enabling MIG mode does not automatically create MIG instances.
- Before changing MIG configuration on a production GPU, you may need to stop workloads and plan a maintenance window.

## 5. Listing MIG Profiles

To view the GPU instance profiles available on a MIG-supported, MIG-enabled GPU:

```bash
sudo nvidia-smi mig -lgip
```

To check the placements available for creating GPU instances:

```bash
sudo nvidia-smi mig -lgipp
```

These commands help you understand which profiles the GPU supports and which profile combinations can be created.

## 6. Creating MIG Instances

For example, to create two MIG instances using the `3g.20gb` profile on an A100 40GB:

```bash
sudo nvidia-smi mig -cgi 3g.20gb,3g.20gb -C
```

In this command:

- `mig` — the MIG management command
- `-cgi` — create GPU instance profiles
- `3g.20gb,3g.20gb` — the two GPU instance profiles to create
- `-C` — also create the corresponding compute instances

Success depends on the GPU model, driver version, current MIG configuration, and available placement.

To view the created GPU instances:

```bash
sudo nvidia-smi mig -lgi
```

To list MIG devices:

```bash
nvidia-smi -L
```

**Caution** — Use the profile example above only on a suitable GPU that supports MIG. This MIG configuration cannot be created on a Tesla T4 or A10G.

Source: NVIDIA MIG Getting Started Guide (https://docs.nvidia.com/datacenter/tesla/mig-user-guide/getting-started-with-mig.html)

## 7. MIG on a Multi-GPU Server

Suppose a server has 4 MIG-capable GPUs. If each GPU supports a configuration with up to 7 MIG instances, then in theory:

```
4 GPUs × 7 instances = 28 instances
```

But this calculation assumes every GPU can be fully populated with 7 instances. The real instance count depends on GPU model, profile size, placement, and resource availability.

## 8. Operational Cautions

- **Maintenance window** — Before enabling MIG mode or creating profiles on a production GPU, stop the workloads and plan a maintenance window.
- **Mode ≠ instances** — Enabling MIG mode alone does not create instances; profiles must be created separately.
- **Blocking processes** — Applications or monitoring processes using the GPU can block enable/change operations.
- **Placement rules** — A profile combination succeeds only if it matches the GPU's supported placements. Pre-check with `-lgipp`.
- **Licensing** — vGPU may require an NVIDIA vGPU software license. For the MIG feature itself, check the platform and software requirements.
- **Support matrix** — For combined vGPU/MIG configurations, check the NVIDIA vGPU product support matrix (https://docs.nvidia.com/vgpu/).

## 9. Choosing Between VM-Based and Container-Based Partitioning

**Choose vGPU for:**

- Virtual Desktop Infrastructure (VDI)
- Virtual workstations
- AI/ML workloads inside VMs
- Managing GPU devices for VMs from the hypervisor

**Choose MIG for:**

- Containerized AI workloads on bare-metal Linux
- HPC and AI inference workloads
- Workloads that need dedicated GPU compute and memory resources each
- Environments that prefer hardware-backed resource partitioning

Because some supported platforms offer MIG-backed vGPU, these two technologies are not always mutually exclusive options.

## 10. Module Summary

- vGPU presents virtual GPU devices to VMs.
- MIG splits a supported physical GPU into hardware GPU instances.
- A MIG profile defines the compute slice and GPU memory resource.
- The maximum MIG instance count depends on the GPU model and profiles.
- nvidia-smi is used to check and manage GPU model, MIG support, MIG mode, and instances.
- In production, assess workload impact and maintenance requirements before changing MIG configuration.

**Key takeaway:** Knowing the GPU memory size alone is not enough to guess a MIG profile. Selection must be based on the MIG profiles and placements the GPU model supports.

## 11. Self-Check Questions and Answers

**Q1.** What does `3g` mean in the profile name `3g.20gb`?

**A1.** It does not mean 3 GPU cores. It indicates the allocated MIG GPU compute slice ratio, containing SMs and other hardware resources.

**Q2.** Does enabling MIG mode immediately give you MIG instances?

**A2.** No. After enabling MIG mode, instances must be created separately with commands such as `nvidia-smi mig -cgi ... -C`.

**Q3.** Which command creates two `3g.20gb` instances on an A100 40GB?

**A3.** `sudo nvidia-smi mig -cgi 3g.20gb,3g.20gb -C` — the `-C` flag also creates the corresponding compute instances.

**Q4.** Can the profile examples be used on a Tesla T4?

**A4.** No. T4 and A10G have no MIG support; the examples only work on MIG-capable GPUs.

**Q5.** Is the "4 GPUs × 7 instances = 28 instances" calculation always accurate?

**A5.** No. It is a theoretical calculation; the real instance count depends on GPU model, profile size, placement, and resource availability.

---

## Technical review notes

- The A100-SXM4-40GB MIG profile table (1g.5gb → 7g.40gb with SM allocation 1/7–7/7) is reproduced from the source material, which cites NVIDIA's supported MIG profiles documentation. The source notes the memory amount in a profile name is a class designation and usable memory may differ slightly — kept as supplied.
- All commands (`nvidia-smi`, `-mig 1`, `mig -lgip`, `mig -lgipp`, `mig -cgi ... -C`, `mig -lgi`, `nvidia-smi -L`) are reproduced from the source material; they are presented as illustrative for MIG-capable GPUs (e.g. A100), not universal.
- The 4 × 7 = 28 instance calculation is kept from the source with its explicit caveat that it is theoretical.
- Licensing claims (possible NVIDIA vGPU software license requirement) are kept non-specific as in the source; verify against the current NVIDIA vGPU licensing documentation before deployment.
