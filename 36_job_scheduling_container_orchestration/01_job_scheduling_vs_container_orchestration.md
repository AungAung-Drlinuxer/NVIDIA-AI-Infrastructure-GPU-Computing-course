# Module 36 — Job Scheduling vs. Container Orchestration

> Part VII · AI Workload Operations and Orchestration — Course Module 36 — topic notes

## မြန်မာဘာသာ version

## 1. Learning Objectives

ဤ Module ကို လေ့လာပြီးနောက် အောက်ပါအကြောင်းအရာများကို နားလည်နိုင်ရမည်။

- Job Scheduling ဆိုတာ ဘာလဲ၊ AI Training မှာ ဘာကြောင့် လိုအပ်သလဲ။
- Container Orchestration ဆိုတာ ဘာလဲ၊ Production Inference မှာ ဘာကြောင့် အသုံးဝင်သလဲ။
- Slurm နဲ့ Kubernetes တို့ရဲ့ အဓိကတာဝန်တွေ ဘာကွာသလဲ။
- Resource Allocation, Job Queue, Priority, Load Balancing, Auto Scaling နဲ့ Self-Healing တို့ကို ခွဲခြားနားလည်ခြင်း။
- Training Cluster နဲ့ Production Inference Platform တို့အတွက် သင့်တော်သော Management Approach ကို ရွေးချယ်ခြင်း။
- လက်တွေ့ AI Infrastructure မှာ Job Scheduling နဲ့ Container Orchestration ကို ပေါင်းစပ်အသုံးပြုပုံကို နားလည်ခြင်း။

## 2. AI Infrastructure မှာ Operations Management ဘာကြောင့်လိုအပ်သလဲ?

GPU Server တွေ၊ High-Speed Network တွေ၊ Storage Systems တွေကို တည်ဆောက်ထားရုံနဲ့ AI Workload တွေကို ထိရောက်စွာ လည်ပတ်နိုင်မှာ မဟုတ်ပါဘူး။

ဘယ် Workload က ဘယ် Resource ကို အသုံးပြုမလဲ၊ ဘယ်အချိန်မှာ စတင်မလဲ၊ Resource မလုံလောက်ရင် ဘယ်လိုစီမံမလဲ၊ Application ပျက်သွားရင် ဘယ်လိုပြန်လည်ထူထောင်မလဲဆိုတာတွေကိုလည်း စီမံဖို့ လိုအပ်ပါတယ်။

AI Infrastructure Operations မှာ အရေးကြီးတဲ့ Management Mechanisms နှစ်ခုက—

- Job Scheduling — Compute Jobs တွေကို Queue, Schedule နဲ့ Allocate လုပ်ခြင်း။
- Container Orchestration — Containerized Applications တွေကို Deploy လုပ်ခြင်း၊ Run လုပ်ခြင်းနဲ့ Lifecycle ကို စီမံခြင်း။

အလွယ်မှတ်ရန်—

- Training Job တစ်ခုက GPU Resources တွေကို အချိန်ကြာကြာ အသုံးပြုပြီး Training ပြီးဆုံးတဲ့အထိ လုပ်ဆောင်နိုင်တယ်။
- Inference Service တစ်ခုက User Request တွေကို အချိန်ပြည့် လက်ခံပြီး Response ပြန်ပေးနေရတယ်။

ပထမအမျိုးအစားမှာ Job Queue နဲ့ Compute Resource Allocation က အရေးကြီးပြီး၊ ဒုတိယအမျိုးအစားမှာ Application Availability, Health Checks နဲ့ Request Traffic Management တို့က အရေးကြီးပါတယ်။

## 3. Job Scheduling ဆိုတာ ဘာလဲ?

Job Scheduling ဆိုတာ Compute Cluster တစ်ခုမှာ Submit လုပ်ထားတဲ့ Jobs တွေကို ဘယ်အချိန်မှာ Run လုပ်မလဲ၊ ဘယ် Compute Nodes တွေမှာ Run လုပ်မလဲ၊ ဘယ် Resource တွေကို Allocate လုပ်ပေးမလဲဆိုတာ စီမံပေးတဲ့ Mechanism ဖြစ်ပါတယ်။

ဥပမာ AI Research Team တစ်ခုမှာ Training Jobs သုံးခု ရှိတယ်ဆိုပါစို့။

- Job A — GPU 8 ခု လိုအပ်တယ်။
- Job B — GPU 2 ခု လိုအပ်တယ်။
- Job C — GPU 4 ခု လိုအပ်တယ်။

Cluster မှာ GPU 8 ခုသာ လွတ်နေတယ်ဆိုရင် Jobs အားလုံးကို တစ်ပြိုင်နက်တည်း Run လုပ်နိုင်မှာ မဟုတ်ပါဘူး။ Scheduler က Job တစ်ခုချင်းစီရဲ့ Resource Requirements, Priority, Queue Policy နဲ့ Cluster အတွင်းရှိ လက်ရှိ Resource Availability တို့ကို အခြေခံပြီး ဘယ် Job ကို စတင်နိုင်မလဲဆိုတာ ဆုံးဖြတ်ပေးပါတယ်။

**Job Scheduler ရဲ့ အဓိကလုပ်ဆောင်ချက်များ**

- Job Submission: အသုံးပြုသူက Job ကို Cluster သို့ Submit လုပ်ခြင်း။
- Queue Management: စောင့်ဆိုင်းနေတဲ့ Jobs တွေကို စီမံခြင်း။
- Priority Management: သတ်မှတ်ထားတဲ့ Policy အတိုင်း Job Priority ကို စီမံခြင်း။
- Resource Allocation: CPU, GPU, RAM နဲ့ Compute Nodes များကို Job အတွက် ခွဲဝေပေးခြင်း။
- Scheduling: Resource လုံလောက်ပြီး Policy နဲ့ကိုက်ညီတဲ့အခါ Job ကို Run လုပ်စေခြင်း။
- Job Monitoring: Job ရဲ့ Running, Pending, Completed သို့မဟုတ် Failed Status များကို စောင့်ကြည့်ခြင်း။

