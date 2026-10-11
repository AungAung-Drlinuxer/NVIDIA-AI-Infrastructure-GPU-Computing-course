# Module 35 — Model Training vs. Model Inference

> Part VI · Course Module 35 — topic notes

## မြန်မာဘာသာ version

## 1. Learning Objectives

ဤ Module ကို လေ့လာပြီးနောက် အောက်ပါအချက်များကို နားလည်နိုင်ရမည်။

- Model Training နှင့် Model Inference တို့၏ ကွာခြားချက်။
- Forward Pass, Backward Pass, Gradient နှင့် Weight Update တို့၏ အခန်းကဏ္ဍ။
- Training နှင့် Inference တို့အတွက် Compute နှင့် Memory လိုအပ်ချက်များ။
- Latency, Throughput နှင့် Scalability တို့၏ အဓိပ္ပာယ်။
- Training Cluster နှင့် Inference Infrastructure တို့ကို ဘယ်လိုစီမံသင့်သလဲ။
- GPU Infrastructure ရွေးချယ်ရာတွင် Training နှင့် Inference ကို ဘယ်လိုခွဲခြားစဉ်းစားရမလဲ။

## 2. AI Infrastructure ပေါ်တွင် အမြဲလုပ်ဆောင်နေရသော အလုပ်နှစ်မျိုး

AI Infrastructure တစ်ခုကို တည်ဆောက်ပြီးသောအခါ အဓိကလုပ်ဆောင်ရသော Workload နှစ်မျိုးမှာ Model Training နှင့် Model Inference တို့ဖြစ်သည်။

- **Model Training** — Training Data များမှ Pattern များကို သင်ယူစေပြီး Model Parameters သို့မဟုတ် Weights များကို Update လုပ်ပေးသည်။
- **Model Inference** — Train လုပ်ပြီးသား Model ကို အသုံးပြုပြီး Input အသစ်များအတွက် Prediction, Classification သို့မဟုတ် Recommendation များ ထုတ်ပေးသည်။

ဥပမာ Credit Card Fraud Detection System တစ်ခုကို စဉ်းစားကြည့်ပါ။

Training အဆင့်တွင် အရင်က ဖြစ်ပွားခဲ့သော Legitimate Transactions နှင့် Fraudulent Transactions များကို အသုံးပြုပြီး Model ကို သင်ကြားပေးသည်။ Inference အဆင့်တွင် Transaction အသစ်တစ်ခု ဝင်လာသောအခါ Model က Fraud ဖြစ်နိုင်ခြေကို ခန့်မှန်းပေးသည်။ အချိန်ကြာလာသည်နှင့်အမျှ Transaction Pattern များ ပြောင်းလဲလာပါက Data အသစ်များဖြင့် Model ကို ပြန်လည် Training လုပ်ရန် လိုအပ်နိုင်သည်။

> **သတိပြုရန်** — Retraining လုပ်တိုင်း Model ၏ Accuracy တိုးတက်မည်ဟု အာမမခံနိုင်ပါ။ Data Quality, Training Configuration နှင့် Evaluation Results တို့ကိုလည်း စစ်ဆေးရမည်။

## 3. Cooking Analogy — Training နဲ့ Inference ကို နှိုင်းယှဉ်ခြင်း

ဟင်းချက်သင်ယူနေသူတစ်ဦးကို စဉ်းစားကြည့်ပါ။

- **Training** — ပါဝင်ပစ္စည်းတွေကို စမ်းသပ်ခြင်း၊ အချိုးအစားပြင်ဆင်ခြင်း၊ အမှားများမှ သင်ယူခြင်းနဲ့ ဟင်းချက်နည်းကို ပိုကောင်းအောင် ပြုပြင်ခြင်းတို့ လုပ်ဆောင်သည်။
- **Inference** — Customer က Order တင်လာသောအခါ ရှိပြီးသား ဟင်းချက်နည်းကို အသုံးပြုပြီး ဟင်းကို ပြင်ဆင်ပေးသည်။ ပုံမှန်အားဖြင့် ဟင်းချက်နည်းကို ပြန်လည်သင်ယူခြင်း သို့မဟုတ် ပြောင်းလဲခြင်း မလုပ်ပါ။

ဤဥပမာအရ Training သည် Model ကို သင်ယူစေသည့်အဆင့်ဖြစ်ပြီး Inference သည် သင်ယူပြီးသား Model ကို လက်တွေ့အသုံးချသည့်အဆင့်ဖြစ်သည်။

## 4. Model Training ဘယ်လိုအလုပ်လုပ်သလဲ?

Training ၏ ရည်ရွယ်ချက်မှာ Training Data များမှ သက်ဆိုင်ရာ Pattern များကို သင်ယူပြီး Model Parameters များကို သင့်လျော်စွာ ပြင်ဆင်ပေးရန် ဖြစ်သည်။

ပုံမှန် Neural Network Training တွင် အဓိကအဆင့်များမှာ—

