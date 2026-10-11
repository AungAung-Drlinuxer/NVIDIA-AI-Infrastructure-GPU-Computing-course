# COURSE OUTLINE — NVIDIA AI Infrastructure & GPU Computing

**Course scope** — AI foundations, GPU computing, NVIDIA data center infrastructure, networking, GPU virtualization, CUDA, AI software stack, infrastructure management, AI workload operations and MLOps.

**Target audience** — IT infrastructure engineers, Cloud/DevOps engineers, AI infrastructure engineers, GPU administrators and learners preparing for NVIDIA AI infrastructure concepts and related certification topics.

---

# PART I — AI Foundations and AI Infrastructure

## Module 01 — AI, ML, DL Fundamentals and AI Adoption

**အဓိကအကြောင်းအရာများ**
- Artificial Intelligence (AI), Machine Learning (ML) နဲ့ Deep Learning (DL) တို့ရဲ့ ဆက်နွယ်မှု
- Supervised, Unsupervised, Self-supervised နဲ့ Reinforcement Learning
- Neural Networks, CNN, RNN နဲ့ Transformer အခြေခံ
- Training vs Inference အခြေခံ
- AI adoption ကို မြန်ဆန်စေတဲ့ data, compute နဲ့ algorithmic breakthroughs
- Generative AI, LLM, multimodal AI, RAG နဲ့ AI agents အခြေခံ
- AI lifecycle နဲ့ AI infrastructure ရဲ့ အခန်းကဏ္ဍ

**သင်ယူရမည့်ရလဒ်** — AI workload တစ်ခုရဲ့ ရည်ရွယ်ချက်နဲ့ လိုအပ်နိုင်တဲ့ infrastructure ကို အခြေခံအားဖြင့် ခွဲခြားနိုင်ရန်။

## Module 02 — AI Data Centers, Energy Consumption and PUE

**အဓိကအကြောင်းအရာများ**
- AI data center တွေမှာ လိုအပ်တဲ့ compute, power, cooling နဲ့ networking
- GPU workload တွေကြောင့် power demand တိုးလာရတဲ့ အကြောင်းရင်း
- IT equipment energy နဲ့ facility energy
- Power Usage Effectiveness (PUE)
- Cooling, rack density နဲ့ power delivery
- AI infrastructure ရဲ့ energy efficiency နဲ့ sustainability

**သင်ယူရမည့်ရလဒ်** — AI infrastructure ရဲ့ performance ကို power, cooling နဲ့ facility efficiency တို့နဲ့ ဆက်စပ်နားလည်ရန်။

## Module 03 — CPU vs GPU and Parallel Computing

**အဓိကအကြောင်းအရာများ**
- CPU architecture နဲ့ GPU architecture
- Sequential processing နဲ့ parallel processing
- GPU cores, threads နဲ့ workload parallelism
- Matrix multiplication နဲ့ tensor operations
- GPU memory အခြေခံ
- GPU က AI workload အချို့မှာ ဘာကြောင့် အားသာသလဲ
- CPU နဲ့ GPU ကို ပေါင်းစပ်အသုံးပြုပုံ

**သင်ယူရမည့်ရလဒ်** — CPU နဲ့ GPU တို့ရဲ့ ကွာခြားချက်၊ သင့်တော်တဲ့ workload နဲ့ parallel computing ရဲ့ အခြေခံကို ရှင်းပြနိုင်ရန်။

## Module 04 — Training vs Inference

**အဓိကအကြောင်းအရာများ**
- Model training နဲ့ inference တို့ရဲ့ ရည်ရွယ်ချက်
- Forward pass, loss, backward pass နဲ့ optimizer
- Training workload ရဲ့ compute နဲ့ memory demand
- Inference latency, throughput နဲ့ cost
- LLM inference ရဲ့ prefill နဲ့ decode
- KV cache
- Distributed training နဲ့ inference scaling
- Training နဲ့ inference အတွက် infrastructure ရွေးချယ်မှု

**သင်ယူရမည့်ရလဒ်** — Training နဲ့ inference အတွက် GPU, memory, networking နဲ့ performance requirements တွေ ဘာကြောင့်ကွာခြားသလဲ နားလည်ရန်။

## Module 05 — AI Infrastructure: Compute, Storage and Cloud

