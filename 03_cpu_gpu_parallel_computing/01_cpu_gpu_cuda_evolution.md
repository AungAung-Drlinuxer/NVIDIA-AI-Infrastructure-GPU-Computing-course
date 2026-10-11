# Module — AI-Centric Data Center Compute: CPU, GPU, CUDA and the Evolution of GPU Computing

> Part I · Course Module 03 (CPU vs GPU and Parallel Computing) — source module notes (Burmese + English)

## 1. AI-Centric Data Center မှာ Compute

AI-centric Data Center ရဲ့ components တွေကို လေ့လာတဲ့အခါ ပထမဆုံး Compute ကနေ စတင်လေ့လာနိုင်ပါတယ်။
နောက်ပိုင်းမှာ—

- Network
- Storage

တို့ကို ဆက်လက်လေ့လာနိုင်ပါတယ်။

**Compute ဆိုတာဘာလဲ?**

ရိုးရိုးရှင်းရှင်းပြောရရင်—

`Compute = Processing Power`

ဖြစ်ပါတယ်။
Data Center ထဲမှာ Compute က workload တွေကို process လုပ်ပေးတဲ့ processing resources တွေကို ဆိုလိုပါတယ်။

## 2. CPU — Traditional Compute

အများစုက Compute လို့ပြောရင် CPU ကို အရင်ဆုံးစဉ်းစားကြပါတယ်။
CPU ဆိုတာ—

`CPU = Central Processing Unit`

ဖြစ်ပါတယ်။
CPU ဟာ computer system ရဲ့ general-purpose processing အတွက် အဓိကအသုံးပြုတဲ့ processor ဖြစ်ပါတယ်။

```
              COMPUTE
                 │
                 ▼
                CPU
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
    Logic     Control    Processing
```

CPU က—

- Operating system
- Applications
- General-purpose computation
- System control
- Sequential / branching workloads

စတာတွေကို run လုပ်နိုင်ပါတယ်။
ဒါကြောင့် Traditional Data Center တွေမှာ CPU ဟာ အဓိက compute component တစ်ခုဖြစ်ပါတယ်။

## 3. Why Does AI Need GPU?

AI-centric Data Center ကို ပြောတဲ့အခါ CPU တစ်ခုတည်းနဲ့ မပြီးတော့ပါဘူး။
နောက်ထပ် အရေးကြီးတဲ့ compute component တစ်ခုက—

`GPU = Graphics Processing Unit`

ဖြစ်ပါတယ်။

```
             AI DATA CENTER COMPUTE
                       │
              ┌────────┴────────┐
              ▼                 ▼
             CPU               GPU
       General Compute    Parallel Compute
```

Modern AI workloads တွေမှာ GPU ဟာ အလွန်အရေးကြီးတဲ့ role တစ်ခု ရှိလာပါတယ်။

## 4. GPU ကို ဘာကြောင့် တီထွင်ခဲ့တာလဲ?

GPU ရဲ့ အစောပိုင်းရည်ရွယ်ချက်က AI မဟုတ်ပါဘူး။
GPU technology ရဲ့အစောပိုင်း development ဟာ—

**Computer Graphics and Video Games**

အတွက် ဖြစ်ပါတယ်။

Video game တွေမှာ—

- 3D environments
- Animation
- Images
- Lighting
- Visual effects

စတာတွေကို real-time နီးပါးနဲ့ render လုပ်ဖို့ processing power အများကြီးလိုပါတယ်။
ဒါကြောင့် graphics processing ကို အထူးပြုလုပ်ပေးနိုင်တဲ့ hardware တွေ တဖြည်းဖြည်း ဖွံ့ဖြိုးလာခဲ့ပါတယ်။

## 5. Early 3D Graphics — Quake and 3D Accelerators

Source material မှာ **Quake** ကို early 3D gaming example အဖြစ် ပြောထားပါတယ်။
1990s ကာလမှာ 3D games တွေဟာ ပိုပြီး realistic ဖြစ်လာပြီး 3D rendering workload လည်း ပိုမိုကြီးလာပါတယ်။

အဲဒီအချိန်မှာ—

**3dfx Voodoo**

လို graphics accelerator တွေကို PC ထဲမှာ ထည့်သွင်းပြီး 3D game performance ကို မြှင့်တင်နိုင်ခဲ့ပါတယ်။
Conceptually—

