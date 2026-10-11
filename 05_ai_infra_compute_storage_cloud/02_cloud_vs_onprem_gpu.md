# Module 05 — Cloud vs On-Prem GPU Infrastructure

> Part I · AI Foundations and AI Infrastructure — Course Module 05 (AI Infrastructure: Compute, Storage and Cloud) — topic notes

## မြန်မာဘာသာ version

### 1. Cloud လား၊ On-Prem လား?

AI workload တွေကို infrastructure တည်ဆောက်တဲ့အခါ အမြဲတွေ့ရမယ့် မေးခွန်းတစ်ခုက—Application ကို On-Premises Data Center မှာ host လုပ်မလား? Cloud ကို အသုံးပြုမလား? ဖြစ်ပါတယ်။

အရေးကြီးဆုံးက—**Cloud က အမြဲပိုကောင်းတာမဟုတ်ပါဘူး။ On-Prem ကလည်း အမြဲပိုကောင်းတာမဟုတ်ပါဘူး။** ဘယ်ဟာကို ရွေးမလဲဆိုတာ Use Case / Business Requirement ပေါ်မှာ မူတည်ပါတယ်။

### 2. Cloud vs On-Prem — High-Level Idea

အလွယ်ဆုံးမှတ်မယ်ဆိုရင်—

```
Cloud
  ↓
Scale + Flexibility + Low Entry Cost

On-Prem
  ↓
Security + Sovereignty + Control
```

ဒါပေမယ့် ဒီဟာက absolute rule မဟုတ်ပါဘူး။ Cloud နဲ့ On-Prem နှစ်ခုလုံးမှာ security, cost, scalability, compliance စတာတွေကို design အလိုက် ရနိုင်ပါတယ်။

ဒီ module ရဲ့ source framing အရ exam အတွက် အဓိက contrast က—

- **Cloud** → Flexible scaling and low cost barrier to entry
- **On-Prem** → Security, sovereignty, and control

ဖြစ်ပါတယ်။

### 3. Cloud Advantage #1 — Low Cost Barrier to Entry

Cloud ရဲ့ အဓိကအားသာချက်တစ်ခုက—**Low cost barrier to entry** ဖြစ်ပါတယ်။

ဥပမာ AI model တစ်ခု train လုပ်ချင်တယ်ဆိုပါစို့။ On-Prem မှာဆိုရင်—

```
Buy GPU Servers
      ↓
Buy Network
      ↓
Buy Storage
      ↓
Build Data Center
      ↓
Power
      ↓
Cooling
      ↓
Monitoring
      ↓
Operations
```

အရင်ဆုံး infrastructure အတွက် ငွေအများကြီး ရင်းနှီးမြှုပ်နှံရနိုင်ပါတယ်။

Cloud မှာတော့—

```
Need GPU
   ↓
Provision Cloud GPU
   ↓
Run Training
   ↓
Pay for Usage
   ↓
Decommission
```

လိုမျိုးလုပ်နိုင်ပါတယ်။

### 4. Pay-As-You-Go

Cloud ရဲ့ အရေးကြီးတဲ့ concept တစ်ခုက—**Pay as you go** ဖြစ်ပါတယ်။ ဆိုလိုတာက infrastructure ကို အမြဲတမ်း ဝယ်ထားစရာမလိုဘဲ အသုံးပြုတဲ့ resource အပေါ် မူတည်ပြီး ပေးချေနိုင်တာပါ။ ဥပမာ—

```
GPU Instance
     │
     ▼
Model Training
     │
     ▼
Training Complete
     │
     ▼
Decommission
```

ဒီလိုလုပ်ခြင်းအားဖြင့် training မလုပ်တော့တဲ့အချိန်မှာ GPU resource ကို ဆက်လက်ပေးချေနေရတာကို ရှောင်နိုင်ပါတယ်။

### 5. Cloud မှာ Data Center ကို ကိုယ်တိုင်မတည်ဆောက်ရ

On-Prem AI infrastructure တည်ဆောက်မယ်ဆိုရင်—Data center space, Electricity, Cooling, Network, Storage, Compute, Hardware maintenance, Monitoring, Infrastructure operations စတာတွေကို ကိုယ်တိုင် စီမံရနိုင်ပါတယ်။

