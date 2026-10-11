# Module 04 — Training vs Inference

> Part I · AI Foundations and AI Infrastructure — Course Module 04 (Training vs Inference) — topic notes

## မြန်မာဘာသာ version

### 1. ပြဿနာ — Model တစ်ခုတည်းမှ Workload အဆင့် နှစ်ဆင့်

AI system တစ်ခုကို တည်ဆောက်ပြီး လုပ်ငန်းခွင်မှာ အသုံးချတဲ့အခါ GPU infrastructure ပေါ်မှာ အဓိက run နေတဲ့ workload နှစ်မျိုး ရှိပါတယ် — **Model Training** နဲ့ **Model Inference**။ နှစ်မျိုးစလုံးက တူညီတဲ့ model code ကို အသုံးပြုပေမယ့် resource ကို ဘယ်လိုသုံးသလဲ၊ ဘာက bottleneck ဖြစ်သလဲ၊ ဘယ်လို metric တွေနဲ့ တိုင်းတာသလဲဆိုတာ အလွန်ကွာခြားပါတယ်။

- **Model Training** — Training data တွေကနေ pattern တွေကို သင်ယူပြီး model weights တွေကို update လုပ်တဲ့ အဆင့်။
- **Model Inference** — train လုပ်ပြီးသား model ကို အသုံးပြုပြီး input အသစ်တွေအတွက် prediction ထုတ်ပေးတဲ့ အဆင့်။

Infrastructure engineer တစ်ယောက်အနေနဲ့ GPU တွေ ရွေးချယ်ဝယ်ယူတာ၊ cluster ဒီဇိုင်းဆွဲတာ၊ budget ချမှတ်တာတွေ လုပ်တဲ့အခါ ဒီ workload နှစ်မျိုးကို မခွဲခြားဘူးဆိုရင် မှားယွင်းတဲ့ ဆုံးဖြတ်ချက်တွေ ချမိတတ်ပါတယ်။ ဥပမာ — training အတွက် ကောင်းတဲ့ configuration က inference latency requirement ကို ဖြည့်ဆည်းပေးနိုင်ချင်မှ မဟုတ်သလို၊ inference အတွက် ပြည့်စုံတဲ့ sizing က training အတွက် memory မလောက်တတ်ပါဘူး။

ဤ module သည် Part I ၏ မိတ်ဆက်အဆင့် ဖြစ်ပါသည်။ Training vs Inference ကို ပိုမိုနက်ရှိုင်းစွာ လေ့လာချင်ပါက **Module 35 — Model Training vs. Model Inference** ကို ဆက်လက်ဖတ်ရှုပါ။

### 2. အဓိက Concept များ — Training Loop နှင့် Inference

**Training loop** တစ်ခုမှာ အဓိကအဆင့် လေးဆင့် ပါဝင်ပါတယ် — forward pass, loss calculation, backward pass (gradients) နဲ့ optimizer update။ ဒီအဆင့်တွေကို batch ပေါင်းများစွာ၊ epoch ပေါင်းများစွာ ကြိမ်ဖန်များစွာ ပြန်လည်လုပ်ဆောင်ပါတယ်။

```text
              +---------------------------+
              |       Training Loop       |
              v                           |
        [Input Batch]                     |
              |                           |
              v                           |
        [Forward Pass] ----> Prediction   |
              |                           |
              v                           |
        [Loss Calculation]                |
              |                           |
              v                           |
        [Backward Pass] ----> Gradients   |
              |                           |
              v                           |
        [Optimizer Update] --> Weights ---+
        (batch များ / epoch များ ဆက်လက်ပြန်လည်လုပ်ဆောင်)
```

Training အတွင်း GPU memory မှာ သိမ်းဆည်းထားရတဲ့ အရာတွေက — **weights, gradients, optimizer states နှင့် stored activations** တို့ ဖြစ်ပါတယ်။ လက်တွေ့ခန့်မှန်းချက်အနေနဲ့ mixed-precision Adam optimizer အတွက် activation များ မထည့်သေးမီ parameter တစ်ခုလျှင် ပျမ်းမျှ **~16 bytes/parameter** လောက် လိုအပ်သည်ဟု ဆိုလေ့ရှိပါတယ် (rule-of-thumb / engineering estimate သာ ဖြစ်ပါသည်)။

