# Module — AI-Centric Data Center Compute: CPU, GPU, CUDA and the Evolution of GPU Computing

> Part I · Course Module 03 (CPU vs GPU and Parallel Computing) — topic notes

## 1. CPU နှင့် GPU ကွာခြားချက်ကို နားလည်ရန် Compute Engine နှိုင်းယှဉ်ခြင်း

AI-Centric Data Center မှာ Compute ကို နားလည်ဖို့ CPU နဲ့ GPU ရဲ့ ကွာခြားချက်ကို အရင်ဆုံး ကြည့်ပါမယ်။
ထိပ်ဆုံးကနေ အောက်ဆုံးအထိ CPU Evolution ကို လိုက်ကြည့်ရအောင်။

**CPU Evolution**

```
        CPU
         │
   ┌─────┴─────┐
   │           │
Single-Core   Multi-Core
   │           │
   ▼           ▼
 1 Core      Many Cores
```

- **Single-Core CPU** — ယခင်က CPU တစ်လုံးမှာ Core တစ်ခုတည်းသာ ပါဝင်ပါတယ်။
- **Multi-Core CPU** — နောက်ပိုင်းမှာ CPU တစ်လုံးအတွင်းမှာ Core များစွာ ပါဝင်လာပါတယ်။

ဒီနေရာမှာ Core ဆိုတာ CPU ထဲက Processing Element ဖြစ်ပါတယ်။
Core အများကြီးရှိရင် Task များစွာကို တပြိုင်နက် လုပ်ဆောင်နိုင်ပါတယ်။

## 2. CPU vs GPU — Computing Model

**CPU Computing Model**

```
       CPU
        │
   ┌────┴────┐
   │         │
 Core 1   Core 2 ... Core N
   │         │
 Task A   Task B
```

- CPU မှာ Core အနည်းငယ်သာ ရှိတတ်ပေမယ့် Core တစ်ခုချင်းစီက အလွန် Powerful ဖြစ်ပါတယ်။
- ဒါကြောင့် CPU က General-Purpose Computing မှာ အသုံးများပါတယ်။

**GPU Computing Model**

```
            GPU
             │
   ┌─────┬─────┬─────┐
   │     │     │     │
  SM    SM    SM    SM
   │     │     │     │
 CUDA  CUDA  CUDA  CUDA
Cores Cores Cores Cores
```

- GPU မှာ Core အများကြီး ပါဝင်ပါတယ်။
- ဒါကြောင့် GPU က Parallel Computing အတွက် အလွန်သင့်တော်ပါတယ်။

> NVIDIA GPU Architecture မှာ—SM (Streaming Multiprocessor) နဲ့ CUDA Cores တွေ ပါဝင်ပါတယ်။

## 3. ဘာကြောင့် AI အတွက် GPU လိုအပ်ရတာလဲ?

AI Training နဲ့ AI Inference မှာ—

```
  ┌──────────────────────┐
  │   Matrix Operation   │
  │   Vector Operation   │
  │   Tensor Operation   │
  └──────────────────────┘
```

ဒီလို Mathematical Operations များစွာကို တပြိုင်နက် လုပ်ဆောင်ရပါတယ်။
CPU က Sequential / General-Purpose Processing မှာ ကောင်းမွန်ပေမယ့် AI လို Massive Parallel Computing အတွက် GPU က ပိုသင့်တော်ပါတယ်။

ဒါကြောင့် AI-Centric Data Center တွေမှာ GPU က Main Compute Engine ဖြစ်လာပါတယ်။

## 4. GPU ရဲ့ အဓိက အားသာချက်များ

- 1. Large-Scale Parallel Computing
- 2. AI Training Acceleration
- 3. AI Inference Acceleration
- 4. High-Performance Computing (HPC)
- 5. Data Analytics
- 6. Graphics Rendering

**ဒါပေမယ့် GPU က CPU ကို အစားထိုးတာ မဟုတ်ပါဘူး။**
CPU နဲ့ GPU က ကွဲပြားတဲ့ Computing Roles တွေ ရှိပါတယ်။