**အဓိကအကြောင်းအရာများ**
- AI infrastructure ရဲ့ အဓိက components
- CPU, GPU, system memory နဲ့ local storage
- Shared storage နဲ့ distributed storage
- On-premises, cloud နဲ့ hybrid infrastructure
- Compute resource provisioning နဲ့ scalability
- Data locality နဲ့ storage performance
- Infrastructure cost, availability နဲ့ capacity planning

**သင်ယူရမည့်ရလဒ်** — AI workload တစ်ခုအတွက် compute, storage နဲ့ deployment environment ကို အခြေခံရွေးချယ်နိုင်ရန်။

## Module 06 — NVIDIA AI Infrastructure and DGX Systems

**အဓိကအကြောင်းအရာများ**
- NVIDIA AI infrastructure ecosystem
- DGX systems ရဲ့ အခန်းကဏ္ဍ
- GPU server နဲ့ integrated AI systems
- CPU, GPU, memory, storage နဲ့ networking တို့ရဲ့ ဆက်နွယ်မှု
- NVIDIA software stack အခြေခံ
- DGX infrastructure ကို AI training နဲ့ inference အတွက် အသုံးပြုပုံ
- Enterprise AI infrastructure ရဲ့ design considerations

**သင်ယူရမည့်ရလဒ်** — NVIDIA AI server တစ်ခုကို isolated GPU တစ်ခုအဖြစ်မဟုတ်ဘဲ integrated infrastructure အဖြစ် နားလည်ရန်။

---

# PART II — GPU Interconnects, Networking and Data Movement

## Module 07 — NVLink and NVSwitch

**အဓိကအကြောင်းအရာများ**
- PCIe နဲ့ NVLink
- GPU-to-GPU communication
- NVLink bandwidth နဲ့ topology
- NVSwitch ရဲ့ switching role
- Multi-GPU systems
- GPU communication bottlenecks
- GPU memory sharing နဲ့ pooling အတွက် software support လိုအပ်ချက်

**သင်ယူရမည့်ရလဒ်** — NVLink နဲ့ NVSwitch တို့ရဲ့ ကွာခြားချက်၊ multi-GPU communication မှာ သူတို့ရဲ့ အခန်းကဏ္ဍကို နားလည်ရန်။

## Module 08 — HPC Fabric and InfiniBand

**အဓိကအကြောင်းအရာများ**
- HPC cluster architecture
- InfiniBand fabric အခြေခံ
- Host Channel Adapter (HCA)
- InfiniBand switches နဲ့ subnet
- OpenSM (Open Subnet Manager)
- NVIDIA Unified Fabric Manager (UFM)
- RDMA နဲ့ low-latency networking
- Multi-node AI training အတွက် fabric requirements

**သင်ယူရမည့်ရလဒ်** — InfiniBand-based AI/HPC cluster ရဲ့ အခြေခံ network architecture ကို ရှင်းပြနိုင်ရန်။

## Module 09 — InfiniBand vs NVIDIA Spectrum-X Ethernet

**အဓိကအကြောင်းအရာများ**
- InfiniBand နဲ့ Ethernet တို့ရဲ့ အခြေခံကွာခြားချက်
- NVIDIA Quantum InfiniBand switch platform
- NVIDIA Spectrum Ethernet switch platform
- Spectrum-X နဲ့ AI networking
- RoCE (RDMA over Converged Ethernet)
- Congestion management နဲ့ flow control
- Bandwidth, latency, tail latency နဲ့ network utilization
- Workload, operations နဲ့ cost အပေါ်မူတည်ပြီး fabric ရွေးချယ်ခြင်း

**သင်ယူရမည့်ရလဒ်** — InfiniBand နဲ့ AI-optimized Ethernet တို့ကို workload requirements အပေါ်အခြေခံပြီး နှိုင်းယှဉ်နိုင်ရန်။

## Module 10 — DMA, RDMA and GPU Data Movement

**အဓိကအကြောင်းအရာများ**
- Direct Memory Access (DMA)
- Remote Direct Memory Access (RDMA)
- CPU involvement နဲ့ data-copy overhead
- Host memory နဲ့ GPU memory
- Network adapter, HCA/NIC နဲ့ memory registration
- Latency နဲ့ throughput
- AI workload မှာ data movement က ဘာကြောင့်အရေးကြီးသလဲ

**သင်ယူရမည့်ရလဒ်** — DMA နဲ့ RDMA တို့ရဲ့ ကွာခြားချက်၊ data transfer ကို ပိုမိုထိရောက်စေတဲ့ အကြောင်းရင်းကို နားလည်ရန်။

## Module 11 — GPUDirect RDMA