Cloud မှာတော့ ဒီ infrastructure foundation ကို cloud provider က ပံ့ပိုးပေးထားပြီး၊ ကိုယ်လိုအပ်တဲ့ compute resources ကို provision လုပ်ပြီး အသုံးပြုနိုင်ပါတယ်။

ဒါကြောင့်—**Cloud lowers the initial infrastructure barrier.**

### 6. On-Prem Advantage #1 — Security

On-Prem ရဲ့ အဓိကအားသာချက်တစ်ခုက—**Security and control** ဖြစ်ပါတယ်။

Sensitive data တွေကို ကိုယ့်ရဲ့ controlled environment အတွင်းမှာ ထားနိုင်ပါတယ်။

```
On-Prem Data Center
┌─────────────────────────────┐
│                             │
│  Sensitive Data             │
│  AI Models                  │
│  GPU Infrastructure         │
│  Internal Applications      │
│                             │
└─────────────────────────────┘
```

Data ကို ဘယ်သူ access လုပ်နိုင်မလဲ၊ ဘယ်လို security controls ထားမလဲဆိုတာကို ကိုယ်တိုင် စီမံနိုင်ပါတယ်။

### 7. On-Prem Advantage #2 — Data Sovereignty

နောက်ထပ်အရေးကြီးတဲ့ concept က—**Data Sovereignty** ဖြစ်ပါတယ်။

Data sovereignty ဆိုတာ data ကို သတ်မှတ်ထားတဲ့ location / jurisdiction အတွင်းမှာ ထိန်းချုပ်ထားရမယ့် requirement မျိုးကို ရည်ညွှန်းနိုင်ပါတယ်။

ဥပမာ—ဥပဒေအရ data ကို သတ်မှတ်ထားတဲ့နိုင်ငံ သို့မဟုတ် location အတွင်းမှာပဲ သိမ်းထားရမယ် ဆိုတဲ့ requirement ရှိရင် On-Prem infrastructure က သင့်တော်တဲ့ရွေးချယ်မှု ဖြစ်နိုင်ပါတယ်။

### 8. Cloud vs On-Prem — Cost

**Cloud** — Pay as you go ဆိုတဲ့ model ရှိတဲ့အတွက် upfront investment နည်းနိုင်ပါတယ်။

```
Cloud
↓
No large initial hardware purchase
↓
Pay for usage
```

**On-Prem** — On-Prem မှာတော့ AI data center တည်ဆောက်တဲ့အခါ Compute, Network, Storage, Security, Power, Cooling, Physical infrastructure စတာတွေအတွက် upfront investment မြင့်နိုင်ပါတယ်။

```
On-Prem
↓
Large Upfront Investment
↓
Hardware + Facility + Power + Cooling
```

### 9. Scalability

Cloud ရဲ့ အရေးကြီးတဲ့အားသာချက်က—**Flexible Scalability** ဖြစ်ပါတယ်။ လိုအပ်ချက်တိုးလာရင် resource ကို မြန်မြန်တိုးနိုင်ပါတယ်။

```
1 GPU Node
     ↓
10 GPU Nodes
     ↓
100 GPU Nodes
     ↓
1000 GPU Nodes
```

Cloud environment မှာ demand အလိုက် scale up / scale down လုပ်နိုင်တဲ့ flexibility ပိုရှိနိုင်ပါတယ်။

### 10. On-Prem Scalability

On-Prem မှာ scalability က ကိုယ်ပိုင် physical infrastructure ပေါ်မှာ မူတည်ပါတယ်။ ဥပမာ—

```
Existing Hardware
      +
Available Rack Space
      +
Power Capacity
      +
Cooling Capacity
      +
Network Capacity
```

ဒီ resources တွေက limitation ဖြစ်နိုင်ပါတယ်။

ဒါကြောင့်—**On-Prem scalability is constrained by the hardware, physical space, power, and facility capacity you have.**

### 11. Compliance and Control

နောက်ထပ် comparison axis တစ်ခုက—**Compliance and Control** ဖြစ်ပါတယ်။

Cloud မှာ compliance policy တွေ၊ standards တွေ၊ security controls တွေဟာ cloud provider ရဲ့ services နဲ့ policies တွေအပေါ် တစ်စိတ်တစ်ပိုင်း မူတည်နိုင်ပါတယ်။

On-Prem မှာတော့ organization က infrastructure ကို ကိုယ်တိုင်ထိန်းချုပ်နိုင်တဲ့အတွက်—Compliance policies, Data handling, Security controls, Local regulations, Internal standards တွေကို ကိုယ်တိုင် implement လုပ်နိုင်ပါတယ်။

