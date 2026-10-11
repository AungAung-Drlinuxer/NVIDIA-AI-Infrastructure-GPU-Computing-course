# Module 03 — CPU vs GPU: Private Jet vs Commercial Flight

> Part I · AI Foundations and AI Infrastructure — Course Module 03 (CPU vs GPU and Parallel Computing) — topic notes

## မြန်မာဘာသာ version

### 1. အကြမ်းဖျဉ်း

AI-Centric Data Center မှာ အဓိက Compute Device နှစ်ခုက အတူတကွ အလုပ်လုပ်ပါတယ်—

- CPU — Central Processing Unit
- GPU — Graphics Processing Unit

နှစ်ခုလုံးက Data ကို Process လုပ်ပေမယ့် Architecture နဲ့ အားသာချက်တွေက မတူကြပါဘူး။
နားလည်လွယ်အောင် နှိုင်းယှဉ်ချက် နှစ်ခုနဲ့ ရှင်းပြပါမယ်—

- CPU → Private Jet
- GPU → Commercial Flight

### 2. CPU — Private Jet နှိုင်းယှဉ်ချက်

Private Jet က မြန်ပါတယ်၊ Powerful ဖြစ်ပါတယ်၊ ဒါပေမယ့် ခရီးသည် အနည်းငယ်ပဲ သယ်နိုင်ပါတယ်။

CPU ကလည်း ဒီလိုပါပဲ—

```
        CPU = Private Jet
               │
    ┌──────────┴──────────┐
    │                     │
 ခရီးသည်နည်း        အလွန်မြန်
    │                     │
 Task နည်း          Latency နည်း
    │                     │
 လိုက်လျောညီထွေ     Flexible
```

- ခရီးသည် အနည်းငယ်ပဲ သယ်နိုင် — Task နည်းနည်းပဲ လုပ်နိုင်
- နေရာတိုင်းကို သွားနိုင် — Flexible execution
- အလွန်တုံ့ပြန်မှုမြန် — Low latency
- တစ်နေရာချင်းစီ ဈေးကြီး — Powerful individual cores

### 3. GPU — Commercial Flight နှိုင်းယှဉ်ချက်

Commercial Flight က ခရီးသည် ရာပေါင်းများစွာကို တပြိုင်နက် သယ်ဆောင်ဖို့ ဒီဇိုင်းထုတ်ထားပါတယ်။

GPU ကလည်း ဒီလိုပါပဲ—

```
        GPU = Commercial Flight
               │
    ┌──────────┴──────────┐
    │                     │
ခရီးသည်များစွာ      အလွန်ထိရောက်
    │                     │
 Task များစွာ        Throughput ကို ဦးစားပေး
    │                     │
 Parallel           Scalable
```

- ခရီးသည် များစွာကို တပြိုင်နက် သယ်နိုင် — Parallel task များစွာ
- လူအုပ်ကြီးကို ရွှေ့ဖို့ optimize လုပ်ထား — High throughput
- ပြည့်နေရင် အထိရောက်ဆုံး — Parallel utilization
- တစ်နေရာချင်းစီက Private Jet လောက် မထူးခြား — Simpler cores

### 4. Computing မှာ "ခရီးသည်" ဆိုတာ ဘာကိုဆိုလိုတာလဲ?

ဒီနှိုင်းယှဉ်ချက်မှာ—

- ခရီးသည် → Task / Operation
- လေဆိပ် → Input Data
- ခရီးဆုံး → Output / Result

CPU က Task နည်းနည်းကို မြန်မြန် ကိုင်တွယ်ပါတယ်။
GPU က Operation ထောင်ပေါင်းများစွာ သို့မဟုတ် သန်းပေါင်းများစွာကို တပြိုင်နက် Process လုပ်ပါတယ်။

### 5. ရိုးရှင်းသော ဥပမာ

အလွန်ရှည်လျားသော ခြံစည်းရိုးတစ်ခုကို ဆေးသုတ်ရမယ်ဆိုပါစို့။