- **Forward Pass** — Input Data ကို Model မှတစ်ဆင့် ဖြတ်သန်းစေပြီး Prediction ထုတ်ပေးသည်။
- **Loss Calculation** — Prediction နှင့် Expected Answer တို့ကြား ကွာခြားချက်ကို Loss Function ဖြင့် တွက်ချက်သည်။
- **Backward Pass** — Loss မှတစ်ဆင့် Model Parameters များအပေါ် Gradient များကို တွက်ချက်သည်။
- **Weight Update** — Optimizer က Gradient များကို အသုံးပြုပြီး Model Weights များကို Update လုပ်သည်။
- **Iteration** — Batch များစွာနှင့် Epoch များစွာကို ဆက်လက်လုပ်ဆောင်သည်။

**အဓိက Technical Terms**

| Term | အဓိပ္ပာယ် |
|---|---|
| Forward Pass | Input မှ Prediction ထုတ်ပေးသည့် computation |
| Loss Function | Prediction အမှားကို တိုင်းတာပေးသည့် function |
| Backward Pass | Gradient များကို တွက်ချက်သည့်အဆင့် |
| Gradient | Loss ပြောင်းလဲမှုနှင့် Parameter များ၏ ဆက်နွယ်မှုကို ဖော်ပြသော တန်ဖိုးများ |
| Optimizer | Gradient များကို အသုံးပြုပြီး Parameters များကို Update လုပ်ပေးသည် |
| Weights | Model က သင်ယူထားသော Parameters များထဲမှ အဓိကအစိတ်အပိုင်း |
| Epoch | Training Dataset တစ်ခုလုံးကို တစ်ကြိမ် ဖြတ်သန်းသင်ယူခြင်း |
| Batch | Training အတွင်း တစ်ကြိမ်တည်း အသုံးပြုသော Data အစု |

Forward Pass သည် Training နှင့် Inference နှစ်မျိုးစလုံးတွင် အသုံးပြုနိုင်သည်။ သို့သော် ပုံမှန် Training တွင် Backward Pass နှင့် Weight Update တို့လည်း ပါဝင်သည်။

## 5. Model Inference ဘယ်လိုအလုပ်လုပ်သလဲ?

Inference သည် Train လုပ်ပြီးသား Model ကို အသုံးပြုပြီး Input အသစ်များအတွက် Output ထုတ်ပေးခြင်း ဖြစ်သည်။

ဥပမာ—

- Fraud Detection — Transaction တစ်ခု Fraud ဖြစ်နိုင်ခြေကို ခန့်မှန်းခြင်း။
- Recommendation System — အသုံးပြုသူအတွက် သင့်တော်သော Product များကို အကြံပြုခြင်း။
- Image Classification — ပုံတစ်ပုံကို သတ်မှတ်ထားသော Category များအနက် တစ်ခုအဖြစ် ခွဲခြားခြင်း။
- LLM Chatbot — User Prompt ကို အခြေခံပြီး Response ထုတ်ပေးခြင်း။

ပုံမှန် Inference Flow သည်—

```text
New Input
    |
    v
Preprocessing
    |
    v
Trained Model
    |
    v
Forward Pass
    |
    v
Prediction / Response
```

Inference အတွင်း ပုံမှန်အားဖြင့် Gradient တွက်ချက်ခြင်းနှင့် Weight Update လုပ်ခြင်း မလိုအပ်ပါ။ သို့သော် Input Processing, Model Execution, Output Processing နှင့် Network Communication တို့ကြောင့် Latency ဖြစ်ပေါ်နိုင်သည်။

အရေးကြီးသည်မှာ Model သည် Training လုပ်စဉ် မသင်ယူခဲ့သော Category အသစ်ကို အလိုအလျောက် မှန်ကန်စွာ သိရှိနိုင်မည်ဟု မဆိုလိုပါ။ Output သည် Model ၏ Training Data, Architecture နှင့် သင်ယူထားသော Capabilities တို့အပေါ် မူတည်သည်။

## 6. Training vs. Inference — အဓိကကွာခြားချက်များ

| အချက် | Model Training | Model Inference |
|---|---|---|
| ရည်ရွယ်ချက် | Data မှ Pattern များကို သင်ယူရန် | Input အသစ်အတွက် Prediction ထုတ်ရန် |
| Model Weights | Training အတွင်း Update လုပ်သည် | ပုံမှန်အားဖြင့် မပြောင်းလဲပါ |
| Forward Pass | ပါဝင်သည် | ပါဝင်သည် |
| Backward Pass | ပုံမှန် Gradient-based Training တွင် ပါဝင်သည် | ပုံမှန်အားဖြင့် မလိုအပ်ပါ |
| Optimizer | Weight Update အတွက် အသုံးပြုသည် | ပုံမှန် Inference အတွက် မလိုအပ်ပါ |
| Compute | များသောအားဖြင့် မြင့်မားသည် | Model နှင့် workload ပေါ်မူတည်သည် |
| Memory | Weights, Activations, Gradients နှင့် Optimizer States လိုအပ်နိုင်သည် | Weights, Activations နှင့် အခြား runtime memory လိုအပ်သည် |
| အဓိက Performance Goal | Training Speed, Convergence, Scalability | Latency, Throughput, Efficiency |
| အသုံးပြုမှုပုံစံ | Training Jobs, Experiments, Retraining | Online APIs, Batch Prediction, Production Services |

