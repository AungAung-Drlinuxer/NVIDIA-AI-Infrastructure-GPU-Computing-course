# NVIDIA AI Infrastructure & GPU Computing — Complete Learning Roadmap

**Course scope** — AI foundations, GPU computing, NVIDIA data center infrastructure,
networking, GPU virtualization, CUDA, AI software stack, infrastructure management,
AI workload operations and MLOps.

**Target audience** — IT infrastructure engineers, Cloud/DevOps engineers,
AI infrastructure engineers, GPU administrators and learners preparing for
NVIDIA AI infrastructure concepts and related certification topics.

---

## Course structure

- **8 Parts · 41 Modules + Final Capstone**
- အသေးစိတ် outline ကို `COURSE_OUTLINE.md` တွင် ကြည့်ပါ

| Part | အကြောင်းအရာ | Modules |
|---|---|---|
| I | AI Foundations and AI Infrastructure | 01–06 |
| II | GPU Interconnects, Networking and Data Movement | 07–14 |
| III | GPU Drivers, Virtualization and CUDA | 15–18 |
| IV | CUDA, Multi-GPU Communication and NVIDIA Software Stack | 19–23 |
| V | NVIDIA Monitoring, Cluster Management and Tool Selection | 24–28 |
| VI | NVIDIA AI Software, Workloads and Enterprise Platform | 29–35 |
| VII | AI Workload Operations and Orchestration | 36–38 |
| VIII | MLOps and End-to-End Production AI | 39–41 |

## Module file structure (ဖြည့်စွက်ပြီးနောက်)

module တစ်ခုချင်းအတွက် —

```
NN_module-name/
├── explanation.md   ← သင်ခန်းစာ အဓိက အကြောင်းအရာ
├── exercise.md      ← လက်တွေ့ လေ့ကျင့်ခန်း
└── solution.md      ← အဖြေနှင့် ရှင်းလင်းချက်
```

## Suggested Study Method

Module တစ်ခုချင်းစီကို အောက်ပါအစီအစဉ်အတိုင်း လေ့လာပါ —

1. **Concept** — အဓိကအယူအဆကို နားလည်ပါ
2. **Architecture** — Components တွေ ဘယ်လိုချိတ်ဆက်နေသလဲ လေ့လာပါ
3. **Tools** — သက်ဆိုင်ရာ NVIDIA သို့မဟုတ် ecosystem tools တွေရဲ့ အခန်းကဏ္ဍကို ခွဲခြားပါ
4. **Hands-on** — ရရှိနိုင်တဲ့ lab environment မှာ သင့်တော်တဲ့ commands သို့မဟုတ် workflow တွေကို စမ်းသပ်ပါ
5. **Troubleshooting** — Failure ဖြစ်တဲ့အခါ ဘယ် layer ကို စစ်ဆေးရမလဲ သင်ယူပါ
6. **Review** — ကိုယ့်စကားနဲ့ ရှင်းပြပြီး scenario-based questions တွေကို ဖြေကြည့်ပါ

⚠️ Product-specific commands, profile limits, supported GPU models နဲ့ licensing details တွေကိုတော့ သက်ဆိုင်ရာ official documentation နဲ့ စစ်ဆေးပြီးမှ အသုံးပြုပါ။ Software version အလိုက် feature နဲ့ configuration ကွာခြားနိုင်ပါတယ်။

## Writing rules

chapter များ ရေးသား/ပြင်ဆင်ရာတွင် `WRITING_RULES.md` ကို လိုက်နာပါ —
natural Burmese + English technical terms၊ problem → concept → comparison →
configuration → cautions → self-check progression၊ version-dependent claims
များ flag ပြုလုပ်ခြင်း၊ chapter အဆုံး "Technical review notes" ပါဝင်ခြင်း။