```
        ┌────────────────────┐
        │   AI Application   │
        └─────────┬──────────┘
                  │
          ┌───────┴───────┐
          │               │
       CPU Role       GPU Role
          │               │
   Control Flow     Parallel Compute
   OS / System      Matrix / Tensor
   General Task     AI Acceleration
```

## 5. GPU Computing ရဲ့ သမိုင်း — 1990s

ယခု GPU တွေ ဒီလောက် Powerful ဖြစ်လာတာ တိုက်ရိုက်မဟုတ်ပါဘူး။
1990s ခုနှစ်များမှာ CPU က—

- Main Computing Device
- Primary Compute Engine
- General-Purpose Processor

အဖြစ် အသုံးပြုခဲ့ပါတယ်။
Graphics Display အတွက်လည်း CPU ကပဲ အဓိက တာဝန်ယူခဲ့ပါတယ်။

## 6. GeForce 256 — "GPU" ဆိုတဲ့ Term စတင်ခြင်း

1999 ခုနှစ်မှာ **NVIDIA** က **GeForce 256** ကို မိတ်ဆက်ခဲ့ပါတယ်။
ဒီ Product ကို—

> "The World's First GPU" — The World's First Graphics Processing Unit

လို့ ဖော်ပြခဲ့ပါတယ်။

အဓိပ္ပါယ်က—Graphics အတွက် သီးသန့် Compute Engine တစ်ခု ပေါ်လာပါပြီ။
ဒီအချိန်ကစပြီး **GPU = Graphics Processing Unit** ဆိုတဲ့ Term ကို အသုံးပြုလာပါတယ်။

```
1999 → NVIDIA GeForce 256 → "The World's First GPU"
```

## 7. GPU ရဲ့ မူလ ရည်ရွယ်ချက် — Graphics Acceleration

ပထမဆုံး GPU တွေက—

- 3D Graphics
- Video Rendering
- Graphics Acceleration

အတွက် အဓိက ဒီဇိုင်းထုတ်ထားတာဖြစ်ပါတယ်။

ဒီအချိန်က GPU ကို AI / Data Center Computing အတွက် မသုံးရသေးပါဘူး။
GPU က Video Game / Graphics Rendering အတွက် အဓိက အသုံးပြုခဲ့ကြတာဖြစ်ပါတယ်။

## 8. GPU Architecture ရဲ့ အားသာချက်

GPU Architecture ရဲ့ အားသာချက်က—

```
         ┌───────────────────────────┐
         │    Graphics Rendering     │
         │    Parallel Pixels        │
         │    Multiple Vertices      │
         │    Texture Processing     │
         └───────────────────────────┘
```

**Parallel Processing** ဖြစ်ပါတယ်။
GPU က Pixel / Vertex များစွာကို တပြိုင်နက် Process လုပ်နိုင်ပါတယ်။
ဒါကြောင့် Graphics Rendering မှာ CPU ထက် ပိုမြန်ပါတယ်။

## 9. အရေးကြီးတဲ့ Transition — Parallel Processing Power

GPU မှာ ရှိပြီးသား—

```
   ┌─────────────────────────────┐
   │      Parallel Processing    │
   │      Thousands of Cores     │
   │      Matrix-Like Operations │
   └─────────────────────────────┘
```

ဒီ Power ကို Video Game ထဲမှာပဲ သုံးမလား? AI / Data Center Computing အတွက် ပြောင်းသုံးလို့ မရဘူးလား?

ဆိုတဲ့ Idea ပေါ်လာပါတယ်။
ဒီ Transition က **General-Purpose GPU (GPGPU)** ဆိုတဲ့ ခေတ်ကို ဦးဆောင်သွားပါတယ်။

**GPGPU ဆိုတာ ဘာလဲ?**

- GPGPU = General-Purpose computing on Graphics Processing Unit
- GPU ရဲ့ Parallel Processing Power ကို Graphics မဟုတ်တဲ့ Computing Task များအတွက် အသုံးပြုခြင်း

## 10. General-Purpose GPU (GPGPU) ရဲ့ နမူနာ အသုံးပြုမှုများ