**ကျွမ်းကျင်သော ဆေးသမား တစ်ယောက်တည်း (CPU)**

```
ဆေးသမား 1 ──► ခြံစည်းရိုး တစ်ခုလုံးကို တစ်ယောက်တည်း ဆေးသုတ်

အလွန်ကျွမ်းကျင်
တစ်ကွက်ချင်းစီ အလွန်မြန်
ဒါပေမယ့် ဆေးသမား တစ်ယောက်တည်း
```

**ဆေးသမား များစွာ (GPU)**

```
ဆေးသမား 1  ──► အပိုင်း 1
ဆေးသမား 2  ──► အပိုင်း 2
ဆေးသမား 3  ──► အပိုင်း 3
ဆေးသမား 4  ──► အပိုင်း 4
    ...           ...
ဆေးသမား N  ──► အပိုင်း N

လုပ်သား များစွာ
တစ်ယောက်ချင်းစီ တစ်ကွက်စီ ဆေးသုတ်
ခြံစည်းရိုး တစ်ခုလုံး အတူပြီးသွား
```

ခြံစည်းရိုးဆေးသုတ်တာက တစ်လျှောက်လုံး အလုပ်တူတူ ထပ်ခါထပ်ခါ လုပ်ရတာ — သဘာဝအတိုင်း parallel ဖြစ်တဲ့ workload ပါ။ GPU က ဒီလိုအလုပ်မျိုးမှာ အကောင်းဆုံး ဖြစ်ပါတယ်။

### 6. CPU ရဲ့ အားသာချက် — Sequential နှင့် ရှုပ်ထွေးသော အလုပ်များ

CPU က အောက်ပါလိုအပ်တဲ့ အလုပ်တွေအတွက် optimize လုပ်ထားပါတယ်—

- ရှုပ်ထွေးသော ဆုံးဖြတ်ချက်ချခြင်း
- Branching logic
- Operating system task များ
- Application control flow
- Latency နည်းသော တစ်ခုချင်း operation များ

ဥပမာများ—

```
Operating System စီမံခန့်ခွဲမှု
Application Logic
Database Transaction
Control Flow / ဆုံးဖြတ်ချက်ချခြင်း
General-Purpose Computing
```

နှိုင်းယှဉ်ချက်အရ — private jet က လမ်းကြောင်း မြန်မြန်ပြောင်းနိုင်၊ လေဆိပ်ငယ်တွေဆင်းနိုင်၊ ရှုပ်ထွေးတဲ့ ခရီးစဉ်တွေကို လိုက်လျောနိုင်ပါတယ်။

### 7. GPU ရဲ့ အားသာချက် — Parallel အလုပ်များ

GPU က ဆင်တူတဲ့ operation များစွာ ပါဝင်တဲ့ အလုပ်တွေအတွက် optimize လုပ်ထားပါတယ်—

- Matrix multiplication
- Vector operation များ
- Image processing
- AI training
- AI inference
- Scientific simulation

```
   ┌────────────────────────────────┐
   │  Data အမျိုးမျိုးပေါ်မှာ      │
   │  operation တူတူ ထပ်ခါထပ်ခါ   │
   │                                │
   │  Matrix · Vector · Tensor      │
   │  Pixel · Neuron · Simulation   │
   └────────────────────────────────┘
```

နှိုင်းယှဉ်ချက်အရ — commercial flight က ခရီးသည် ရာပေါင်းများစွာကို လမ်းကြောင်းတစ်ခုတည်းပေါ်မှာ တပြိုင်နက် ပို့ဆောင်ပါတယ်။

### 8. CPU က ဘယ်အခါ ပိုကောင်းသလဲ?

Workload မှာ အောက်ပါတွေ ပါဝင်ရင် CPU ကို သုံးပါ—