Job Scheduler တစ်ခုက Cluster Resource Utilization ကို တိုးတက်စေဖို့ ကူညီနိုင်ပါတယ်။ ဒါပေမဲ့ Scheduler ရှိရုံနဲ့ GPU တွေ အမြဲ 100% Utilization ရရှိမယ်လို့တော့ မဆိုနိုင်ပါဘူး။ Workload ရဲ့ သဘောသဘာဝနဲ့ Resource Bottlenecks တွေကလည်း သက်ရောက်မှုရှိပါတယ်။

## 4. Analogy 1 — Airport Air Traffic Controller

Job Scheduling ကို နားလည်ဖို့ Airport တစ်ခုရဲ့ Air Traffic Controller နဲ့ နှိုင်းယှဉ်ကြည့်ရအောင်။

- Airport မှာ Runway တွေက အကန့်အသတ်ရှိသလို AI Cluster မှာလည်း GPU, CPU, Memory နဲ့ Compute Nodes တွေက အကန့်အသတ်ရှိပါတယ်။
- လေယာဉ်တစ်စင်းစီက Runway အသုံးပြုဖို့ လိုအပ်သလို Training Job တစ်ခုစီကလည်း GPU နဲ့ Compute Resources တွေ လိုအပ်ပါတယ်။

Air Traffic Controller က ဘယ်လေယာဉ်ကို အရင် Take Off လုပ်ခွင့်ပေးမလဲ၊ ဘယ် Runway ကို အသုံးပြုမလဲ၊ ဘယ်လိုအခြေအနေမှာ စောင့်ဆိုင်းရမလဲဆိုတာ စီမံပါတယ်။

အလားတူ Job Scheduler ကလည်း—

- ဘယ် Job ကို အရင် Run လုပ်မလဲ။
- ဘယ် Job က Resource ပိုလိုအပ်သလဲ။
- ဘယ် Node တွေမှာ Resource လုံလောက်သလဲ။
- ဘယ် Job ကို Pending အဖြစ် ထားရမလဲ။
- Job ပြီးသွားတဲ့အခါ Resource တွေကို ဘယ်လို ပြန်လည်အသုံးပြုနိုင်မလဲ။

ဆိုတာတွေကို စီမံပေးပါတယ်။

ဒီ Analogy ရဲ့ အဓိကသင်ခန်းစာက — Job Scheduling ရဲ့ ရည်ရွယ်ချက်ဟာ Compute Resources တွေကို Policy နဲ့ကိုက်ညီစွာ ခွဲဝေပြီး Jobs တွေကို ထိရောက်စွာ လုပ်ဆောင်စေဖို့ ဖြစ်ပါတယ်။

## 5. Slurm ဆိုတာ ဘာလဲ?

Slurm (Simple Linux Utility for Resource Management) သည် Open-Source Workload Manager နဲ့ Job Scheduler ဖြစ်ပြီး HPC နှင့် AI Training Cluster များမှာ အသုံးများပါတယ်။

Slurm က User တွေ Submit လုပ်တဲ့ Compute Jobs တွေကို Queue ထဲမှာ စီမံပြီး သတ်မှတ်ထားတဲ့ Resources တွေနဲ့ Cluster Nodes များပေါ်မှာ Run လုပ်ပေးနိုင်ပါတယ်။

**Slurm Job Workflow**

```
Researcher / Data Scientist
           |
           v
     Submit Training Job
           |
           v
       Slurm Queue
           |
           v
  Scheduler Selects Resources
           |
           v
  Allocate CPU / GPU / Memory
           |
           v
   Run on Compute Node(s)
           |
           v
  Completed / Failed / Requeue
```

ဥပမာ Slurm ကို အသုံးပြုပြီး Training Job တစ်ခု Submit လုပ်နိုင်ပါတယ်။

```bash
#!/bin/bash
#SBATCH --job-name=ai-training
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --gres=gpu:2
#SBATCH --mem=32G
#SBATCH --time=04:00:00

python train.py
```

ဤ Script သည် Slurm အတွက် Job Request ဥပမာတစ်ခု ဖြစ်ပါတယ်။ Cluster ရဲ့ GPU Configuration, Slurm GRES Configuration နဲ့ Python Environment တို့ကို ကိုက်ညီအောင် ပြင်ဆင်ထားဖို့ လိုအပ်ပါတယ်။

Job ကို Submit လုပ်ရန်—

```bash
sbatch train-job.sh
```

Queue ထဲရှိ Job များကို ကြည့်ရန်—

```bash
squeue
```

အသုံးပြုသူရဲ့ Job တစ်ခုအကြောင်း အသေးစိတ်ကြည့်ရန်—

```bash
scontrol show job <JOB_ID>
```

ဤနေရာမှာ `<JOB_ID>` ကို အမှန်တကယ် Job ID နဲ့ အစားထိုးရပါမယ်။

**Slurm ရဲ့ အားသာချက်များ**

- Long-running Training Jobs များကို စီမံနိုင်ခြင်း။
- Queue နဲ့ Priority Policy များကို အသုံးပြုနိုင်ခြင်း။
- CPU, GPU, Memory နဲ့ Node Resources များကို ခွဲဝေပေးနိုင်ခြင်း။
- Multi-Node Training Workload များကို Schedule လုပ်နိုင်ခြင်း။
- HPC Cluster များတွင် Shared Resource Management ကို ပံ့ပိုးပေးခြင်း။

သတိပြုရန် — Slurm သည် Training Job များအတွက်သာ အသုံးပြုရတဲ့ Software မဟုတ်ပါဘူး။ အခြား Batch Computing နှင့် HPC Workload များကိုလည်း စီမံနိုင်ပါတယ်။

## 6. Container Orchestration ဆိုတာ ဘာလဲ?

Container Orchestration ဆိုတာ Containerized Applications တွေကို Cluster အတွင်း Deploy လုပ်ခြင်း၊ Schedule လုပ်ခြင်း၊ Scale လုပ်ခြင်းနဲ့ Health ကို စီမံခြင်းတို့အတွက် အသုံးပြုတဲ့ System ဖြစ်ပါတယ်။

ဥပမာ AI Inference API တစ်ခုကို Container Image အဖြစ် တည်ဆောက်ထားတယ်ဆိုပါစို့။ Production Environment မှာ Container တစ်ခုတည်းနဲ့ Request အားလုံးကို ကိုင်တွယ်ဖို့ မလုံလောက်နိုင်ပါဘူး။

ထို့ကြောင့်—