**Inference** မှာမူ ရှေ့သို့သာ မောင်းနှင်ပါတယ် — **forward pass only**။ Gradient တွက်ချက်ခြင်းနှင့် weight update လုပ်ခြင်း မလိုအပ်ပါဘူး။ Memory မှာ **weights + runtime state** (LLM အတွက်ဆိုရင် KV cache) သာ သိမ်းဆည်းထားရပါတယ်။

```text
[New Input] ---> [Forward Pass] ---> [Prediction / Response]
                  (weights ပြောင်းလဲခြင်း မရှိပါ)
```

### 3. Training vs Inference — နှိုင်းယှဉ်ဇယား

| အချက် | Model Training | Model Inference |
|---|---|---|
| Memory ထဲတွင် သိမ်းဆည်းထားသည်များ | weights + gradients + optimizer states + stored activations (mixed-precision Adam အတွက် activations မထည့်မီ ~16 bytes/parameter — rule-of-thumb) | weights + runtime state (LLM: KV cache); gradients နှင့် optimizer states မလိုအပ်ပါ |
| ပဓာန bottleneck | healthy batch size များတွင် **compute-bound** (large matrix multiplications များက Tensor Cores ကို ပြည့်ဝစွာ အသုံးချနိုင်) | single-stream autoregressive decode သည် **memory-bandwidth-bound** (token တစ်ခုချင်းစီအတွက် weights + ကြီးထွားလာသော KV cache ကို HBM မှ ဖတ်ရ) |
| Batch size ကြောင့် ဖြစ်ပေါ်မှု | batch ကြီးလေ hardware utilization ကောင်းလေ (memory ကန့်သတ်ချက်အထိ) | batching က throughput ကို ကောင်းစေ (weights ကို တစ်ကြိမ်ဖတ်ပြီး batch တစ်ခုလုံးတွင် ပြန်အသုံးပြု) — သို့သော် per-request latency တိုးလာနိုင် |
| တိုင်းတာသော metric များ | **throughput** (samples/sec)၊ time-to-convergence; per-step latency က သိပ်အရေးမကြီးပါ | **latency-sensitive** — TTFT (time-to-first-token), TPOT (time-per-output-token), tokens/sec |
| Scaling ပုံစံ | GPU များတွင် အလုပ်ကို ခွဲဝေပြီး gradient sync လုပ် (data/tensor/pipeline parallelism; NCCL ဖြင့် NVLink/InfiniBand ပေါ်တွင် ညှိနှိုင်း) | replicas/instances များဖြင့် scale out (model ကြီးများအတွက် tensor parallelism ပါ အသုံးပြု) |
| Precision အသုံးပြုမှု | mixed precision training အသုံးများ (memory ခန့်မှန်းချက်က mixed-precision Adam ကို အခြေခံ) | deployment တွင် model ကို တင်ဆင်နိုင်ရန် **quantization** အသုံးများ (Module 42 ကို ကြည့်ပါ) |

### 4. LLM Inference ကို နက်ရှိုင်းစွာ ကြည့်ခြင်း — Prefill, Decode နှင့် KV Cache

LLM inference မှာ အဆင့်နှစ်ဆင့် (phase) ရှိပါတယ်။

- **Prefill phase** — input token အားလုံးကို parallel အဖြစ် တစ်ပြိုင်နက် စီမံဆောင်ရွက်ပြီး KV cache ကို တည်ဆောက်ကာ ပထမဆုံး output token ကို ထုတ်ပေးပါတယ်။ ဒီအဆင့်က **compute-bound** ဖြစ်ပြီး GPU ၏ parallel compute ကို ပြည့်ဝစွာ အသုံးချနိုင်ပါတယ်။
- **Decode phase** — token တစ်ခုချင်းစီကို autoregressive ပုံစံဖြင့် ထုတ်ပေးပါတယ်။ step တိုင်းမှာ စုဆောင်းထားပြီးသား KV cache ကို ပြန်လည်ဖတ်ရပါတယ်။ ဒီအဆင့်က compute ပမာဏ နည်းပေမယ့် **memory-bandwidth-bound** ဖြစ်ပါတယ်။