- Sequential processing
- ရှုပ်ထွေးသော branching
- Operating system နှင့် application control
- Latency နည်းသော တစ်ခုချင်း operation များ
- General-purpose computing

ဥပမာ—

```
User Request
     │
     ▼
Application Logic
     │
     ▼
Database Query
     │
     ▼
Response
```

ဒီလို workflow က တစ်ဆင့်ချင်းစီ ဆုံးဖြတ်ချက်ချရတာကို မူတည်ပါတယ် — CPU ရဲ့ အားသာချက်ပါ။

### 9. GPU က ဘယ်အခါ ပိုကောင်းသလဲ?

Workload မှာ အောက်ပါတွေ ပါဝင်ရင် GPU ကို သုံးပါ—

- Data အများကြီး
- ထပ်ခါထပ်ခါ လုပ်ရတဲ့ operation များ
- Matrix / vector computation
- Parallel processing
- AI workload များ

ဥပမာ—

```
Dataset ကြီး
     │
     ▼
ဆင်တူသော Operation ထောင်ပေါင်းများစွာ
     │
     ▼
GPU က Parallel Process လုပ်
     │
     ▼
Result
```

### 10. CPU vs GPU — Parallel Processing

**CPU**

```
Task 1 ──► Core 1 ──────────────────► Result 1
Task 2 ──► Core 2 ──────────────────► Result 2
Task 3 ──► Core 3 ──────────────────► Result 3

Core နည်းနည်း ဒါပေမယ့် Powerful
တစ်ခုချင်းစီက Task ရှုပ်ထွေးတစ်ခုစီကို ကိုင်တွယ်
```

**GPU**

```
Task 1  ──► Core 1   ──► R1
Task 2  ──► Core 2   ──► R2
Task 3  ──► Core 3   ──► R3
...
Task N  ──► Core N   ──► RN

Core များစွာ (ရိုးရှင်း)
အားလုံး တပြိုင်နက် လုပ်ဆောင်
```

### 11. AI အတွက် GPU ဘာကြောင့် လိုအပ်ရတာလဲ?

AI workload တွေမှာ သင်္ချာ operation အများကြီး ပါဝင်ပါတယ်—

```
AI Model Training
      │
      ▼
Matrix Multiplication
      │
      ▼
Operation သန်းပေါင်း / ဘီလီယံပေါင်းများစွာ
      │
      ▼
Parallel Processing လိုအပ်
      │
      ▼
GPU Acceleration
```

ဒါကြောင့် GPU တွေက modern AI ရဲ့ အဓိက compute engine တွေ ဖြစ်လာပါတယ်။

### 12. CPU နှင့် GPU အတူတကွ အလုပ်လုပ်ခြင်း

AI-Centric Data Center မှာ CPU နဲ့ GPU က ပြိုင်ဘက်မဟုတ်ဘဲ ပူးပေါင်းဆောင်ရွက်ပါတယ်—

```
             AI Application
                   │
         ┌─────────┴─────────┐
         │                   │
     CPU Role            GPU Role
         │                   │
  Application Logic    Parallel Computing
  Data Preparation     AI Training
  System Management    AI Inference
  Control Flow         Matrix / Tensor Operations
```

CPU က ဘာလုပ်ရမလဲ ဆုံးဖြတ်ပါတယ်။ GPU က လေးလံတဲ့ parallel computation ကို ဆောင်ရွက်ပါတယ်။

### 13. အဓိက နှိုင်းယှဉ်ချက်

| Aspect | CPU (Private Jet) | GPU (Commercial Flight) |
|---|---|---|
| Design | Core နည်းနည်း Powerful | Core များစွာ Parallel |
| အကောင်းဆုံး | ရှုပ်ထွေးသော sequential task | ရိုးရှင်းသော parallel task |
| Latency | Task တစ်ခုချင်း အလွန်နည်း | Task တစ်ခုချင်း ပိုမြင့်၊ throughput ပိုမြင့် |
| Throughput | နိမ့် | အလွန်မြင့် |
| Flexibility | အလွန်မြင့် | Parallel workload ပေါ် အာရုံစိုက် |
| AI Training | သိပ်မသင့် | အလွန်သင့်တော် |
| AI Inference | လုပ်နိုင်ပေမယ့် ပိုနှေး | အလွန်သင့်တော် |
| နှိုင်းယှဉ်ချက် | Private Jet | Commercial Flight |