- Image Processing
- Scientific Simulation
- Data Analytics
- Finance Modeling
- Medical Imaging
- AI Training
- AI Inference
- HPC
- Video Processing
- Molecular Simulation

ဒီလို Application တွေအားလုံးက Parallelism လိုအပ်တဲ့ Workload တွေဖြစ်ပါတယ်။

## 11. GPU Programming ရဲ့ စိန်ခေါ်မှု

GPU က Parallel Processing မှာ Powerful ဖြစ်ပေမယ့် GPU Programming က မလွယ်ကူပါဘူး။
အကြောင်းက GPU Hardware ကို တိုက်ရိုက် Program ရေးဖို့ ခက်ခဲပါတယ်။

ဒီအချိန်မှာ လိုအပ်ချက်က—

- Developers တွေ GPU ကို လွယ်လွယ်ကူကူ အသုံးပြုနိုင်ဖို့
- AI / HPC / Data Center Application တွေအတွက် GPU Acceleration ပေးနိုင်ဖို့
- CPU Programming နဲ့ ရင်းနှီးတဲ့ Programming Model ရှိဖို့

## 12. CUDA — Compute Unified Device Architecture

2006 ခုနှစ်မှာ **NVIDIA** က **CUDA** ကို မိတ်ဆက်ခဲ့ပါတယ်။

**CUDA = Compute Unified Device Architecture**

- CUDA က GPU Programming အတွက် Software Platform / Programming Model ဖြစ်ပါတယ်။
- CUDA ရဲ့ ရည်ရွယ်ချက်က—GPU ရဲ့ Parallel Processing Power ကို General-Purpose Computing အတွက် လွယ်ကူစွာ အသုံးပြုနိုင်အောင် ကူညီပေးခြင်းဖြစ်ပါတယ်။

**CUDA မတိုင်ခင် vs CUDA ပြီးနောက်**

```
CUDA မတိုင်ခင်          CUDA ပြီးနောက်
      │                       │
      ▼                       ▼
GPU Programming         Developer များအတွက်
အလွန်ခက်ခဲ           ပိုမိုလွယ်ကူလာ
```

## 13. ဘာကြောင့် CUDA က အရေးကြီးရတာလဲ?

CUDA က GPU Hardware ကို Abstract လုပ်ပေးပြီး Developer များအတွက် Programming Model ပေးပါတယ်။

**CUDA မတိုင်ခင်** — GPU Programming က Low-Level / Vendor-Specific / ခက်ခဲ
**CUDA ပြီးနောက်** — GPU Programming က Structured / Scalable / AI Workloads အတွက် သင့်တော်

CUDA က—

- Developer Tools
- Libraries
- Runtime
- Compiler Support
- Ecosystem

တွေကို ပေးပါတယ်။
ဒါကြောင့် CUDA က GPU Computing ရဲ့ Foundation ဖြစ်လာပါတယ်။

## 14. GPU Computing ရဲ့ တိုးတက်မှု Timeline

```
1999 ──► NVIDIA GeForce 256 ──► First GPU
2006 ──► CUDA ──► GPU General-Purpose Computing
2012 ──► AlexNet ──► GPU for Deep Learning
```

**GPU Evolution**

```
Gaming
  │
  ▼
Graphics
  │
  ▼
GPU
  │
  ▼
GPGPU
  │
  ▼
CUDA
  │
  ▼
Machine Learning
  │
  ▼
Deep Learning
  │
  ▼
Modern AI
```

## 15. AlexNet — 2012 Deep Learning Breakthrough

2012 ခုနှစ်မှာ Deep Learning Model **AlexNet** က ImageNet Competition မှာ အနိုင်ရခဲ့ပါတယ်။

ဒီ Event က AI ရဲ့ သမိုင်းမှာ အရေးကြီးတဲ့ Turning Point ဖြစ်ခဲ့ပါတယ်။
အကြောင်းက AlexNet ကို GPU နဲ့ Train ခဲ့တာဖြစ်ပါတယ်။

ဒီအချိန်ကစပြီး—

```
     ┌──────────────────────────────┐
     │   GPU for AI Training        │
     │   GPU for Deep Learning      │
     │   GPU for Data Center        │
     │   GPU for Inference          │
     └──────────────────────────────┘
```