**အဓိကအကြောင်းအရာများ**
- GPUDirect RDMA concept
- GPU memory နဲ့ supported peer devices
- NIC/HCA မှ GPU memory သို့ data path
- Multi-node GPU communication
- CPU data-copy overhead လျှော့ချခြင်း
- Hardware, driver, platform နဲ့ software compatibility
- GPUDirect RDMA troubleshooting အခြေခံ

**သင်ယူရမည့်ရလဒ်** — Supported hardware/software environment မှာ GPU memory နဲ့ network device တို့အကြား direct data transfer ဘယ်လိုလုပ်ဆောင်နိုင်သလဲ နားလည်ရန်။

## Module 12 — GPUDirect Storage (GDS)

**အဓိကအကြောင်းအရာများ**
- Storage-to-GPU data movement
- Traditional storage-to-host-to-GPU data path
- GPUDirect Storage concept
- Supported storage, GPU နဲ့ driver requirements
- Data loading bottlenecks
- AI training မှာ storage throughput ရဲ့ အရေးပါမှု
- GDS deployment considerations

**သင်ယူရမည့်ရလဒ်** — Storage data ကို GPU memory သို့ ပိုမိုထိရောက်စွာ ပို့ဆောင်နိုင်တဲ့ နည်းလမ်းကို နားလည်ရန်။

## Module 13 — GPUDirect RDMA vs GPUDirect Storage

**အဓိကအကြောင်းအရာများ**
- GPUDirect RDMA နဲ့ GDS တို့ရဲ့ ရည်ရွယ်ချက်
- Network-to-GPU vs Storage-to-GPU data paths
- အသုံးပြုရမယ့် workload အမျိုးအစား
- Shared concepts နဲ့ မတူညီတဲ့ requirements
- Compatibility, performance နဲ့ troubleshooting
- Real-world architecture examples

**သင်ယူရမည့်ရလဒ်** — GPU data movement technology တစ်ခုချင်းစီကို သင့်တော်တဲ့ use case နဲ့ ချိတ်ဆက်ရွေးချယ်နိုင်ရန်။

## Module 14 — Layer 3 Networking, DGX OS and GPU Software Foundations

**အဓိကအကြောင်းအရာများ**
- Layer 2 နဲ့ Layer 3 networking အခြေခံ
- IP addressing, routing နဲ့ network connectivity
- DGX OS ရဲ့ အခန်းကဏ္ဍ
- Operating system, kernel နဲ့ GPU driver
- CUDA software stack နဲ့ compatibility
- Container runtime နဲ့ GPU access
- Bare-metal နဲ့ virtualized GPU environment

**သင်ယူရမည့်ရလဒ်** — GPU server ရဲ့ OS, networking, driver နဲ့ runtime layer တွေ ဘယ်လိုဆက်စပ်နေသလဲ နားလည်ရန်။

---

# PART III — GPU Drivers, Virtualization and CUDA

## Module 15 — NVIDIA GPU Drivers and nvidia-smi

**အဓိကအကြောင်းအရာများ**
- NVIDIA GPU driver ရဲ့ အခန်းကဏ္ဍ
- GPU driver နဲ့ CUDA Toolkit တို့ရဲ့ ကွာခြားချက်
- nvidia-smi output ကို ဖတ်ရှုခြင်း
- GPU utilization, memory, temperature နဲ့ power
- Driver-supported CUDA version နဲ့ installed Toolkit version
- nvcc --version
- lspci, uname -r နဲ့ OS information
- Common driver troubleshooting

**သင်ယူရမည့်ရလဒ်** — Linux GPU server ပေါ်မှာ GPU visibility, driver status နဲ့ basic utilization ကို စစ်ဆေးနိုင်ရန်။

## Module 16 — GPU Virtualization Fundamentals

**အဓိကအကြောင်းအရာများ**
- GPU sharing နဲ့ virtualization
- Virtual machines နဲ့ containers
- Physical GPU, virtual GPU နဲ့ GPU instances
- GPU resource allocation
- Isolation, performance နဲ့ resource constraints
- GPU passthrough အခြေခံ
- Virtualized AI workload use cases

**သင်ယူရမည့်ရလဒ်** — GPU virtualization ဘာကြောင့်လိုအပ်သလဲ၊ ဘယ်လို deployment options တွေရှိသလဲ နားလည်ရန်။

## Module 17 — NVIDIA vGPU vs MIG: Conceptual Fundamentals

