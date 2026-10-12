# NVIDIA AI Infrastructure & GPU Computing — Complete Learning Roadmap

NVIDIA AI infrastructure နှင့် GPU computing ကို အခြေခံမှ ထုတ်လုပ်မှုအဆင့်အထိ
ဘာသာစကားနှစ်မျိုး (မြန်မာ + English) ဖြင့် ရေးသားထားသော သင်ခန်းစာစု (course) ဖြစ်ပါသည်။

A bilingual (Myanmar + English) course covering AI foundations, GPU computing,
NVIDIA data center infrastructure, networking, GPU virtualization, CUDA,
AI software stack, infrastructure management, AI workload operations and MLOps —
from fundamentals to production.

**Target audience** — IT infrastructure engineers, Cloud/DevOps engineers,
AI infrastructure engineers, GPU administrators and learners preparing for
NVIDIA AI infrastructure concepts and related certification topics.

---

## Course structure

- **8 Parts · 42 Modules + Final Capstone**
- အသေးစိတ် outline ကို [`COURSE_OUTLINE.md`](COURSE_OUTLINE.md) တွင် ကြည့်ပါ
- Module တိုင်းတွင် **မြန်မာဘာသာ version အပြည့်** + **English Version အပြည့်** ပါဝင်ပါသည်

### Part I — AI Foundations and AI Infrastructure (Modules 01–06)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 01 | AI, ML, DL နှင့် Generative AI အခြေခံများ | [`01_ai_ml_dl_fundamentals/`](01_ai_ml_dl_fundamentals/) |
| 02 | AI-Centric Data Center နှင့် Energy/PUE | [`02_ai_data_centers_energy_pue/`](02_ai_data_centers_energy_pue/) |
| 03 | CPU vs GPU နှင့် Parallel Computing | [`03_cpu_gpu_parallel_computing/`](03_cpu_gpu_parallel_computing/) |
| 04 | Training vs Inference (အခြေခံ) | [`04_training_vs_inference/`](04_training_vs_inference/) |
| 05 | AI Infrastructure — Compute, Storage, Cloud | [`05_ai_infra_compute_storage_cloud/`](05_ai_infra_compute_storage_cloud/) |
| 06 | NVIDIA Technology Stack တစ်ခုလုံး (RTX → DGX → SuperPOD → Networking) | [`06_nvidia_technology_stack/`](06_nvidia_technology_stack/) |

### Part II — GPU Interconnects, Networking and Data Movement (Modules 07–14)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 07 | GPU Interconnects နှင့် AI Data Center Networking | [`07_gpu_interconnects_networking/`](07_gpu_interconnects_networking/) |
| 08 | HPC Fabric နှင့် InfiniBand | [`08_hpc_fabric_infiniband/`](08_hpc_fabric_infiniband/) |
| 09 | InfiniBand vs Spectrum-X | [`09_infiniband_vs_spectrum_x/`](09_infiniband_vs_spectrum_x/) |
| 10 | DMA, RDMA နှင့် GPU Data Movement | [`10_dma_rdma_gpu_data_movement/`](10_dma_rdma_gpu_data_movement/) |
| 11 | GPUDirect RDMA | [`11_gpudirect_rdma/`](11_gpudirect_rdma/) |
| 12 | GPUDirect Storage (GDS) | [`12_gpudirect_storage/`](12_gpudirect_storage/) |
| 13 | GPUDirect RDMA vs GPUDirect Storage | [`13_gpudirect_rdma_vs_gds/`](13_gpudirect_rdma_vs_gds/) |
| 14 | DGX OS နှင့် GPU Software Foundations | [`14_dgx_os_gpu_software_foundations/`](14_dgx_os_gpu_software_foundations/) |

### Part III — GPU Drivers, Virtualization and CUDA (Modules 15–18)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 15 | GPU Drivers နှင့် nvidia-smi | [`15_gpu_drivers_nvidia_smi/`](15_gpu_drivers_nvidia_smi/) |
| 16 | GPU Virtualization Fundamentals | [`16_gpu_virtualization_fundamentals/`](16_gpu_virtualization_fundamentals/) |
| 17 | vGPU vs MIG (conceptual) | [`17_vgpu_vs_mig/`](17_vgpu_vs_mig/) |
| 18 | vGPU/MIG Profiles နှင့် Operations | [`18_vgpu_mig_operations/`](18_vgpu_mig_operations/) |