```text
Prompt tokens:  [t1  t2  t3  ...  tN]
                      |
                      v
   +------------------------------------+
   | Prefill — token အားလုံး parallel  |   <-- TTFT ကို ဤနေရာတွင် တိုင်းတာ
   | စီမံပြီး KV cache တည်ဆောက်ကာ     |
   | ပထမဆုံး output token ထုတ်ပေး      |
   +------------------------------------+
                      |
                      v
   +------------------------------------+
   | Decode — token တစ်ခုချင်းစီ         |   <-- TPOT ကို ဤနေရာတွင် တိုင်းတာ
   | ထုတ်ပြီး KV cache ကို step တိုင်း    |
   | ဖတ်ရှု: t(N+1), t(N+2), ...        |
   +------------------------------------+
```

ဒီ phase နှစ်ခုအတွက် latency metric တွေကို ခွဲခြားတိုင်းတာပါတယ် — **TTFT** (time-to-first-token) က prefill ကို ကိုယ်စားပြုပြီး၊ **TPOT** (time-per-output-token) က decode ကို ကိုယ်စားပြုပါတယ်။

**KV cache ဆိုတာ ဘာလဲ?**

Attention mechanism မှာ အသုံးပြုတဲ့ key/value tensors တွေကို GPU memory မှာ သိမ်းဆည်းထားခြင်း ဖြစ်ပါတယ်။ မသိမ်းထားရင် decode step တိုင်းမှာ prompt တစ်ခုလုံးကို ပြန်လည်တွက်ချက်ရမှာ ဖြစ်လို့ ကုန်ကျစရိတ် အလွန်ကြီးမားပါတယ်။ KV cache ရဲ့ ပမာဏက sequence length, batch size, layer အရေအတွက်, KV heads နဲ့ precision တို့နဲ့အတူ ကြီးထွားလာပါတယ်။ GQA (grouped-query attention) နှင့် MQA (multi-query attention) လို technique တွေက KV heads အရေအတွက်ကို လျှော့ချခြင်းဖြင့် KV cache ကို လျှော့ချပေးနိုင်ပါတယ်။

> **NVIDIA blog example, illustrative** — 7B parameter Llama-2 model၊ FP16၊ batch size 1၊ sequence length 4096 အတွက် KV cache ပမာဏသည် ခန့်မှန်းချေ ~2 GB ရှိသည်ဟု NVIDIA blog တစ်ခုတွင် ဥပမာပြထားပါသည်။ ဤဂဏန်းသည် သရုပ်ပြရန် ရည်ရွယ်သော ဥပမာသာ ဖြစ်ပြီး benchmark မဟုတ်ပါ။

### 5. Infrastructure ရွေးချယ်မှုအပေါ် သက်ရောက်မှု

Workload နှစ်မျိုးက hardware ကို မတူညီတဲ့ ဘက်ကနေ ဖိအားပေးပါတယ်။

**Training အတွက် အရေးကြီးသည်များ**

- **FLOPS (Tensor Cores)** — large matrix multiplications တွေကို ပြည့်ဝစွာ တွက်ချက်နိုင်မှု
- **VRAM headroom** — weights, gradients, optimizer states, activations အားလုံး တစ်ပြိုင်နက် တင်ထားနိုင်ရန်
- **GPU-to-GPU interconnect မြန်နှုန်း** — gradient synchronization ကို **NCCL** ဖြင့် **NVLink / InfiniBand** ပေါ်တွင် လုပ်ဆောင်ရန်

**Inference အတွက် အရေးကြီးသည်များ**

- **Memory bandwidth** — decode step တိုင်းမှာ weights + KV cache ကို HBM ကနေ ဖတ်ရမှုက ပဓာန bottleneck ဖြစ်
- **VRAM capacity** — weights + KV cache ကို တင်ဆင်ထားနိုင်ရန် နေရာလုံလောက်မှု
- **Latency SLA** — TTFT, TPOT နှင့် tokens/sec ပန်းတိုင်များကို ဖြည့်ဆည်းနိုင်ရန်
- Deployment တွင် model ကို တင်ဆင်နိုင်ရန် **quantization** အသုံးပြုလေ့ရှိ (**Module 42** ကို ကြည့်ပါ)