**အဓိကအကြောင်းအရာများ**
- NVIDIA vGPU အခြေခံ
- Multi-Instance GPU (MIG) အခြေခံ
- vGPU နဲ့ MIG တို့ရဲ့ design goals
- VM-level GPU access
- Hardware-backed GPU partitioning
- Isolation, resource allocation နဲ့ utilization
- Use-case comparison
- MIG-backed vGPU အပါအဝင် implementation ကွဲပြားမှုများ

**သင်ယူရမည့်ရလဒ်** — vGPU နဲ့ MIG ကို အဓိပ္ပာယ်တူ technology တွေအဖြစ် မယူဆဘဲ ရည်ရွယ်ချက်နဲ့ implementation အပေါ်အခြေခံပြီး ခွဲခြားနိုင်ရန်။

## Module 18 — NVIDIA vGPU and MIG: Profiles, Configuration and Operations

**အဓိကအကြောင်းအရာများ**
- vGPU profiles နဲ့ supported configuration
- MIG-capable GPU architectures
- MIG profiles နဲ့ GPU instance allocation
- GPU model အလိုက် profile ကွာခြားမှု
- nvidia-smi ဖြင့် MIG mode စစ်ဆေးခြင်း
- MIG instance listing နဲ့ creation commands
- Resource allocation နဲ့ workload placement
- Operational considerations နဲ့ troubleshooting

**သင်ယူရမည့်ရလဒ်** — Supported GPU နဲ့ software version ပေါ်မူတည်ပြီး vGPU/MIG configuration ကို ဖတ်ရှုနားလည်နိုင်ရန်။

---

# PART IV — CUDA, Multi-GPU Communication and NVIDIA Software Stack

## Module 19 — CUDA Fundamentals: GPU Programming and Parallel Computing

**အဓိကအကြောင်းအရာများ**
- CUDA ဆိုတာဘာလဲ
- GPU programming model
- CPU နဲ့ GPU တို့ရဲ့ အခန်းကဏ္ဍ
- CUDA kernel အခြေခံ
- Parallel computation နဲ့ threads
- CUDA Toolkit နဲ့ CUDA driver
- Python frameworks က GPU ကို ဘယ်လိုအသုံးပြုသလဲ

**သင်ယူရမည့်ရလဒ်** — CUDA ရဲ့ အဓိကရည်ရွယ်ချက်နဲ့ GPU computing နဲ့ ဆက်စပ်မှုကို ရှင်းပြနိုင်ရန်။

## Module 20 — CUDA Architecture, Programming Model and Software Stack

**အဓိကအကြောင်းအရာများ**
- CUDA threads, blocks နဲ့ grids
- Thread hierarchy
- GPU execution model
- CUDA Runtime နဲ့ Toolkit
- GPU memory concepts
- CUDA libraries နဲ့ frameworks
- Application-to-GPU execution flow
- CUDA compatibility နဲ့ software dependencies

**သင်ယူရမည့်ရလဒ်** — CUDA programming model နဲ့ high-level framework မှ GPU execution အထိ software stack ကို နားလည်ရန်။

## Module 21 — CUDA Execution, Memory and Programming Workflow

**အဓိကအကြောင်းအရာများ**
- Host code နဲ့ device code
- Kernel launch အခြေခံ
- Host-to-device နဲ့ device-to-host data transfer
- GPU memory allocation
- Synchronization အခြေခံ
- Kernel execution နဲ့ performance considerations
- CUDA program lifecycle
- Common CUDA programming concepts

**သင်ယူရမည့်ရလဒ်** — CUDA application တစ်ခုက GPU ပေါ်မှာ အလုပ်လုပ်ပုံကို conceptual workflow အဖြစ် နားလည်ရန်။

## Module 22 — NCCL: Multi-GPU and Multi-Node Communication

**အဓိကအကြောင်းအရာများ**
- NVIDIA Collective Communications Library (NCCL)
- Multi-GPU communication
- AllReduce, Broadcast နဲ့ ReduceScatter
- Distributed training communication
- NVLink, PCIe နဲ့ network transport
- NCCL နဲ့ RDMA တို့ရဲ့ ဆက်နွယ်မှု
- Communication bottlenecks
- Multi-GPU training မှာ NCCL ရဲ့ အခန်းကဏ္ဍ

**သင်ယူရမည့်ရလဒ်** — NCCL က collective communication ကို ဘယ်လိုပံ့ပိုးသလဲ၊ hardware interconnect နဲ့ ဘယ်လိုကွာခြားသလဲ နားလည်ရန်။