```
Before 3D Accelerator

CPU
 │
 └── Game + Graphics Processing
             │
             ▼
          Display


With Graphics Accelerator

CPU
 │
 └── Game Logic

GPU / Graphics Accelerator
 │
 └── 3D Graphics Rendering
             │
             ▼
          Display
```

ဒီကနေ specialized parallel processing ရဲ့ အရေးပါမှုကို မြင်နိုင်ပါတယ်။

## 6. GPU Evolution Through Gaming

Gaming industry တိုးတက်လာတာနဲ့အမျှ graphics processing requirement တွေလည်း တိုးလာပါတယ်။
ဥပမာ—

- Quake
- Unreal Tournament
- Quake III Arena
- Doom

လို games တွေမှာ ပိုပြီး realistic graphics နဲ့ real-time rendering လိုအပ်လာပါတယ်။
ဒီလို graphics workload တွေကို handle လုပ်ဖို့ GPU technology တွေကလည်း ပိုမိုတိုးတက်လာပါတယ်။

## 7. NVIDIA GeForce 256

1999 ခုနှစ်မှာ NVIDIA က—

**GeForce 256**

ကို ထုတ်လုပ်ခဲ့ပြီး NVIDIA က ၎င်းကို "the world's first GPU" လို့ market လုပ်ခဲ့ပါတယ်။

ဒီနေရာမှာ source transcript ရဲ့ အရေးကြီးတဲ့ concept က—

GPU ဆိုတဲ့ term ကို NVIDIA က popularize လုပ်ခဲ့ပြီး graphics processing ကို dedicated hardware နဲ့ ပိုမိုလုပ်ဆောင်နိုင်လာခြင်း

ဖြစ်ပါတယ်။

## 8. From Graphics to General-Purpose Computing

GPU development ရဲ့ အရေးကြီးဆုံး turning point တစ်ခုက—

GPU ကို Graphics အတွက်ပဲ မသုံးဘဲ General-Purpose Computing အတွက် အသုံးပြုနိုင်မလား?

ဆိုတဲ့ idea ဖြစ်ပါတယ်။
Researchers တွေက ဒီလိုစဉ်းစားလာကြပါတယ်—

- GPU ထဲမှာ အလွန်များတဲ့ processing capability ရှိနေတယ်။
- Graphics rendering အတွက်ပဲ သုံးရမလား?
- အခြား computational workloads တွေအတွက်ပါ အသုံးပြုလို့မရဘူးလား?

ဒီ concept ကို—

`GPGPU = General-Purpose computing on Graphics Processing Units`

လို့ ခေါ်ပါတယ်။

## 9. Ian Buck and Brook

Source transcript ထဲမှာ **"Ian Beck"** လို့ ပါလာပေမယ့် technical context အရ—

**Ian Buck**

ကို ရည်ညွှန်းတာဖြစ်နိုင်ပါတယ်။ (မူရင်းတွင် "Ian Beck" ဟု ပါရှိ — technical correction: Ian Buck)

Ian Buck ဟာ GPU ကို graphics အပြင် general-purpose computing အတွက် အသုံးပြုနိုင်ရေးမှာ အရေးကြီးတဲ့ researcher/developer တစ်ဦးဖြစ်ပါတယ်။
သူနဲ့ team က **Brook** ဆိုတဲ့ early GPU programming system/language ကို develop လုပ်ခဲ့ပါတယ်။
Brook ဟာ GPU ကို general-purpose computation အတွက် အသုံးပြုဖို့ စမ်းသပ်တဲ့ early approach တစ်ခုဖြစ်ပါတယ်။

အဓိက idea က—

```
Traditional

GPU → Graphics


GPGPU Idea

GPU → Graphics
     → Scientific Computing
     → Numerical Computing
     → General Computation
```

ဖြစ်ပါတယ်။

## 10. CUDA — Major Turning Point

2006 ခုနှစ်မှာ NVIDIA က—

`CUDA = Compute Unified Device Architecture`

ကို မိတ်ဆက်ခဲ့ပါတယ်။

CUDA က GPU ကို graphics အတွက်သာမက general-purpose computing အတွက် programmer တွေ အသုံးပြုနိုင်အောင် programming platform / ecosystem တစ်ခုကို ပေးခဲ့ပါတယ်။
ဒီဟာက GPU computing evolution မှာ အလွန်အရေးကြီးတဲ့ turning point ဖြစ်ပါတယ်။