### Part IV — CUDA, Multi-GPU Communication and NVIDIA Software Stack (Modules 19–23)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 19 | CUDA Fundamentals | [`19_cuda_fundamentals/`](19_cuda_fundamentals/) |
| 20 | CUDA Architecture နှင့် Programming Model | [`20_cuda_architecture_programming_model/`](20_cuda_architecture_programming_model/) |
| 21 | Installing CUDA Toolkit | [`21_installing_cuda_toolkit/`](21_installing_cuda_toolkit/) |
| 22 | NCCL — Multi-GPU Communication | [`22_nccl_multi_gpu_communication/`](22_nccl_multi_gpu_communication/) |
| 23 | NVLink/NVSwitch/PCIe/RDMA vs NCCL | [`23_nvlink_nvswitch_pcie_rdma_nccl/`](23_nvlink_nvswitch_pcie_rdma_nccl/) |

### Part V — NVIDIA Monitoring, Cluster Management and Tool Selection (Modules 24–28)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 24 | GPU Monitoring နှင့် Data Center Management | [`24_gpu_monitoring_data_center_management/`](24_gpu_monitoring_data_center_management/) |
| 25 | nvidia-smi Monitoring | [`25_nvidia_smi_monitoring/`](25_nvidia_smi_monitoring/) |
| 26 | DCGM — Health နှင့် Diagnostics | [`26_dcgm_health_diagnostics/`](26_dcgm_health_diagnostics/) |
| 27 | NVIDIA Base Command Manager (BCM) | [`27_base_command_manager/`](27_base_command_manager/) |
| 28 | Infrastructure Tools ရွေးချယ်မှု နှင့် Exam Scenarios | [`28_infrastructure_tools_scenarios/`](28_infrastructure_tools_scenarios/) |

### Part VI — NVIDIA AI Software, Workloads and Enterprise Platform (Modules 29–35, 42)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 29 | AI Applications နှင့် Domain Technologies (Clara, Merlin, NIM) | [`29_ai_applications_domain_technologies/`](29_ai_applications_domain_technologies/) |
| 30 | AI Software Stack Integration | [`30_ai_software_stack_integration/`](30_ai_software_stack_integration/) |
| 31 | NVIDIA AI Enterprise နှင့် AI Factory | [`31_ai_enterprise_ai_factory/`](31_ai_enterprise_ai_factory/) |
| 32 | AI Workflow — Data to Inference | [`32_ai_workflow_data_to_inference/`](32_ai_workflow_data_to_inference/) |
| 33 | ML Frameworks (PyTorch, TensorFlow, XGBoost) | [`33_ml_frameworks_pytorch_tensorflow/`](33_ml_frameworks_pytorch_tensorflow/) |
| 34 | CUDA/cuDNN နှင့် GPU-Accelerated Frameworks | [`34_cuda_cudnn_gpu_accelerated_frameworks/`](34_cuda_cudnn_gpu_accelerated_frameworks/) |
| 35 | Model Training vs Model Inference | [`35_model_training_vs_inference/`](35_model_training_vs_inference/) |
| 42 | AI Model Optimization နှင့် Inference Serving (Supplemental) | [`42_model_optimization_inference_serving/`](42_model_optimization_inference_serving/) |

### Part VII — AI Workload Operations and Orchestration (Modules 36–38)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 36 | Job Scheduling vs Container Orchestration | [`36_job_scheduling_container_orchestration/`](36_job_scheduling_container_orchestration/) |
| 37 | Slurm vs Kubernetes for AI Workloads | [`37_slurm_vs_kubernetes_ai_workloads/`](37_slurm_vs_kubernetes_ai_workloads/) |
| 38 | Slurm + Kubernetes Integration နှင့် GPU Tools | [`38_slurm_kubernetes_integration/`](38_slurm_kubernetes_integration/) |