- Inference Service Replica အများအပြား Run လုပ်ရနိုင်တယ်။
- Container တစ်ခု Failure ဖြစ်သွားရင် အစားထိုး Instance အသစ် လိုအပ်နိုင်တယ်။
- Request များလာတဲ့အခါ Replica အရေအတွက် တိုးဖို့ လိုအပ်နိုင်တယ်။
- Server တစ်ခု Failure ဖြစ်ရင် အခြား Node ပေါ်မှာ Workload တွေ ပြန်လည် Run လုပ်ဖို့ လိုအပ်နိုင်တယ်။

Container Orchestration Platform က ဒီလို Workload Lifecycle ကို စီမံပေးပါတယ်။

**Container Orchestration ရဲ့ အဓိကလုပ်ဆောင်ချက်များ**

- Deployment: Application Version နဲ့ Container Configuration ကို အသုံးပြုပြီး Workload များကို Deploy လုပ်ခြင်း။
- Placement / Scheduling: Workload များကို သင့်တော်သော Nodes များပေါ်တွင် Run လုပ်စေခြင်း။
- Health Checks: Application သည် အသုံးပြုနိုင်သောအခြေအနေရှိမရှိ စစ်ဆေးခြင်း။
- Self-Healing: သတ်မှတ်ထားသော Desired State နဲ့ မကိုက်ညီတော့တဲ့ Workload များကို ပြန်လည်ညှိနှိုင်းခြင်း။
- Scaling: လိုအပ်ချက်အရ Replica အရေအတွက်ကို ပြောင်းလဲခြင်း။
- Service Discovery and Load Balancing: Service Instances များဆီသို့ Request များကို ပို့ဆောင်ပေးခြင်း။
- Rolling Updates: Application Version အသစ်ကို တဖြည်းဖြည်း Deploy လုပ်ခြင်း။

## 7. Analogy 2 — Smart City Traffic Management

Container Orchestration ကို Smart City တစ်ခုရဲ့ Traffic Management System နဲ့ နှိုင်းယှဉ်ကြည့်နိုင်ပါတယ်။ Traffic Flow ကို စီမံပြီး လိုအပ်တဲ့အခါ လမ်းကြောင်းနဲ့ Capacity ကို ပြောင်းလဲစီမံတဲ့ System ဖြစ်ပါတယ်။

- Container တစ်ခုချင်းစီကို Application Instance တစ်ခုအဖြစ် စဉ်းစားနိုင်ပါတယ်။ Instance အများအပြားက Request များကို တစ်ပြိုင်နက် ကိုင်တွယ်နိုင်ပါတယ်။

Smart City Traffic System က—

- Traffic များတဲ့လမ်းကြောင်းကို စီမံပေးတယ်။
- လိုအပ်ရင် Capacity ထပ်တိုးနိုင်အောင် ကူညီတယ်။
- လမ်းပိတ်တာ၊ Accident ဖြစ်တာတွေကြောင့် Traffic မပြတ်တောက်အောင် အခြားလမ်းကြောင်းတွေ အသုံးပြုနိုင်စေတယ်။

Container Orchestration မှာလည်း—

- Replica အများအပြားက Request တွေကို လက်ခံနိုင်တယ်။
- Replica အသစ်တွေ ထပ်တိုးနိုင်တယ်။
- Container Failure ဖြစ်ရင် Replacement Instance တွေ ဖန်တီးနိုင်တယ်။
- Service ကို Healthy ဖြစ်နေတဲ့ Instances တွေဆီသို့ Route လုပ်နိုင်တယ်။

သို့သော် ဤ Analogy သည် Concept ကို နားလည်ရန်သာ ဖြစ်ပါတယ်။ Kubernetes က Traffic Engineering System တစ်ခုလို Network Traffic အားလုံးကို ကိုယ်တိုင်ထိန်းချုပ်တာ မဟုတ်ပါဘူး။ Kubernetes Services, Ingress Controllers, Gateways နဲ့ သက်ဆိုင်ရာ Network Components တွေက Request Routing ကို အကောင်အထည်ဖော်ပေးကြပါတယ်။

## 8. Kubernetes က Inference Services တွေကို ဘယ်လိုစီမံသလဲ?

Kubernetes သည် Containerized Workload များကို Deploy လုပ်ခြင်း၊ Scale လုပ်ခြင်းနဲ့ Lifecycle ကို စီမံပေးနိုင်သော Container Orchestration Platform ဖြစ်ပါတယ်။

Inference Service တစ်ခုအတွက် Kubernetes က အောက်ပါ Components များကို အသုံးပြုနိုင်ပါတယ်။

- Deployment: Application Replica များ၏ Desired State ကို သတ်မှတ်ခြင်း။
- Pod: Application Container များ Run လုပ်သည့် Unit။
- Service: Pod များအတွက် Stable Network Endpoint ပေးခြင်း။
- Readiness Probe: Pod က Request လက်ခံရန် အသင့်ဖြစ်မဖြစ် စစ်ဆေးခြင်း။
- Liveness Probe: Container ကို ဆက်လက် Run ထားသင့်သလား စစ်ဆေးခြင်း။
- Horizontal Pod Autoscaler (HPA): သတ်မှတ်ထားသော Metrics အပေါ်မူတည်ပြီး Replica အရေအတွက် ပြောင်းလဲခြင်း။

ဥပမာ Inference API တစ်ခုအတွက် အခြေခံ Kubernetes Deployment ကို အောက်ပါအတိုင်း ရေးနိုင်ပါတယ်။

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: inference-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: inference-api
  template:
    metadata:
      labels:
        app: inference-api
    spec:
      containers:
        - name: inference-api
          image: example.com/inference-api:1.0
          ports:
            - containerPort: 8080