## 7. Compute Requirement က ဘာကြောင့်ကွာသလဲ?

### 7.1 Training — High Compute Requirement ဖြစ်နိုင်ခြင်း

Training အတွင်း Model သည် Data များကို ဖြတ်သန်းပြီး Prediction တွက်ချက်ရုံသာမက Gradient များတွက်ချက်ကာ Weights များကို Update လုပ်ရသည်။

Large Deep Learning Model များအတွက်—

- Dataset ကြီးမားနိုင်သည်။
- Model Parameters များ များပြားနိုင်သည်။
- Training Iteration များစွာ လိုအပ်နိုင်သည်။
- Distributed Training အတွက် GPU များစွာ လိုအပ်နိုင်သည်။
- GPU-to-GPU Communication နှင့် Data Loading တို့ကိုလည်း စီမံရသည်။

ထို့ကြောင့် Training Cluster များတွင် High-Performance GPU များ၊ Fast Interconnects နှင့် Efficient Storage/Data Pipelines တို့သည် အရေးကြီးနိုင်သည်။

သို့သော် Model အားလုံးကို Multi-Node Cluster ဖြင့် Train လုပ်ရန် မလိုပါ။ Model သေးငယ်ခြင်း သို့မဟုတ် Dataset နည်းပါးခြင်းတို့အတွက် Single GPU သို့မဟုတ် CPU ပင် လုံလောက်နိုင်သည်။

### 7.2 Inference — Compute ပမာဏက Workload ပေါ်မူတည်ခြင်း

Inference သည် ပုံမှန်အားဖြင့် Forward Pass ကိုသာ လုပ်ဆောင်သဖြင့် Training ထက် Compute နှင့် Memory နည်းနိုင်သည်။ သို့သော် Model Size နှင့် Request Volume ပေါ်မူတည်၍ Inference သည်လည်း ကြီးမားသော GPU Resources လိုအပ်နိုင်သည်။

ဥပမာ LLM Inference တွင်—

- Model Weights များကို GPU Memory ထဲတွင် ထားရနိုင်သည်။
- Prompt Processing နှင့် Token Generation တို့သည် Compute လိုအပ်သည်။
- Autoregressive generation အတွင်း KV Cache ကဲ့သို့ Memory Structures များ လိုအပ်နိုင်သည်။
- Concurrent Requests များ များပြားလာပါက Memory နှင့် Throughput လိုအပ်ချက် မြင့်တက်နိုင်သည်။

ထို့ကြောင့် "Inference က Compute နည်းတယ်" ဆိုသည်မှာ အထွေထွေသဘောတရားသာဖြစ်ပြီး Workload အားလုံးအတွက် မမှန်နိုင်ပါ။

## 8. Memory Requirement က ဘာကြောင့်ကွာသလဲ?

**Training Memory**

Training အတွင်း Memory လိုအပ်ချက်ကို အောက်ပါအရာများက သက်ရောက်စေသည်။

- Model Weights။
- Forward Pass မှ Activations များ။
- Gradients။
- Optimizer States။
- Batch Size။
- Sequence Length သို့မဟုတ် Input Resolution။
- Precision နှင့် Training Technique များ။

Model Size တစ်ခုတည်းကို ကြည့်ပြီး Training Memory လိုအပ်ချက်ကို တိတိကျကျ ဆုံးဖြတ်လို့ မရပါ။

**Inference Memory**

Inference အတွက် လိုအပ်သော Memory သည်—

- Model Weights။
- Intermediate Activations နှင့် Runtime Buffers။
- Batch Size နှင့် Concurrent Requests။
- LLM များအတွက် KV Cache။
- Framework နှင့် Serving Engine ၏ Memory Overhead တို့အပေါ် မူတည်သည်။

Quantization သို့မဟုတ် Pruning ကဲ့သို့ Optimization နည်းလမ်းများကို အသုံးပြုပါက အချို့ Model များ၏ Memory Footprint ကို လျှော့ချနိုင်သည်။ သို့သော် Optimization တိုင်းကို Model အားလုံးတွင် မဖြစ်မနေ အသုံးပြုရန် မလိုအပ်ပါ။ Accuracy နှင့် Performance အပေါ် သက်ရောက်မှုကိုလည်း စစ်ဆေးရမည်။

## 9. Latency vs. Throughput

Inference Infrastructure ကို Design လုပ်ရာတွင် အလွန်အရေးကြီးသော Metrics နှစ်ခုမှာ Latency နှင့် Throughput ဖြစ်သည်။

