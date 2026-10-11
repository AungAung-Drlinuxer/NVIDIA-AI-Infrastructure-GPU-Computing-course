# NCA-AIIO Training PDF ↔ Course Modules Mapping

Source PDF: **NVIDIA NCA - AIIO Certification** (PowerPoint slide deck, 160 slides,
author: Ashish Prajapati) — user-provided study material for the
NVIDIA Certified Associate – AI Infrastructure and Operations (NCA-AIIO) exam.

**Exam facts (slide 4):** 50 questions · 60 minutes · $125 · prerequisites: basic
data center infrastructure knowledge.

**Exam topics (slides 5–6):** accelerated computing use cases · AI/ML/DL ·
GPU architecture · NVIDIA's software suite · infrastructure & operations
considerations → deck sections: 1. Fundamentals, 2. Inside an AI Centric Data
Center, 3. NVIDIA Technology Stack, 4. AI Workflows.

## PDF structure → course Parts

| PDF section | Slides | Course Part |
|---|---|---|
| 1. Fundamentals | 7–15 | Part I (Modules 01–04) |
| 2. Inside an AI Centric Data Center | 16–24 | Part I (Modules 02, 05) + Part II start |
| 3. NVIDIA Technology Stack | 57–145 | Parts I–VII (Modules 03, 06–38) |
| 4. AI Workflows | 146–160 | Parts VI–VIII (Modules 32–41) |

## Slide images added to modules

Rendered slide JPGs live in [`assets/nca-aiio-slides/`](assets/nca-aiio-slides/)
(71 slides: screenshots, diagrams and visual analogies — text-only slides were
skipped because the module files already contain that content).