### 14. Exam အတွက် မှတ်သားရန် အချက်များ

```
CPU = Core နည်းနည်း Powerful = Sequential / ရှုပ်ထွေးသောအလုပ် = Private Jet
GPU = Core များစွာ Parallel = ထပ်ခါထပ်ခါ parallel အလုပ် = Commercial Flight
```

တစ်ကြောင်းတည်း အကျဉ်းချုပ် — CPU က ခရီးသည် အနည်းငယ်အတွက် မြန်ပြီး လိုက်လျောနိုင်တဲ့ private jet၊ GPU က ခရီးသည် များစွာကို တပြိုင်နက် ထိရောက်စွာ ပို့ဆောင်နိုင်တဲ့ commercial flight ဖြစ်ပါတယ်။

### Self-Check Questions (အဖြေများပါဝင်သည်)

**Q1. CPU နဲ့ GPU အတွက် ဘယ်နှိုင်းယှဉ်ချက် နှစ်ခုကို သုံးထားသလဲ?**
Answer: CPU → Private Jet (ခရီးသည်နည်း၊ မြန်၊ flexible၊ low latency)၊ GPU → Commercial Flight (ခရီးသည်များစွာ၊ high throughput၊ ပြည့်မှ အထိရောက်ဆုံး) ဖြစ်ပါတယ်။

**Q2. ဒီနှိုင်းယှဉ်ချက်မှာ "ခရီးသည်"၊ "လေဆိပ်"၊ "ခရီးဆုံး" တွေက ဘာတွေကို ကိုယ်စားပြုသလဲ?**
Answer: ခရီးသည် → Task / Operation၊ လေဆိပ် → Input Data၊ ခရီးဆုံး → Output / Result ဖြစ်ပါတယ်။

**Q3. CPU က ဘယ်အခါ ပိုကောင်းသလဲ?**
Answer: Sequential processing၊ ရှုပ်ထွေးသော branching၊ operating system နှင့် application control၊ latency နည်းသော တစ်ခုချင်း operation များ၊ general-purpose computing ပါဝင်တဲ့ workload တွေမှာ ပိုကောင်းပါတယ်။

**Q4. GPU က ဘယ်အခါ ပိုကောင်းသလဲ?**
Answer: Data အများကြီး၊ ထပ်ခါထပ်ခါ ဆင်တူလုပ်ရတဲ့ operation များ၊ matrix / vector computation၊ parallel processing၊ AI workload များ ပါဝင်တဲ့အခါ ပိုကောင်းပါတယ်။

**Q5. AI-Centric Data Center မှာ CPU နဲ့ GPU က ဘယ်လို အတူတကွ အလုပ်လုပ်သလဲ?**
Answer: CPU က application logic, data preparation, system management, control flow တွေကို ကိုင်တွယ်ပြီး GPU က parallel computing, AI training, AI inference, matrix / tensor operations တွေကို ဆောင်ရွက်ပါတယ်။ CPU က ဘာလုပ်ရမလဲ ဆုံးဖြတ်ပြီး GPU က လေးလံတဲ့ parallel computation ကို လုပ်ပါတယ်။

## English Version

### 1. Introduction

In an AI-Centric Data Center, two major compute devices work together:

- CPU — Central Processing Unit
- GPU — Graphics Processing Unit

Both process data, but their architecture and strengths are fundamentally different.

To make this easier to understand, we use two simple analogies:

- CPU → Private Jet
- GPU → Commercial Flight

### 2. CPU — The Private Jet Analogy