## Module 23 — NVLink, NVSwitch, PCIe, RDMA and NCCL

**အဓိကအကြောင်းအရာများ**
- GPU interconnect technologies
- PCIe နဲ့ NVLink တို့ရဲ့ အခန်းကဏ္ဍ
- NVSwitch နဲ့ GPU fabric
- DMA/RDMA data movement
- NCCL software layer
- Hardware, transport mechanism နဲ့ communication library တို့ကို ခွဲခြားခြင်း
- Multi-GPU topology နဲ့ communication efficiency
- End-to-end communication architecture

**သင်ယူရမည့်ရလဒ်** — GPU-to-GPU communication ကို hardware, data transfer နဲ့ software library အဆင့်တွေခွဲပြီး ရှင်းပြနိုင်ရန်။

---

# PART V — NVIDIA Monitoring, Cluster Management and Tool Selection

## Module 24 — NVIDIA GPU Monitoring and Data Center Management

**အဓိကအကြောင်းအရာများ**
- GPU monitoring ရဲ့ ရည်ရွယ်ချက်
- nvidia-smi နဲ့ DCGM တို့ရဲ့ ကွာခြားချက်
- GPU utilization, memory, temperature နဲ့ power
- Prometheus နဲ့ Grafana integration
- Metrics, dashboards နဲ့ alerting
- GPU fleet visibility
- Infrastructure monitoring နဲ့ model monitoring
- Operational troubleshooting

**သင်ယူရမည့်ရလဒ်** — GPU monitoring tools တွေကို သင့်တော်တဲ့ operational requirement နဲ့ ချိတ်ဆက်နိုင်ရန်။

## Module 25 — GPU Telemetry, Metrics and Monitoring Architecture

**အဓိကအကြောင်းအရာများ**
- GPU metrics collection
- Exporters နဲ့ monitoring endpoints
- Prometheus time-series metrics
- Grafana dashboards
- Alert rules နဲ့ notifications
- GPU-level နဲ့ application-level metrics
- Monitoring data ကို troubleshooting မှာ အသုံးပြုခြင်း
- Monitoring pipeline ရဲ့ limitations

**သင်ယူရမည့်ရလဒ်** — GPU telemetry ကို စုဆောင်း၊ သိမ်းဆည်း၊ မြင်သာအောင်ပြသပြီး သတိပေးချက်များ ထုတ်ပေးနိုင်တဲ့ architecture ကို နားလည်ရန်။

## Module 26 — NVIDIA DCGM: Health, Diagnostics and GPU Monitoring

**အဓိကအကြောင်းအရာများ**
- Data Center GPU Manager (DCGM)
- GPU health monitoring
- GPU telemetry
- Diagnostics နဲ့ health checks
- dcgmi CLI အခြေခံ
- DCGM Exporter
- Prometheus-compatible metrics
- GPU fault investigation နဲ့ troubleshooting

**သင်ယူရမည့်ရလဒ်** — DCGM ရဲ့ အခန်းကဏ္ဍကို GPU status inspection နဲ့ broader monitoring stack တို့မှ ခွဲခြားနိုင်ရန်။

## Module 27 — NVIDIA Base Command Manager (BCM)

**အဓိကအကြောင်းအရာများ**
- BCM ရဲ့ အခန်းကဏ္ဍ
- AI/HPC cluster provisioning
- Node configuration နဲ့ management
- Cluster administration
- Infrastructure lifecycle
- Compute cluster နဲ့ GPU resources
- BCM နဲ့ Slurm/Kubernetes integration concepts
- Base Command Manager နဲ့ Base Command Platform တို့ရဲ့ ကွာခြားချက်

**သင်ယူရမည့်ရလဒ်** — Cluster infrastructure management ကို job scheduling နဲ့ application orchestration တို့မှ ခွဲခြားနားလည်ရန်။

## Module 28 — NVIDIA Infrastructure Tools: Scenarios and Tool Selection

**အဓိကအကြောင်းအရာများ**
- nvidia-smi — local GPU status
- DCGM — GPU telemetry, health နဲ့ diagnostics
- DCGM Exporter — monitoring metrics export
- OpenSM — InfiniBand subnet management
- BCM — cluster infrastructure management
- Slurm — HPC/AI job scheduling
- Kubernetes — container orchestration
- Scenario-based tool selection နဲ့ exam-style questions