### 12. Cloud vs On-Prem Comparison

| Aspect | Cloud | On-Prem |
|---|---|---|
| Initial Cost | Low barrier to entry | High upfront investment |
| Payment Model | Pay as you go | Hardware ownership / CapEx |
| Scaling | Very flexible | Limited by physical infrastructure |
| Security Control | Provider + customer shared responsibility | High direct control |
| Data Sovereignty | Depends on provider/location/configuration | Strong direct control |
| Compliance | Provider capabilities + customer requirements | Organization-controlled |
| Data Center | Provider operates infrastructure | Organization operates infrastructure |
| Power/Cooling | Provider-managed facility | Organization-managed |
| Hardware | Provision when needed | Must purchase/install |
| Dependency | Cloud provider | Less dependent on external cloud provider |
| Best Fit | Experimentation, burst workloads, flexible scaling | Sensitive data, strict control, sovereignty |

### 13. When Should You Choose On-Prem?

Source ရဲ့ logic အရ ဒီလို scenario တွေမှာ On-Prem က သင့်တော်နိုင်ပါတယ်။

**Scenario 1 — High Security Requirement**

```
Sensitive Data
      ↓
Need Strong Control
      ↓
On-Prem
```

**Scenario 2 — Data Sovereignty**

```
Legal / Regulatory Requirement
          ↓
Data Must Stay Locally
          ↓
On-Prem
```

**Scenario 3 — Strong Compliance Control** — Organization က compliance policies တွေကို infrastructure အဆင့်ထိ ကိုယ်တိုင် control လုပ်ချင်တယ်။

**Scenario 4 — Predictable Long-Term Workload** — Infrastructure ကို အမြဲတမ်း သုံးမယ်၊ workload က stable ဖြစ်တယ်ဆိုရင် ကိုယ်ပိုင် infrastructure တည်ဆောက်ထားခြင်းက use case အလိုက် သင့်တော်နိုင်ပါတယ်။

### 14. When Should You Choose Cloud?

**Scenario 1 — Experimentation** — AI/ML model တွေကို စမ်းသပ်နေတယ်။ Hardware အများကြီး မဝယ်ချင်သေးဘူး။

```
Experiment
    ↓
Cloud GPU
    ↓
Train
    ↓
Stop
```

**Scenario 2 — Temporary High Compute Demand** — အချိန်တိုအတွင်း GPU အများကြီး လိုတယ်။ Cloud က scale up လုပ်ဖို့ သင့်တော်နိုင်ပါတယ်။

**Scenario 3 — Flexible Scaling**

```
Low Demand
    ↓
Scale Down

High Demand
    ↓
Scale Up
```

**Scenario 4 — Low Upfront Investment** — အခုချိန်မှာ data center hardware အတွက် capital မရင်းနှီးချင်သေးဘူး။

### 15. Very Important — Cloud Training → On-Prem Production

Source မှာ exam အတွက် အရေးကြီးတဲ့ practical pattern တစ်ခုကို ပြထားပါတယ်။

```
              CLOUD
                │
                ▼
          GPU Training
                │
                ▼
          Trained Model
                │
                │
                ▼
             ON-PREM
                │
                ▼
         Production Inference
```

ဥပမာ—GPU အများကြီးလိုတဲ့ training workload ကို Cloud မှာ run မယ်။ Training ပြီးသွားရင်—Trained model ကို On-Prem environment ထဲ ပြန်ယူလာမယ်။ ပြီးတော့—Production environment မှာ On-Prem GPU server ပေါ်မှာ deploy လုပ်မယ်။ ဒီ architecture က ဖြစ်နိုင်ပါတယ်။

### 16. Retraining Workflow

နောက်ပိုင်းမှာ model ကို ပြန် train လုပ်ဖို့ လိုလာနိုင်ပါတယ်။ ဒီလို cycle တစ်ခု ဖြစ်နိုင်ပါတယ်—

```
CLOUD
             │
             ▼
        Train Model
             │
             ▼
      Trained Model
             │
             ▼
          ON-PREM
             │
             ▼
       Production
             │
             │
       Need Retraining
             │
             ▼
          CLOUD
             │
             ▼
        Retrain
             │
             ▼
      Updated Model
             │
             ▼
          ON-PREM
```