A private jet is fast, powerful, and designed for a small number of passengers.

A CPU is similar:

```
        CPU = Private Jet
               │
    ┌──────────┴──────────┐
    │                     │
 Few Passengers        Very Fast
    │                     │
 Few Tasks             Low Latency
    │                     │
 Specialized            Flexible
```

- It can carry only a few passengers — fewer tasks
- It can go almost anywhere — flexible execution
- It is highly responsive — low latency
- Each seat is expensive — powerful individual cores

### 3. GPU — The Commercial Flight Analogy

A commercial flight is designed to carry hundreds of passengers at once.

A GPU is similar:

```
        GPU = Commercial Flight
               │
    ┌──────────┴──────────┐
    │                     │
Many Passengers        Highly Efficient
    │                     │
 Many Tasks           Throughput Focused
    │                     │
 Parallel              Scalable
```

- It carries many passengers simultaneously — many parallel tasks
- It is optimized for moving large groups — high throughput
- It is most efficient when fully loaded — parallel utilization
- Individual seats are less specialized than a private jet — simpler cores

### 4. What Does "Passenger" Mean in Computing?

In this analogy:

- Passenger → Task / Operation
- Airport → Input Data
- Destination → Output / Result

A CPU handles a small number of complex tasks quickly. A GPU handles thousands or millions of simple operations simultaneously.

### 5. A Simple Example

Imagine you need to paint a very long fence.

**One expert painter (CPU)**

```
Painter 1 ──► Paint entire fence alone

Very skilled
Very fast per section
But only one painter
```

**Many painters (GPU)**

```
Painter 1  ──► Section 1
Painter 2  ──► Section 2
Painter 3  ──► Section 3
Painter 4  ──► Section 4
    ...          ...
Painter N  ──► Section N

Many workers
Each paints one section
The whole fence is finished together
```

Painting a fence requires the same repetitive work along its entire length — a naturally parallel workload. This is exactly the type of work a GPU excels at.

### 6. CPU Strengths — Sequential and Complex Work

A CPU is optimized for work that requires:

- Complex decision-making
- Branching logic
- Operating system tasks
- Application control flow
- Low-latency individual operations

Examples:

```
Operating System Management
Application Logic
Database Transactions
Control Flow / Decision Making
General-Purpose Computing
```

In the analogy: the private jet can change routes quickly, land at small airports, and adapt to complex itineraries.

### 7. GPU Strengths — Parallel Work

A GPU is optimized for work that contains many similar operations:

- Matrix multiplication
- Vector operations
- Image processing
- AI training
- AI inference
- Scientific simulation

```
   ┌────────────────────────────────┐
   │  Same operation repeated many  │
   │  times on different data       │
   │                                │
   │  Matrix · Vector · Tensor      │
   │  Pixel · Neuron · Simulation   │
   └────────────────────────────────┘
```

In the analogy: the commercial flight moves hundreds of passengers along the same route at once.

### 8. When Is the CPU Better?

Use a CPU when the workload involves:

- Sequential processing
- Complex branching
- Operating system and application control
- Low-latency single operations
- General-purpose computing

Example:

```
User Request
     │
     ▼
Application Logic
     │
     ▼
Database Query
     │
     ▼
Response
```

This kind of workflow depends on decision-making step by step — the CPU's strength.

### 9. When Is the GPU Better?

Use a GPU when the workload involves:

- Large amounts of data
- Repetitive operations
- Matrix / vector computation
- Parallel processing
- AI workloads

Example:

```
Large Dataset
     │
     ▼
Thousands of Similar Operations
     │
     ▼
GPU Processes in Parallel
     │
     ▼
Result
```

### 10. CPU vs GPU — Parallel Processing

**CPU**

```
Task 1 ──► Core 1 ──────────────────► Result 1
Task 2 ──► Core 2 ──────────────────► Result 2
Task 3 ──► Core 3 ──────────────────► Result 3

Few powerful cores
Each handles one complex task
```