**သင်ယူရမည့်ရလဒ်** — ပြဿနာတစ်ခုချင်းစီအတွက် သင့်တော်တဲ့ NVIDIA သို့မဟုတ် ecosystem tool ကို ရွေးချယ်နိုင်ရန်။

---

# PART VI — NVIDIA AI Software, Workloads and Enterprise Platform

## Module 29 — NVIDIA AI Applications and Domain-Specific Technologies

**အဓိကအကြောင်းအရာများ**
- AI application categories
- Computer vision
- Natural language processing
- Generative AI နဲ့ LLM workloads
- Healthcare/life sciences နဲ့ domain-specific AI
- Recommendation systems
- NVIDIA AI software ecosystem
- Application requirement နဲ့ infrastructure selection

**သင်ယူရမည့်ရလဒ်** — AI use case တစ်ခုနဲ့ သက်ဆိုင်တဲ့ model, framework နဲ့ infrastructure requirement တွေကို ချိတ်ဆက်နားလည်ရန်။

## Module 30 — NVIDIA AI Software Stack and Component Integration

**အဓိကအကြောင်းအရာများ**
- GPU drivers
- CUDA Toolkit နဲ့ CUDA libraries
- cuBLAS နဲ့ cuDNN
- NCCL
- PyTorch နဲ့ TensorFlow
- NVIDIA container ecosystem
- Inference optimization နဲ့ serving components
- Software compatibility နဲ့ dependency management

**သင်ယူရမည့်ရလဒ်** — NVIDIA software components တွေဟာ အဆင့်အလိုက် ဘယ်လိုပေါင်းစပ်အလုပ်လုပ်သလဲ နားလည်ရန်။

## Module 31 — NVIDIA AI Enterprise and AI Factory

**အဓိကအကြောင်းအရာများ**
- NVIDIA AI Enterprise ရဲ့ ရည်ရွယ်ချက်
- Application development software
- AI frameworks နဲ့ libraries
- NVIDIA NIM နဲ့ NeMo
- GPU software နဲ့ infrastructure management
- Kubernetes နဲ့ GPU Operator
- Run နဲ့ workload orchestration
- Base Command Manager ရဲ့ အခန်းကဏ္ဍ
- AI Factory ဆိုတဲ့ integrated production-system concept
- AI Enterprise နဲ့ AI Factory တို့ရဲ့ ကွာခြားချက်

**သင်ယူရမည့်ရလဒ်** — Enterprise AI software platform နဲ့ complete AI production infrastructure တို့ရဲ့ ဆက်နွယ်မှုကို ရှင်းပြနိုင်ရန်။

## Module 32 — AI Workflow: Data Processing to Inference

**အဓိကအကြောင်းအရာများ**
- Data processing နဲ့ preparation
- Model training
- Model optimization
- Inference deployment
- RAPIDS
- PyTorch/TensorFlow
- TensorRT
- Triton Inference Server
- Workflow integration နဲ့ performance considerations

**သင်ယူရမည့်ရလဒ်** — AI application တစ်ခုရဲ့ end-to-end workflow ကို tools နဲ့ ချိတ်ဆက်ဖော်ပြနိုင်ရန်။

## Module 33 — PyTorch and TensorFlow: ML Framework Fundamentals

**အဓိကအကြောင်းအရာများ**
- ML framework ဆိုတာဘာလဲ
- Model architecture နဲ့ layers
- Loss functions နဲ့ optimizers
- Data loaders နဲ့ training loops
- GPU acceleration
- Checkpoints နဲ့ model evaluation
- Distributed training အခြေခံ
- Framework ရွေးချယ်မှုနဲ့ use cases

**သင်ယူရမည့်ရလဒ်** — ML framework တစ်ခုရဲ့ အခန်းကဏ္ဍကို GPU driver, CUDA နဲ့ inference-serving platform တို့မှ ခွဲခြားနိုင်ရန်။

## Module 34 — NVIDIA GPU Libraries and Framework Acceleration

**အဓိကအကြောင်းအရာများ**
- CUDA ecosystem
- cuBLAS နဲ့ linear algebra
- cuDNN နဲ့ deep-learning primitives
- NCCL နဲ့ collective communication
- RAPIDS နဲ့ GPU-accelerated data science
- Framework backends နဲ့ optimized kernels
- Hardware/software compatibility
- Performance optimization concepts

**သင်ယူရမည့်ရလဒ်** — NVIDIA GPU libraries တွေက framework နဲ့ application performance ကို ဘယ်လိုပံ့ပိုးသလဲ နားလည်ရန်။