ဒါဟာ Hybrid Cloud / On-Prem architecture ရဲ့ practical example တစ်ခု ဖြစ်ပါတယ်။

### 17. Cloud + On-Prem Are Not Mutually Exclusive

အရေးကြီးဆုံး takeaway က—Cloud or On-Prem ဆိုပြီး တစ်ခုတည်းကို မဖြစ်မနေ ရွေးရမယ်လို့ မဆိုလိုပါဘူး။ နှစ်ခုကို workload အလိုက် တွဲသုံးနိုင်ပါတယ်။

```
             AI Platform
                 │
        ┌────────┴────────┐
        │                 │
      CLOUD             ON-PREM
        │                 │
   Training /         Production /
   Burst Compute     Sensitive Data
        │                 │
        └────────┬────────┘
                 │
          Hybrid Architecture
```

### 18. Tools for Cloud ↔ On-Prem Communication

Cloud provider environment နဲ့ On-Prem environment ကြား communication လုပ်နိုင်ဖို့ tools / connectivity technologies တွေ ရှိပါတယ်။

ဒါကြောင့်—Cloud မှာ workload run လုပ်ပြီး On-Prem မှာ production workload run လုပ်တဲ့ architecture ကို တည်ဆောက်နိုင်ပါတယ်။

Source မှာ specific tool names မဖော်ပြထားတဲ့အတွက် ဒီ module မှာ specific products တွေကို မထည့်ထားပါဘူး။

### 19. Exam Perspective

Exam မှာ—"When would you choose Cloud over On-Prem?" လို့ မေးလာရင်—

**Cloud** — Low cost barrier to entry, Pay as you go, Flexible scaling, Experimentation, Temporary / burst compute, Avoid large upfront infrastructure investment ဆိုတာတွေကို စဉ်းစားပါ။

Exam မှာ—"When would you choose On-Prem over Cloud?" လို့ မေးလာရင်—

**On-Prem** — Security, Data sovereignty, Compliance control, Local regulation, Sensitive data, Less dependency on external provider policies ဆိုတာတွေကို စဉ်းစားပါ။

### 20. Exam Shortcut

```
                 CLOUD
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Low Entry   Pay-Go     Scaling
       Cost


                ON-PREM
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
    Security   Sovereignty   Control
```

One-line memory:
- **Cloud = Flexibility + Scale + Pay-as-you-go**
- **On-Prem = Security + Sovereignty + Control**

### 21. Final Mental Model

မေးခွန်းကို—"Which one is better?" လို့ မစဉ်းစားပါနဲ့။ အစား—"Which one is better for this use case?" လို့ စဉ်းစားပါ။

```
                USE CASE
                    │
          ┌─────────┴─────────┐
          │                   │
     Need flexibility?    Need control?
          │                   │
          ▼                   ▼
        CLOUD              ON-PREM
          │                   │
      Scale easily        Secure/control
      Pay for usage       Data sovereignty
      Low entry cost      Compliance
```

ဒါပေမယ့် real-world architecture မှာ—

```
Cloud Training
      ↓
Trained Model
      ↓
On-Prem Production
      ↓
Retraining when needed
      ↓
Cloud
```

လို Hybrid approach ကိုလည်း အသုံးပြုနိုင်ပါတယ်။

**Final Exam Answer** — Cloud is preferred when we need a low barrier to entry, pay-as-you-go economics, flexible scaling, experimentation, or temporary high compute capacity. On-Prem is preferred when we require strong control over sensitive data, data sovereignty, compliance, local regulations, and infrastructure. Neither is universally better. The correct choice depends on the workload, security requirements, compliance requirements, budget, scalability needs, and business use case.

### ကိုယ်တိုင်စစ်ဆေးရန် မေးခွန်းများ (Self-Check Questions)

1. **Cloud ကို ရွေးချယ်သင့်တဲ့ အဓိကအကြောင်းရင်း သုံးချက်က ဘာတွေလဲ?**
   **A:** Low cost barrier to entry, Pay-as-you-go economics, Flexible scaling (experimentation နဲ့ temporary / burst compute အပါအဝင်)။
2. **On-Prem ကို ရွေးချယ်သင့်တဲ့ အဓိကအကြောင်းရင်း သုံးချက်က ဘာတွေလဲ?**
   **A:** Security, Data sovereignty, Compliance / control (local regulation အပါအဝင်)။