ဥပမာ User တစ်ဦးက Chatbot ကို မေးခွန်းပို့ပြီး Response စတင်ရရှိရန် 200 milliseconds ကြာပါက ထိုအချိန်သည် Latency ကို နားလည်ရန် ဥပမာတစ်ခုဖြစ်သည်။

ဥပမာ Server တစ်ခုက တစ်စက္ကန့်လျှင် Prediction 1,000 ခု လုပ်ဆောင်နိုင်ပါက Throughput သည် 1,000 predictions/second ဖြစ်သည်။

Inference Service တစ်ခုသည် Latency နည်းပြီး Throughput မြင့်မားရန် လိုအပ်နိုင်သည်။ သို့သော် Batching ကို ပိုမိုအသုံးပြုခြင်းက Throughput ကို တိုးစေနိုင်သော်လည်း အချို့ Request များအတွက် စောင့်ဆိုင်းချိန်ကို တိုးစေနိုင်သည်။ ထို့ကြောင့် Application SLA နှင့် User Experience ကို ထည့်သွင်းစဉ်းစားရမည်။

Training အတွက်တော့ Model ကို လက်ခံနိုင်သော Quality အဆင့်သို့ ရောက်အောင် အချိန်တိုအတွင်း သင်ယူစေနိုင်ခြင်း၊ GPU Utilization နှင့် Distributed Scaling တို့သည် အရေးကြီးသည်။

## 10. Training နှင့် Inference Scaling Strategy

### 10.1 Training — Distributed Scaling

ကြီးမားသော Model များကို Train လုပ်ရာတွင် GPU အများအပြားကို အသုံးပြုနိုင်သည်။ လိုအပ်ပါက Multi-Node Cluster ကို အသုံးပြုနိုင်သည်။

Scaling နည်းလမ်းများတွင်—

- Data Parallelism: GPU တစ်ခုချင်းစီတွင် Model Copy များထားပြီး Data Batch မတူညီသောအပိုင်းများကို သင်ယူစေခြင်း။
- Model Parallelism: Model ၏ အစိတ်အပိုင်းများကို GPU များအကြား ခွဲဝေထားခြင်း။
- Pipeline Parallelism: Model အစိတ်အပိုင်းများကို အဆင့်ဆင့်ခွဲပြီး မတူညီသော GPU များတွင် လုပ်ဆောင်စေခြင်း။

GPU အရေအတွက်တိုးလာတိုင်း Training Performance သည် အချိုးညီတိုးတက်မည်ဟု မဆိုနိုင်ပါ။ Communication Overhead, Workload Balance, Memory Limitations နှင့် Software Configuration တို့က Scaling Efficiency ကို သက်ရောက်စေသည်။

### 10.2 Inference — Elastic Scaling

Production Inference Workload များတွင် Request Volume သည် အချိန်နှင့်အမျှ ပြောင်းလဲနိုင်သည်။

ဥပမာ—

- Normal Period တွင် Server အနည်းငယ်ဖြင့် Request များကို ကိုင်တွယ်နိုင်သည်။
- Peak Period တွင် Request များ တိုးလာပါက Replica သို့မဟုတ် GPU Instance များ ထပ်တိုးရန် လိုအပ်နိုင်သည်။
- Demand လျော့နည်းလာပါက Capacity ကို ပြန်လျှော့ချနိုင်သည်။

ဤနည်းလမ်းကို Elastic Scaling ဟု ခေါ်သည်။ Kubernetes Horizontal Pod Autoscaler, KEDA သို့မဟုတ် Cloud Auto Scaling Services များက သင့်လျော်သော Architecture များတွင် ကူညီပေးနိုင်သည်။

Serverless Inference ကို အသုံးပြုပါက Infrastructure ကို ကိုယ်တိုင်စီမံရမှု လျော့နည်းနိုင်သော်လည်း Capacity, Cold Start, GPU Availability, Cost နှင့် Model Size တို့၏ ကန့်သတ်ချက်များကို စဉ်းစားရမည်။

## 11. Infrastructure ရွေးချယ်မှု — Scenario များ

| Scenario | အဓိကလိုအပ်ချက် | စဉ်းစားသင့်သည့်အချက် |
|---|---|---|
| Large LLM Training | High Compute, High Memory Bandwidth | Multi-GPU, Interconnect, Distributed Training |
| Small Model Experiment | Flexible Development Environment | Single GPU သို့မဟုတ် CPU လုံလောက်နိုင်ခြင်း |
| Real-Time Fraud Detection | Low Latency, Reliable Serving | Model Serving, Request Latency, Availability |
| High-Volume Recommendation API | High Throughput | Batching, Replicas, GPU Utilization |
| LLM Chatbot with Long Context | Memory Capacity, KV Cache Management | GPU Memory, Concurrency, Context Length |
| Periodic Model Retraining | Scheduled Training Capacity | Data Pipeline, Training Window, Model Validation |