**GPU**

```
Task 1  ──► Core 1   ──► R1
Task 2  ──► Core 2   ──► R2
Task 3  ──► Core 3   ──► R3
...
Task N  ──► Core N   ──► RN

Many simpler cores
All work simultaneously
```

### 11. Why AI Needs GPUs

AI workloads contain enormous numbers of mathematical operations:

```
AI Model Training
      │
      ▼
Matrix Multiplication
      │
      ▼
Millions / Billions of Operations
      │
      ▼
Parallel Processing Required
      │
      ▼
GPU Acceleration
```

This is why GPUs are the primary compute engines of modern AI.

### 12. CPU and GPU Work Together

In an AI-Centric Data Center, the CPU and GPU do not compete — they cooperate.

```
             AI Application
                   │
         ┌─────────┴─────────┐
         │                   │
     CPU Role            GPU Role
         │                   │
  Application Logic    Parallel Computing
  Data Preparation     AI Training
  System Management    AI Inference
  Control Flow         Matrix / Tensor Operations
```

The CPU decides what to do. The GPU performs the heavy parallel computation.

### 13. Key Comparison

| Aspect | CPU (Private Jet) | GPU (Commercial Flight) |
|---|---|---|
| Design | Few powerful cores | Many parallel cores |
| Best at | Complex sequential tasks | Simple parallel tasks |
| Latency | Very low per task | Higher per task, higher throughput |
| Throughput | Lower | Very high |
| Flexibility | Very high | Focused on parallel workloads |
| AI Training | Less suitable | Highly suitable |
| AI Inference | Possible but slower | Highly suitable |
| Analogy | Private Jet | Commercial Flight |

### 14. Exam Memory Points

```
CPU = Few powerful cores = Sequential / complex work = Private Jet
GPU = Many parallel cores = Repetitive parallel work = Commercial Flight
```

One-line summary: The CPU is a private jet — fast and flexible for a few passengers; the GPU is a commercial flight — efficient at moving many passengers at once.

### Self-Check Questions (with answers)

**Q1. Which two analogies are used for the CPU and the GPU?**
Answer: CPU → Private Jet (few passengers, very fast, flexible, low latency); GPU → Commercial Flight (many passengers, high throughput, most efficient when fully loaded).

**Q2. In this analogy, what do "passenger," "airport," and "destination" represent?**
Answer: Passenger → Task / Operation; Airport → Input Data; Destination → Output / Result.

**Q3. When is a CPU the better choice?**
Answer: When the workload involves sequential processing, complex branching, operating system and application control, low-latency single operations, or general-purpose computing.

**Q4. When is a GPU the better choice?**
Answer: When the workload involves large amounts of data, repetitive similar operations, matrix / vector computation, parallel processing, or AI workloads.

**Q5. How do the CPU and GPU work together in an AI-centric data center?**
Answer: The CPU handles application logic, data preparation, system management, and control flow; the GPU handles parallel computing, AI training, AI inference, and matrix / tensor operations. The CPU decides what to do; the GPU performs the heavy parallel computation.

---

## Technical review notes

- Corrections made: the file previously listed the English version before the Burmese version and labeled the Burmese block "မြန်မာဘာသာ — …" instead of the standard section names; the order and labels now follow the bilingual skeleton (Burmese section first, then English). Body content is unchanged in meaning, and the Burmese "Exam Memory Points" heading was rendered in Burmese.
- Corrections made: self-check questions with answers were added to both language sections (5 per side, drawn only from the material in this file), as required by WRITING_RULES.
- Needs verification: the comparison table's latency/throughput/flexibility rows are qualitative trade-off descriptions (private jet vs commercial flight), not benchmark figures — do not cite them as measured performance claims.
- The fence-painting and aviation comparisons are pedagogical analogies, not claims about actual hardware behavior; no technical specifications or benchmark numbers are asserted in this module.