3. **Data Sovereignty ဆိုတာ ဘာလဲ?**
   **A:** Data ကို သတ်မှတ်ထားတဲ့ location / jurisdiction အတွင်းမှာပဲ ထိန်းချုပ်သိမ်းဆည်းထားရမယ်ဆိုတဲ့ requirement ကို ရည်ညွှန်းပါတယ်။
4. **Cloud Training → On-Prem Production pattern က ဘယ်လိုအလုပ်လုပ်သလဲ?**
   **A:** Training workload ကို Cloud မှာ run ပြီး trained model ကို On-Prem ပြန်ယူကာ production GPU server ပေါ်မှာ deploy လုပ်ပါတယ်။ လိုအပ်ရင် model ကို Cloud မှာ ပြန် train လုပ်နိုင်ပါတယ်။
5. **Cloud နဲ့ On-Prem ထဲက တစ်ခုတည်းကိုပဲ ရွေးရမှာလား?**
   **A:** မဟုတ်ပါ။ Workload အလိုက် နှစ်ခုလုံးကို တွဲသုံးတဲ့ hybrid architecture တည်ဆောက်နိုင်ပါတယ်။

## English Version

### 1. Cloud or On-Prem?

When you build infrastructure for AI workloads, one question always comes up: should you host the application in an On-Premises Data Center, or should you use the Cloud?

The most important point is that **the Cloud is not always better, and On-Prem is not always better either.** Which one you choose depends on the Use Case / Business Requirement.

### 2. Cloud vs On-Prem — High-Level Idea

The easiest way to remember the difference is:

```
Cloud
  ↓
Scale + Flexibility + Low Entry Cost

On-Prem
  ↓
Security + Sovereignty + Control
```

However, this is not an absolute rule. With both Cloud and On-Prem you can get security, cost efficiency, scalability, and compliance depending on the design.

According to this module's source framing, the key contrast for the exam is:

- **Cloud** → Flexible scaling and low cost barrier to entry
- **On-Prem** → Security, sovereignty, and control

### 3. Cloud Advantage #1 — Low Cost Barrier to Entry

One of the main advantages of Cloud is the **low cost barrier to entry**.

Suppose you want to train an AI model. With On-Prem, you have to go through all of this first:

```
Buy GPU Servers
      ↓
Buy Network
      ↓
Buy Storage
      ↓
Build Data Center
      ↓
Power
      ↓
Cooling
      ↓
Monitoring
      ↓
Operations
```

You must invest a large amount of money in infrastructure upfront.

With Cloud, you can instead do something like this:

```
Need GPU
   ↓
Provision Cloud GPU
   ↓
Run Training
   ↓
Pay for Usage
   ↓
Decommission
```

### 4. Pay-As-You-Go

An important Cloud concept is **pay-as-you-go**. It means you never have to buy infrastructure outright — you pay based on the resources you actually use. For example:

```
GPU Instance
     │
     ▼
Model Training
     │
     ▼
Training Complete
     │
     ▼
Decommission
```

This way, you avoid continuing to pay for GPU resources at times when no training is running.

### 5. You Do Not Build the Data Center Yourself in the Cloud

Building on-prem AI infrastructure means you have to manage data center space, electricity, cooling, network, storage, compute, hardware maintenance, monitoring, and infrastructure operations yourself.

With Cloud, the cloud provider supplies this infrastructure foundation, and you simply provision and use the compute resources you need.

That is why **cloud lowers the initial infrastructure barrier.**

### 6. On-Prem Advantage #1 — Security

One of the main advantages of On-Prem is **security and control**.

You can keep sensitive data inside your own controlled environment.

```
On-Prem Data Center
┌─────────────────────────────┐
│                             │
│  Sensitive Data             │
│  AI Models                  │
│  GPU Infrastructure         │
│  Internal Applications      │
│                             │
└─────────────────────────────┘
```

You decide for yourself who can access the data and what security controls to put in place.

### 7. On-Prem Advantage #2 — Data Sovereignty

Another important concept is **data sovereignty**.

Data sovereignty refers to the requirement that data must be kept under control within a specified location / jurisdiction.

For example, if the law requires data to be stored only within a certain country or location, on-prem infrastructure can be the appropriate choice.

### 8. Cloud vs On-Prem — Cost

**Cloud** — Because of the pay-as-you-go model, upfront investment can be low.