အဓိကအချက်က Physical Infrastructure တူညီနေသော်လည်း Training နဲ့ Inference အတွက် Resource Allocation, Software Configuration, Scheduling နဲ့ Performance Tuning တို့ကို မတူညီစွာ စီမံနိုင်သည်။

## 12. Exam Notes

- Training: Model Parameters များကို Data မှ သင်ယူပြီး Update လုပ်ခြင်း။
- Inference: Train လုပ်ပြီးသား Model ဖြင့် Input အသစ်များအတွက် Prediction ထုတ်ခြင်း။
- Forward Pass: Model မှ Prediction တွက်ချက်ခြင်း။
- Backward Pass: Training အတွင်း Gradient တွက်ချက်ခြင်း။
- Weight Update: Optimizer က Model Parameters များကို Update လုပ်ခြင်း။
- Training Performance: Training Time, Convergence, GPU Utilization နှင့် Scalability။
- Inference Performance: Latency, Throughput, Memory Efficiency နှင့် Serving Cost။
- Training Memory: Weights, Activations, Gradients, Optimizer States တို့ကြောင့် များနိုင်သည်။
- Inference Memory: Weights, Activations, Runtime Buffers နှင့် LLM KV Cache တို့ လိုအပ်နိုင်သည်။
- Training Scaling: Distributed Training ဖြင့် Multi-GPU/Multi-Node အသုံးပြုနိုင်သည်။
- Inference Scaling: Request Demand အလိုက် Replicas သို့မဟုတ် Capacity ကို Elastic Scaling လုပ်နိုင်သည်။

## 13. Review Questions

**မေးခွန်း ၁ — Model Training နှင့် Model Inference တို့၏ အဓိကကွာခြားချက်က ဘာလဲ?**

အဖြေ — Training သည် Data မှ Pattern များကို သင်ယူပြီး Model Weights များကို Update လုပ်ခြင်းဖြစ်သည်။ Inference သည် Train လုပ်ပြီးသား Model ဖြင့် Input အသစ်များအတွက် Prediction ထုတ်ခြင်းဖြစ်ပြီး Model Weights များကို ပုံမှန်အားဖြင့် မပြောင်းလဲပါ။

**မေးခွန်း ၂ — Forward Pass နှင့် Backward Pass တို့ ဘာကွာသလဲ?**

အဖြေ — Forward Pass သည် Input ကို Model မှတစ်ဆင့် ဖြတ်သန်းစေပြီး Prediction ထုတ်ပေးသည် — Training နှင့် Inference နှစ်မျိုးစလုံးတွင် ပါဝင်သည်။ Backward Pass သည် Loss မှတစ်ဆင့် Gradient များကို တွက်ချက်ပေးပြီး ပုံမှန် Training တွင်သာ လိုအပ်ပြီး Inference တွင် မလိုအပ်ပါ။

**မေးခွန်း ၃ — Training Memory တွင် ဘယ်လို Data Structures များ ပါဝင်နိုင်သလဲ?**

အဖြေ — Model Weights, Forward Pass မှ Activations, Gradients နှင့် Optimizer States တို့ ပါဝင်နိုင်သည်။ Batch Size, Sequence Length/Input Resolution နှင့် Precision တို့ပေါ်လည်း မူတည်နိုင်သည်။

**မေးခွန်း ၄ — Latency နှင့် Throughput တို့ကို ဥပမာဖြင့် ရှင်းပြပါ။**

အဖြေ — Latency သည် Request တစ်ခုအတွက် Response စတင်ရရှိရန် ကြာချိန်ဖြစ်သည် — ဥပမာ Chatbot Response စတင်ရရှိရန် 200 ms ကြာခြင်း။ Throughput သည် တစ်စက္ကန့်အတွင်း လုပ်ဆောင်နိုင်သော ပမာဏဖြစ်သည် — ဥပမာ တစ်စက္ကန့်လျှင် Prediction 1,000 ခု (1,000 predictions/second)။

**မေးခွန်း ၅ — Inference သည် Training ထက် အမြဲတမ်း Compute နည်းသလား?**

အဖြေ — မဟုတ်ပါ။ Inference သည် ပုံမှန် Forward Pass သာ လုပ်သောကြောင့် နည်းနိုင်သော်လည်း Large LLM များ၊ Request Volume များပြားခြင်းနှင့် KV Cache ကြောင့် Inference သည်လည်း ကြီးမားသော GPU Compute နှင့် Memory လိုအပ်နိုင်သည်။

## 14. Module Summary

- Model Training နှင့် Model Inference တို့သည် AI Infrastructure ၏ အဓိက Workload နှစ်မျိုးဖြစ်သည်။
- Training တွင် Model သည် Data မှ သင်ယူပြီး Weights များကို Update လုပ်သည်။ ထို့ကြောင့် Gradient Computation, Optimizer States နှင့် Distributed Training တို့ကြောင့် Compute နှင့် Memory လိုအပ်ချက် မြင့်မားနိုင်သည်။
- Inference တွင် Train လုပ်ပြီးသား Model ကို အသုံးပြုကာ Prediction ထုတ်ပေးသည်။ ပုံမှန်အားဖြင့် Forward Pass ကိုသာ လုပ်ဆောင်သော်လည်း Large LLM များနှင့် High-Volume Services များတွင် GPU Compute နှင့် Memory လိုအပ်ချက်များ အလွန်မြင့်မားနိုင်သည်။

