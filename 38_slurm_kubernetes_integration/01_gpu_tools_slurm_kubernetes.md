# Module 38 — Slurm and Kubernetes Integration with NVIDIA Infrastructure

> Part VII · AI Workload Operations and Orchestration — topic notes

## မြန်မာဘာသာ version

### 1. Learning Objectives

ဒီ Module မှာ အောက်ပါအကြောင်းအရာတွေကို လေ့လာပါမယ်။

- Slurm က Multi-Node NVIDIA GPU Training ကို ဘယ်လို Schedule လုပ်သလဲ။
- DGX Systems နဲ့ DGX SuperPOD တို့ရဲ့ ဆက်စပ်မှု။
- DCGM ရဲ့ GPU Health Monitoring နဲ့ Performance Telemetry အခန်းကဏ္ဍ။
- NCCL က Multi-GPU နဲ့ Multi-Node Training အတွက် ဘယ်လို Communication လုပ်ပေးသလဲ။
- GPUDirect RDMA က GPU Memory အကြား Data Transfer ကို ဘယ်လို အရှိန်မြှင့်ပေးနိုင်သလဲ။
- Kubernetes နဲ့ NVIDIA GPU Operator ဘယ်လိုပေါင်းစပ်အလုပ်လုပ်သလဲ။
- GPU Operator, Device Plugin, DCGM Exporter နဲ့ MIG Manager တို့ရဲ့ တာဝန်များ။
- GPU Scheduling, Autoscaling, Load Balancing နဲ့ Workload Rescheduling တို့ ဘာကွာသလဲ။
- Slurm နဲ့ Kubernetes တို့ကို AI Infrastructure တစ်ခုမှာ ဘယ်လိုပေါင်းစပ်အသုံးပြုနိုင်သလဲ။

### 2. NVIDIA AI Infrastructure ကို နားလည်ခြင်း

AI Training အကြီးစားတစ်ခုကို Run လုပ်ဖို့ GPU တစ်ခုတည်းနဲ့ မလုံလောက်နိုင်ပါဘူး။ GPU အများအပြား၊ Compute Nodes အများအပြား၊ High-Speed Network နဲ့ Shared Storage တို့ လိုအပ်နိုင်ပါတယ်။

ဥပမာ AI Training Cluster တစ်ခုမှာ အောက်ပါ Components တွေ ပါဝင်နိုင်ပါတယ်။

- GPU Compute: NVIDIA DGX Systems နဲ့ အခြား GPU Servers များ။
- GPU Interconnect: NVLink နဲ့ NVSwitch ကဲ့သို့ GPU-to-GPU Communication Technologies။
- Cluster Network: InfiniBand သို့မဟုတ် သင့်လျော်တဲ့ High-Performance Ethernet။
- Communication Software: NCCL။
- Memory Transfer Technology: GPUDirect RDMA ကို ပံ့ပိုးထားတဲ့ Hardware နဲ့ Software။
- Job Scheduler: Slurm။
- GPU Monitoring: DCGM နဲ့ သက်ဆိုင်ရာ Monitoring Tools။
- Storage: Training Datasets, Model Checkpoints နဲ့ Model Artifacts များအတွက် Storage Systems။

ဒီ Components တွေက တာဝန်မတူကြပါဘူး။ Slurm က Job နဲ့ Resource Allocation ကို စီမံပြီး NCCL က Distributed GPU Workloads တွေအကြား Communication ကို ဆောင်ရွက်ပေးပါတယ်။ DCGM က GPU Health နဲ့ Telemetry ကို စောင့်ကြည့်ပေးပါတယ်။

### 3. Slurm နဲ့ NVIDIA DGX Infrastructure

#### 3.1 DGX Systems နဲ့ Multi-Node Training

NVIDIA DGX System တစ်ခုမှာ GPU အများအပြား ပါဝင်နိုင်ပါတယ်။ Training Workload ကြီးလာတဲ့အခါ DGX Systems အများအပြားကို Cluster အဖြစ် ချိတ်ဆက်အသုံးပြုနိုင်ပါတယ်။

ဥပမာ—

```
DGX Node 1 — Training Process များ
DGX Node 2 — Training Process များ
DGX Node 3 — Training Process များ
DGX Node 4 — Training Process များ
```

Slurm က သတ်မှတ်ထားတဲ့ Resource Requirements, Job Priority, Cluster Configuration နဲ့ Resource Availability တို့ကို အခြေခံပြီး Job ကို Schedule လုပ်ပေးနိုင်ပါတယ်။

```
                 AI Researcher
                       |
                       v
                 Slurm Scheduler
                       |
              Resource Allocation
                       |
         +-------------+-------------+
         |             |             |
         v             v             v
      DGX Node 1    DGX Node 2    DGX Node 3
       GPUs           GPUs           GPUs
         |             |             |
         +-------------+-------------+
                       |
              Distributed Training
```

ဤပုံမှာ Slurm က ဘယ် Nodes တွေကို Job အတွက် အသုံးပြုမလဲဆိုတာ စီမံပေးတဲ့ အယူအဆကို ပြထားပါတယ်။

DGX SuperPOD ဆိုတာ DGX Servers တွေကို High-Performance Networking, Storage နဲ့ Management Infrastructure တို့နဲ့ ပေါင်းစပ်ထားတဲ့ ကြီးမားတဲ့ AI Infrastructure Architecture ဖြစ်ပါတယ်။ Slurm ကဲ့သို့ Scheduler ကို အသုံးပြုပြီး သင့်လျော်တဲ့ Training Jobs တွေကို ဒီလို Cluster များပေါ်မှာ Run လုပ်နိုင်ပါတယ်။ သို့သော် DGX SuperPOD တိုင်းမှာ Slurm ကို မဖြစ်မနေ သုံးရမယ်လို့ မဆိုလိုပါဘူး။

#### 3.2 Slurm က ဘာတွေကို စီမံပေးသလဲ?

Slurm ရဲ့ အဓိကတာဝန်တွေက—

- User က Submit လုပ်တဲ့ Job ကို လက်ခံခြင်း။
- Job အတွက် CPU, GPU, Memory နဲ့ Node Requirements ကို စစ်ဆေးခြင်း။
- Scheduling Policy နဲ့ Resource Availability ကို အခြေခံပြီး Job ကို Queue ထဲမှာ စောင့်စေခြင်း သို့မဟုတ် Run လုပ်ပေးခြင်း။
- Job အတွက် သတ်မှတ်ထားတဲ့ Compute Resources တွေကို Allocate လုပ်ခြင်း။
- Job Status နဲ့ Resource Allocation ကို စီမံခြင်း။