## Module 35 — AI Model Optimization and Inference Serving

**အဓိကအကြောင်းအရာများ**
- Inference performance requirements
- Latency နဲ့ throughput
- Quantization နဲ့ precision selection
- Model optimization နဲ့ memory usage
- TensorRT
- TensorRT-LLM ဆိုင်ရာ အခြေခံအယူအဆများ
- Triton Inference Server
- NVIDIA NIM
- Serving architecture နဲ့ benchmarking

**သင်ယူရမည့်ရလဒ်** — Model optimization နဲ့ inference serving တို့ရဲ့ မတူညီတဲ့တာဝန်တွေကို နားလည်ပြီး လိုအပ်ချက်အလိုက် tool ရွေးချယ်နိုင်ရန်။

---

# PART VII — AI Workload Operations and Orchestration

## Module 36 — Job Scheduling vs Container Orchestration

**အဓိကအကြောင်းအရာများ**
- HPC job scheduling
- Slurm ရဲ့ အခန်းကဏ္ဍ
- Kubernetes ရဲ့ အခန်းကဏ္ဍ
- Batch jobs နဲ့ long-running services
- Resource allocation နဲ့ desired state
- Self-healing, scaling နဲ့ workload lifecycle
- Slurm နဲ့ Kubernetes ကို နှိုင်းယှဉ်ခြင်း
- Training နဲ့ inference architecture examples

**သင်ယူရမည့်ရလဒ်** — AI training jobs နဲ့ production inference services အတွက် scheduling/orchestration requirements ကို ခွဲခြားနိုင်ရန်။

## Module 37 — NVIDIA GPUs and Slurm Integration

**အဓိကအကြောင်းအရာများ**
- Slurm cluster architecture
- GPU resource allocation
- GPU-enabled job submission
- Multi-GPU နဲ့ multi-node training
- CUDA application execution
- NCCL communication
- GPUDirect RDMA ရဲ့ အခန်းကဏ္ဍ
- DCGM monitoring နဲ့ Slurm operations
- Job troubleshooting နဲ့ resource utilization

**သင်ယူရမည့်ရလဒ်** — Slurm-managed AI/HPC cluster မှာ GPU workload တစ်ခုကို schedule လုပ်ပြီး စောင့်ကြည့်ပုံကို နားလည်ရန်။

## Module 38 — NVIDIA GPUs and Kubernetes Integration

**အဓိကအကြောင်းအရာများ**
- Kubernetes GPU workloads
- NVIDIA GPU Operator
- GPU drivers, device plugin နဲ့ container toolkit
- GPU resource requests နဲ့ scheduling
- MIG resource exposure အခြေခံ
- DCGM Exporter နဲ့ monitoring
- GPU-enabled inference services
- Autoscaling နဲ့ scheduling limitations
- Training/inference workload deployment examples

**သင်ယူရမည့်ရလဒ်** — Kubernetes ပေါ်မှာ GPU-enabled containers တွေကို deploy လုပ်ပြီး infrastructure components နဲ့ ပေါင်းစပ်ပုံကို နားလည်ရန်။

---

# PART VIII — MLOps and End-to-End Production AI

## Module 39 — Introduction to MLOps

**အဓိကအကြောင်းအရာများ**
- MLOps ဆိုတာဘာလဲ
- DevOps နဲ့ MLOps တို့ရဲ့ ဆက်နွယ်မှု
- ML lifecycle
- Data, code နဲ့ model versioning
- Automated training pipelines
- Model evaluation နဲ့ registry
- Production deployment
- Infrastructure နဲ့ model monitoring
- Feedback နဲ့ retraining

**သင်ယူရမည့်ရလဒ်** — Model development ကနေ production operation အထိ MLOps က ဘာကြောင့်လိုအပ်သလဲ နားလည်ရန်။

## Module 40 — Why We Need MLOps

**အဓိကအကြောင်းအရာများ**
- Model တည်ဆောက်ခြင်းနဲ့ production operation
- Production deployment challenges
- Reliability, repeatability နဲ့ traceability
- CI/CD for ML
- Event-driven scheduling
- Automated retraining နဲ့ scheduled retraining
- Model Registry နဲ့ versioning
- Controlled rollout နဲ့ rollback
- Data drift နဲ့ production monitoring

**သင်ယူရမည့်ရလဒ်** — MLOps က manual ML operations ကို ဘယ်လိုလျှော့ချပြီး production reliability ကို ဘယ်လိုတိုးတက်စေသလဲ ရှင်းပြနိုင်ရန်။