```
Cloud
↓
No large initial hardware purchase
↓
Pay for usage
```

**On-Prem** — Building an AI data center can require a high upfront investment in compute, network, storage, security, power, cooling, and physical infrastructure.

```
On-Prem
↓
Large Upfront Investment
↓
Hardware + Facility + Power + Cooling
```

### 9. Scalability

An important Cloud advantage is **flexible scalability**. When demand grows, you can add resources quickly.

```
1 GPU Node
     ↓
10 GPU Nodes
     ↓
100 GPU Nodes
     ↓
1000 GPU Nodes
```

A Cloud environment gives you more flexibility to scale up and scale down according to demand.

### 10. On-Prem Scalability

With On-Prem, scalability depends on your own physical infrastructure. For example:

```
Existing Hardware
      +
Available Rack Space
      +
Power Capacity
      +
Cooling Capacity
      +
Network Capacity
```

These resources can become limitations.

That is why **on-prem scalability is constrained by the hardware, physical space, power, and facility capacity you have.**

### 11. Compliance and Control

Another comparison axis is **compliance and control**.

In the Cloud, compliance policies, standards, and security controls can depend in part on the cloud provider's services and policies.

With On-Prem, the organization controls the infrastructure directly, so it can implement compliance policies, data handling, security controls, local regulations, and internal standards itself.

### 12. Cloud vs On-Prem Comparison

| Aspect | Cloud | On-Prem |
|---|---|---|
| Initial Cost | Low barrier to entry | High upfront investment |
| Payment Model | Pay as you go | Hardware ownership / CapEx |
| Scaling | Very flexible | Limited by physical infrastructure |
| Security Control | Provider + customer shared responsibility | High direct control |
| Data Sovereignty | Depends on provider/location/configuration | Strong direct control |
| Compliance | Provider capabilities + customer requirements | Organization-controlled |
| Data Center | Provider operates infrastructure | Organization operates infrastructure |
| Power/Cooling | Provider-managed facility | Organization-managed |
| Hardware | Provision when needed | Must purchase/install |
| Dependency | Cloud provider | Less dependent on external cloud provider |
| Best Fit | Experimentation, burst workloads, flexible scaling | Sensitive data, strict control, sovereignty |

### 13. When Should You Choose On-Prem?

Following the source's logic, On-Prem can be a good fit in scenarios like these.

**Scenario 1 — High Security Requirement**

```
Sensitive Data
      ↓
Need Strong Control
      ↓
On-Prem
```

**Scenario 2 — Data Sovereignty**

```
Legal / Regulatory Requirement
          ↓
Data Must Stay Locally
          ↓
On-Prem
```

**Scenario 3 — Strong Compliance Control** — The organization wants to control compliance policies itself, right down to the infrastructure level.

**Scenario 4 — Predictable Long-Term Workload** — If the infrastructure will be used permanently and the workload is stable, building your own infrastructure can be the appropriate choice depending on the use case.

### 14. When Should You Choose Cloud?

**Scenario 1 — Experimentation** — You are experimenting with AI/ML models, and you do not want to buy a lot of hardware yet.

```
Experiment
    ↓
Cloud GPU
    ↓
Train
    ↓
Stop
```

**Scenario 2 — Temporary High Compute Demand** — You need a lot of GPUs for a short period. Cloud can be a good fit for scaling up.

**Scenario 3 — Flexible Scaling**

```
Low Demand
    ↓
Scale Down

High Demand
    ↓
Scale Up
```

**Scenario 4 — Low Upfront Investment** — Right now, you do not want to commit capital to data center hardware.

### 15. Very Important — Cloud Training → On-Prem Production

The source shows an important practical pattern for the exam.

```
              CLOUD
                │
                ▼
          GPU Training
                │
                ▼
          Trained Model
                │
                │
                ▼
             ON-PREM
                │
                ▼
         Production Inference
```

For example, run a training workload that needs many GPUs in the Cloud. When training is finished, bring the trained model back into the On-Prem environment. Then deploy it on on-prem GPU servers in the production environment. This architecture is perfectly valid.

### 16. Retraining Workflow

Later, the model may need to be retrained. A cycle like this is possible:

```
CLOUD
             │
             ▼
        Train Model
             │
             ▼
      Trained Model
             │
             ▼
          ON-PREM
             │
             ▼
       Production
             │
             │
       Need Retraining
             │
             ▼
          CLOUD
             │
             ▼
        Retrain
             │
             ▼
      Updated Model
             │
             ▼
          ON-PREM
```

This is a practical example of a hybrid Cloud / On-Prem architecture.

### 17. Cloud + On-Prem Are Not Mutually Exclusive

The most important takeaway: choosing Cloud or On-Prem does not mean you are forced to pick one and only one. You can combine both according to the workload.

```
             AI Platform
                 │
        ┌────────┴────────┐
        │                 │
      CLOUD             ON-PREM
        │                 │
   Training /         Production /
   Burst Compute     Sensitive Data
        │                 │
        └────────┬────────┘
                 │
          Hybrid Architecture
```

### 18. Tools for Cloud ↔ On-Prem Communication

Tools and connectivity technologies exist for communication between the cloud provider environment and the On-Prem environment.

That is what makes it possible to build an architecture that runs workloads in the Cloud while running production workloads On-Prem.

The source does not name specific tools, so this module does not list specific products.

### 19. Exam Perspective

If the exam asks, "When would you choose Cloud over On-Prem?" —

Think of **Cloud** — low cost barrier to entry, pay-as-you-go, flexible scaling, experimentation, temporary / burst compute, and avoiding large upfront infrastructure investment.

If the exam asks, "When would you choose On-Prem over Cloud?" —

Think of **On-Prem** — security, data sovereignty, compliance control, local regulation, sensitive data, and less dependency on external provider policies.

### 20. Exam Shortcut

```
                 CLOUD
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Low Entry   Pay-Go     Scaling
       Cost


                ON-PREM
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
    Security   Sovereignty   Control
```

One-line memory:
- **Cloud = Flexibility + Scale + Pay-as-you-go**
- **On-Prem = Security + Sovereignty + Control**

### 21. Final Mental Model

Do not think of the question as "Which one is better?" Instead, think "Which one is better for this use case?"

```
                USE CASE
                    │
          ┌─────────┴─────────┐
          │                   │
     Need flexibility?    Need control?
          │                   │
          ▼                   ▼
        CLOUD              ON-PREM
          │                   │
      Scale easily        Secure/control
      Pay for usage       Data sovereignty
      Low entry cost      Compliance
```

But in real-world architecture, you can also use a hybrid approach like this:

```
Cloud Training
      ↓
Trained Model
      ↓
On-Prem Production
      ↓
Retraining when needed
      ↓
Cloud
```

**Final Exam Answer** — Cloud is preferred when we need a low barrier to entry, pay-as-you-go economics, flexible scaling, experimentation, or temporary high compute capacity. On-Prem is preferred when we require strong control over sensitive data, data sovereignty, compliance, local regulations, and infrastructure. Neither is universally better. The correct choice depends on the workload, security requirements, compliance requirements, budget, scalability needs, and business use case.

### Self-check questions

1. **What are three main reasons to choose Cloud?**
   **A:** Low cost barrier to entry, pay-as-you-go economics, and flexible scaling (including experimentation and temporary / burst compute).
2. **What are three main reasons to choose On-Prem?**
   **A:** Security, data sovereignty, and compliance control (including local regulation).
3. **What is data sovereignty?**
   **A:** The requirement that data be kept under control within a specified location / jurisdiction.
4. **How does the Cloud training → On-Prem production pattern work?**
   **A:** Run the training workload in the Cloud, bring the trained model back to the On-Prem environment, and deploy it on on-prem GPU servers for production inference. Retraining can be done in the Cloud when needed.
5. **Must you choose only one of Cloud or On-Prem?**
   **A:** No — both can be combined per workload in a hybrid architecture.

## Technical review notes

- Corrections made: header and topic-note blockquote updated to the course bilingual convention; the existing Burmese and English bodies moved under `## မြန်မာဘာသာ version` and `## English Version`; numbered section headings normalized to `###` under each language section (numbering unchanged, 1–21 in both); the stray emoji variation selector (U+FE0F) that followed the `↔` in the section 18 heading removed in both language sections.
- Added five self-check questions with concise answers at the end of each language section.
- Claims needing verification: none of this module's claims involve commands, benchmarks, or product specifications. The comparison table states general trade-offs; provider-specific capabilities (for example sovereign-cloud offerings) vary by vendor and should be checked against the provider's documentation before being cited in an exam answer.