```
Before CUDA

GPU
 │
 └── Mainly Graphics


With CUDA

GPU
 │
 ├── Graphics
 ├── Scientific Computing
 ├── Data Processing
 ├── Machine Learning
 └── AI Workloads
```

## 11. Why CUDA Was Important for AI

CUDA ရဲ့အရေးကြီးမှုက—

**Developers could use NVIDIA GPUs for general-purpose computation.**

ဆိုတာပါ။

AI နဲ့ Machine Learning workloads တွေမှာ matrix operations, vector operations, tensor operations စတဲ့ parallel mathematical computations အများကြီးလိုအပ်ပါတယ်။
GPU architecture က ဒီလို parallel computation တွေအတွက် သင့်တော်တာကြောင့် GPU ကို Machine Learning အတွက် အသုံးပြုနိုင်ဖို့ CUDA ecosystem က အရေးကြီးတဲ့ foundation ဖြစ်လာပါတယ်။

## 12. AlexNet — GPU + Deep Learning Breakthrough

GPU computing နဲ့ Machine Learning သမိုင်းမှာ အရေးကြီးတဲ့ breakthrough တစ်ခုက—

**AlexNet**

ဖြစ်ပါတယ်။

2012 ခုနှစ်မှာ AlexNet က large-scale image recognition competition ဖြစ်တဲ့ **ImageNet challenge** မှာ ထူးခြားတဲ့ performance ပြသခဲ့ပါတယ်။
အရေးကြီးတာက AlexNet ဟာ GPUs ကို အသုံးပြုပြီး Deep Neural Network ကို train လုပ်ခဲ့တာ ဖြစ်ပါတယ်။

```
Large Image Dataset
        │
        ▼
     AlexNet
        │
   GPU Training
        │
        ▼
Better Image Recognition
```

ဒါဟာ GPU တွေဟာ gaming နဲ့ graphics အတွက်ပဲ အသုံးဝင်တာမဟုတ်ဘဲ Deep Learning training အတွက်လည်း အလွန်အစွမ်းထက်တယ် ဆိုတာကို လက်တွေ့အနေနဲ့ ပြသခဲ့တဲ့ အရေးကြီးတဲ့ milestone တစ်ခုဖြစ်ပါတယ်။

## 13. GPU Evolution — Big Picture

GPU ရဲ့ evolution ကို ဒီလိုမှတ်နိုင်ပါတယ်—

```
Graphics Rendering
       │
       ▼
3D Gaming
       │
       ▼
Graphics Accelerators
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

ဒါဟာ AI-centric Data Center တွေမှာ GPU ဘာကြောင့် အရေးကြီးလာသလဲဆိုတာကို နားလည်ဖို့ အလွန်အရေးကြီးတဲ့ history ဖြစ်ပါတယ်။

## 14. CPU vs GPU — Basic Concept

CPU နဲ့ GPU ကို ရိုးရိုးရှင်းရှင်း ဒီလိုမှတ်နိုင်ပါတယ်။

| Feature | CPU | GPU |
|---|---|---|
| Full Name | Central Processing Unit | Graphics Processing Unit |
| Original Focus | General-purpose computing | Graphics processing |
| Architecture | Fewer powerful cores | Many parallel processing units |
| Strength | General-purpose / control / branching | Parallel computation |
| AI Role | General compute / orchestration | Accelerated AI computation |
| Modern Use | OS, applications, control | AI, graphics, HPC, parallel workloads |

> **သတိပြုရန်**: CPU vs GPU ကို "CPU = sequential, GPU = parallel" လို့ အလွန်အကျွံ ရိုးရှင်းအောင် မမှတ်သင့်ပါဘူး။ CPU တွေလည်း parallelism support လုပ်နိုင်ပြီး GPU တွေလည်း sequential/control operations လုပ်နိုင်ပါတယ်။ အဓိကက GPU architecture က highly parallel workloads အတွက် အထူးသင့်တော်တာ ဖြစ်ပါတယ်။

## 15. Why GPU Is Powerful for AI

AI training မှာ အောက်ပါ operations တွေ အများကြီးလုပ်ရပါတယ်—

- Matrix multiplication
- Vector operations
- Tensor operations
- Neural network computation
- Forward pass
- Backpropagation

ဒီ operations တွေမှာ parallel computation အများကြီး အသုံးပြုနိုင်ပါတယ်။
Conceptually—

```
AI Model
   │
   ▼