**Scaling ပုံစံ ကွာခြားချက်** — Training က GPU များကြား အလုပ်ခွဲဝေပြီး gradient sync ဖြင့် scale လုပ်ပါတယ်။ Inference ကတော့ replica/instance များ ထပ်တိုးခြင်းဖြင့် scale လုပ်ပြီး၊ model ကြီးများအတွက် tensor parallelism ကို အသုံးပြုပါတယ်။

ဆက်စပ် module များ — **Module 35** (နက်ရှိုင်းသော ရှင်းလင်းချက်)၊ **Module 38** (Slurm / Kubernetes ဖြင့် operation လုပ်ဆောင်မှု)၊ **Module 42** (quantization နှင့် deployment)။

### 6. သတိပြုရန် အချက်များ

- Training နဲ့ Inference နှစ်မျိုးစလုံးက **GPU hardware တူညီသည်** — ကွာခြားတာက **bottleneck** ဖြစ်ပါတယ်။ "inference GPU" ဆိုတဲ့ သီးသန့် hardware အမျိုးအစား မရှိပါဘူး။
- ဤ module ပါ ဂဏန်းအားလုံး (~16 bytes/parameter, ~2 GB စသည်) သည် **rule-of-thumb / illustrative** ခန့်မှန်းချက်များသာ ဖြစ်ပါသည်။ Benchmark မဟုတ်၊ အာမခံချက် မဟုတ်ပါ။
- Inference performance သည် model architecture, batch size, sequence length နှင့် serving configuration ပေါ် မူတည်ပါတယ် — ကိုယ်ပိုင် workload ဖြင့် တိုင်းတာပါ။
- ဤ module သည် မိတ်ဆက်အဆင့်သာ ဖြစ်ပြီး training/inference operation အသေးစိတ်ကို Module 35 တွင် ဖော်ပြထားပါသည်။

### 7. မိမိကိုယ်ကို စစ်ဆေးရန် မေးခွန်းများနှင့် အဖြေများ

**မေးခွန်း ၁ — Training loop ရဲ့ အဆင်းလေးဆင့်က ဘာတွေလဲ?**
အဖြေ — Forward pass → Loss calculation → Backward pass (gradient တွက်ချက်ခြင်း) → Optimizer update။ ဒီအဆင့်တွေကို batch များစွာဖြင့် ပြန်လည်လုပ်ဆောင်ပါတယ်။

**မေးခွန်း ၂ — Inference မှာ backward pass နဲ့ optimizer လိုအပ်သလား?**
အဖြေ — မလိုအပ်ပါ။ Inference မှာ forward pass သာ လုပ်ဆောင်ပြီး weights များ ပြောင်းလဲခြင်း မရှိပါဘူး။

**မေးခွန်း ၃ — LLM ရဲ့ decode phase မှာ ပဓာန bottleneck က ဘာလဲ?**
အဖြေ — Memory bandwidth။ Token တစ်ခုချင်းစီ ထုတ်လုပ်ရာတွင် weights နဲ့ ကြီးထွားလာသော KV cache ကို HBM ကနေ ဖတ်ရပါတယ်။

**မေးခွန်း ၄ — TTFT နဲ့ TPOT က ဘယ် phase တွေကို တိုင်းတာသလဲ?**
အဖြေ — TTFT (time-to-first-token) က prefill phase ကို တိုင်းတာပြီး၊ TPOT (time-per-output-token) က decode phase ကို တိုင်းတာပါတယ်။

**မေးခွန်း ၅ — KV cache က ဘာကြောင့် လိုအပ်သလဲ?**
အဖြေ — Attention ရဲ့ key/value tensors ကို GPU memory မှာ သိမ်းထားလို့ decode step တိုင်းမှာ prompt တစ်ခုလုံးကို ပြန်လည်တွက်ချက်စရာ မလိုဘဲ ဖြစ်စေလို့ ဖြစ်ပါတယ်။

## English Version

### 1. The Problem — Two Workload Phases from One Model

When an AI system is built and put into production, two workloads dominate the GPU infrastructure: **model training** and **model inference**. Both run the same model code, but they use hardware in very different ways — different bottlenecks, different metrics, and different planning assumptions.

- **Model training** — the phase that learns patterns from training data and updates the model weights.
- **Model inference** — the phase that uses the trained model to produce predictions for new inputs.