AI Industry မှာ GPU ရဲ့ အခန်းကဏ္ဍ အလွန်ကြီးမားလာပါတယ်။

## 16. CPU vs GPU — အဓိက ကွာခြားချက်များ

| Feature | CPU | GPU |
|---|---|---|
| ရည်ရွယ်ချက် | General-Purpose Computing | Parallel Computing |
| Architecture | Complex Core + Large Cache | Many Small Cores |
| Parallelism | Limited Parallelism | Massive Parallelism |
| အသုံးပြုမှု | OS / Application / Database | AI Training / Inference / HPC |
| Graphics Rendering | အလယ်အလတ် | အလွန်ကောင်း |
| Matrix Operation | နှေးနိုင် | အလွန်မြန် |
| AI Acceleration | မသင့် | အလွန်သင့်တော် |

ဒါပေမယ့် CPU က AI-Centric Data Center မှာ အရေးကြီးဆဲပါပဲ။

- Data Preprocessing
- Data Loading
- System Management
- Application Logic
- Storage / Network Management

စတဲ့ Task တွေအတွက် CPU က ဆက်လက်အသုံးဝင်ပါတယ်။

## 17. CPU vs GPU — System Integration

**AI-Centric Data Center** မှာ CPU နဲ့ GPU က—

```
          ┌─────────────────────┐
          │   AI-Centric Data   │
          │       Center        │
          └──────────┬──────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
     ┌───┴────┐             ┌────┴────┐
     │  CPU   │             │   GPU   │
     │ System │             │ AI/HPC  │
     │ Control│             │Compute  │
     └────────┘             └─────────┘
```

- CPU က System-Level Control အတွက်
- GPU က AI/Compute Acceleration အတွက်

ဒီလို ပေါင်းစပ်အသုံးပြုပါတယ်။
ဒါကြောင့် AI-Centric Data Center မှာ **CPU + GPU + Software Ecosystem** က အဓိက Compute Foundation ဖြစ်ပါတယ်။

## 18. AI-Centric Data Center — Compute Summary

**AI-Centric Data Center ရဲ့ Compute ကို အကျဉ်းချုပ်ကြည့်ရင်—**

- CPU က General-Purpose Computing အတွက် အဓိကဖြစ်ပါတယ်။
- GPU က Parallel Computing အတွက် အဓိကဖြစ်ပါတယ်။
- GPU က Video Game / Graphics ကနေ စတင်ခဲ့ပေမယ့် AI / Data Center အတွက် အရေးကြီးလာပါတယ်။
- CUDA က GPU Programming ကို လွယ်ကူစေပါတယ်။
- AlexNet (2012) က GPU for Deep Learning ကို စတင်အသိအမှတ်ပြုစေပါတယ်။

**Exam အတွက် အရေးကြီးသော Memory Points**

```
GPU = Graphics Processing Unit
CUDA = Compute Unified Device Architecture
GPGPU = General-Purpose computing on Graphics Processing Unit
GeForce 256 = "World's First GPU" (1999)
CUDA = Introduced by NVIDIA (2006)
AlexNet = GPU Deep Learning Breakthrough (2012)
```

---

## English Version — AI-Centric Data Center Compute: CPU, GPU, CUDA and the Evolution of GPU Computing

### 1. Understanding the CPU and GPU Through Compute Engine Evolution

To understand Compute in an AI-Centric Data Center, we first need to understand the difference between the CPU and the GPU.

**CPU Evolution**

```
        CPU
         │
   ┌─────┴─────┐
   │           │
Single-Core   Multi-Core
   │           │
   ▼           ▼
 1 Core      Many Cores
```

- **Single-Core CPU** — Earlier CPUs contained only one core.
- **Multi-Core CPU** — Later CPUs began containing many cores inside a single CPU.

A Core is a processing element inside the CPU. Having many cores allows many tasks to be processed simultaneously.

### 2. CPU vs GPU — Computing Models

**CPU Computing Model**

```
       CPU
        │
   ┌────┴────┐
   │         │
 Core 1   Core 2 ... Core N
   │         │
 Task A   Task B
```