| file | slide | content | inserted into |
|---|---|---|---|
| `slide-001.jpg` | p1 | Section divider / recap | `—` |
| `slide-004.jpg` | p4 | Exam details (50Q/60min/$125) | `—` |
| `slide-005.jpg` | p5 | Exam topics | `—` |
| `slide-009.jpg` | p9 | Drivers of AI evolution: data explosion / algorithm breakthrough / compute growth | `01_ai_ml_dl_fundamentals/01_drivers_of_ai_evolution.md` |
| `slide-014.jpg` | p14 | Transformer model | `01_ai_ml_dl_fundamentals/05_transformer_model.md` |
| `slide-015.jpg` | p15 | Transformer example — next word prediction | `01_ai_ml_dl_fundamentals/05_transformer_model.md` |
| `slide-017.jpg` | p17 | Inside an AI-centric data center | `02_ai_data_centers_energy_pue/01_inside_ai_centric_data_center.md` |
| `slide-019.jpg` | p19 | Average data center electricity use by size | `02_ai_data_centers_energy_pue/02_data_center_energy_pue.md` |
| `slide-020.jpg` | p20 | Power consumption: old vs modern data centers | `02_ai_data_centers_energy_pue/02_data_center_energy_pue.md` |
| `slide-021.jpg` | p21 | PUE definition | `02_ai_data_centers_energy_pue/02_data_center_energy_pue.md` |
| `slide-022.jpg` | p22 | Compute / Network / Storage | `02_ai_data_centers_energy_pue/01_inside_ai_centric_data_center.md` |
| `slide-028.jpg` | p28 | CPU vs GPU analogy — private jet vs commercial flight | `03_cpu_gpu_parallel_computing/02_cpu_vs_gpu_private_jet.md` |
| `slide-029.jpg` | p29 | CPU vs GPU comparison table | `03_cpu_gpu_parallel_computing/02_cpu_vs_gpu_private_jet.md` |
| `slide-030.jpg` | p30 | CPU vs GPU architecture diagram | `03_cpu_gpu_parallel_computing/03_cpu_gpu_high_level_architecture.md` |
| `slide-031.jpg` | p31 | Beyond Moore's law | `03_cpu_gpu_parallel_computing/01_cpu_gpu_cuda_evolution.md` |
| `slide-033.jpg` | p33 | Pilots and crew analogy | `03_cpu_gpu_parallel_computing/04_dpu.md` |
| `slide-035.jpg` | p35 | DPU: offload, accelerate, isolate | `03_cpu_gpu_parallel_computing/04_dpu.md` |
| `slide-038.jpg` | p38 | A modern server (photo) | `03_cpu_gpu_parallel_computing/04_dpu.md` |
| `slide-040.jpg` | p40 | Network inside an AI data center (in-band / out-of-band) | `07_gpu_interconnects_networking/01_networking_in_ai_data_center.md` |
| `slide-041.jpg` | p41 | Why network separation matters | `07_gpu_interconnects_networking/01_networking_in_ai_data_center.md` |
| `slide-042.jpg` | p42 | Network layout recap | `07_gpu_interconnects_networking/01_networking_in_ai_data_center.md` |
| `slide-045.jpg` | p45 | Evolution of roads (analogy) | `07_gpu_interconnects_networking/02_network_fabrics_comparison.md` |
| `slide-046.jpg` | p46 | Highway vs bullet train | `07_gpu_interconnects_networking/03_ethernet_vs_infiniband.md` |
| `slide-047.jpg` | p47 | Ethernet vs InfiniBand | `07_gpu_interconnects_networking/03_ethernet_vs_infiniband.md` |
| `slide-052.jpg` | p52 | Storage analogy — pantry | `05_ai_infra_compute_storage_cloud/01_storage_in_ai_data_centers.md` |
| `slide-058.jpg` | p58 | NVIDIA history | `06_nvidia_technology_stack/01_nvidia_history_and_evolution.md` |
| `slide-059.jpg` | p59 | NVIDIA timeline visual | `06_nvidia_technology_stack/01_nvidia_history_and_evolution.md` |
| `slide-063.jpg` | p63 | NVIDIA RTX series | `06_nvidia_technology_stack/03_nvidia_physical_layer_rtx_series.md` |
| `slide-064.jpg` | p64 | DGX A100 platform | `06_nvidia_technology_stack/05_nvidia_dgx_platform_dgx_a100.md` |
| `slide-065.jpg` | p65 | DGX SuperPOD | `06_nvidia_technology_stack/06_nvidia_dgx_superpod.md` |
| `slide-066.jpg` | p66 | ConnectX InfiniBand HCAs/NICs | `06_nvidia_technology_stack/07_nvidia_networking_infiniband_connectx.md` |
| `slide-067.jpg` | p67 | BlueField DPUs / SuperNICs | `06_nvidia_technology_stack/08_nvidia_bluefield_dpu_supernic.md` |
| `slide-068.jpg` | p68 | NVIDIA reference architectures | `06_nvidia_technology_stack/09_nvidia_reference_architectures.md` |
| `slide-070.jpg` | p70 | GPU cores section visual 1 | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` |
| `slide-071.jpg` | p71 | GPU cores section visual 2 | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` |
| `slide-072.jpg` | p72 | GPU cores — school analogy | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` |
| `slide-073.jpg` | p73 | GPU cores: CUDA/Tensor/RT table | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` |
| `slide-074.jpg` | p74 | Cyberpunk 2077 example | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` |
| `slide-075.jpg` | p75 | NVIDIA GPU families vs core types | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` |
| `slide-076.jpg` | p76 | DGX definition | `06_nvidia_technology_stack/04_nvidia_dgx_systems.md` |
| `slide-078.jpg` | p78 | NVIDIA Grace CPU | `06_nvidia_technology_stack/04_nvidia_dgx_systems.md` |
| `slide-081.jpg` | p81 | DGX H100 + BMC | `06_nvidia_technology_stack/05_nvidia_dgx_platform_dgx_a100.md` |
| `slide-084.jpg` | p84 | GPU Interconnect / GPU Direct / HPC Fabric | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` |
| `slide-085.jpg` | p85 | Skybridge analogy | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` |
| `slide-086.jpg` | p86 | Interconnect visual 1 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` |
| `slide-087.jpg` | p87 | Interconnect visual 2 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` |
| `slide-088.jpg` | p88 | Interconnect visual 3 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` |
| `slide-089.jpg` | p89 | Interconnect visual 4 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` |
| `slide-090.jpg` | p90 | NVLink | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` |
| `slide-091.jpg` | p91 | NVSwitch | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` |
| `slide-092.jpg` | p92 | GPU interconnect recap | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` |
| `slide-095.jpg` | p95 | GPU interconnect recap 2 | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` |
| `slide-103.jpg` | p103 | OS / driver / virtualization section | `14_dgx_os_gpu_software_foundations/01_dgx_os_gpu_drivers_virtualization.md` |
| `slide-105.jpg` | p105 | OS/driver section recap | `15_gpu_drivers_nvidia_smi/01_gpu_drivers_nvidia_smi.md` |
| `slide-107.jpg` | p107 | Virtualization section | `16_gpu_virtualization_fundamentals/01_gpu_virtualization_fundamentals.md` |
| `slide-108.jpg` | p108 | VM virtualization diagram | `16_gpu_virtualization_fundamentals/01_gpu_virtualization_fundamentals.md` |
| `slide-111.jpg` | p111 | Co-working space analogy | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` |
| `slide-112.jpg` | p112 | Shared co-working space | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` |
| `slide-113.jpg` | p113 | More isolation | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` |
| `slide-114.jpg` | p114 | vGPU vs MIG — simple explanation | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` |
| `slide-115.jpg` | p115 | vGPU vs MIG diagram | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` |
| `slide-118.jpg` | p118 | GPU programming — CUDA & NCCL | `19_cuda_fundamentals/01_cuda_gpu_programming_parallel_computing.md` |
| `slide-120.jpg` | p120 | CUDA analogy — chefs & translator | `19_cuda_fundamentals/01_cuda_gpu_programming_parallel_computing.md` |
| `slide-124.jpg` | p124 | Installing CUDA — where the toolkit fits | `21_installing_cuda_toolkit/01_installing_cuda_toolkit.md` |
| `slide-129.jpg` | p129 | Monitoring & management section | `24_gpu_monitoring_data_center_management/01_gpu_monitoring_data_center_management.md` |
| `slide-130.jpg` | p130 | Dashboard analogy | `24_gpu_monitoring_data_center_management/01_gpu_monitoring_data_center_management.md` |
| `slide-131.jpg` | p131 | nvidia-smi / DCGM / BCM | `24_gpu_monitoring_data_center_management/01_gpu_monitoring_data_center_management.md` |
| `slide-139.jpg` | p139 | Clara / Merlin / NIM section | `29_ai_applications_domain_technologies/01_clara_merlin_nim.md` |
| `slide-144.jpg` | p144 | NVIDIA AI Factory | `31_ai_enterprise_ai_factory/01_ai_enterprise_ai_factory.md` |
| `slide-147.jpg` | p147 | Framework analogy — house building | `33_ml_frameworks_pytorch_tensorflow/01_ml_frameworks.md` |
| `slide-151.jpg` | p151 | Job scheduling vs container orchestration | `36_job_scheduling_container_orchestration/01_job_scheduling_vs_container_orchestration.md` |

## Full page mapping (all 160 slides)

✅ = slide image added to the module.

| page | content | module | img |
|---|---|---|---|
| 1 | Section divider / recap | `—` | ✅ |
| 2 | Section divider / recap | `—` |  |
| 3 | NVIDIA Certifications map (NCA/NCP) | `—` |  |
| 4 | Exam details (50Q/60min/$125) | `—` | ✅ |
| 5 | Exam topics | `—` | ✅ |
| 6 | Exam topics → 4 sections | `—` |  |
| 7 | Section divider / recap | `—` |  |
| 8 | Section divider / recap | `—` |  |
| 9 | Drivers of AI evolution: data explosion / algorithm breakthrough / compute growth | `01_ai_ml_dl_fundamentals/01_drivers_of_ai_evolution.md` | ✅ |
| 10 | AI use cases across industries | `01_ai_ml_dl_fundamentals/02_ai_use_cases_across_industries.md` |  |
| 11 | Section divider / recap | `—` |  |
| 12 | AI, ML, DL, Gen AI definitions | `01_ai_ml_dl_fundamentals/03_ai_ml_dl_genai.md` |  |
| 13 | Chess analogy | `01_ai_ml_dl_fundamentals/04_ai_ml_dl_genai_chess_analogy.md` |  |
| 14 | Transformer model | `01_ai_ml_dl_fundamentals/05_transformer_model.md` | ✅ |
| 15 | Transformer example — next word prediction | `01_ai_ml_dl_fundamentals/05_transformer_model.md` | ✅ |
| 16 | Section divider / recap | `—` |  |
| 17 | Inside an AI-centric data center | `02_ai_data_centers_energy_pue/01_inside_ai_centric_data_center.md` | ✅ |
| 18 | Key constraints: power, heat, rack density | `02_ai_data_centers_energy_pue/01_inside_ai_centric_data_center.md` |  |
| 19 | Average data center electricity use by size | `02_ai_data_centers_energy_pue/02_data_center_energy_pue.md` | ✅ |
| 20 | Power consumption: old vs modern data centers | `02_ai_data_centers_energy_pue/02_data_center_energy_pue.md` | ✅ |
| 21 | PUE definition | `02_ai_data_centers_energy_pue/02_data_center_energy_pue.md` | ✅ |
| 22 | Compute / Network / Storage | `02_ai_data_centers_energy_pue/01_inside_ai_centric_data_center.md` | ✅ |
| 23 | Section divider / recap | `—` |  |
| 24 | Section divider / recap | `—` |  |
| 25 | Why we needed GPUs | `03_cpu_gpu_parallel_computing/01_cpu_gpu_cuda_evolution.md` |  |
| 26 | GPUs for ML: 2006 CUDA, 2012 AlexNet | `03_cpu_gpu_parallel_computing/01_cpu_gpu_cuda_evolution.md` |  |
| 27 | Section divider / recap | `—` |  |
| 28 | CPU vs GPU analogy — private jet vs commercial flight | `03_cpu_gpu_parallel_computing/02_cpu_vs_gpu_private_jet.md` | ✅ |
| 29 | CPU vs GPU comparison table | `03_cpu_gpu_parallel_computing/02_cpu_vs_gpu_private_jet.md` | ✅ |
| 30 | CPU vs GPU architecture diagram | `03_cpu_gpu_parallel_computing/03_cpu_gpu_high_level_architecture.md` | ✅ |
| 31 | Beyond Moore's law | `03_cpu_gpu_parallel_computing/01_cpu_gpu_cuda_evolution.md` | ✅ |
| 32 | Section divider / recap | `—` |  |
| 33 | Pilots and crew analogy | `03_cpu_gpu_parallel_computing/04_dpu.md` | ✅ |
| 34 | DPU definition | `03_cpu_gpu_parallel_computing/04_dpu.md` |  |
| 35 | DPU: offload, accelerate, isolate | `03_cpu_gpu_parallel_computing/04_dpu.md` | ✅ |
| 36 | CPU/GPU/DPU processor comparison | `03_cpu_gpu_parallel_computing/04_dpu.md` |  |
| 37 | DPU market leaders | `03_cpu_gpu_parallel_computing/04_dpu.md` |  |
| 38 | A modern server (photo) | `03_cpu_gpu_parallel_computing/04_dpu.md` | ✅ |
| 39 | Section divider / recap | `—` |  |
| 40 | Network inside an AI data center (in-band / out-of-band) | `07_gpu_interconnects_networking/01_networking_in_ai_data_center.md` | ✅ |
| 41 | Why network separation matters | `07_gpu_interconnects_networking/01_networking_in_ai_data_center.md` | ✅ |
| 42 | Network layout recap | `07_gpu_interconnects_networking/01_networking_in_ai_data_center.md` | ✅ |
| 43 | Network fabric — purpose/implementation table | `07_gpu_interconnects_networking/02_network_fabrics_comparison.md` |  |
| 44 | Section divider / recap | `—` |  |
| 45 | Evolution of roads (analogy) | `07_gpu_interconnects_networking/02_network_fabrics_comparison.md` | ✅ |
| 46 | Highway vs bullet train | `07_gpu_interconnects_networking/03_ethernet_vs_infiniband.md` | ✅ |
| 47 | Ethernet vs InfiniBand | `07_gpu_interconnects_networking/03_ethernet_vs_infiniband.md` | ✅ |
| 48 | Ethernet vs InfiniBand table | `07_gpu_interconnects_networking/03_ethernet_vs_infiniband.md` |  |
| 49 | Converged Ethernet (RoCE) | `07_gpu_interconnects_networking/04_converged_ethernet_roce.md` |  |
| 50 | NVIDIA InfiniBand vs Converged Ethernet implementations | `09_infiniband_vs_spectrum_x/01_infiniband_vs_spectrum_x.md` |  |
| 51 | Section divider / recap | `—` |  |
| 52 | Storage analogy — pantry | `05_ai_infra_compute_storage_cloud/01_storage_in_ai_data_centers.md` | ✅ |
| 53 | Storage types (NVMe, parallel FS, NFS, object) | `05_ai_infra_compute_storage_cloud/01_storage_in_ai_data_centers.md` |  |
| 54 | Section divider / recap | `—` |  |
| 55 | Cloud vs on-prem key advantages | `05_ai_infra_compute_storage_cloud/02_cloud_vs_onprem_gpu.md` |  |
| 56 | Cloud vs on-prem details | `05_ai_infra_compute_storage_cloud/02_cloud_vs_onprem_gpu.md` |  |
| 57 | Section divider / recap | `—` |  |
| 58 | NVIDIA history | `06_nvidia_technology_stack/01_nvidia_history_and_evolution.md` | ✅ |
| 59 | NVIDIA timeline visual | `06_nvidia_technology_stack/01_nvidia_history_and_evolution.md` | ✅ |
| 60 | Six-layer stack (partial) | `06_nvidia_technology_stack/02_nvidia_technology_stack_six_layers.md` |  |
| 61 | Section divider / recap | `—` |  |
| 62 | Six-layer stack (full) | `06_nvidia_technology_stack/02_nvidia_technology_stack_six_layers.md` |  |
| 63 | NVIDIA RTX series | `06_nvidia_technology_stack/03_nvidia_physical_layer_rtx_series.md` | ✅ |
| 64 | DGX A100 platform | `06_nvidia_technology_stack/05_nvidia_dgx_platform_dgx_a100.md` | ✅ |
| 65 | DGX SuperPOD | `06_nvidia_technology_stack/06_nvidia_dgx_superpod.md` | ✅ |
| 66 | ConnectX InfiniBand HCAs/NICs | `06_nvidia_technology_stack/07_nvidia_networking_infiniband_connectx.md` | ✅ |
| 67 | BlueField DPUs / SuperNICs | `06_nvidia_technology_stack/08_nvidia_bluefield_dpu_supernic.md` | ✅ |
| 68 | NVIDIA reference architectures | `06_nvidia_technology_stack/09_nvidia_reference_architectures.md` | ✅ |
| 69 | Section divider / recap | `—` |  |
| 70 | GPU cores section visual 1 | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` | ✅ |
| 71 | GPU cores section visual 2 | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` | ✅ |
| 72 | GPU cores — school analogy | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` | ✅ |
| 73 | GPU cores: CUDA/Tensor/RT table | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` | ✅ |
| 74 | Cyberpunk 2077 example | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` | ✅ |
| 75 | NVIDIA GPU families vs core types | `03_cpu_gpu_parallel_computing/05_gpu_cores_cuda_tensor_rt.md` | ✅ |
| 76 | DGX definition | `06_nvidia_technology_stack/04_nvidia_dgx_systems.md` | ✅ |
| 77 | DGX family timeline | `06_nvidia_technology_stack/04_nvidia_dgx_systems.md` |  |
| 78 | NVIDIA Grace CPU | `06_nvidia_technology_stack/04_nvidia_dgx_systems.md` | ✅ |
| 79 | DGX platform OS/software stack | `14_dgx_os_gpu_software_foundations/01_dgx_os_gpu_drivers_virtualization.md` |  |
| 80 | DGX A100 vs DGX H100/H200 | `06_nvidia_technology_stack/05_nvidia_dgx_platform_dgx_a100.md` |  |
| 81 | DGX H100 + BMC | `06_nvidia_technology_stack/05_nvidia_dgx_platform_dgx_a100.md` | ✅ |
| 82 | DGX A100 vs H100/H200 (cont.) | `06_nvidia_technology_stack/05_nvidia_dgx_platform_dgx_a100.md` |  |
| 83 | Six-layer stack recap | `06_nvidia_technology_stack/02_nvidia_technology_stack_six_layers.md` |  |
| 84 | GPU Interconnect / GPU Direct / HPC Fabric | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` | ✅ |
| 85 | Skybridge analogy | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` | ✅ |
| 86 | Interconnect visual 1 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` | ✅ |
| 87 | Interconnect visual 2 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` | ✅ |
| 88 | Interconnect visual 3 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` | ✅ |
| 89 | Interconnect visual 4 | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` | ✅ |
| 90 | NVLink | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` | ✅ |
| 91 | NVSwitch | `07_gpu_interconnects_networking/05_nvlink_nvswitch.md` | ✅ |
| 92 | GPU interconnect recap | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` | ✅ |
| 93 | InfiniBand | `08_hpc_fabric_infiniband/01_hpc_fabric_infiniband.md` |  |
| 94 | NVIDIA IB vs CE table | `09_infiniband_vs_spectrum_x/01_infiniband_vs_spectrum_x.md` |  |
| 95 | GPU interconnect recap 2 | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` | ✅ |
| 96 | Section divider / recap | `—` |  |
| 97 | DMA | `10_dma_rdma_gpu_data_movement/01_dma_rdma_gpu_data_movement.md` |  |
| 98 | RDMA | `10_dma_rdma_gpu_data_movement/01_dma_rdma_gpu_data_movement.md` |  |
| 99 | GPUDirect RDMA | `11_gpudirect_rdma/01_gpudirect_rdma.md` |  |
| 100 | GPUDirect Storage | `12_gpudirect_storage/01_gpudirect_storage.md` |  |
| 101 | GPUDirect RDMA vs Storage comparison | `13_gpudirect_rdma_vs_gds/01_gpudirect_rdma_vs_gds.md` |  |
| 102 | Six-layer stack recap | `06_nvidia_technology_stack/02_nvidia_technology_stack_six_layers.md` |  |
| 103 | OS / driver / virtualization section | `14_dgx_os_gpu_software_foundations/01_dgx_os_gpu_drivers_virtualization.md` | ✅ |
| 104 | DGX OS | `14_dgx_os_gpu_software_foundations/01_dgx_os_gpu_drivers_virtualization.md` |  |
| 105 | OS/driver section recap | `15_gpu_drivers_nvidia_smi/01_gpu_drivers_nvidia_smi.md` | ✅ |
| 106 | GPU drivers | `15_gpu_drivers_nvidia_smi/01_gpu_drivers_nvidia_smi.md` |  |
| 107 | Virtualization section | `16_gpu_virtualization_fundamentals/01_gpu_virtualization_fundamentals.md` | ✅ |
| 108 | VM virtualization diagram | `16_gpu_virtualization_fundamentals/01_gpu_virtualization_fundamentals.md` | ✅ |
| 109 | CPU vs GPU virtualization table | `16_gpu_virtualization_fundamentals/01_gpu_virtualization_fundamentals.md` |  |
| 110 | Why virtualize GPUs | `16_gpu_virtualization_fundamentals/01_gpu_virtualization_fundamentals.md` |  |
| 111 | Co-working space analogy | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` | ✅ |
| 112 | Shared co-working space | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` | ✅ |
| 113 | More isolation | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` | ✅ |
| 114 | vGPU vs MIG — simple explanation | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` | ✅ |
| 115 | vGPU vs MIG diagram | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` | ✅ |
| 116 | vGPU vs MIG table | `17_vgpu_vs_mig/01_vgpu_vs_mig_conceptual.md` |  |
| 117 | Section divider / recap | `—` |  |
| 118 | GPU programming — CUDA & NCCL | `19_cuda_fundamentals/01_cuda_gpu_programming_parallel_computing.md` | ✅ |
| 119 | Section divider / recap | `—` |  |
| 120 | CUDA analogy — chefs & translator | `19_cuda_fundamentals/01_cuda_gpu_programming_parallel_computing.md` | ✅ |
| 121 | The CUDA difference | `19_cuda_fundamentals/01_cuda_gpu_programming_parallel_computing.md` |  |
| 122 | CUDA bridges software & hardware | `20_cuda_architecture_programming_model/01_cuda_architecture_programming_model.md` |  |
| 123 | CUDA = parallel computing platform | `19_cuda_fundamentals/01_cuda_gpu_programming_parallel_computing.md` |  |
| 124 | Installing CUDA — where the toolkit fits | `21_installing_cuda_toolkit/01_installing_cuda_toolkit.md` | ✅ |
| 125 | Section divider / recap | `—` |  |
| 126 | NCCL | `22_nccl_multi_gpu_communication/01_nccl.md` |  |
| 127 | NVLink/NVSwitch/PCIe/RDMA vs NCCL table | `23_nvlink_nvswitch_pcie_rdma_nccl/01_nvlink_nvswitch_pcie_rdma_vs_nccl.md` |  |
| 128 | Six-layer stack recap | `06_nvidia_technology_stack/02_nvidia_technology_stack_six_layers.md` |  |
| 129 | Monitoring & management section | `24_gpu_monitoring_data_center_management/01_gpu_monitoring_data_center_management.md` | ✅ |
| 130 | Dashboard analogy | `24_gpu_monitoring_data_center_management/01_gpu_monitoring_data_center_management.md` | ✅ |
| 131 | nvidia-smi / DCGM / BCM | `24_gpu_monitoring_data_center_management/01_gpu_monitoring_data_center_management.md` | ✅ |
| 132 | nvidia-smi | `25_nvidia_smi_monitoring/01_nvidia_smi.md` |  |
| 133 | DCGM | `26_dcgm_health_diagnostics/01_dcgm.md` |  |
| 134 | BCM | `27_base_command_manager/01_base_command_manager.md` |  |
| 135 | Section divider / recap | `—` |  |
| 136 | Monitoring vs management capabilities | `28_infrastructure_tools_scenarios/01_tool_selection_exam_scenarios.md` |  |
| 137 | Use nvidia-smi / DCGM / BCM when | `28_infrastructure_tools_scenarios/01_tool_selection_exam_scenarios.md` |  |
| 138 | Section divider / recap | `—` |  |
| 139 | Clara / Merlin / NIM section | `29_ai_applications_domain_technologies/01_clara_merlin_nim.md` | ✅ |
| 140 | NIMs / Clara / Merlin definitions | `29_ai_applications_domain_technologies/01_clara_merlin_nim.md` |  |
| 141 | Full software stack (6 layers) | `30_ai_software_stack_integration/01_ai_software_stack_integration.md` |  |
| 142 | Section divider / recap | `—` |  |
| 143 | NVIDIA AI Enterprise | `31_ai_enterprise_ai_factory/01_ai_enterprise_ai_factory.md` |  |
| 144 | NVIDIA AI Factory | `31_ai_enterprise_ai_factory/01_ai_enterprise_ai_factory.md` | ✅ |
| 145 | Section divider / recap | `—` |  |
| 146 | Steps in AI workflow | `32_ai_workflow_data_to_inference/01_ai_workflow_data_to_inference.md` |  |
| 147 | Framework analogy — house building | `33_ml_frameworks_pytorch_tensorflow/01_ml_frameworks.md` | ✅ |
| 148 | ML frameworks | `33_ml_frameworks_pytorch_tensorflow/01_ml_frameworks.md` |  |
| 149 | NVIDIA differentiator (cuDNN engine) | `34_cuda_cudnn_gpu_accelerated_frameworks/01_cuda_cudnn_gpu_accelerated_frameworks.md` |  |
| 150 | Training vs inference | `35_model_training_vs_inference/01_model_training_vs_inference.md` |  |
| 151 | Job scheduling vs container orchestration | `36_job_scheduling_container_orchestration/01_job_scheduling_vs_container_orchestration.md` | ✅ |
| 152 | Slurm vs Kubernetes table | `37_slurm_vs_kubernetes_ai_workloads/01_slurm_vs_kubernetes.md` |  |
| 153 | Slurm — NVIDIA integration | `38_slurm_kubernetes_integration/01_gpu_tools_slurm_kubernetes.md` |  |
| 154 | Kubernetes — NVIDIA integration | `38_slurm_kubernetes_integration/01_gpu_tools_slurm_kubernetes.md` |  |
| 155 | Section divider / recap | `—` |  |
| 156 | MLOps analogy — kitchen | `39_introduction_to_mlops/01_introduction_to_mlops.md` |  |
| 157 | MLOps — with/without | `40_why_we_need_mlops/01_why_we_need_mlops.md` |  |
| 158 | Why we need MLOps | `40_why_we_need_mlops/01_why_we_need_mlops.md` |  |
| 159 | What is MLOps | `39_introduction_to_mlops/01_introduction_to_mlops.md` |  |
| 160 | NVIDIA tools for MLOps | `41_nvidia_tools_for_mlops/01_nvidia_tools_for_mlops.md` |  |

## Credits

Slide images: NVIDIA NCA-AIIO training deck (NVIDIA NCA - AIIO Certification,
Ashish Prajapati) — user-provided study material, educational use only.