Large Mathematical Operations
   │
   ├── Matrix Operations
   ├── Vector Operations
   └── Tensor Operations
           │
           ▼
     Parallel Processing
           │
           ▼
           GPU
```

ဒါကြောင့် modern AI infrastructure မှာ GPU ဟာ အဓိက compute accelerator ဖြစ်လာပါတယ်။

## 16. The Key Story to Remember

ဒီ lesson ရဲ့ main story ကို—

**GPU was not originally created for AI.**

လို့ စမှတ်ပါ။
ပြီးရင်—

```
Gaming
   ↓
Graphics
   ↓
GPU
   ↓
General-Purpose GPU Computing
   ↓
CUDA
   ↓
Machine Learning
   ↓
AlexNet
   ↓
Deep Learning
   ↓
Modern AI
```

ဆိုတဲ့ evolution ကို မှတ်ထားပါ။

## Exam Preparation

**Q1. What is Compute?**
Answer: Compute refers to the processing resources or processing power used to execute workloads.

**Q2. What is CPU?**
CPU = Central Processing Unit. It is a general-purpose processor used to execute a wide variety of computing tasks.

**Q3. What is GPU?**
GPU = Graphics Processing Unit. It was originally developed for graphics processing and is highly suitable for massively parallel computation.

**Q4. Why are GPUs useful for AI?**
Because many AI and Deep Learning operations involve highly parallel mathematical computations such as matrix and tensor operations.

**Q5. What was the original purpose of GPUs?**
The original purpose of GPU technology was primarily graphics processing, especially 3D graphics and gaming.

**Q6. What is GPGPU?**
GPGPU = General-Purpose computing on Graphics Processing Units. It means using GPUs for general computational workloads beyond graphics.

**Q7. What is CUDA?**
CUDA = Compute Unified Device Architecture. It is NVIDIA's platform and programming ecosystem that enables developers to use NVIDIA GPUs for general-purpose computing.

**Q8. Why was CUDA important?**
CUDA made GPU computing much more accessible to developers beyond traditional graphics workloads and helped enable GPU acceleration for scientific computing, Machine Learning, and AI.

**Q9. What was AlexNet's significance?**
AlexNet demonstrated the effectiveness of GPU-accelerated Deep Learning by training a deep neural network on GPUs and achieving a major breakthrough in ImageNet image recognition in 2012.

**Exam Shortcut**

```
Compute
Compute = Processing Power

CPU
CPU → General-Purpose Computing

GPU
GPU → Highly Parallel Computation

CUDA
CUDA → Use NVIDIA GPUs for General-Purpose Computing

AlexNet
AlexNet → GPU-Accelerated Deep Learning Breakthrough
```

**One-Line Evolution**
Gaming → Graphics → GPU → GPGPU → CUDA → Machine Learning → AlexNet → Deep Learning → Modern AI

---

## English Version — AI-Centric Data Center Compute: CPU, GPU, CUDA and the Evolution of GPU Computing

### 1. Compute in an AI-Centric Data Center

When studying the components of an AI-centric data center, we can start with Compute. Later we can continue with Network and Storage.

What is Compute?

Simply put:

`Compute = Processing Power`

In a data center, Compute refers to the processing resources that process workloads.

### 2. CPU — Traditional Compute

When most people think of Compute, they first think of the CPU.

`CPU = Central Processing Unit`

The CPU is the main processor used for general-purpose processing in a computer system.

```
              COMPUTE
                 │
                 ▼
                CPU
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
    Logic     Control    Processing
```

A CPU can run:

- Operating systems
- Applications
- General-purpose computation
- System control
- Sequential / branching workloads

Therefore, in traditional data centers, the CPU is a primary compute component.

### 3. Why Does AI Need a GPU?

When discussing an AI-centric data center, the CPU alone is not enough. Another important compute component is:

`GPU = Graphics Processing Unit`

```
             AI DATA CENTER COMPUTE
                       │
              ┌────────┴────────┐
              ▼                 ▼
             CPU               GPU
       General Compute    Parallel Compute