စာမေးပွဲအတွက် အဓိကမှတ်သားရန် —

- Training → Learning, Gradients, Weight Updates, Convergence, Distributed Scaling။
- Inference → Predictions, Low Latency, High Throughput, Memory Efficiency, Elastic Scaling။

Infrastructure ကို ရွေးချယ်ရာတွင် "Training က အမြဲကြီးပြီး Inference က အမြဲသေးတယ်" ဟု မမှတ်ယူဘဲ Model Size, Request Volume, Latency Target, Memory Requirement နှင့် Application SLA တို့ကို အခြေခံပြီး ဆုံးဖြတ်သင့်သည်။

## English Version

## 1. Learning Objectives

After studying this module you should be able to —

- Explain the difference between Model Training and Model Inference.
- Describe the roles of Forward Pass, Backward Pass, Gradient, and Weight Update.
- Explain the Compute and Memory requirements of Training and Inference.
- Define Latency, Throughput, and Scalability.
- Describe how a Training Cluster and an Inference Infrastructure should be managed.
- Explain how to distinguish Training from Inference when choosing GPU Infrastructure.

## 2. The Two Workloads That Run on AI Infrastructure

Once an AI Infrastructure is built, the two main workloads it handles are Model Training and Model Inference.

- **Model Training** — Learns patterns from Training Data and updates Model Parameters (Weights).
- **Model Inference** — Uses a trained model to produce Predictions, Classifications, or Recommendations for new inputs.

Consider a Credit Card Fraud Detection System as an example.

At the Training stage, historical Legitimate Transactions and Fraudulent Transactions are used to teach the model. At the Inference stage, when a new Transaction arrives, the model estimates the probability that it is Fraud. Over time, as Transaction Patterns change, the model may need to be retrained with new data.

> **Note** — Retraining does not guarantee that the model's Accuracy will improve. Data Quality, Training Configuration, and Evaluation Results must also be checked.

## 3. Cooking Analogy — Comparing Training and Inference

Think of someone learning to cook.

- **Training** — Testing ingredients, adjusting proportions, learning from mistakes, and improving the recipe.
- **Inference** — When a Customer places an Order, the existing recipe is used to prepare the dish. Normally the recipe is not re-learned or changed.

In this example, Training is the stage where the model learns, and Inference is the stage where the learned model is applied in practice.

## 4. How Does Model Training Work?

The purpose of Training is to learn the relevant patterns from Training Data and adjust Model Parameters accordingly.

The main steps in a typical Neural Network Training run are—

- **Forward Pass** — Pass Input Data through the model and produce a Prediction.
- **Loss Calculation** — Measure the difference between the Prediction and the Expected Answer with a Loss Function.
- **Backward Pass** — Compute Gradients of the model parameters from the Loss.
- **Weight Update** — The Optimizer uses the Gradients to update the Model Weights.
- **Iteration** — Repeat over many Batches and Epochs.

**Key Technical Terms**

| Term | Meaning |
|---|---|
| Forward Pass | The computation that produces a Prediction from the input |
| Loss Function | The function that measures prediction error |
| Backward Pass | The step that computes Gradients |
| Gradient | Values that describe how Loss changes with respect to the Parameters |
| Optimizer | Uses the Gradients to update the Parameters |
| Weights | The main components among the Parameters the model has learned |
| Epoch | One full pass through the Training Dataset |
| Batch | A group of data used at one time during Training |

Forward Pass can be used in both Training and Inference. Typical Training, however, also includes Backward Pass and Weight Update.

## 5. How Does Model Inference Work?

Inference is using a trained model to produce outputs for new inputs.

Examples—

- Fraud Detection — Estimating the probability that a Transaction is Fraud.
- Recommendation System — Suggesting suitable Products for a user.
- Image Classification — Classifying an image into one of the defined Categories.
- LLM Chatbot — Generating a Response based on a User Prompt.

A typical Inference Flow is—

```text
New Input
    |
    v
Preprocessing
    |
    v
Trained Model
    |
    v
Forward Pass
    |
    v
Prediction / Response
```

Inference normally does not require Gradient computation or Weight Update. However, Latency can still arise from Input Processing, Model Execution, Output Processing, and Network Communication.

Importantly, this does not mean the model will automatically recognize a new Category it never learned during Training. The Output depends on the model's Training Data, Architecture, and learned Capabilities.

## 6. Training vs. Inference — Key Differences