## Module 41 — NVIDIA Tools for MLOps

**အဓိကအကြောင်းအရာများ**
- RAPIDS နဲ့ data preparation
- NeMo Curator နဲ့ generative AI data curation
- PyTorch, TensorFlow နဲ့ NeMo for training
- DGX infrastructure နဲ့ workload scheduling
- TAO Toolkit နဲ့ model customization
- TensorRT နဲ့ inference optimization
- Triton Inference Server နဲ့ model serving
- NVIDIA NIM နဲ့ packaged inference APIs
- NGC Catalog/Registry နဲ့ software/model assets
- DCGM နဲ့ GPU monitoring
- Base Command Manager နဲ့ cluster infrastructure
- Slurm/Kubernetes နဲ့ workload management
- Model Registry, CI/CD နဲ့ end-to-end lifecycle integration

**သင်ယူရမည့်ရလဒ်** — NVIDIA technologies တွေကို MLOps lifecycle အဆင့်အလိုက် ချိတ်ဆက်ပြီး production AI architecture တစ်ခုအတွက် tool selection လုပ်နိုင်ရန်။

---

# Final Course Capstone — Design an Enterprise AI Infrastructure

သင်ခန်းစာအားလုံးပြီးဆုံးတဲ့အခါ အောက်ပါအချက်တွေပါဝင်တဲ့ conceptual architecture တစ်ခုကို ကိုယ်တိုင်ရေးဆွဲပါ။

- **Compute layer** — CPU, GPU, memory နဲ့ AI servers
- **Interconnect layer** — PCIe, NVLink/NVSwitch နဲ့ လိုအပ်သလို InfiniBand/Ethernet
- **Data layer** — Storage, data preparation နဲ့ model artifacts
- **Software layer** — GPU drivers, CUDA, libraries နဲ့ ML frameworks
- **Training layer** — Training workflow နဲ့ Slurm သို့မဟုတ် သင့်တော်တဲ့ scheduler
- **Optimization layer** — Supported model အတွက် TensorRT သို့မဟုတ် သင့်တော်တဲ့ optimization technology
- **Inference layer** — Triton, NIM သို့မဟုတ် သင့်တော်တဲ့ serving platform
- **Orchestration layer** — လိုအပ်သလို Kubernetes နဲ့ container management
- **Monitoring layer** — DCGM, Prometheus/Grafana နဲ့ application metrics
- **MLOps layer** — Data/model versioning, CI/CD, evaluation, registry, controlled deployment နဲ့ retraining workflow

ဒီ capstone ရဲ့ ရည်ရွယ်ချက်က NVIDIA product နာမည်တွေကို အလွတ်ကျက်ဖို့ မဟုတ်ပါဘူး။ Workload requirement တစ်ခုကိုကြည့်ပြီး သင့်တော်တဲ့ infrastructure, software နဲ့ operational workflow ကို ရွေးချယ်နိုင်ဖို့ ဖြစ်ပါတယ်။

---

# Suggested Study Method

Module တစ်ခုချင်းစီကို အောက်ပါအစီအစဉ်အတိုင်း လေ့လာပါ။

1. **Concept** — အဓိကအယူအဆကို နားလည်ပါ။
2. **Architecture** — Components တွေ ဘယ်လိုချိတ်ဆက်နေသလဲ လေ့လာပါ။
3. **Tools** — သက်ဆိုင်ရာ NVIDIA သို့မဟုတ် ecosystem tools တွေရဲ့ အခန်းကဏ္ဍကို ခွဲခြားပါ။
4. **Hands-on** — ရရှိနိုင်တဲ့ lab environment မှာ သင့်တော်တဲ့ commands သို့မဟုတ် workflow တွေကို စမ်းသပ်ပါ။
5. **Troubleshooting** — Failure ဖြစ်တဲ့အခါ ဘယ် layer ကို စစ်ဆေးရမလဲ သင်ယူပါ။
6. **Review** — ကိုယ့်စကားနဲ့ ရှင်းပြပြီး scenario-based questions တွေကို ဖြေကြည့်ပါ။

⚠️ Product-specific commands, profile limits, supported GPU models နဲ့ licensing details တွေကိုတော့ သက်ဆိုင်ရာ official documentation နဲ့ စစ်ဆေးပြီးမှ အသုံးပြုပါ။ Software version အလိုက် feature နဲ့ configuration ကွာခြားနိုင်ပါတယ်။