### Part VIII — MLOps and End-to-End Production AI (Modules 39–41)

| Module | အကြောင်းအရာ / Topic | Folder |
|---|---|---|
| 39 | Introduction to MLOps | [`39_introduction_to_mlops/`](39_introduction_to_mlops/) |
| 40 | Why We Need MLOps | [`40_why_we_need_mlops/`](40_why_we_need_mlops/) |
| 41 | NVIDIA Tools for MLOps | [`41_nvidia_tools_for_mlops/`](41_nvidia_tools_for_mlops/) |

### Final Capstone

- **Design an Enterprise AI Infrastructure** — [`COURSE_OUTLINE.md`](COURSE_OUTLINE.md) အောက်ဆုံးတွင်

---

## Module တစ်ခုချင်းအတွင်း ပါဝင်သောအရာ / What's inside each module

- **မြန်မာဘာသာ version အပြည့်** + **English Version အပြည့်** (section အမှတ်အသား နှစ်ဖက် တူညီ)
- ပြဿနာ → အဓိကအယူအဆ → နှိုင်းယှဉ်ခြင်း → ဥပမာ/configuration → သတိပြုရန် → ကိုယ်တိုင်စစ်ဆေးမေးခွန်းများ (self-check Q&A) progression
- **Mermaid diagram ပုံများ** (GitHub/Obsidian/VS Code တွင် ပုံအဖြစ် render ဖြစ်) — ပုံတိုင်းနောက် ပုံရှင်းလင်းချက် (figure walkthrough) ပါ
- **Technical review notes** — ပြင်ဆင်ပြီး အချက်များ နှင့် verify လုပ်ရန် ကျန်အချက်များ

## Visual assets

| Type | နေရာ / Location | မှတ်ချက် |
|---|---|---|
| Mermaid figures | module ဖိုင်များအတွင်း inline | Figure 1, 2, ... နံပါတ်ပေး |

## Suggested Study Method

Module တစ်ခုချင်းစီကို အောက်ပါအစီအစဉ်အတိုင်း လေ့လာပါ —

1. **Concept** — အဓိကအယူအဆကို နားလည်ပါ
2. **Architecture** — Components တွေ ဘယ်လိုချိတ်ဆက်နေသလဲ ပုံများဖြင့် လေ့လာပါ
3. **Tools** — သက်ဆိုင်ရာ NVIDIA သို့မဟုတ် ecosystem tools တွေရဲ့ အခန်းကဏ္ဍကို ခွဲခြားပါ
4. **Hands-on** — ရရှိနိုင်တဲ့ lab environment မှာ သင့်တော်တဲ့ commands သို့မဟုတ် workflow တွေကို စမ်းသပ်ပါ
5. **Troubleshooting** — Failure ဖြစ်တဲ့အခါ ဘယ် layer ကို စစ်ဆေးရမလဲ သင်ယူပါ
6. **Review** — self-check questions တွေကို ဖြေပြီး ကိုယ့်စကားနဲ့ ရှင်းပြကြည့်ပါ

⚠️ Product-specific commands, profile limits, supported GPU models နဲ့ licensing details တွေကိုတော့ သက်ဆိုင်ရာ official documentation နဲ့ စစ်ဆေးပြီးမှ အသုံးပြုပါ။ Software version အလိုက် feature နဲ့ configuration ကွာခြားနိုင်ပါတယ်။

## Writing rules

chapter များ ရေးသား/ပြင်ဆင်ရာတွင် [`WRITING_RULES.md`](WRITING_RULES.md) ကို လိုက်နာပါ —
natural Burmese + English technical terms၊ problem → concept → comparison →
configuration → cautions → self-check progression၊ visual aids (Mermaid figures) စည်းမျဉ်းများ၊ version-dependent
claims များ flag ပြုလုပ်ခြင်း၊ chapter အဆုံး "Technical review notes" ပါဝင်ခြင်း။

## Credits

- Course content — မူရင်း source content များမှ ပြန်လည်ရေးသားခြင်း + public NVIDIA documentation များမှ ဖြည့်စွက်ခြင်း