```

ဤ YAML သည် အယူအဆကို နားလည်ရန် ရိုးရှင်းသော ဥပမာသာ ဖြစ်ပါတယ်။ `example.com/inference-api:1.0` ကို အမှန်တကယ် အသုံးပြုနိုင်သော Container Image နဲ့ အစားထိုးရပါမယ်။ GPU လိုအပ်ပါက Container ရဲ့ Resource Requirements, NVIDIA GPU Operator သို့မဟုတ် သက်ဆိုင်ရာ Device Plugin နဲ့ GPU Node Configuration တို့ကိုလည်း မှန်ကန်စွာ ပြင်ဆင်ရပါမယ်။

Deployment ထဲမှာ `replicas: 3` သတ်မှတ်ထားခြင်းက ပုံမှန်အခြေအနေမှာ Replica သုံးခုကို ထိန်းသိမ်းရန် Kubernetes ကို ညွှန်ကြားတာ ဖြစ်ပါတယ်။ ဒါပေမဲ့ Request Load အလိုက် Auto Scaling ဖြစ်စေဖို့ HPA သို့မဟုတ် အခြား Scaling Mechanism ကို ထပ်မံ Configure လုပ်ရပါတယ်။

**Kubernetes Self-Healing ဆိုတာ ဘာလဲ?**

ဥပမာ Pod တစ်ခု Failure ဖြစ်သွားတယ်ဆိုပါစို့။ Kubernetes ရဲ့ Controllers တွေက Desired State နဲ့ လက်ရှိအခြေအနေကို နှိုင်းယှဉ်ပြီး လိုအပ်သလို Pod အသစ်ကို ဖန်တီးနိုင်ပါတယ်။

သို့သော် Self-Healing သည် Application ပြဿနာအားလုံးကို အလိုအလျောက် ဖြေရှင်းပေးနိုင်တာ မဟုတ်ပါဘူး။ Container ကို ပြန်ဖန်တီးပေးနိုင်ပေမဲ့ Application Code Bug, Database Failure, Invalid Configuration သို့မဟုတ် Model Loading Failure တို့ကို အလိုအလျောက် ပြုပြင်ပေးနိုင်မည်ဟု မဆိုလိုပါ။

## 9. Job Scheduling vs. Container Orchestration — အဓိကကွာခြားချက်

| အချက် | Job Scheduling | Container Orchestration |
|---|---|---|
| အဓိကရည်ရွယ်ချက် | Compute Jobs များကို Queue, Schedule နဲ့ Allocate လုပ်ခြင်း | Containerized Applications များ၏ Lifecycle ကို စီမံခြင်း |
| ပုံမှန် Workload | Training Jobs, Batch Jobs, HPC Jobs | Inference APIs, Microservices, Long-running Applications |
| Scheduling | Job စတင်ချိန်နှင့် Resource Allocation | Pod/Container များအတွက် Node Placement ပါဝင်သည် |
| Priority / Queue | အရေးကြီးသော အင်္ဂါရပ် | အခြေခံ Kubernetes မှာ Slurm ကဲ့သို့ HPC Queue Policy များနဲ့ မတူညီ |
| Resource Management | Job တစ်ခုစီအတွက် Resource Requests ကို စီမံခြင်း | Pod Resource Requests/Limits နဲ့ Cluster Capacity ကို စီမံခြင်း |
| Health Management | Job Status နဲ့ Failure Tracking | Probes, Controllers နဲ့ Desired State Reconciliation |
| Scaling | Job Resource Allocation နဲ့ Cluster Capacity တိုးချဲ့ခြင်း | Replica Scaling နဲ့ Workload Scaling |
| Typical Tool | Slurm | Kubernetes |

အဓိကမှတ်သားရန် — Kubernetes ကိုယ်တိုင်လည်း Workload Scheduling လုပ်ပါတယ်။ ထို့ကြောင့် “Slurm က Scheduling လုပ်ပြီး Kubernetes က Scheduling မလုပ်ဘူး” ဆိုတာ မမှန်ပါဘူး။

အမှန်တကယ် ကွာခြားချက်က Slurm ဟာ HPC/Batch Job Scheduling နဲ့ Compute Resource Management အတွက် အထူးပြုထားပြီး Kubernetes ဟာ Containerized Applications တွေရဲ့ Desired State နဲ့ Lifecycle ကို စီမံတဲ့ Platform ဖြစ်တာပါ။

## 10. လက်တွေ့ AI Infrastructure Architecture

AI Platform တစ်ခုမှာ ဒီနည်းလမ်းနှစ်ခုကို ပေါင်းစပ်အသုံးပြုနိုင်ပါတယ်။

```
                   AI Infrastructure
                           |
              +------------+------------+
              |                         |
              v                         v
       Model Training             Model Inference
              |                         |
              v                         v
        Training Jobs              Container Images
              |                         |
              v                         v
           Slurm                    Kubernetes
              |                         |
              v                         v
       GPU Compute Nodes          Inference Pods
              |                         |
              v                         v
        Model Artifacts             Service Endpoint
              |                         |
              +------------+------------+
                           |
                           v
                 Monitoring / Operations