- A CPU typically has fewer cores, but each core is very powerful.
- This is why CPUs are widely used for general-purpose computing.

**GPU Computing Model**

```
            GPU
             │
   ┌─────┬─────┬─────┐
   │     │     │     │
  SM    SM    SM    SM
   │     │     │     │
 CUDA  CUDA  CUDA  CUDA
Cores Cores Cores Cores
```

- A GPU contains many cores.
- This is why GPUs are highly suitable for parallel computing.

> In NVIDIA GPU architectures, these structures include Streaming Multiprocessors (SMs) and CUDA cores.

### 3. Why Do We Need GPUs for AI?

In AI Training and AI Inference, large numbers of mathematical operations are performed simultaneously:

```
  ┌──────────────────────┐
  │   Matrix Operation   │
  │   Vector Operation   │
  │   Tensor Operation   │
  └──────────────────────┘
```

Although CPUs are strong at sequential and general-purpose processing, GPUs are better suited for the massive parallel computing required by AI workloads.

For this reason, GPUs have become the main compute engines of AI-Centric Data Centers.

### 4. Key Advantages of GPUs

1. Large-Scale Parallel Computing
2. AI Training Acceleration
3. AI Inference Acceleration
4. High-Performance Computing (HPC)
5. Data Analytics
6. Graphics Rendering

**Important:** The GPU does not replace the CPU. The CPU and the GPU serve different compute roles.

```
        ┌────────────────────┐
        │   AI Application   │
        └─────────┬──────────┘
                  │
          ┌───────┴───────┐
          │               │
       CPU Role       GPU Role
          │               │
   Control Flow     Parallel Compute
   OS / System      Matrix / Tensor
   General Task     AI Acceleration
```

### 5. History of GPU Computing — The 1990s

The modern GPU did not become this powerful overnight. During the 1990s, the CPU was the main computing device, primary compute engine, and general-purpose processor. Even graphics display tasks were primarily handled by the CPU.

### 6. The GeForce 256 and the Beginning of the Term "GPU"

In 1999, NVIDIA introduced the GeForce 256 and described it as:

> "The World's First GPU"

This meant that a dedicated compute engine designed for graphics emerged. From this point onward, the term GPU — Graphics Processing Unit — became widely used.

```
1999 → NVIDIA GeForce 256 → "The World's First GPU"
```

### 7. The Original Purpose of the GPU — Graphics Acceleration

The earliest GPUs were designed primarily for: 3D Graphics, Video Rendering, Graphics Acceleration. At that time, GPUs were not yet used for AI or data center computing. They were mainly used for video games and graphics rendering.

### 8. The Strength of the GPU Architecture

The main strength of the GPU architecture is parallel processing.

```
         ┌───────────────────────────┐
         │    Graphics Rendering     │
         │    Parallel Pixels        │
         │    Multiple Vertices      │
         │    Texture Processing     │
         └───────────────────────────┘
```

A GPU can process many pixels and vertices simultaneously, making it much faster than a CPU for graphics rendering.

### 9. The Important Transition — Parallel Processing Power

```
   ┌─────────────────────────────┐
   │      Parallel Processing    │
   │      Thousands of Cores     │
   │      Matrix-Like Operations │
   └─────────────────────────────┘
```

This led to an important question: If GPUs already have massive parallel processing power for graphics, could this same power be used for non-graphics computing as well?

This idea led to the GPGPU era.

**What is GPGPU?**

- GPGPU = General-Purpose computing on Graphics Processing Unit
- The use of the parallel processing power of GPUs for computing tasks other than graphics

### 10. Example Applications of GPGPU

Image Processing · Scientific Simulation · Data Analytics · Financial Modeling · Medical Imaging · AI Training · AI Inference · High-Performance Computing (HPC) · Video Processing · Molecular Simulation.

All of these workloads benefit from parallel processing.

### 11. The Challenge of GPU Programming

Although GPUs are extremely powerful for parallel computing, programming a GPU is not easy because the GPU hardware is difficult to program directly.

This created the need for:

- A way for developers to use GPUs more easily
- GPU acceleration for AI, HPC, and data center applications
- A programming model that is familiar to CPU programmers