```

In modern AI workloads, the GPU plays an extremely important role.

### 4. Why Was the GPU Invented?

The early purpose of the GPU was not AI. Early GPU technology development was for:

**Computer Graphics and Video Games**

Video games require large amounts of processing power to render, nearly in real time:

- 3D environments
- Animation
- Images
- Lighting
- Visual effects

Therefore, hardware specialized for graphics processing gradually developed.

### 5. Early 3D Graphics — Quake and 3D Accelerators

The source material mentions **Quake** as an early 3D gaming example. In the 1990s, 3D games became more realistic and 3D rendering workloads grew larger.

At that time, graphics accelerators such as the **3dfx Voodoo** were installed in PCs to improve 3D game performance. Conceptually:

```
Before 3D Accelerator

CPU
 │
 └── Game + Graphics Processing
             │
             ▼
          Display


With Graphics Accelerator

CPU
 │
 └── Game Logic

GPU / Graphics Accelerator
 │
 └── 3D Graphics Rendering
             │
             ▼
          Display
```

From this, we can see the importance of specialized parallel processing.

### 6. GPU Evolution Through Gaming

As the gaming industry advanced, graphics processing requirements also increased. For example, games such as:

- Quake
- Unreal Tournament
- Quake III Arena
- Doom

required more realistic graphics and real-time rendering. To handle these graphics workloads, GPU technology also improved.

### 7. NVIDIA GeForce 256

In 1999, NVIDIA released the **GeForce 256** and marketed it as "the world's first GPU."

The important concept from the source transcript is:

NVIDIA popularized the term GPU and enabled graphics processing to be performed with dedicated hardware.

### 8. From Graphics to General-Purpose Computing

One of the most important turning points in GPU development was the idea:

What if GPUs could be used not just for graphics, but for General-Purpose Computing?

Researchers began thinking:

- GPUs contain enormous processing capability.
- Should it only be used for graphics rendering?
- Can it not be used for other computational workloads as well?

This concept is called:

`GPGPU = General-Purpose computing on Graphics Processing Units`

### 9. Ian Buck and Brook

The transcript says "Ian Beck," but the technical context points to **Ian Buck**.

Ian Buck was an important researcher in the development of GPU general-purpose computing. He and his collaborators developed **Brook**, an early system/programming approach for using GPUs for general-purpose computation.

The important idea was:

```
Traditional

GPU → Graphics


GPGPU Idea

GPU → Graphics
     → Scientific Computing
     → Numerical Computing
     → General Computation
```

This was an important conceptual step toward modern GPU computing.

### 10. CUDA — A Major Turning Point

In 2006, NVIDIA introduced:

`CUDA = Compute Unified Device Architecture`

CUDA provided a platform and programming ecosystem that allowed developers to use NVIDIA GPUs for general-purpose computing. This was a major turning point in GPU computing.

```
Before CUDA

GPU
 │
 └── Mainly Graphics


With CUDA

GPU
 │
 ├── Graphics
 ├── Scientific Computing
 ├── Data Processing
 ├── Machine Learning
 └── AI Workloads
```

### 11. Why Was CUDA Important for AI?

CUDA made it much easier for developers to use NVIDIA GPUs for workloads beyond graphics.

AI and Machine Learning involve many mathematical operations, including:

- Matrix multiplication
- Vector operations
- Tensor operations
- Neural network computation

These operations can contain large amounts of parallelism. Therefore, GPU acceleration became highly useful for Machine Learning and Deep Learning.

### 12. AlexNet — GPU and Deep Learning Breakthrough

A major milestone in the history of GPU computing and Machine Learning was:

**AlexNet**

In 2012, AlexNet achieved a major breakthrough in image recognition on the ImageNet challenge. An important aspect was that AlexNet used GPUs to train a Deep Neural Network.

```
Large Image Dataset
        │
        ▼
     AlexNet
        │
   GPU Training
        │
        ▼
Better Image Recognition
```

This demonstrated in practice that GPUs could be extremely effective for Deep Learning workloads. It helped accelerate the adoption of GPUs as important computing platforms for Machine Learning and Deep Learning.

### 13. GPU Evolution — Big Picture

The overall evolution can be remembered as:

```
Graphics Rendering
       │
       ▼
3D Gaming
       │
       ▼
Graphics Accelerators
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