```

ဤ Architecture မှာ Training Job တွေကို Slurm က စီမံနိုင်ပြီး Training ပြီးသွားတဲ့ Model Artifact ကို Model Registry သို့မဟုတ် သင့်လျော်သော Storage ထဲမှာ သိမ်းဆည်းနိုင်ပါတယ်။ ထို့နောက် Model ကို Inference Container ထဲတွင် ထည့်သွင်းပြီး Kubernetes ပေါ်မှာ Deploy လုပ်နိုင်ပါတယ်။

လက်တွေ့ Production Platform မှာ Model Versioning, Validation, CI/CD, Security, Monitoring, GPU Scheduling နဲ့ Rollback တို့ကိုလည်း ထည့်သွင်းစဉ်းစားရပါတယ်။

ဤနည်းလမ်းသည် Architecture တစ်မျိုးသာ ဖြစ်ပါတယ်။ Kubernetes ပေါ်မှာ Training နဲ့ Inference နှစ်မျိုးစလုံးကို စီမံနိုင်သလို အခြား Platform များနဲ့လည်း ပေါင်းစပ်အသုံးပြုနိုင်ပါတယ်။

## 11. Exam Notes

- Job Scheduling: Jobs များကို Queue ထဲတွင် စီမံပြီး Compute Resources များ ခွဲဝေပေးခြင်း။
- Slurm: HPC နှင့် AI Training Jobs များအတွက် အသုံးများသော Open-Source Workload Manager။
- Container Orchestration: Containerized Applications များကို Deploy, Manage, Scale နဲ့ Recover လုပ်ခြင်း။
- Kubernetes: Container Orchestration Platform။
- Pod: Kubernetes တွင် Deploy လုပ်ပြီး Run လုပ်သော အခြေခံ Workload Unit။
- Deployment: Replica များ၏ Desired State ကို ထိန်းသိမ်းပေးသည်။
- Service: Pod များအတွက် Stable Network Access ပေးသည်။
- Auto Scaling: Workload Demand သို့မဟုတ် Metrics အလိုက် Capacity ကို ပြောင်းလဲခြင်း။
- Self-Healing: Desired State နဲ့ မကိုက်ညီတော့သော Workload များကို ပြန်လည်ညှိနှိုင်းခြင်း။
- အရေးကြီးသောအချက်: Kubernetes တွင်လည်း Scheduler ပါဝင်သည်။ Slurm နဲ့ Kubernetes ကို အစားထိုးတူညီသော Tools များအဖြစ် မယူဆသင့်ပါ။

**စာမေးပွဲအတွက် အခြေအနေများ (Exam Scenarios)**

- Long-running, Queued Distributed Training Jobs များအတွက် → Slurm ကဲ့သို့ HPC Job Scheduler ကို ရွေးချယ်ပါ။
- Replicated Inference APIs များအတွက် → Kubernetes ကဲ့သို့ Container Orchestration Platform ကို ရွေးချယ်ပါ။
- Kubernetes ပေါ်မှာ GPU အသုံးပြုရန် → NVIDIA GPU Operator သို့မဟုတ် သက်ဆိုင်ရာ Device Plugin က GPU-related Software Components များကို Provision နဲ့ Manage လုပ်ပေးပါတယ်။
- Monitoring အတွက် → DCGM (NVIDIA Data Center GPU Manager) က GPU များကို Monitor လုပ်ခြင်း၊ Health စစ်ဆေးခြင်း၊ Diagnostics နဲ့ Telemetry ရယူခြင်းအတွက် အသုံးပြုပြီး Kubernetes Environment မှာ DCGM Exporter က GPU Metrics များကို Prometheus-compatible Endpoint မှတစ်ဆင့် Expose လုပ်နိုင်ပါတယ်။ ထို့နောက် Prometheus နဲ့ Grafana Dashboards / Alerts တို့နဲ့ ပေါင်းစပ်အသုံးပြုနိုင်ပါတယ်။

Monitoring Architecture ဥပမာ—

```
NVIDIA GPUs
     |
     v
    DCGM
     |
     v
 DCGM Exporter
     |
     v
 Prometheus
     |
     v
 Grafana Dashboards / Alerts