### 12. CUDA — Compute Unified Device Architecture

In 2006, NVIDIA introduced CUDA.

**CUDA = Compute Unified Device Architecture**

- CUDA is a software platform and programming model for GPU computing.
- Its purpose is to make the parallel processing power of GPUs easier to use for general-purpose computing.

**Before CUDA vs After CUDA**

```
CUDA မတိုင်ခင်          CUDA ပြီးနောက်
      │                       │
      ▼                       ▼
GPU Programming         For Developers
Very Difficult          Becomes Easier
```

### 13. Why Is CUDA Important?

CUDA abstracts GPU hardware complexity and provides a programming model for developers.

**Before CUDA** — GPU programming was low-level, vendor-specific, and difficult.
**After CUDA** — GPU programming became more structured and scalable, suitable for AI workloads.

CUDA provides: Developer Tools · Libraries · Runtime · Compiler Support · Ecosystem.

For these reasons, CUDA became the foundation of GPU computing.

### 14. The Evolution of GPU Computing

```
1999 ──► NVIDIA GeForce 256 ──► First GPU
2006 ──► CUDA ──► GPU General-Purpose Computing
2012 ──► AlexNet ──► GPU for Deep Learning
```

**GPU Evolution**

```
Gaming
  │
  ▼
Graphics
  │
  ▼
GPU
  │
  ▼
GPGPU
  │
  ▼
CUDA
  │
  ▼
Machine Learning
  │
  ▼
Deep Learning
  │
  ▼
Modern AI
```

### 15. AlexNet — The 2012 Deep Learning Breakthrough

In 2012, the deep learning model AlexNet won the ImageNet competition. This was an important turning point in AI history because AlexNet was trained using GPUs.

From that point onward:

```
     ┌──────────────────────────────┐
     │   GPU for AI Training        │
     │   GPU for Deep Learning      │
     │   GPU for Data Center        │
     │   GPU for Inference          │
     └──────────────────────────────┘
```

The role of GPUs in the AI industry became extremely important.

### 16. CPU vs GPU — Key Differences

| Feature | CPU | GPU |
|---|---|---|
| Purpose | General-Purpose Computing | Parallel Computing |
| Architecture | Complex Cores + Large Cache | Many Smaller Cores |
| Parallelism | Limited Parallelism | Massive Parallelism |
| Typical Use | OS, Applications, Databases | AI Training, Inference, HPC |
| Graphics Rendering | Moderate | Excellent |
| Matrix Operations | Slower | Very Fast |
| AI Acceleration | Less Suitable | Highly Suitable |

However, CPUs remain important in an AI-Centric Data Center for:

- Data Preprocessing
- Data Loading
- System Management
- Application Logic
- Storage and Network Management

### 17. CPU and GPU in an AI-Centric Data Center

```
          ┌─────────────────────┐
          │   AI-Centric Data   │
          │       Center        │
          └──────────┬──────────┘
                     │
         ┌───────────┴───────────┐
         │                       │
     ┌───┴────┐             ┌────┴────┐
     │  CPU   │             │   GPU   │
     │ System │             │ AI/HPC  │
     │ Control│             │Compute  │
     └────────┘             └─────────┘
```

- The CPU provides system-level control.
- The GPU provides AI and compute acceleration.

Therefore, CPU + GPU + Software Ecosystem together form the compute foundation of an AI-Centric Data Center.

### 18. AI-Centric Data Center — Compute Summary

- The CPU is primarily designed for general-purpose computing.
- The GPU is designed for parallel computing.
- Although the GPU began in gaming and graphics, it became essential for AI and data centers.
- CUDA made GPU programming much more accessible.
- AlexNet (2012) demonstrated the importance of GPUs for deep learning.

**Exam Memory Points**

```
GPU = Graphics Processing Unit
CUDA = Compute Unified Device Architecture
GPGPU = General-Purpose computing on Graphics Processing Unit
GeForce 256 = "World's First GPU" (1999)
CUDA = Introduced by NVIDIA (2006)
AlexNet = GPU Deep Learning Breakthrough (2012)
```