This evolution helps explain why GPUs have become fundamental components of modern AI-centric data centers.

### 14. CPU vs GPU — Basic Concept

| Feature | CPU | GPU |
|---|---|---|
| Full Name | Central Processing Unit | Graphics Processing Unit |
| Original Focus | General-purpose computing | Graphics processing |
| Architecture | Fewer powerful cores | Many parallel processing units |
| Strength | General-purpose, control, branching | Parallel computation |
| AI Role | General compute / orchestration | Accelerated AI computation |
| Modern Use | OS, applications, control | AI, graphics, HPC, parallel workloads |

**Important:** It is an oversimplification to say "CPU = sequential" and "GPU = parallel." CPUs also support parallelism, and GPUs can perform sequential/control operations. The key distinction is that GPU architectures are particularly well suited to highly parallel workloads.

### 15. Why Are GPUs Powerful for AI?

AI training involves many mathematical operations, including:

- Matrix multiplication
- Vector operations
- Tensor operations
- Neural network computation
- Forward propagation
- Backpropagation

Many of these operations can be performed in parallel.

```
AI Model
   │
   ▼
Large Mathematical Operations
   │
   ├── Matrix Operations
   ├── Vector Operations
   └── Tensor Operations
           │
           ▼
     Parallel Processing
           │
           ▼
           GPU
```

This is one of the fundamental reasons GPUs became important for modern AI infrastructure.

### 16. The Key Story to Remember

The most important point is:

**GPU technology was not originally created for AI.**

The evolution can be summarized as:

```
Gaming
   ↓
Graphics
   ↓
GPU
   ↓
General-Purpose GPU Computing
   ↓
CUDA
   ↓
Machine Learning
   ↓
AlexNet
   ↓
Deep Learning
   ↓
Modern AI
```

This history explains how a processor originally designed for graphics became one of the most important components of modern AI-centric data centers.

### Exam Preparation

**Q1. What is Compute?**
Compute refers to the processing resources or processing power used to execute workloads.

**Q2. What is a CPU?**
CPU = Central Processing Unit. It is a general-purpose processor designed to execute a wide range of computing tasks.

**Q3. What is a GPU?**
GPU = Graphics Processing Unit. It was originally developed for graphics processing and is highly suitable for massively parallel computation.

**Q4. Why are GPUs useful for AI?**
Because many AI and Deep Learning workloads involve highly parallel mathematical operations, including matrix and tensor operations.

**Q5. What was the original purpose of GPUs?**
The original purpose of GPU technology was primarily graphics processing, especially 3D graphics and gaming.

**Q6. What is GPGPU?**
GPGPU = General-Purpose computing on Graphics Processing Units. It refers to using GPUs for general computational workloads beyond graphics.

**Q7. What is CUDA?**
CUDA = Compute Unified Device Architecture. It is NVIDIA's platform and programming ecosystem for using NVIDIA GPUs for general-purpose computing.

**Q8. Why was CUDA important?**
CUDA made GPU computing accessible to a broader range of developers and enabled GPU acceleration for scientific computing, Machine Learning, and AI workloads.

**Q9. What was the significance of AlexNet?**
AlexNet demonstrated the effectiveness of GPU-accelerated Deep Learning by training a deep neural network on GPUs and achieving a major breakthrough in ImageNet image recognition in 2012.

**Exam Shortcut**

```
Compute
Compute → Processing Power

CPU
CPU → General-Purpose Computing

GPU
GPU → Highly Parallel Computation

CUDA
CUDA → NVIDIA GPU General-Purpose Computing

AlexNet
AlexNet → GPU-Accelerated Deep Learning Breakthrough
```

**One-Line Evolution**
Gaming → Graphics → GPU → GPGPU → CUDA → Machine Learning → AlexNet → Deep Learning → Modern AI

---

## Technical review notes

- Source transcript says "Ian Beck" — technical context points to **Ian Buck** (Brook GPU programming language, Stanford). Correction noted inline in both versions, မူရင်း wording ကိုလည်း ဖော်ပြထားသည်။
- 3dfx Voodoo, Quake, Unreal Tournament, Quake III Arena, Doom — source ၏ examples အတိုင်း ထားသည်။
- AlexNet 2012, CUDA 2006, GeForce 256 1999 — source dates အတိုင်း။