```

သတိပြုရန် — DCGM နဲ့ DCGM Exporter တို့သည် Monitoring Tools များသာ ဖြစ်ပြီး Kubernetes Scheduler ကို အစားထိုးတဲ့ Tools များ မဟုတ်ပါဘူး။ Pod Placement ကို ဆုံးဖြတ်တဲ့ Scheduling ကတော့ Resource Requests, Allocatable Resources နဲ့ Scheduling Constraints များအပေါ် အခြေခံပြီး ဆောင်ရွက်ပါတယ်။

## 12. Review Questions

**Question 1 — Job Scheduling ရဲ့ အဓိကတာဝန်က ဘာလဲ?**

အဖြေ — Compute Cluster ထဲမှာ Submit လုပ်ထားတဲ့ Jobs တွေကို ဘယ်အချိန်၊ ဘယ် Compute Nodes တွေမှာ Run မလဲဆုံးဖြတ်ပြီး CPU, GPU, RAM နဲ့ Node Resources တွေကို Queue, Priority, Resource Availability နဲ့ Scheduling Policy အပေါ် မူတည်ပြီး ခွဲဝေပေးခြင်း ဖြစ်ပါတယ်။

**Question 2 — Slurm ကို AI Training Cluster မှာ ဘာကြောင့် အသုံးပြုနိုင်သလဲ?**

အဖြေ — Slurm က Long-running Training Jobs တွေကို Queue နဲ့ Priority Policy များနဲ့ စီမံပြီး CPU, GPU, Memory နဲ့ Node Resources တွေကို ခွဲဝေပေးနိုင်လို့ ဖြစ်ပါတယ်။ Multi-Node Training Workload များကိုလည်း Schedule လုပ်နိုင်ပြီး HPC Cluster မှာ Shared Resource Management ကိုလည်း ပံ့ပိုးပေးပါတယ်။

**Question 3 — Container Orchestration က Production Inference Service တွေကို ဘယ်လိုကူညီသလဲ?**

အဖြေ — Inference Service Replica အများအပြား Run ခြင်း၊ Failure ဖြစ်ရင် Replacement Instance ဖန်တီးပေးခြင်း (Self-Healing)၊ Request Load အလိုက် Replica အရေအတွက် ပြောင်းခြင်း (Scaling)၊ Healthy Instances များဆီသို့ Request ပို့ပေးခြင်း (Load Balancing) နဲ့ Rolling Updates တို့ကို စီမံပေးနိုင်လို့ ဖြစ်ပါတယ်။

**Question 4 — Kubernetes မှာ Deployment, Pod နဲ့ Service တို့ ဘာကွာသလဲ?**

အဖြေ — Pod က Application Container များ Run လုပ်တဲ့ Workload Unit ဖြစ်ပြီး၊ Deployment က Replica များ၏ Desired State ကို သတ်မှတ် ထိန်းသိမ်းပေးတာဖြစ်ပြီး၊ Service က Pod များအတွက် Stable Network Endpoint ပေးတာ ဖြစ်ပါတယ်။

**Question 5 — Kubernetes မှာ Scheduler ပါဝင်တယ်ဆိုရင် Slurm နဲ့ ဘယ်လိုကွာခြားသလဲ?**

အဖြေ — Slurm ဟာ HPC/Batch Job Scheduling နဲ့ Compute Resource Management အတွက် အထူးပြုထားပြီး Kubernetes ဟာ Containerized Applications တွေရဲ့ Desired State နဲ့ Lifecycle ကို စီမံတဲ့ Platform ဖြစ်ပါတယ်။ နှစ်ခုစလုံးက Workload Scheduling လုပ်ပေမဲ့ ရည်ရွယ်ချက်နဲ့ စီမံတဲ့ပုံစံ မတူကြပါဘူး။

## 13. Module Summary

- Job Scheduling နဲ့ Container Orchestration တို့ဟာ AI Infrastructure Operations မှာ မတူညီသော ရည်ရွယ်ချက်များအတွက် အသုံးပြုကြပါတယ်။
- Job Scheduling က Compute Jobs များကို Queue, Priority, Resource Availability နဲ့ Scheduling Policy များအပေါ်မူတည်ပြီး Run လုပ်ပေးပါတယ်။ Slurm သည် HPC နဲ့ AI Training Jobs များအတွက် အသုံးများသော Tool ဖြစ်ပါတယ်။
- Container Orchestration က Containerized Applications များကို Deploy လုပ်ခြင်း၊ Health စစ်ဆေးခြင်း၊ Scaling လုပ်ခြင်းနဲ့ Desired State ကို ထိန်းသိမ်းခြင်းတို့ကို စီမံပေးပါတယ်။ Kubernetes သည် Production Inference Services နှင့် Microservices များအတွက် အသုံးများသော Platform ဖြစ်ပါတယ်။

စာမေးပွဲအတွက် အဓိကမှတ်သားရန်—

- Training Jobs → Slurm ကဲ့သို့ HPC Job Scheduler ကို စဉ်းစားပါ။
- Production Inference Services → Kubernetes ကဲ့သို့ Container Orchestration Platform ကို စဉ်းစားပါ။
- လက်တွေ့တွင် Training နှင့် Inference နှစ်မျိုးစလုံးကို Platform တစ်ခုတည်းဖြင့်လည်း စီမံနိုင်ပြီး Slurm နဲ့ Kubernetes ကို ပေါင်းစပ်အသုံးပြုနိုင်ပါတယ်။

## English Version

## 1. Learning Objectives

After studying this module, you should understand the following topics.

- What Job Scheduling is and why AI Training needs it.
- What Container Orchestration is and why it is useful for Production Inference.
- How the main responsibilities of Slurm and Kubernetes differ.
- Telling apart Resource Allocation, Job Queue, Priority, Load Balancing, Auto Scaling, and Self-Healing.
- Choosing the right Management Approach for a Training Cluster and a Production Inference Platform.
- How Job Scheduling and Container Orchestration are combined in real AI Infrastructure.

## 2. Why does AI Infrastructure need Operations Management?

Building GPU Servers, High-Speed Networks, and Storage Systems alone is not enough to run AI Workloads efficiently.

You also have to manage which workload uses which resource, when it starts, what to do when resources are insufficient, and how to recover when an application fails.

The two important Management Mechanisms in AI Infrastructure Operations are—

- Job Scheduling — queuing, scheduling, and allocating Compute Jobs.
- Container Orchestration — deploying, running, and managing the Lifecycle of Containerized Applications.

Easy to remember—

- A Training Job uses GPU Resources for a long time and runs until training finishes.
- An Inference Service accepts user Requests around the clock and keeps returning Responses.

For the first type, Job Queue and Compute Resource Allocation are what matter; for the second type, Application Availability, Health Checks, and Request Traffic Management are what matter.

## 3. What is Job Scheduling?

Job Scheduling is the mechanism that manages when the Jobs submitted to a Compute Cluster will run, on which Compute Nodes they will run, and which Resources are allocated to them.

Suppose an AI Research Team has three Training Jobs.

- Job A — needs 8 GPUs.
- Job B — needs 2 GPUs.
- Job C — needs 4 GPUs.

If only 8 GPUs are free in the Cluster, not all Jobs can run at the same time. Based on each Job's Resource Requirements, Priority, Queue Policy, and the current Resource Availability inside the Cluster, the Scheduler decides which Job can start.

**Main functions of a Job Scheduler**

- Job Submission: the user submits a Job to the Cluster.
- Queue Management: managing the waiting Jobs.
- Priority Management: managing Job Priority according to the configured Policy.
- Resource Allocation: distributing CPU, GPU, RAM, and Compute Nodes for the Job.
- Scheduling: running the Job when resources are sufficient and the Policy matches.
- Job Monitoring: watching the Job's Running, Pending, Completed, or Failed status.

A Job Scheduler can help improve Cluster Resource Utilization. But having a Scheduler does not mean GPUs will always reach 100% Utilization. The nature of the Workload and Resource Bottlenecks also have an impact.

## 4. Analogy 1 — Airport Air Traffic Controller

To understand Job Scheduling, compare it with an Airport's Air Traffic Controller.

- Runways at an Airport are limited, just as GPU, CPU, Memory, and Compute Nodes in an AI Cluster are limited.
- Each aircraft needs a Runway to use, just as each Training Job needs GPU and Compute Resources.

The Air Traffic Controller manages which aircraft may take off first, which Runway to use, and when to wait.

Likewise, a Job Scheduler manages—

- Which Job to run first.
- Which Job needs more Resources.
- Which Nodes have enough Resources.
- Which Job to keep Pending.
- How Resources can be reused when a Job finishes.

The main lesson of this Analogy is — the purpose of Job Scheduling is to distribute Compute Resources consistently with Policy and run Jobs efficiently.

## 5. What is Slurm?

Slurm (Simple Linux Utility for Resource Management) is an Open-Source Workload Manager and Job Scheduler widely used in HPC and AI Training Clusters.

Slurm manages the Compute Jobs users submit in a Queue and can run them on Cluster Nodes with the specified Resources.

**Slurm Job Workflow**

```
Researcher / Data Scientist
           |
           v
     Submit Training Job
           |
           v
       Slurm Queue
           |
           v
  Scheduler Selects Resources
           |
           v
  Allocate CPU / GPU / Memory
           |
           v
   Run on Compute Node(s)
           |
           v
  Completed / Failed / Requeue
```

For example, you can submit a Training Job using Slurm.

```bash
#!/bin/bash
#SBATCH --job-name=ai-training
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --gres=gpu:2
#SBATCH --mem=32G
#SBATCH --time=04:00:00