Slurm မှာ NVIDIA GPU အသုံးပြုဖို့ GPU Resources တွေကို GRES (Generic Resources) နဲ့ သတ်မှတ်နိုင်ပါတယ်။ အောက်ပါ Job Script Directive က ဥပမာတစ်ခု ဖြစ်ပါတယ်။

```bash
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=1
#SBATCH --gres=gpu:4
#SBATCH --time=08:00:00
```

ဤဥပမာမှာ Node 2 ခုကို တောင်းဆိုထားပြီး Node တစ်ခုစီအတွက် Task တစ်ခုနဲ့ GPU 4 ခု တောင်းဆိုထားပါတယ်။ အမှန်တကယ် အသုံးပြုနိုင်မှုက Slurm Cluster Configuration နဲ့ Resource Availability အပေါ် မူတည်ပါတယ်။

အသုံးဝင်တဲ့ Slurm Commands များ—

```
sbatch train-job.sh
squeue
sinfo
scontrol show job <JOB_ID>
```

သတိပြုရန် — Slurm က GPU Resources တွေကို Allocate လုပ်ပေးနိုင်ပေမဲ့ CUDA Application ရဲ့ Code ကို အလိုအလျောက် ပြုပြင်ပေးတာ၊ GPU Architecture တစ်ခုချင်းစီနဲ့ အလိုအလျောက် Compatible ဖြစ်အောင် ပြောင်းပေးတာမျိုး မဟုတ်ပါဘူး။ Application Environment, Driver, Libraries နဲ့ Training Framework တွေကလည်း သင့်လျော်ရပါမယ်။

### 4. DCGM — GPU Health နဲ့ Performance Monitoring

DCGM (NVIDIA Data Center GPU Manager) က NVIDIA Data Center GPU များကို Monitoring နဲ့ Management လုပ်ဖို့ အသုံးပြုတဲ့ Tool Suite ဖြစ်ပါတယ်။

DCGM က အောက်ပါအချက်တွေကို စောင့်ကြည့်နိုင်ပါတယ်—

- GPU Utilization
- GPU Memory Usage
- Temperature
- Power Consumption
- GPU Health နဲ့ Error Information
- သက်ဆိုင်ရာ Performance Metrics
- Diagnostic Information

DCGM ရဲ့ ရည်ရွယ်ချက်က GPU Infrastructure ကို စောင့်ကြည့်ခြင်း၊ ပြဿနာရှာဖွေခြင်းနဲ့ Operational Reliability ကို ကူညီပေးခြင်း ဖြစ်ပါတယ်။

သို့သော် အရေးကြီးတဲ့ ကွာခြားချက်တစ်ခု ရှိပါတယ် — **Monitoring လုပ်ခြင်းနဲ့ Scheduling ဆုံးဖြတ်ခြင်းဟာ တာဝန်မတူပါဘူး။**

ဥပမာ DCGM က GPU Utilization 95% ဖြစ်နေတယ်ဆိုတာကို ဖော်ပြပေးနိုင်ပါတယ်။ ဒါပေမဲ့ Slurm က အဲဒီ Metrics ကို အခြေခံပြီး Job ကို အခြား Node ဆီ အလိုအလျောက် ပြောင်းရွှေ့မယ်လို့ မဆိုလိုပါဘူး။ ထိုသို့ ဆုံးဖြတ်ဖို့ သက်ဆိုင်ရာ Scheduler Integration သို့မဟုတ် Policy လိုအပ်နိုင်ပါတယ်။