| Aspect | Model Training | Model Inference |
|---|---|---|
| Purpose | Learn patterns from Data | Produce Predictions for new inputs |
| Model Weights | Updated during Training | Normally unchanged |
| Forward Pass | Included | Included |
| Backward Pass | Included in typical gradient-based training | Normally not needed |
| Optimizer | Used for Weight Update | Not needed for typical Inference |
| Compute | Usually high | Depends on the model and workload |
| Memory | May need Weights, Activations, Gradients, and Optimizer States | Needs Weights, Activations, and other runtime memory |
| Main Performance Goal | Training Speed, Convergence, Scalability | Latency, Throughput, Efficiency |
| Usage Pattern | Training Jobs, Experiments, Retraining | Online APIs, Batch Prediction, Production Services |

## 7. Why Do Compute Requirements Differ?

### 7.1 Training — Can Require High Compute

During Training the model does not only pass data through and compute Predictions — it also computes Gradients and updates Weights.

For Large Deep Learning Models—

- The Dataset can be large.
- There can be many Model Parameters.
- Many Training Iterations may be required.
- Distributed Training may need many GPUs.
- GPU-to-GPU Communication and Data Loading must also be managed.

Therefore, in a Training Cluster, High-Performance GPUs, Fast Interconnects, and Efficient Storage/Data Pipelines can be important.

However, not every model needs to be trained on a Multi-Node Cluster. For small models or small datasets, a Single GPU or even a CPU can be sufficient.

### 7.2 Inference — Compute Depends on the Workload

Inference normally runs only a Forward Pass, so it can need less Compute and Memory than Training. But depending on Model Size and Request Volume, Inference can also require substantial GPU Resources.

For example, in LLM Inference—

- Model Weights may need to reside in GPU Memory.
- Prompt Processing and Token Generation require Compute.
- Autoregressive generation may need Memory Structures such as the KV Cache.
- If Concurrent Requests grow, Memory and Throughput requirements can rise.

So "Inference needs less Compute" is only a general principle and may not hold for every workload.

## 8. Why Do Memory Requirements Differ?

**Training Memory**

Memory requirements during Training are affected by—

- Model Weights.
- Activations from the Forward Pass.
- Gradients.
- Optimizer States.
- Batch Size.
- Sequence Length or Input Resolution.
- Precision and Training Techniques.

Training Memory requirements cannot be determined accurately from Model Size alone.

**Inference Memory**

Memory needed for Inference depends on—

- Model Weights.
- Intermediate Activations and Runtime Buffers.
- Batch Size and Concurrent Requests.
- KV Cache for LLMs.
- Memory Overhead of the Framework and Serving Engine.

Using optimization methods such as Quantization or Pruning can reduce the Memory Footprint of some models. However, not every optimization must be applied to every model. The impact on Accuracy and Performance must also be checked.

## 9. Latency vs. Throughput

Two very important metrics when designing an Inference Infrastructure are Latency and Throughput.

For example, if a User sends a question to a Chatbot and it takes 200 milliseconds before the Response starts arriving, that time is an example of Latency.

For example, if a Server can perform 1,000 Predictions per second, the Throughput is 1,000 predictions/second.

An Inference Service may need both low Latency and high Throughput. However, using more Batching can increase Throughput while also increasing the waiting time for some Requests. Application SLA and User Experience must therefore be taken into account.

For Training, what matters is teaching the model to an acceptable Quality level in a short time — GPU Utilization and Distributed Scaling are important.

## 10. Training and Inference Scaling Strategies

### 10.1 Training — Distributed Scaling

Large models can be trained with many GPUs, and a Multi-Node Cluster can be used when needed.

Scaling approaches include—

- Data Parallelism: Keep model copies on each GPU and train on different parts of the data batch.
- Model Parallelism: Split parts of the model across GPUs.
- Pipeline Parallelism: Split model stages and run them on different GPUs.

Training Performance does not improve in direct proportion to the number of GPUs. Communication Overhead, Workload Balance, Memory Limitations, and Software Configuration all affect Scaling Efficiency.

### 10.2 Inference — Elastic Scaling

In Production Inference Workloads, Request Volume can change over time.

For example—

- In a Normal Period, a few Servers can handle the Requests.
- In a Peak Period, if Requests increase, additional Replicas or GPU Instances may be needed.
- When Demand falls, Capacity can be scaled back down.

This approach is called Elastic Scaling. Kubernetes Horizontal Pod Autoscaler, KEDA, or Cloud Auto Scaling Services can help in suitable architectures.

Using Serverless Inference can reduce hands-on infrastructure management, but the limits of Capacity, Cold Start, GPU Availability, Cost, and Model Size must be considered.

## 11. Infrastructure Choices — Scenarios

| Scenario | Main Requirement | What to Consider |
|---|---|---|
| Large LLM Training | High Compute, High Memory Bandwidth | Multi-GPU, Interconnect, Distributed Training |
| Small Model Experiment | Flexible Development Environment | A Single GPU or CPU can be sufficient |
| Real-Time Fraud Detection | Low Latency, Reliable Serving | Model Serving, Request Latency, Availability |
| High-Volume Recommendation API | High Throughput | Batching, Replicas, GPU Utilization |
| LLM Chatbot with Long Context | Memory Capacity, KV Cache Management | GPU Memory, Concurrency, Context Length |
| Periodic Model Retraining | Scheduled Training Capacity | Data Pipeline, Training Window, Model Validation |