python train.py
```

This script is an example Job Request for Slurm. It must be prepared to match the Cluster's GPU Configuration, Slurm GRES Configuration, and Python Environment.

To submit the Job—

```bash
sbatch train-job.sh
```

To see the Jobs in the Queue—

```bash
squeue
```

To see details of one of the user's Jobs—

```bash
scontrol show job <JOB_ID>
```

Here `<JOB_ID>` must be replaced with the real Job ID.

**Advantages of Slurm**

- Can manage Long-running Training Jobs.
- Can use Queue and Priority Policies.
- Can allocate CPU, GPU, Memory, and Node Resources.
- Can schedule Multi-Node Training Workloads.
- Supports Shared Resource Management in HPC Clusters.

Note — Slurm is not software that can only be used for Training Jobs. It can also manage other Batch Computing and HPC Workloads.

## 6. What is Container Orchestration?

Container Orchestration is a system used for Deploying, Scheduling, Scaling, and managing the Health of Containerized Applications inside a Cluster.

Suppose an AI Inference API is built as a Container Image. In a Production Environment, a single Container may not be enough to handle all Requests.

Therefore—

- Multiple Inference Service Replicas may need to run.
- If a Container fails, a replacement Instance may be needed.
- When Requests increase, the number of Replicas may need to grow.
- If a Server fails, Workloads may need to run again on another Node.

A Container Orchestration Platform manages this kind of Workload Lifecycle.

**Main functions of Container Orchestration**

- Deployment: deploying Workloads using the Application Version and Container Configuration.
- Placement / Scheduling: running Workloads on suitable Nodes.
- Health Checks: checking whether the Application is in a usable state.
- Self-Healing: reconciling Workloads that no longer match the configured Desired State.
- Scaling: changing the number of Replicas as needed.
- Service Discovery and Load Balancing: delivering Requests to Service Instances.
- Rolling Updates: deploying a new Application Version gradually.

## 7. Analogy 2 — Smart City Traffic Management

Container Orchestration can be compared to a Smart City's Traffic Management System — a system that manages Traffic Flow and changes routes and Capacity when needed.

- Each Container can be thought of as an Application Instance. Multiple Instances can handle Requests concurrently.

The Smart City Traffic System—

- Manages busy routes.
- Helps add Capacity when needed.
- Keeps Traffic unbroken by using alternative routes when there are road closures or accidents.

Container Orchestration likewise—

- Multiple Replicas can accept Requests.
- New Replicas can be added.
- Replacement Instances can be created when a Container fails.
- The Service can be routed to Healthy Instances.

However, this Analogy is only for understanding the Concept. Kubernetes does not control all Network Traffic like a Traffic Engineering System. Kubernetes Services, Ingress Controllers, Gateways, and the related Network Components implement Request Routing.

## 8. How does Kubernetes manage Inference Services?

Kubernetes is a Container Orchestration Platform that can Deploy, Scale, and manage the Lifecycle of Containerized Workloads.

For an Inference Service, Kubernetes can use the following Components.

- Deployment: defines the Desired State of Application Replicas.
- Pod: the Unit where Application Containers run.
- Service: gives a Stable Network Endpoint for Pods.
- Readiness Probe: checks whether a Pod is ready to accept Requests.
- Liveness Probe: checks whether the Container should keep running.
- Horizontal Pod Autoscaler (HPA): changes the number of Replicas based on configured Metrics.

For example, a basic Kubernetes Deployment for an Inference API can be written as follows.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: inference-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: inference-api
  template:
    metadata:
      labels:
        app: inference-api
    spec:
      containers:
        - name: inference-api
          image: example.com/inference-api:1.0
          ports:
            - containerPort: 8080
```

This YAML is only a simple example for understanding the concept. `example.com/inference-api:1.0` must be replaced with a real usable Container Image. If GPUs are needed, the Container's Resource Requirements, the NVIDIA GPU Operator or the relevant Device Plugin, and the GPU Node Configuration must also be set up correctly.

Setting `replicas: 3` in the Deployment instructs Kubernetes to normally maintain three Replicas. However, to make Auto Scaling follow Request Load, HPA or another Scaling Mechanism must be configured additionally.

**What is Kubernetes Self-Healing?**

Suppose a Pod fails. Kubernetes' Controllers compare the Desired State with the current state and can create a new Pod as needed.

However, Self-Healing cannot automatically fix every Application problem. It can recreate the Container, but that does not mean it automatically fixes Application Code Bugs, Database Failures, Invalid Configuration, or Model Loading Failures.

## 9. Job Scheduling vs. Container Orchestration — Key Differences

| Aspect | Job Scheduling | Container Orchestration |
|---|---|---|
| Main purpose | Queues, schedules, and allocates Compute Jobs | Manages the Lifecycle of Containerized Applications |
| Typical workload | Training Jobs, Batch Jobs, HPC Jobs | Inference APIs, Microservices, Long-running Applications |
| Scheduling | Job start time and Resource Allocation | Includes Node Placement for Pods/containers |
| Priority / Queue | An important feature | In basic Kubernetes, unlike Slurm-style HPC Queue Policies |
| Resource Management | Manages Resource Requests for each Job | Manages Pod Resource Requests/Limits and Cluster Capacity |
| Health Management | Job Status and Failure Tracking | Probes, Controllers, and Desired State Reconciliation |
| Scaling | Job Resource Allocation and Cluster Capacity expansion | Replica Scaling and Workload Scaling |
| Typical Tool | Slurm | Kubernetes |

Key point to remember — Kubernetes itself also does Workload Scheduling. So the claim "Slurm does Scheduling and Kubernetes does not" is wrong.

The real difference is that Slurm specializes in HPC/Batch Job Scheduling and Compute Resource Management, while Kubernetes is a Platform that manages the Desired State and Lifecycle of Containerized Applications.

## 10. Practical AI Infrastructure Architecture

These two approaches can be combined in one AI Platform.

```
                   AI Infrastructure
                           |
              +------------+------------+
              |                         |
              v                         v
       Model Training             Model Inference
              |                         |
              v                         v
        Training Jobs              Container Images
              |                         |
              v                         v
           Slurm                    Kubernetes
              |                         |
              v                         v
       GPU Compute Nodes          Inference Pods
              |                         |
              v                         v
        Model Artifacts             Service Endpoint
              |                         |
              +------------+------------+
                           |
                           v
                 Monitoring / Operations
```

In this Architecture, Training Jobs can be managed by Slurm, and the Model Artifact after training can be stored in a Model Registry or suitable Storage. Then the Model can be put into an Inference Container and Deployed on Kubernetes.

In a real Production Platform, Model Versioning, Validation, CI/CD, Security, Monitoring, GPU Scheduling, and Rollback must also be considered.

This approach is only one Architecture. Both Training and Inference can be managed on Kubernetes, and other Platforms can be combined as well.

## 11. Exam Notes