Reference: NVIDIA DCGM Documentation (https://developer.nvidia.com/dcgm)

### 5. NCCL — Distributed GPU Communication

NCCL (NVIDIA Collective Communications Library) က Multi-GPU နဲ့ Multi-Node Training အတွက် Collective Communication Operations များကို ဆောင်ရွက်ပေးတဲ့ Library ဖြစ်ပါတယ်။

Distributed Training မှာ Model Training ကို GPU အများအပြားနဲ့ ခွဲလုပ်နိုင်ပါတယ်။ ဒါပေမဲ့ Training Processes တွေက Gradient သို့မဟုတ် Model Data တွေကို မျှဝေဖို့ လိုအပ်နိုင်ပါတယ်။

NCCL မှာ အသုံးများတဲ့ Operations တွေက—

- **AllReduce**: Process အများအပြားက Value တွေကို Reduce လုပ်ပြီး Result ကို ပါဝင်တဲ့ Process များဆီ ပြန်လည်ဖြန့်ဝေခြင်း။
- **Broadcast**: Process တစ်ခုမှ Data ကို အခြား Process များဆီ ဖြန့်ဝေခြင်း။
- **ReduceScatter**: Data ကို Reduce လုပ်ပြီး Result ကို Process များအကြား ခွဲဝေခြင်း။
- **AllGather**: Process များဆီက Data တွေကို စုစည်းပြီး Process အားလုံးဆီ ဖြန့်ဝေခြင်း။

ဥပမာ Data Parallel Training မှာ GPU တစ်ခုစီက Mini-batch တစ်ခုကို အသုံးပြုပြီး Gradient တွေ တွက်ချက်နိုင်ပါတယ်။ နောက်တစ်ဆင့်မှာ Gradient တွေကို စုစည်းပြီး Model Parameters ကို Update လုပ်ဖို့ NCCL AllReduce ကို အသုံးပြုနိုင်ပါတယ်။

```
GPU 0 -> Local Gradients
GPU 1 -> Local Gradients
GPU 2 -> Local Gradients
GPU 3 -> Local Gradients
            |
            v
      NCCL AllReduce
            |
            v
  Synchronized Gradients
            |
            v
     Model Update
```

NCCL က Communication Operations တွေကို ပံ့ပိုးပေးပြီး အသုံးပြုနိုင်တဲ့ Hardware နဲ့ Software Configuration အပေါ်မူတည်ကာ NVLink, PCIe သို့မဟုတ် Network Transports တွေကို အသုံးချနိုင်ပါတယ်။

Slurm က NCCL ကို အစားထိုးတာ မဟုတ်သလို NCCL က Slurm ကို အစားထိုးတာလည်း မဟုတ်ပါဘူး။ Slurm က Job ကို Schedule လုပ်ပြီး NCCL က Distributed Training Processes တွေအကြား Data Communication ကို ကူညီပေးပါတယ်။

### 6. GPUDirect RDMA — GPU Memory အကြား Data Transfer

GPUDirect RDMA က Supported Network Adapters နဲ့ GPU များအကြား Data Transfer ကို GPU Memory နဲ့ ပိုမိုတိုက်ရိုက်ချိတ်ဆက်ပေးနိုင်တဲ့ Technology ဖြစ်ပါတယ်။

Traditional Data Path တစ်ခုမှာ Data ကို GPU Memory နဲ့ Host Memory အကြား ကူးပြောင်းရတာတွေ ပါဝင်နိုင်ပါတယ်။ Supported GPUDirect RDMA Path တစ်ခုမှာတော့ Network Adapter က GPU Memory ကို တိုက်ရိုက် Access လုပ်နိုင်တဲ့ Path ကို အသုံးပြုနိုင်ပါတယ်။

Traditional-style path (conceptual)

```
GPU Memory
    |
    v
Host Memory
    |
    v
Network Adapter
    |
    v
Remote Network
```

Supported GPUDirect RDMA Path—

```
GPU Memory
    |
    v
Compatible Network Adapter
    |
    v
High-Speed Network
    |
    v
Remote Network Adapter
    |
    v
Remote GPU Memory
```

GPUDirect RDMA ရဲ့ အကျိုးကျေးဇူးတွေက—

- အချို့ Data Transfer Paths တွေမှာ Host Memory Staging ကို လျှော့ချနိုင်ခြင်း။
- CPU ပါဝင်ရတဲ့ Data Movement အချို့ကို လျှော့ချနိုင်ခြင်း။
- Latency နဲ့ Data Transfer Overhead ကို လျှော့ချနိုင်ခြင်း။
- Multi-Node Distributed Training Performance ကို ကူညီပေးနိုင်ခြင်း။

သို့သော် GPUDirect RDMA က CPU ကို လုံးဝဖယ်ရှားပေးတာ မဟုတ်ပါဘူး။ Setup, Memory Registration, Drivers နဲ့ Application Coordination တို့မှာ CPU နဲ့ Software Stack ပါဝင်နေနိုင်ပါတယ်။ ထို့အပြင် Hardware နဲ့ Software Support လည်း လိုအပ်ပါတယ်။

GPUDirect RDMA ကို GPU-to-GPU Communication နဲ့ ဆက်စပ်ပြီး ပြောလေ့ရှိပေမဲ့ ၎င်းဟာ GPU Memory နဲ့ Supported Peer Device, ဥပမာ Network Adapter တို့အကြား Data Path ကို အဓိက ပံ့ပိုးပေးတာဖြစ်ပါတယ်။ NCCL ကဲ့သို့ Library က အဆိုပါ Technology ကို အသုံးပြုနိုင်တဲ့ Communication Path တစ်ခုအဖြစ် အသုံးချနိုင်ပါတယ်။

### 7. Kubernetes နဲ့ NVIDIA GPU Infrastructure

Slurm ကဲ့သို့ Job Scheduler အစား Kubernetes ကို အသုံးပြုတဲ့အခါ Containerized AI Applications တွေကို GPU-enabled Cluster ပေါ်မှာ Deploy လုပ်နိုင်ပါတယ်။

ဥပမာ—

- LLM Inference Server
- Image Classification API
- Model Serving Microservice
- GPU-Accelerated Data Processing Service

Kubernetes က Pods တွေကို Schedule လုပ်ပြီး Application Desired State ကို ထိန်းသိမ်းပေးပါတယ်။ NVIDIA GPU Operator က Kubernetes Environment မှာ GPU Software Components တွေကို Provision နဲ့ Manage လုပ်ဖို့ ကူညီပေးနိုင်ပါတယ်။

#### 7.1 NVIDIA GPU Operator

GPU Operator က ပုံမှန်အားဖြင့် Configuration အပေါ်မူတည်ပြီး အောက်ပါ Components များကို စီမံနိုင်ပါတယ်—

- NVIDIA GPU Driver
- NVIDIA Container Toolkit
- Kubernetes NVIDIA Device Plugin
- GPU Feature Discovery
- DCGM Exporter
- MIG Manager

GPU Operator ဟာ Host မှာ CUDA Toolkit အပြည့်အစုံကို မဖြစ်မနေ Install လုပ်ပေးတဲ့ Tool တစ်ခုလို့ နားလည်ခြင်း မသင့်တော်ပါဘူး။ ၎င်းရဲ့ အဓိကတာဝန်က GPU ကို Containerized Workloads တွေက အသုံးပြုနိုင်ဖို့ လိုအပ်တဲ့ GPU Software Components တွေကို စီမံပေးတာ ဖြစ်ပါတယ်။ CUDA Libraries သို့မဟုတ် Runtime Components တွေကို Application Container Image မှာ ထည့်သွင်းထားဖို့လည်း လိုအပ်နိုင်ပါတယ်။

Official documentation: NVIDIA GPU Operator (https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/index.html)

#### 7.2 GPU Device Plugin နဲ့ Kubernetes Scheduling

NVIDIA Device Plugin က GPU Resources တွေကို Kubernetes က အသိအမှတ်ပြုပြီး Allocate လုပ်နိုင်ဖို့ ကူညီပေးပါတယ်။

ဥပမာ GPU-enabled Node တစ်ခုမှာ GPU 4 ခု ရှိတယ်ဆိုပါစို့။ Kubernetes က `nvidia.com/gpu` ကဲ့သို့ Resource Name နဲ့ GPU Resources တွေကို ကြေညာနိုင်ပါတယ်။

အောက်ပါ YAML က ဥပမာတစ်ခု ဖြစ်ပါတယ်။

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gpu-inference
spec:
  replicas: 1
  selector:
    matchLabels:
      app: gpu-inference
  template:
    metadata:
      labels:
        app: gpu-inference
    spec:
      containers:
        - name: inference
          image: your-registry/your-inference-image:latest
          resources:
            limits:
              nvidia.com/gpu: 1
```

ဤဥပမာမှာ Pod တစ်ခုအတွက် GPU 1 ခု တောင်းဆိုထားပါတယ်။ `your-registry/your-inference-image:latest` ကို အမှန်တကယ် Application Image နဲ့ အစားထိုးရပါမယ်။

GPU Resource ကို Kubernetes က သိရှိနိုင်ဖို့ Compatible Driver, Container Runtime Integration နဲ့ Device Plugin တို့ကို မှန်ကန်စွာ Configure လုပ်ထားဖို့ လိုအပ်ပါတယ်။

#### 7.3 DCGM Exporter, Prometheus နဲ့ Grafana

Kubernetes Environment မှာ DCGM Exporter ကို အသုံးပြုပြီး GPU Metrics တွေကို Monitoring System ဆီ ပို့နိုင်ပါတယ်။

```
NVIDIA GPU
    |
    v
DCGM Exporter
    |
    v
Prometheus
    |
    v
Grafana Dashboards
```

ဤ Architecture က GPU Utilization, Memory Usage, Temperature နဲ့ အခြား Metrics တွေကို ကြည့်ရှုနိုင်စေပါတယ်။

သို့သော် Prometheus နဲ့ Grafana က Monitoring အတွက် ဖြစ်ပြီး Kubernetes Scheduler က GPU Metrics အားလုံးကို အလိုအလျောက် အသုံးပြုစေဖို့ မဖြစ်မနေ မလုပ်ပေးပါဘူး။ Metrics-based Scheduling သို့မဟုတ် Scaling အတွက် သီးခြား Configuration နဲ့ Integration လိုအပ်နိုင်ပါတယ်။

### 8. MIG Awareness နဲ့ GPU Resource Allocation

MIG (Multi-Instance GPU) က Supported NVIDIA GPU များကို Hardware-backed GPU Instances များအဖြစ် ခွဲဝေပေးနိုင်တဲ့ Technology ဖြစ်ပါတယ်။

ဥပမာ MIG-capable GPU တစ်ခုကို သင့်လျော်တဲ့ MIG Profiles တွေနဲ့ ခွဲထားနိုင်ပါတယ်။ Kubernetes နဲ့ NVIDIA Device Plugin Configuration က MIG Instances တွေကို Resource အဖြစ် ဖော်ပြနိုင်ပြီး Workloads တွေက သတ်မှတ်ထားတဲ့ Resource Type ကို တောင်းဆိုနိုင်ပါတယ်။

NVIDIA GPU Operator မှာ MIG Manager ပါဝင်တဲ့ Configuration ကို အသုံးပြုပါက MIG Configuration ကို စီမံနိုင်ပါတယ်။ (https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-operator-mig.html)

သတိပြုရန်—

- GPU အားလုံးက MIG ကို မပံ့ပိုးပါဘူး။
- MIG Profile နဲ့ Instance အရေအတွက်က GPU Model အပေါ် မူတည်ပါတယ်။
- MIG Configuration ပြောင်းလဲတဲ့အခါ Workloads တွေကို ရပ်နားဖို့ သို့မဟုတ် Node ကို Reboot လုပ်ဖို့ လိုအပ်နိုင်ပါတယ်။
- MIG Awareness ရှိတယ်ဆိုတာ Live GPU Utilization ကို ကြည့်ပြီး Application ကို အမြဲတမ်း အလိုအလျောက် ရွှေ့ပြောင်းပေးမယ်လို့ မဆိုလိုပါဘူး။

### 9. GPU Scheduling, Load Balancing နဲ့ Workload Rebalancing

ဒီအပိုင်းက နားလည်မှုလွဲနိုင်တဲ့ အရေးကြီးဆုံးအချက် ဖြစ်ပါတယ်။

**GPU Scheduling** — Kubernetes Scheduler က Pod တစ်ခုကို သင့်တော်တဲ့ Node မှာ နေရာချထားဖို့ ဆောင်ရွက်ပါတယ်။ GPU Resource Request, Node Capacity, Labels နဲ့ Scheduling Constraints တွေကို အခြေခံနိုင်ပါတယ်။

**Load Balancing** — Kubernetes Service က သင့်တော်တဲ့ Backend Pods များဆီ Network Traffic ကို ပို့ဆောင်ပေးနိုင်ပါတယ်။ Application-Level Load Balancer တွေကိုလည်း အသုံးပြုနိုင်ပါတယ်။

Traffic Load Balancing နဲ့ GPU Compute Load Balancing ဟာ တစ်ထပ်တည်း မဟုတ်ပါဘူး။ Network Request တွေကို ခွဲဝေပေးတာက GPU Utilization ကို အလိုအလျောက် အညီအမျှ ဖြစ်စေမယ်လို့ မဆိုလိုပါဘူး။

**Autoscaling** — HPA က သတ်မှတ်ထားတဲ့ Metrics အပေါ်မူတည်ပြီး Pod Replica အရေအတွက်ကို ပြောင်းလဲနိုင်ပါတယ်။ GPU Workload အတွက် အသုံးဝင်ဖို့ သင့်လျော်တဲ့ Metrics, Resource Requests နဲ့ Scaling Policy တွေ လိုအပ်ပါတယ်။

**Node Maintenance နဲ့ Rescheduling** — GPU Node တစ်ခုကို Cluster မှ ဖယ်ရှားမယ်ဆိုရင် သင့်လျော်တဲ့ Maintenance Workflow, Drain နဲ့ Workload Controller Configuration တို့ကို အသုံးပြုနိုင်ပါတယ်။ သို့သော် GPU Workload အားလုံးကို အခြား Node တစ်ခုဆီ ချက်ချင်းရွှေ့ပြောင်းပေးနိုင်တာ မဟုတ်ပါဘူး။ GPU Pod တစ်ခုမှာ အသုံးပြုနေတဲ့ GPU Memory State ကို အခြား Node ဆီ အလိုအလျောက် Live-Migrate လုပ်ပေးမယ်လို့လည်း မယူဆသင့်ပါဘူး။

အကျဉ်းချုပ်အားဖြင့် NVIDIA GPU Operator က GPU Software Components တွေကို စီမံပေးတယ်။ Kubernetes က Pod Scheduling နဲ့ Workload Lifecycle ကို စီမံတယ်။ Autoscaling, Metrics-based Scheduling နဲ့ Workload Migration တို့အတွက် သက်ဆိုင်ရာ Feature နဲ့ Configuration တွေ လိုအပ်ပါတယ်။

### 10. Slurm vs. Kubernetes — NVIDIA Integration Comparison

| Capability | Slurm + NVIDIA | Kubernetes + NVIDIA |
|---|---|---|
| Primary workload pattern | HPC, Batch Training | Containerized Inference နဲ့ အခြား Apps |
| Resource scheduling | Jobs, Queues, GRES | Pods, Resource Requests, Scheduling Constraints |
| GPU access | Slurm GPU/GRES Configuration နဲ့ Compatible Runtime | Device Plugin နဲ့ Container Runtime Integration |
| GPU monitoring | DCGM နဲ့ သက်ဆိုင်ရာ Monitoring Integration | DCGM Exporter နဲ့ Monitoring Stack |
| Distributed GPU communication | NCCL နဲ့ Supported Communication Stack | NCCL နဲ့ Supported Communication Stack |
| GPU Memory Data Path | GPUDirect RDMA ကို Supported Hardware/Software မှာ အသုံးပြုနိုင် | GPUDirect RDMA ကို Supported Hardware/Software မှာ အသုံးပြုနိုင် |
| MIG | Cluster နဲ့ GPU Management Configuration ပေါ်မူတည် | Device Plugin, MIG Strategy နဲ့ MIG Manager Configuration ပေါ်မူတည် |
| Autoscaling | Job/Cluster Capacity Management | HPA/Other Scalers နဲ့ Cluster Capacity Management |
| Automatic GPU load balancing | Default အနေဖြင့် မယူဆသင့် | Default အနေဖြင့် မယူဆသင့် |

### 11. Review Questions

1. Slurm က DGX Cluster မှာ ဘယ်တာဝန်တွေကို စီမံပေးသလဲ?
2. DCGM နဲ့ NCCL တို့ရဲ့ အဓိကကွာခြားချက်က ဘာလဲ?
3. NCCL AllReduce ကို Distributed Training မှာ ဘာကြောင့် အသုံးပြုသလဲ?
4. GPUDirect RDMA က Data Transfer Overhead ကို ဘယ်လိုလျှော့ချနိုင်သလဲ?
5. NVIDIA GPU Operator က ဘယ်လို Components တွေကို စီမံနိုင်သလဲ?
6. Kubernetes Device Plugin ရဲ့ အခန်းကဏ္ဍက ဘာလဲ?
7. DCGM Exporter က Kubernetes Scheduler ကို အလိုအလျောက် အစားထိုးပေးသလား?
8. MIG Awareness ရှိခြင်းနဲ့ Live GPU Load Balancing တို့ ဘာကွာသလဲ?
9. Kubernetes မှာ Pod Replica တိုးခြင်းနဲ့ GPU Node တိုးခြင်းတို့ ဘာကွာသလဲ?
10. Slurm, NCCL, GPUDirect RDMA နဲ့ DCGM တို့ကို AI Training Architecture တစ်ခုမှာ ဘယ်လိုပေါင်းစပ်အသုံးပြုနိုင်သလဲ?

**အဖြေများ (အကျဉ်း)**

1. Multi-Node GPU Training Jobs များအတွက် Job Scheduling နဲ့ Resource Allocation (Jobs, Queues, GRES) ကို စီမံပါတယ်။ Application Code ကို ပြုပြင်ပေးတာ မဟုတ်ပါဘူး။
2. DCGM က GPU Health, Diagnostics နဲ့ Telemetry ကို စောင့်ကြည့်တဲ့ Monitoring Tool ဖြစ်ပြီး NCCL က Multi-GPU/Multi-Node Processes တွေအကြား Collective Communication ကို ဆောင်ရွက်ပေးတဲ့ Library ဖြစ်ပါတယ်။
3. GPU တစ်ခုစီရဲ့ Local Gradients တွေကို စုစည်း Reduce လုပ်ပြီး Synchronized Result ကို Process အားလုံးဆီ ပြန်ဖြန့်ဝေပေးလို့ Model Parameters ကို Update လုပ်နိုင်ဖို့ ဖြစ်ပါတယ်။
4. Network Adapter က GPU Memory ကို တိုက်ရိုက် Access လုပ်နိုင်တဲ့ Path ကို အသုံးပြုခြင်းကြောင့် Host Memory Staging နဲ့ CPU ပါဝင်ရမှု အချို့ကို လျှော့ချပြီး Latency နဲ့ Overhead ကျဆင်းနိုင်ပါတယ်။ (Hardware/Software Support လိုအပ်ပါတယ်)
5. Configuration ပေါ်မူတည်ပြီး GPU Driver, NVIDIA Container Toolkit, Device Plugin, GPU Feature Discovery, DCGM Exporter နဲ့ MIG Manager တို့ကို စီမံနိုင်ပါတယ်။
6. GPU Resources တွေကို `nvidia.com/gpu` ကဲ့သို့ Resource Name အဖြစ် ကြေညာပေးခြင်းဖြင့် Kubernetes က အသိအမှတ်ပြုပြီး Allocate လုပ်နိုင်စေပါတယ်။
7. မပေးပါဘူး။ DCGM Exporter က Metrics တွေကို Expose လုပ်ပေးရုံဖြစ်ပြီး Metrics-based Scheduling/Scaling အတွက် သီးခြား Configuration နဲ့ Integration လိုအပ်ပါတယ်။
8. MIG Awareness ဆိုတာ GPU ကို Hardware-backed Instances တွေအဖြစ် ခွဲဝေဖော်ပြခြင်းဖြစ်ပြီး Live GPU Load Balancing ဆိုတာ Workload ကို အလိုအလျောက် ရွှေ့ပြောင်းပေးခြင်းဖြစ်ပါတယ်။ နောက်တစ်ခုက Default အနေနဲ့ မရှိပါဘူး။
9. Pod Replica တိုးခြင်းက Service Instance အရေအတွက် တိုးခြင်းဖြစ်ပြီး GPU Node တိုးခြင်းက Cluster ရဲ့ Physical GPU Capacity ကို တိုးခြင်းဖြစ်ပါတယ်။
10. Slurm က Training Job တွေကို Schedule/Allocate လုပ်ပေးပြီး NCCL က Processes အကြား Communication လုပ်ပါတယ်။ GPUDirect RDMA က Data Path ကို ပိုမိုတိုက်ရိုက်ဖြစ်စေပြီး DCGM က GPU Health နဲ့ Performance ကို စောင့်ကြည့်ပါတယ်။

### 12. Module Summary

- **Slurm**: Multi-Node GPU Training Jobs များအတွက် Scheduling နဲ့ Resource Allocation ကို စီမံတယ်။
- **DCGM**: GPU Health, Diagnostics နဲ့ Performance Telemetry ကို စောင့်ကြည့်တယ်။
- **NCCL**: Multi-GPU နဲ့ Multi-Node Training အတွက် Collective Communication Operations တွေကို ဆောင်ရွက်ပေးတယ်။
- **GPUDirect RDMA**: Supported Hardware နဲ့ Software Configuration တွေမှာ GPU Memory နဲ့ Network Adapter အကြား Data Transfer Path ကို ပိုမိုတိုက်ရိုက်ဖြစ်စေနိုင်တယ်။
- **NVIDIA GPU Operator**: Kubernetes Environment မှာ GPU Software Components တွေကို Provision နဲ့ Manage လုပ်ဖို့ ကူညီပေးတယ်။
- **NVIDIA Device Plugin**: GPU Resources တွေကို Kubernetes က အသိအမှတ်ပြုပြီး Allocate လုပ်နိုင်ဖို့ ကူညီပေးတယ်။
- **DCGM Exporter**: GPU Metrics တွေကို Monitoring System တွေက ရယူနိုင်အောင် Expose လုပ်တယ်။
- **MIG Manager**: သင့်လျော်တဲ့ Configuration မှာ MIG-capable GPU များ၏ MIG Configuration ကို စီမံနိုင်တယ်။

အဓိကမှတ်သားရန် — GPU Monitoring, Job Scheduling, Collective Communication နဲ့ GPU Memory Data Transfer တို့ဟာ မတူညီတဲ့ တာဝန်တွေဖြစ်ပါတယ်။ NVIDIA AI Infrastructure မှာ ဒီ Components တွေကို သင့်လျော်စွာ ပေါင်းစပ်အသုံးပြုခြင်းက Training နဲ့ Inference Workloads တွေကို ပိုမိုထိရောက်စွာ လည်ပတ်စေနိုင်ပါတယ်။

## English Version

### 1. Learning Objectives

In this module, you will learn about the following topics—

- How Slurm schedules multi-node NVIDIA GPU training.
- How DGX Systems and DGX SuperPOD relate to each other.
- The role of DCGM in GPU health monitoring and performance telemetry.
- How NCCL handles communication for multi-GPU and multi-node training.
- How GPUDirect RDMA accelerates data transfer between GPU memories.
- How Kubernetes and the NVIDIA GPU Operator work together.
- The responsibilities of the GPU Operator, Device Plugin, DCGM Exporter and MIG Manager.
- How GPU scheduling, autoscaling, load balancing and workload rescheduling differ.
- How Slurm and Kubernetes can be combined in an AI infrastructure.

### 2. Understanding NVIDIA AI Infrastructure

A large-scale AI training run may not be feasible with a single GPU. Many GPUs, many compute nodes, a high-speed network and shared storage may be required.

An example AI training cluster may include the following components—

- GPU Compute: NVIDIA DGX Systems and other GPU servers.
- GPU Interconnect: GPU-to-GPU communication technologies such as NVLink and NVSwitch.
- Cluster Network: InfiniBand or suitable high-performance Ethernet.
- Communication Software: NCCL.
- Memory Transfer Technology: hardware and software that support GPUDirect RDMA.
- Job Scheduler: Slurm.
- GPU Monitoring: DCGM and related monitoring tools.
- Storage: storage systems for training datasets, model checkpoints and model artifacts.

These components have different responsibilities. Slurm manages jobs and resource allocation, NCCL handles communication between distributed GPU workloads, and DCGM monitors GPU health and telemetry.

### 3. Slurm and NVIDIA DGX Infrastructure

#### 3.1 DGX Systems and Multi-Node Training

An NVIDIA DGX system can include many GPUs. As the training workload grows, multiple DGX systems can be connected and used as a cluster.

Example—

```
DGX Node 1 — Training Processes
DGX Node 2 — Training Processes
DGX Node 3 — Training Processes
DGX Node 4 — Training Processes
```

Slurm can schedule jobs based on the specified resource requirements, job priority, cluster configuration and resource availability.

```
                 AI Researcher
                       |
                       v
                 Slurm Scheduler
                       |
              Resource Allocation
                       |
         +-------------+-------------+
         |             |             |
         v             v             v
      DGX Node 1    DGX Node 2    DGX Node 3
       GPUs           GPUs           GPUs
         |             |             |
         +-------------+-------------+
                       |
              Distributed Training
```

This diagram shows the concept of Slurm deciding which nodes are used for a job.

A DGX SuperPOD is a large AI infrastructure architecture that combines DGX servers with high-performance networking, storage and management infrastructure. Suitable training jobs can be run on such clusters with a scheduler such as Slurm. However, this does not mean that every DGX SuperPOD must use Slurm.

#### 3.2 What Does Slurm Manage?

Slurm's main responsibilities are—

- Accepting jobs submitted by users.
- Checking the CPU, GPU, memory and node requirements for a job.
- Holding a job in a queue or running it based on scheduling policy and resource availability.
- Allocating the specified compute resources for a job.
- Managing job status and resource allocation.

To use NVIDIA GPUs with Slurm, GPU resources can be specified with GRES (Generic Resources). The following job script directive is an example.

```bash
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=1
#SBATCH --gres=gpu:4
#SBATCH --time=08:00:00
```

This example requests two nodes, with one task per node and four GPUs per node. Actual availability depends on the Slurm cluster configuration and resource availability.

Useful Slurm commands—

```
sbatch train-job.sh
squeue
sinfo
scontrol show job <JOB_ID>
```

Note that Slurm can allocate GPU resources, but it does not automatically rewrite CUDA application code or make it compatible with each GPU architecture. The application environment, drivers, libraries and training framework must also be appropriate.

### 4. DCGM — GPU Health and Performance Monitoring

DCGM (NVIDIA Data Center GPU Manager) is a tool suite for monitoring and managing NVIDIA data center GPUs.

DCGM can observe—

- GPU utilization
- GPU memory usage
- Temperature
- Power consumption
- GPU health and error information
- Relevant performance metrics
- Diagnostic information

DCGM's purpose is to help monitor GPU infrastructure, find problems and support operational reliability.

There is one important distinction, however: **monitoring and scheduling decisions are different responsibilities.**

For example, DCGM can report that GPU utilization is at 95%. That does not mean Slurm will automatically move the job to another node based on those metrics. Doing so may require the relevant scheduler integration or policy.

Reference: NVIDIA DCGM Documentation (https://developer.nvidia.com/dcgm)

### 5. NCCL — Distributed GPU Communication

NCCL (NVIDIA Collective Communications Library) is a library that performs collective communication operations for multi-GPU and multi-node training.

Distributed training can split model training across many GPUs. However, the training processes may need to share gradients or model data.

Common NCCL operations—

- **AllReduce**: reduces values from many processes and redistributes the result to all participating processes.
- **Broadcast**: distributes data from one process to the other processes.
- **ReduceScatter**: reduces data and splits the result among processes.
- **AllGather**: gathers data from processes and distributes it to all processes.

For example, in data-parallel training each GPU can compute gradients on one mini-batch. In the next step, NCCL AllReduce can be used to combine the gradients and update the model parameters.

```
GPU 0 -> Local Gradients
GPU 1 -> Local Gradients
GPU 2 -> Local Gradients
GPU 3 -> Local Gradients
            |
            v
      NCCL AllReduce
            |
            v
  Synchronized Gradients
            |
            v
     Model Update
```

NCCL provides the communication operations and can use NVLink, PCIe or network transports depending on the available hardware and software configuration.

Slurm does not replace NCCL, and NCCL does not replace Slurm. Slurm schedules jobs; NCCL helps with data communication between distributed training processes.

### 6. GPUDirect RDMA — Data Transfer Between GPU Memories

GPUDirect RDMA is a technology that connects data transfers between supported network adapters and GPUs more directly with GPU memory.

A traditional data path may involve copying data between GPU memory and host memory. A supported GPUDirect RDMA path lets the network adapter access GPU memory directly.

Traditional-style path (conceptual)

```
GPU Memory
    |
    v
Host Memory
    |
    v
Network Adapter
    |
    v
Remote Network
```

Supported GPUDirect RDMA path—

```
GPU Memory
    |
    v
Compatible Network Adapter
    |
    v
High-Speed Network
    |
    v
Remote Network Adapter
    |
    v
Remote GPU Memory
```

Benefits of GPUDirect RDMA—

- It can reduce host memory staging on some data transfer paths.
- It can reduce some CPU-involved data movement.
- It can reduce latency and data transfer overhead.
- It can help multi-node distributed training performance.

However, GPUDirect RDMA does not remove the CPU entirely. Setup, memory registration, drivers and application coordination may still involve the CPU and the software stack. Hardware and software support are also required.

GPUDirect RDMA is often discussed in connection with GPU-to-GPU communication, but its main contribution is a data path between GPU memory and a supported peer device such as a network adapter. A library like NCCL can use that technology as one available communication path.

### 7. Kubernetes and NVIDIA GPU Infrastructure

When Kubernetes is used instead of a job scheduler like Slurm, containerized AI applications can be deployed on a GPU-enabled cluster.

Examples—

- LLM inference server
- Image classification API
- model serving microservice
- GPU-accelerated data processing service

Kubernetes schedules Pods and maintains the desired state of the application. The NVIDIA GPU Operator helps provision and manage GPU software components in a Kubernetes environment.

#### 7.1 NVIDIA GPU Operator

Depending on the configuration, the GPU Operator can manage the following components—

- NVIDIA GPU driver
- NVIDIA Container Toolkit
- Kubernetes NVIDIA Device Plugin
- GPU Feature Discovery
- DCGM Exporter
- MIG Manager

The GPU Operator should not be understood as a tool that necessarily installs the complete CUDA Toolkit on the host. Its main responsibility is to manage the GPU software components that containerized workloads need in order to use the GPU. CUDA libraries or runtime components may still need to be included in the application container image.

Official documentation: NVIDIA GPU Operator (https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/index.html)

#### 7.2 GPU Device Plugin and Kubernetes Scheduling

The NVIDIA Device Plugin helps Kubernetes recognize and allocate GPU resources.

For example, suppose a GPU-enabled node has four GPUs. Kubernetes can advertise GPU resources with a resource name such as `nvidia.com/gpu`.

The following YAML is an example.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gpu-inference
spec:
  replicas: 1
  selector:
    matchLabels:
      app: gpu-inference
  template:
    metadata:
      labels:
        app: gpu-inference
    spec:
      containers:
        - name: inference
          image: your-registry/your-inference-image:latest
          resources:
            limits:
              nvidia.com/gpu: 1
```

This example requests one GPU for one Pod. `your-registry/your-inference-image:latest` must be replaced with the actual application image.

For Kubernetes to recognize GPU resources, a compatible driver, container runtime integration and the Device Plugin must be configured correctly.

#### 7.3 DCGM Exporter, Prometheus and Grafana

In a Kubernetes environment, DCGM Exporter can send GPU metrics to a monitoring system.

```
NVIDIA GPU
    |
    v
DCGM Exporter
    |
    v
Prometheus
    |
    v
Grafana Dashboards
```

This architecture makes GPU utilization, memory usage, temperature and other metrics visible.

However, Prometheus and Grafana are for monitoring; they do not automatically make the Kubernetes scheduler use all GPU metrics. Metrics-based scheduling or scaling may require separate configuration and integration.

### 8. MIG Awareness and GPU Resource Allocation

MIG (Multi-Instance GPU) is a technology that partitions supported NVIDIA GPUs into hardware-backed GPU instances.

For example, a MIG-capable GPU can be divided with appropriate MIG profiles. Kubernetes and NVIDIA Device Plugin configuration can expose MIG instances as resources, and workloads can request the specified resource type.

If a configuration that includes the MIG Manager in the NVIDIA GPU Operator is used, the MIG configuration can be managed. (https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/gpu-operator-mig.html)

Cautions—

- Not all GPUs support MIG.
- MIG profiles and instance counts depend on the GPU model.
- Changing the MIG configuration may require stopping workloads or rebooting the node.
- Having MIG awareness does not mean applications are always moved automatically based on live GPU utilization.

### 9. GPU Scheduling, Load Balancing and Workload Rebalancing

This is the most important point that is easily misunderstood.

**GPU scheduling** — the Kubernetes scheduler places a Pod on a suitable node. It can be based on GPU resource requests, node capacity, labels and scheduling constraints.

**Load balancing** — a Kubernetes Service can send network traffic to suitable backend Pods. Application-level load balancers can also be used.

Traffic load balancing and GPU compute load balancing are not the same thing. Distributing network requests does not automatically even out GPU utilization.

**Autoscaling** — HPA can change the number of Pod replicas based on specified metrics. For GPU workloads to benefit, appropriate metrics, resource requests and a scaling policy are needed.

**Node maintenance and rescheduling** — when a GPU node is removed from the cluster, appropriate maintenance workflows, drain procedures and workload controller configuration can be used. However, not all GPU workloads can be moved to another node immediately. A GPU Pod's in-use GPU memory state should not be assumed to live-migrate automatically to another node.

In short: the NVIDIA GPU Operator manages GPU software components. Kubernetes manages Pod scheduling and workload lifecycles. Autoscaling, metrics-based scheduling and workload migration each require the relevant features and configuration.

### 10. Slurm vs. Kubernetes — NVIDIA Integration Comparison

| Capability | Slurm + NVIDIA | Kubernetes + NVIDIA |
|---|---|---|
| Primary workload pattern | HPC, batch training | Containerized inference and other apps |
| Resource scheduling | Jobs, queues, GRES | Pods, resource requests, scheduling constraints |
| GPU access | Slurm GPU/GRES configuration and compatible runtime | Device Plugin and container runtime integration |
| GPU monitoring | DCGM and related monitoring integration | DCGM Exporter and monitoring stack |
| Distributed GPU communication | NCCL and supported communication stack | NCCL and supported communication stack |
| GPU memory data path | GPUDirect RDMA on supported hardware/software | GPUDirect RDMA on supported hardware/software |
| MIG | Depends on cluster and GPU management configuration | Depends on Device Plugin, MIG strategy and MIG Manager configuration |
| Autoscaling | Job/cluster capacity management | HPA/other scalers and cluster capacity management |
| Automatic GPU load balancing | Should not be assumed by default | Should not be assumed by default |

### 11. Review Questions

1. What responsibilities does Slurm manage in a DGX cluster?
2. What is the main difference between DCGM and NCCL?
3. Why is NCCL AllReduce used in distributed training?
4. How can GPUDirect RDMA reduce data transfer overhead?
5. What components can the NVIDIA GPU Operator manage?
6. What is the role of the Kubernetes Device Plugin?
7. Does DCGM Exporter automatically replace the Kubernetes scheduler?
8. What is the difference between MIG awareness and live GPU load balancing?
9. What is the difference between increasing Pod replicas and adding GPU nodes in Kubernetes?
10. How can Slurm, NCCL, GPUDirect RDMA and DCGM be combined in an AI training architecture?

**Concise answers**

1. It manages job scheduling and resource allocation (jobs, queues, GRES) for multi-node GPU training jobs. It does not modify application code.
2. DCGM is a monitoring tool for GPU health, diagnostics and telemetry; NCCL is a library for collective communication between multi-GPU/multi-node processes.
3. Each GPU's local gradients are reduced and the synchronized result is redistributed to all processes so model parameters can be updated.
4. By letting the network adapter access GPU memory directly, it reduces host memory staging and some CPU involvement, lowering latency and overhead (hardware and software support required).
5. Depending on configuration: GPU driver, NVIDIA Container Toolkit, Device Plugin, GPU Feature Discovery, DCGM Exporter and MIG Manager.
6. It advertises GPU resources under a resource name such as `nvidia.com/gpu` so Kubernetes can recognize and allocate them.
7. No. DCGM Exporter only exposes metrics; metrics-based scheduling or scaling needs separate configuration and integration.
8. MIG awareness partitions GPUs into hardware-backed instances and exposes them; live GPU load balancing moves workloads automatically. The latter does not exist by default.
9. Increasing Pod replicas adds service instances; adding GPU nodes increases the cluster's physical GPU capacity.
10. Slurm schedules and allocates training jobs, NCCL handles inter-process communication, GPUDirect RDMA makes the data path more direct, and DCGM monitors GPU health and performance.

### 12. Module Summary

- **Slurm**: manages scheduling and resource allocation for multi-node GPU training jobs.
- **DCGM**: monitors GPU health, diagnostics and performance telemetry.
- **NCCL**: performs collective communication operations for multi-GPU and multi-node training.
- **GPUDirect RDMA**: can make the data transfer path between GPU memory and the network adapter more direct on supported hardware and software configurations.
- **NVIDIA GPU Operator**: helps provision and manage GPU software components in a Kubernetes environment.
- **NVIDIA Device Plugin**: helps Kubernetes recognize and allocate GPU resources.
- **DCGM Exporter**: exposes GPU metrics so monitoring systems can collect them.
- **MIG Manager**: can manage the MIG configuration of MIG-capable GPUs in a suitable configuration.

Key takeaway — GPU monitoring, job scheduling, collective communication and GPU memory data transfer are different responsibilities. Combining these components appropriately in NVIDIA AI infrastructure helps training and inference workloads run more efficiently.

## Technical review notes

- Sections 1–3 use the module's real source text (module transcript beginning at 'Module 38 — Slurm and Kubernetes Integration with NVIDIA Infrastructure'): learning objectives, understanding NVIDIA AI infrastructure, and Slurm with NVIDIA DGX infrastructure (DGX multi-node training, the AI Researcher → Slurm Scheduler → DGX Nodes diagram, DGX SuperPOD, and what Slurm manages including GRES). The source text is cut mid section 3.2 at the job-script directive example, which continues into the supplied `#SBATCH` block and the rest of the module content. No commands or specifications beyond the supplied `#SBATCH` block and Slurm command list were added.
- The Slurm-vs-Kubernetes comparison in section 10 was reflowed from the source transcript into a Markdown table; content is unchanged.
- The YAML example and all diagrams are reproduced from the source; `your-registry/your-inference-image:latest` is a placeholder as stated in the source.
- The content following the `#SBATCH` example (Slurm commands, DCGM, NCCL, GPUDirect RDMA, Kubernetes sections, and the review questions/summary) is the same module continuing after the job-script example in both a Burmese detailed section and its English version, so the current bilingual structure is kept and section numbering continues unchanged.
- Caveats preserved from source: monitoring ≠ scheduling; DCGM does not move jobs automatically; GPUDirect RDMA does not eliminate the CPU; GPU Operator does not necessarily install the full CUDA Toolkit; MIG is not supported on all GPUs; no default automatic GPU load balancing or live migration is assumed; NGC Registry is not a complete Model Registry; BCM (Base Command Manager) is not the same as Base Command Platform.