The key point is that even with the same Physical Infrastructure, Resource Allocation, Software Configuration, Scheduling, and Performance Tuning can be managed differently for Training and Inference.

## 12. Exam Notes

- Training: Learning from Data and updating Model Parameters.
- Inference: Producing Predictions for new inputs with a trained model.
- Forward Pass: Computing a Prediction through the model.
- Backward Pass: Computing Gradients during Training.
- Weight Update: The Optimizer updates Model Parameters.
- Training Performance: Training Time, Convergence, GPU Utilization, and Scalability.
- Inference Performance: Latency, Throughput, Memory Efficiency, and Serving Cost.
- Training Memory: Can be large due to Weights, Activations, Gradients, and Optimizer States.
- Inference Memory: May need Weights, Activations, Runtime Buffers, and LLM KV Cache.
- Training Scaling: Distributed Training can use Multi-GPU/Multi-Node.
- Inference Scaling: Replicas or Capacity can be Elastically Scaled according to Request Demand.

## 13. Review Questions

**Question 1 — What is the main difference between Model Training and Model Inference?**

Answer — Training learns patterns from Data and updates Model Weights. Inference uses a trained model to produce Predictions for new inputs; the Model Weights normally do not change.

**Question 2 — How do Forward Pass and Backward Pass differ?**

Answer — Forward Pass passes the input through the model and produces a Prediction — it is part of both Training and Inference. Backward Pass computes Gradients from the Loss; it is needed in typical Training but not in Inference.

**Question 3 — What data structures can Training Memory contain?**

Answer — Model Weights, Activations from the Forward Pass, Gradients, and Optimizer States. It also depends on Batch Size, Sequence Length/Input Resolution, and Precision.

**Question 4 — Explain Latency and Throughput with examples.**

Answer — Latency is how long a Request takes before the Response starts — for example, 200 ms before a Chatbot Response begins. Throughput is the amount processed per second — for example, 1,000 Predictions per second (1,000 predictions/second).

**Question 5 — Does Inference always need less Compute than Training?**

Answer — No. Inference normally runs only a Forward Pass, so it can need less — but Large LLMs, high Request Volume, and the KV Cache can make Inference require substantial GPU Compute and Memory as well.

## 14. Module Summary

- Model Training and Model Inference are the two main workloads of AI Infrastructure.
- In Training, the model learns from Data and updates Weights. Gradient Computation, Optimizer States, and Distributed Training can therefore drive high Compute and Memory requirements.
- Inference uses a trained model to produce Predictions. It normally runs only a Forward Pass, but for Large LLMs and High-Volume Services, GPU Compute and Memory requirements can be very high.

Key points for the exam —

- Training → Learning, Gradients, Weight Updates, Convergence, Distributed Scaling.
- Inference → Predictions, Low Latency, High Throughput, Memory Efficiency, Elastic Scaling.

When choosing Infrastructure, do not assume "Training is always big and Inference is always small" — decide based on Model Size, Request Volume, Latency Target, Memory Requirement, and Application SLA.

## Technical review notes

- The supplied source transcript for Module 35 is in Burmese only ("အပိုင်း ၁ — မြန်မာဘာသာဖြင့် အသေးစိတ်ရှင်းလင်းချက်"). The English Version section is a faithful technical translation of that Burmese source, not a separate supplied original; section numbering, tables, and the Inference Flow diagram mirror the Burmese section exactly.
- Source seam artifacts: stray `</box>` closing tags appear after the workload and cooking-analogy bullet items in the source transcript. These were removed and the affected items rendered as bullets. The box labels themselves were lost in the transcript; the **Model Training** / **Model Inference** (section 2) and **Training** / **Inference** (section 3) bullet labels were reconstructed from the immediately preceding sentences, which name the two cases in that order. Content of the bullets is unchanged.
- Review questions: the source lists 8 questions without answers. Per WRITING_RULES these were reduced to 5 representative questions (source questions 1, 2, 3, 5, and 8) and concise answers were added in both language versions.
- The example figures "200 milliseconds" (Latency) and "1,000 predictions/second" (Throughput) are illustrative examples from the source, not benchmark measurements.
- No commands, specifications, URLs, or benchmark numbers were added beyond the source. No technically incorrect claims were found in the module body; statements about Retraining, Scaling Efficiency, Serverless Inference limits, and optimization trade-offs are correctly qualified in the source and kept as written.
- `COURSE_OUTLINE.md` lists Module 35 under a different title ("AI Model Optimization and Inference Serving"); this file follows the title given in the supplied source transcript and the task header requirement ("Module 35 — Model Training vs. Model Inference"). Reconciling the outline title is left to the course editor.