If you size a cluster, pick GPUs, or plan a budget without separating these two workloads, the decisions can go wrong in both directions. A configuration that is great for training may not meet inference latency requirements, and sizing that is perfect for inference may not leave enough memory for training.

This module is the Part I introduction. For an in-depth treatment, continue with **Module 35 — Model Training vs. Model Inference**.

### 2. Core Concepts — The Training Loop and Inference

A **training loop** has four main steps — forward pass, loss calculation, backward pass (gradients), and optimizer update — repeated over many batches and epochs.

```text
              +---------------------------+
              |       Training Loop       |
              v                           |
        [Input Batch]                     |
              |                           |
              v                           |
        [Forward Pass] ----> Prediction   |
              |                           |
              v                           |
        [Loss Calculation]                |
              |                           |
              v                           |
        [Backward Pass] ----> Gradients   |
              |                           |
              v                           |
        [Optimizer Update] --> Weights ---+
        (repeated over many batches / epochs)
```

During training, GPU memory must hold **weights, gradients, optimizer states, and stored activations**. As a practical rule of thumb, mixed-precision Adam training needs on the order of **~16 bytes per parameter** before activations (this is a rule-of-thumb / engineering estimate, not a benchmark).

**Inference** is forward-only: a **forward pass** and nothing else. No gradient computation, no weight updates. Memory holds **weights plus runtime state** — for LLMs, that runtime state is the KV cache.

```text
[New Input] ---> [Forward Pass] ---> [Prediction / Response]
                  (weights stay fixed)
```

### 3. Training vs Inference — Comparison Table

| Aspect | Model Training | Model Inference |
|---|---|---|
| What memory holds | weights + gradients + optimizer states + stored activations (~16 bytes/parameter before activations for mixed-precision Adam — rule-of-thumb) | weights + runtime state (LLM: KV cache); no gradients or optimizer states |
| Main bottleneck | **compute-bound** at healthy batch sizes (large matmuls saturate the Tensor Cores) | single-stream autoregressive decode is **memory-bandwidth-bound** (each token reads weights + the growing KV cache from HBM) |
| Batch size behavior | larger batches improve hardware utilization up to memory limits | batching improves throughput (weights read once, reused across the batch) but adds per-request latency |
| Metrics | **throughput** (samples/sec), time-to-convergence; per-step latency matters less | **latency-sensitive** — TTFT (time-to-first-token), TPOT (time-per-output-token), tokens/sec |
| Scaling pattern | split work across GPUs with gradient synchronization (data/tensor/pipeline parallelism; NCCL over NVLink/InfiniBand) | scale out with replicas/instances (tensor parallelism for large models) |
| Precision | mixed-precision training is common (the memory estimate above assumes mixed-precision Adam) | **quantization** is often used in deployment to fit models (see Module 42) |

### 4. A Deeper Look at LLM Inference — Prefill, Decode, and the KV Cache

LLM inference runs in two phases.

- **Prefill phase** — processes ALL input tokens in parallel to compute the KV cache and produce the first output token. This phase is **compute-bound** and can saturate the GPU's parallel compute.
- **Decode phase** — generates tokens one at a time autoregressively; each step reads the accumulated KV cache. This phase is less computationally intensive but **memory-bandwidth-bound**.

```text
Prompt tokens:  [t1  t2  t3  ...  tN]
                      |
                      v
   +------------------------------------+
   | Prefill — processes all input       |   <-- TTFT is measured here
   | tokens in parallel, builds the KV   |
   | cache, emits the first output token |
   +------------------------------------+
                      |
                      v
   +------------------------------------+
   | Decode — generates tokens one at    |   <-- TPOT is measured here
   | a time, reading the KV cache each   |
   | step: t(N+1), t(N+2), ...           |
   +------------------------------------+
```

Latency is measured separately for each phase: **TTFT** (time-to-first-token) represents prefill, and **TPOT** (time-per-output-token) represents decode.

**What is the KV cache?**

It is the attention key/value tensors kept in GPU memory so they do not have to be recomputed. Without it, every decode step would recompute the entire prompt's attention state — enormously expensive. The KV cache grows with sequence length, batch size, layer count, KV heads, and precision. Techniques such as **GQA** (grouped-query attention) and **MQA** (multi-query attention) shrink it by reducing the number of KV heads.