- Job Scheduling: managing Jobs in a Queue and distributing Compute Resources.
- Slurm: an Open-Source Workload Manager commonly used for HPC and AI Training Jobs.
- Container Orchestration: deploying, managing, scaling, and recovering Containerized Applications.
- Kubernetes: a Container Orchestration Platform.
- Pod: the basic Workload Unit deployed and run in Kubernetes.
- Deployment: maintains the Desired State of Replicas.
- Service: gives Stable Network Access for Pods.
- Auto Scaling: changing Capacity according to Workload Demand or Metrics.
- Self-Healing: reconciling Workloads that no longer match the Desired State.
- Important point: Kubernetes also includes a Scheduler. Slurm and Kubernetes should not be treated as interchangeable identical Tools.

**Exam Scenarios**

- For long-running, queued Distributed Training Jobs → choose a Slurm-style HPC Job Scheduler.
- For Replicated Inference APIs → choose a Kubernetes-style Container Orchestration Platform.
- To use GPUs on Kubernetes → the NVIDIA GPU Operator or the relevant Device Plugin provisions and manages the GPU-related Software Components.
- For Monitoring → DCGM (NVIDIA Data Center GPU Manager) is used for monitoring NVIDIA Data Center GPUs, checking Health, and collecting Diagnostics and Telemetry. In a Kubernetes Environment, DCGM Exporter can expose GPU Metrics through a Prometheus-compatible Endpoint. It can then be combined with Prometheus and Grafana Dashboards / Alerts.

Example Monitoring Architecture—

```
NVIDIA GPUs
     |
     v
    DCGM
     |
     v
 DCGM Exporter
     |
     v
 Prometheus
     |
     v
 Grafana Dashboards / Alerts
```

Caution — DCGM and DCGM Exporter are Monitoring Tools only; they are not Tools that replace the Kubernetes Scheduler. The Scheduling that decides Pod Placement works from Resource Requests, Allocatable Resources, and Scheduling Constraints.

## 12. Review Questions

**Question 1 — What is the main responsibility of Job Scheduling?**

Answer — Deciding when the Jobs submitted to a Compute Cluster will run and on which Compute Nodes, and distributing CPU, GPU, RAM, and Node Resources based on Queue, Priority, Resource Availability, and Scheduling Policy.

**Question 2 — Why can Slurm be used in an AI Training Cluster?**

Answer — Because Slurm manages Long-running Training Jobs with Queue and Priority Policies and allocates CPU, GPU, Memory, and Node Resources. It can also schedule Multi-Node Training Workloads and supports Shared Resource Management in HPC Clusters.

**Question 3 — How does Container Orchestration help Production Inference Services?**

Answer — It can run many Inference Service Replicas, create Replacement Instances on failure (Self-Healing), change the Replica count with Request Load (Scaling), send Requests to Healthy Instances (Load Balancing), and handle Rolling Updates.

**Question 4 — How do Deployment, Pod, and Service differ in Kubernetes?**

Answer — A Pod is the Workload Unit where Application Containers run; a Deployment defines and maintains the Desired State of Replicas; a Service gives a Stable Network Endpoint for Pods.

**Question 5 — If Kubernetes includes a Scheduler, how does it differ from Slurm?**

Answer — Slurm specializes in HPC/Batch Job Scheduling and Compute Resource Management, while Kubernetes is a Platform that manages the Desired State and Lifecycle of Containerized Applications. Both do Workload Scheduling, but their purpose and management style differ.

## 13. Module Summary

- Job Scheduling and Container Orchestration are used for different purposes in AI Infrastructure Operations.
- Job Scheduling runs Compute Jobs based on Queue, Priority, Resource Availability, and Scheduling Policies. Slurm is a commonly used Tool for HPC and AI Training Jobs.
- Container Orchestration manages deploying Containerized Applications, checking Health, Scaling, and maintaining the Desired State. Kubernetes is a commonly used Platform for Production Inference Services and Microservices.

Key points for the exam—

- Training Jobs → think of a Slurm-style HPC Job Scheduler.
- Production Inference Services → think of a Kubernetes-style Container Orchestration Platform.
- In practice, both Training and Inference can be managed with a single Platform, and Slurm and Kubernetes can be combined.

## Technical review notes

- The supplied Module 36 body is Burmese-only (the transcript marks it "အပိုင်း ၁ — AI Infrastructure Operations ကို မြန်မာဘာသာဖြင့် အသေးစိတ်လေ့လာခြင်း"). The English Version is a faithful translation of that body, not source text; technical terms are kept in English in both versions.
- Stray `</box>` markup artifacts inside the two analogy sections (Airport and Smart City) were removed; the meaning of the text is unchanged.
- Section 9's comparison table is flattened into consecutive plain lines in the transcript. It is reconstructed as a Markdown table with every cell preserved verbatim in the original order; the English table is the translation of those same cells.
- The Review Questions are reduced from the source's 8 questions to 5 with concise answers, per the course writing rules; the answers come from the module's own content.
- The sbatch script and the Kubernetes Deployment YAML are preserved verbatim. The source itself notes the script must match the Cluster's GPU Configuration, Slurm GRES Configuration, and Python Environment, and that `example.com/inference-api:1.0` must be replaced with a real Container Image (illustrative, not universal).
- Exam scenarios are preserved as in the source: Slurm for long-running queued (multi-node/distributed) Training Jobs; Kubernetes for Replicated Inference APIs; NVIDIA GPU Operator / Device Plugin for GPU components on Kubernetes. The DCGM → DCGM Exporter → Prometheus → Grafana monitoring chain and its ASCII diagram are preserved from the adjacent Module 37 portion of the same transcript (the Module 36 slice only reaches "Monitoring / Operations" in its architecture diagram). The caveat that DCGM and DCGM Exporter are monitoring tools — not a Kubernetes Scheduler replacement — follows the transcript's own scheduling note that placement is based on Resource Requests, Allocatable Resources, and Scheduling Constraints, not live GPU metrics.
- Source cut/seam points: the Module 36 portion runs from the "Module 36 — Job Scheduling vs. Container Orchestration" line to just before "Module 37 — Slurm vs. Kubernetes for AI Workloads"; the "အပိုင်း ၁" (Part 1) label suggests a matching English-language part exists in the source series but was not supplied. The claim that a Scheduler alone does not guarantee 100% GPU Utilization is preserved as given (no benchmark is attached to it).