> **NVIDIA blog example, illustrative** — for a 7B-parameter Llama-2 model in FP16 with batch size 1 and sequence length 4096, an NVIDIA blog gives a KV cache of roughly ~2 GB. This figure is an illustrative example, not a benchmark.

### 5. Infrastructure Implications

The two workloads stress the hardware from different directions.

**What training wants**

- **FLOPS (Tensor Cores)** — enough parallel compute to saturate large matrix multiplications
- **VRAM headroom** — room to hold weights, gradients, optimizer states, and activations at the same time
- **Fast GPU-to-GPU interconnect** — gradient synchronization with **NCCL** over **NVLink / InfiniBand**

**What inference wants**

- **Memory bandwidth** — reading weights + KV cache from HBM on every decode step is the main bottleneck
- **VRAM capacity** — enough room for weights + the KV cache
- **Latency SLAs** — meeting TTFT, TPOT, and tokens/sec targets
- **Quantization** is commonly used in deployment to fit models (see **Module 42**)

**Different scaling patterns** — training scales by splitting work across GPUs with gradient synchronization. Inference scales by adding replicas/instances, with tensor parallelism for large models.

Related modules — **Module 35** (in-depth treatment), **Module 38** (operations on Slurm / Kubernetes), **Module 42** (quantization and deployment).

### 6. Cautions

- Training and inference run on the **same GPU hardware** — what differs is the **bottleneck**. There is no separate "inference GPU" hardware category.
- Every number in this module (~16 bytes/parameter, ~2 GB, etc.) is a **rule-of-thumb / illustrative** estimate. Not a benchmark, not a guarantee.
- Inference performance depends on the model architecture, batch size, sequence length, and serving configuration — measure it with your own workload.
- This module is an introduction; operational detail lives in Module 35.

### 7. Self-Check Questions and Answers

**Q1 — What are the four steps of the training loop?**
Answer — Forward pass → loss calculation → backward pass (gradient computation) → optimizer update, repeated over many batches.

**Q2 — Does inference need a backward pass or an optimizer?**
Answer — No. Inference runs a forward pass only; the weights do not change.

**Q3 — What is the main bottleneck in the LLM decode phase?**
Answer — Memory bandwidth. Each generated token requires reading the weights and the growing KV cache from HBM.

**Q4 — Which phases do TTFT and TPOT measure?**
Answer — TTFT (time-to-first-token) measures prefill; TPOT (time-per-output-token) measures decode.

**Q5 — Why is the KV cache needed?**
Answer — It stores the attention key/value tensors in GPU memory so each decode step does not have to recompute the whole prompt.

## Official references

- NVIDIA Developer Blog — *Streamlining AI Inference Performance and Deployment with NVIDIA TensorRT-LLM Chunked Prefill* (prefill/decode phases): https://developer.nvidia.com/blog/streamlining-ai-inference-performance-and-deployment-with-nvidia-tensorrt-llm-chunked-prefill/
- NVIDIA Developer Blog — *Mastering LLM Techniques: Inference Optimization* (KV cache example): https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/
- NVIDIA Deep Learning Performance documentation — *Training vs Inference* (needs verification — this URL returned HTTP 404 at the time of writing; the underlying content topic is covered by the two blog posts above): https://docs.nvidia.com/deeplearning/performance/dl-performance-training-inference/index.html

## Technical review notes

- The "~16 bytes/parameter" figure (mixed-precision Adam, before activations) is carried from the research notes as a rule-of-thumb engineering estimate. It is labeled as such in both language versions and is not presented as a measured benchmark.
- The "~2 GB" KV cache figure is labeled exactly "NVIDIA blog example, illustrative" (7B Llama-2, FP16, batch 1, sequence length 4096) in both language versions.
- The third Official reference URL (docs.nvidia.com DL performance training/inference page) returned HTTP 404 when checked; it is listed with a "needs verification" label rather than cited as a live source.
- No commands, benchmark results, or hardware specifications beyond the research notes were introduced. Modules 35, 38, and 42 are cross-referenced for depth and operations, and Module 35 is explicitly named as the in-depth companion to avoid duplication.
