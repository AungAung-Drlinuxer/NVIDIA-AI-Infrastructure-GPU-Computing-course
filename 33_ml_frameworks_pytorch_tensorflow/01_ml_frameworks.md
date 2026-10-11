# Module 33 — Machine Learning Frameworks: PyTorch, TensorFlow, Scikit-learn and XGBoost

> Part VI · Course Module 33 — topic notes

## မြန်မာဘာသာ version

## 1. Learning Objectives

ဤ Module ကို လေ့လာပြီးနောက် အောက်ပါအချက်များကို နားလည်နိုင်ရမည်။

- Machine Learning Framework ဆိုတာ ဘာလဲ။
- Framework မရှိဘဲ Model တစ်ခုတည်ဆောက်ရခြင်းနှင့် Framework ကို အသုံးပြုခြင်းတို့၏ ကွာခြားချက်။
- PyTorch နှင့် TensorFlow တို့က ဘယ်လိုကူညီပေးသလဲ။
- Layer, Loss Function နှင့် Optimizer တို့၏ အခန်းကဏ္ဍ။
- Framework တစ်ခုက NVIDIA GPU ကို ဘယ်လိုအသုံးပြုနိုင်သလဲ။
- PyTorch, TensorFlow, Scikit-learn နှင့် XGBoost တို့ကို ဘယ်အချိန်တွင် အသုံးပြုသင့်သလဲ။

## 2. Framework ဆိုတာဘာလဲ?

Machine Learning Framework ဆိုသည်မှာ Machine Learning Model များကို တည်ဆောက်ခြင်း၊ Training လုပ်ခြင်း၊ Testing လုပ်ခြင်းနှင့် အချို့အခြေအနေများတွင် Deploy လုပ်ခြင်းတို့ကို ပိုမိုလွယ်ကူစေရန် အသင့်ပြင်ဆင်ပေးထားသော Software Tools, Libraries, APIs နှင့် Programming Components များ၏ အစုဖြစ်သည်။

Framework များကို အသုံးပြုခြင်းဖြင့် အခြေခံလုပ်ဆောင်ချက်အားလုံးကို ကိုယ်တိုင် အစမှအဆုံး ရေးသားစရာမလိုတော့ပါ။

**2.1 အိမ်ဆောက်ခြင်းနှင့် နှိုင်းယှဉ်ကြည့်မယ်**

အိမ်တစ်လုံး ဆောက်နေတယ်လို့ စိတ်ကူးကြည့်ပါ။

- သံတူ၊ လွှနှင့် အခြားကိရိယာများကို ကိုယ်တိုင်ပြင်ဆင်ရမယ်။ အုတ်စီနည်း၊ သစ်သားဖြတ်နည်းနဲ့ အခြေခံတည်ဆောက်ပုံတွေကိုလည်း ကိုယ်တိုင် လေ့လာရမယ်။ အချိန်ပိုကုန်ပြီး အမှားများနိုင်တယ်။
- လိုအပ်တဲ့ကိရိယာတွေ၊ အသုံးပြုနိုင်တဲ့ ပစ္စည်းတွေ၊ ဒီဇိုင်းပုံစံတွေနဲ့ လုပ်ငန်းစဉ်အချို့ကို ကြိုတင်ပြင်ဆင်ထားတဲ့ workshop တစ်ခု ရှိနေသလိုပါပဲ။ အခြေခံအရာတွေကို ပြန်တည်ဆောက်နေစရာမလိုဘဲ ကိုယ့်အိမ်ရဲ့ ဒီဇိုင်းနဲ့ လိုအပ်ချက်တွေကို အာရုံစိုက်နိုင်တယ်။

Machine Learning Framework တွေကလည်း အလားတူပဲ ဖြစ်ပါတယ်။

ဥပမာ Neural Network တစ်ခု တည်ဆောက်ချင်တယ်ဆိုရင် Tensor တွေကို ကိုယ်တိုင်စီမံခြင်း၊ Gradient တွေတွက်ချက်ခြင်း၊ Backpropagation ရေးသားခြင်းနဲ့ GPU operation တွေကို ကိုယ်တိုင်ချိတ်ဆက်ခြင်းတို့ကို အစမှရေးနိုင်ပေမဲ့ အလွန်ရှုပ်ထွေးပါတယ်။

PyTorch သို့မဟုတ် TensorFlow ကို အသုံးပြုရင် ဒီလိုလုပ်ဆောင်ချက်များအတွက် အသင့်သုံးနိုင်တဲ့ APIs နဲ့ libraries တွေ ရရှိပါတယ်။

မှတ်ထားရန် — Framework က အလုပ်အားလုံးကို ကိုယ့်အစား အလိုအလျောက်ပြီးစီးပေးတဲ့ အရာမဟုတ်ပါဘူး။ Model architecture ရွေးချယ်ခြင်း၊ Data ပြင်ဆင်ခြင်း၊ Training configuration ချမှတ်ခြင်းနဲ့ Result စစ်ဆေးခြင်းတို့ကို Developer သို့မဟုတ် Data Scientist က ဆုံးဖြတ်လုပ်ဆောင်ရတုန်းပါပဲ။

## 3. Framework က ဘာတွေ ပံ့ပိုးပေးသလဲ?

Machine Learning Framework တစ်ခုမှာ အောက်ပါအရာများ ပါဝင်နိုင်ပါတယ်။

| Component | အဓိပ္ပာယ် | ဥပမာအသုံးပြုပုံ |
|---|---|---|
| Tensor Operations | Multi-dimensional data တွေကို တွက်ချက်ခြင်း | Matrix multiplication |
| Model Layers | Neural Network အစိတ်အပိုင်းများ | Linear layer, convolution layer |
| Loss Functions | ခန့်မှန်းချက်နဲ့ အဖြေမှန် ဘယ်လောက်ကွာသလဲ တိုင်းတာခြင်း | Cross-entropy loss |
| Optimizers | Training အတွင်း Model parameters တွေကို update လုပ်ခြင်း | SGD, Adam |
| Automatic Differentiation | Gradient တွေကို အလိုအလျောက်တွက်ချက်ပေးခြင်း | Backpropagation အတွက် gradient ရယူခြင်း |
| Hardware Support | Supported GPU သို့မဟုတ် အခြား accelerator တွေမှာ operation များလုပ်ဆောင်ခြင်း | NVIDIA GPU ပေါ်မှာ tensor computation လုပ်ခြင်း |
| Model Utilities | Model save/load နှင့် training workflow အချို့ကို ပံ့ပိုးခြင်း | Checkpoint သိမ်းခြင်း၊ Model ပြန်ဖွင့်ခြင်း |

Data Cleaning, Data Versioning, Experiment Tracking နှင့် Production Monitoring တို့ကို Framework တစ်ခုတည်းက အားလုံးပံ့ပိုးပေးမည်ဟု မယူဆသင့်ပါ။ ထိုလုပ်ငန်းများအတွက် သီးခြား Libraries, Platforms သို့မဟုတ် Tools များ လိုအပ်နိုင်ပါတယ်။

## 4. PyTorch ဆိုတာဘာလဲ?

PyTorch သည် Python ကို အခြေခံပြီး Deep Learning Model များကို တည်ဆောက်ခြင်း၊ Training လုပ်ခြင်းနှင့် Experimentation ပြုလုပ်ခြင်းတို့အတွက် အသုံးပြုသော Machine Learning Framework ဖြစ်ပါတယ်။

အဓိကအသုံးဝင်တဲ့ အချက်တွေကတော့—

- Tensor တွေကို ဖန်တီးပြီး တွက်ချက်နိုင်တယ်။
- Neural Network Layers တွေကို တည်ဆောက်နိုင်တယ်။
- Automatic Differentiation နဲ့ Gradient တွေတွက်ချက်နိုင်တယ်။
- Optimizer တွေကို အသုံးပြုပြီး Model parameters တွေကို update လုပ်နိုင်တယ်။
- Supported configuration များတွင် NVIDIA GPU ကို အသုံးပြုနိုင်တယ်။
- Training လုပ်နေစဉ် Model checkpoint များကို သိမ်းဆည်းပြီး ပြန်လည်အသုံးပြုနိုင်တယ်။

PyTorch ကို Research, Prototyping, Computer Vision, Natural Language Processing နဲ့ Large Language Model development တို့မှာ အသုံးပြုကြပါတယ်။

**ရိုးရှင်းသော PyTorch ဥပမာ**

```python
import torch
from torch import nn

# Create a simple neural network layer
model = nn.Linear(3, 1)

# Create one input sample with 3 features
x = torch.tensor([[1.0, 2.0, 3.0]])

# Run the model
prediction = model(x)

print(prediction)
```

ဤဥပမာမှာ—

- `torch` က Tensor နဲ့ အခြေခံ computation များကို ပံ့ပိုးပေးတယ်။
- `nn.Linear(3, 1)` က input feature ၃ ခုကို output တန်ဖိုးတစ်ခုအဖြစ် ပြောင်းပေးနိုင်တဲ့ Linear Layer တစ်ခု ဖန်တီးတယ်။
- `x` က input data ဖြစ်တယ်။
- `model(x)` က Model ကို input ပေးပြီး output တွက်ချက်တယ်။

ဤ code သည် Training မလုပ်သေးပါ။ Model ရဲ့ initial parameters တွေကို အသုံးပြုပြီး forward computation သာ လုပ်ထားတာဖြစ်ပါတယ်။

## 5. TensorFlow ဆိုတာဘာလဲ?

TensorFlow သည် Machine Learning နဲ့ Deep Learning Model များကို တည်ဆောက်ခြင်း၊ Training လုပ်ခြင်းနှင့် Deploy လုပ်ခြင်းတို့အတွက် အသုံးပြုနိုင်သော Framework ဖြစ်ပါတယ်။

TensorFlow ecosystem တွင် `tf.keras` ကဲ့သို့ APIs များမှတစ်ဆင့် Neural Network Model များကို တည်ဆောက်နိုင်ပါတယ်။

အဓိကလုပ်ဆောင်ချက်များမှာ—

- Tensor computation။
- Neural Network Layer များတည်ဆောက်ခြင်း။
- Gradient တွက်ချက်ခြင်းနှင့် Model Training။
- Supported hardware ပေါ်တွင် accelerated computation။
- Model export နှင့် deployment workflow အချို့ကို ပံ့ပိုးခြင်း။

PyTorch နှင့် TensorFlow တို့သည် ရည်ရွယ်ချက်များစွာ တူညီကြသော်လည်း API design, ecosystem, development workflow နှင့် deployment options များ ကွာခြားနိုင်ပါတယ်။

**ရိုးရှင်းသော TensorFlow ဥပမာ**

```python
import tensorflow as tf

# Create a simple neural network layer
model = tf.keras.Sequential([
    tf.keras.layers.Dense(1, input_shape=(3,))
])

# Create one input sample with 3 features
x = tf.constant([[1.0, 2.0, 3.0]])

# Run the model
prediction = model(x)

print(prediction)
```

ဤဥပမာသည် Input feature ၃ ခုမှ output တန်ဖိုးတစ်ခု ထုတ်ပေးမည့် Dense Layer ကို တည်ဆောက်ထားတာဖြစ်ပါတယ်။ PyTorch ဥပမာကဲ့သို့ပင် Model ကို Training မလုပ်ရသေးပါ။

## 6. PyTorch နှင့် TensorFlow နှိုင်းယှဉ်ခြင်း

| အချက် | PyTorch | TensorFlow |
|---|---|---|
| အဓိကရည်ရွယ်ချက် | Deep Learning development နှင့် training | Machine Learning development, training နှင့် deployment |
| Model တည်ဆောက်ခြင်း | `torch.nn` စသည်တို့ | `tf.keras` စသည်တို့ |
| Automatic Differentiation | Autograd | GradientTape နှင့် ဆက်စပ် APIs |
| NVIDIA GPU Acceleration | Supported CUDA configurations | Supported GPU configurations |
| Model Training | ပံ့ပိုးသည် | ပံ့ပိုးသည် |
| Inference | ပံ့ပိုးသည် | ပံ့ပိုးသည် |
| ရွေးချယ်သင့်သည့် Framework | Project, Team နှင့် Ecosystem ပေါ်မူတည် | Project, Team နှင့် Ecosystem ပေါ်မူတည် |

Framework တစ်ခုသည် အမြဲတမ်း နောက်တစ်ခုထက် ပိုကောင်းသည်ဟု မဆိုနိုင်ပါ။ လက်ရှိ project ၏ requirements, model architecture, team experience, library compatibility နဲ့ deployment environment တို့ကို ကြည့်ပြီး ရွေးချယ်သင့်ပါတယ်။

## 7. NVIDIA GPU နဲ့ Framework တွေ ဘယ်လိုအလုပ်လုပ်သလဲ?

Machine Learning Framework သည် NVIDIA GPU ကို အသုံးပြုနိုင်သည့်အခါ Model ၏ သင့်လျော်သော computation များကို GPU ပေါ်တွင် လုပ်ဆောင်စေနိုင်ပါတယ်။

ရိုးရှင်းသော conceptual flow ကတော့—

```
       Python Application
                |
                v
    +------------------------+
    | PyTorch / TensorFlow   |
    | Model + Tensor Ops     |
    +------------------------+
                |
                v
    +------------------------+
    | CUDA / GPU Backend     |
    | Supported Operations   |
    +------------------------+
                |
                v
    +------------------------+
    | NVIDIA GPU             |
    | Parallel Computation   |
    +------------------------+
                |
                v
       Computation Results
```

Python code က framework API မှတစ်ဆင့် model နှင့် tensor operation များကို ဖော်ပြပြီး၊ framework က ထောက်ပံ့ထားသော operation များကို CUDA / GPU backend မှတစ်ဆင့် NVIDIA GPU ပေါ်တွင် parallel computation အဖြစ် ဆောင်ရွက်ပေးပါတယ်။ အားလုံးသော operation များ သို့မဟုတ် အမြဲတမ်း GPU ပေါ်တွင် အလိုအလျောက် မသွားပါ — model architecture, operator support, software version နှင့် configuration ပေါ် မူတည်ပါတယ်။

## 8. Scikit-learn နှင့် XGBoost

Module ခေါင်းစဉ်တွင် Scikit-learn နှင့် XGBoost ကိုလည်း ဖော်ပြထားပါသည်။ မူရင်း transcript တွင် ၎င်းတို့ကို framework ရွေးချယ်မှုအကြောင်းအရာတွင်သာ ဖော်ပြထားပြီး အသေးစိတ် မရှင်းပြထားပါ။ အထွေထွေအားဖြင့်—

- **Scikit-learn** — classification, regression, clustering ကဲ့သို့ classical machine learning algorithms များနှင့် data preprocessing utilities များ ပါဝင်သော Python library ဖြစ်သည်။
- **XGBoost** — gradient boosted decision trees ကို အခြေခံသော gradient boosting library ဖြစ်ပြီး tabular data အတွက် အသုံးများသည်။

PyTorch နှင့် TensorFlow သည် deep learning အတွက် အဓိက ဖြစ်ပြီး၊ Scikit-learn နှင့် XGBoost ကဲ့သို့ tools များက classical machine learning ပြဿနာများအတွက် သင့်လျော်နိုင်ပါသည်။ မည်သည့် framework ကို ရွေးချယ်မည်ဆိုသည်မှာ project ၏ requirements, data အမျိုးအစား, team experience နှင့် ecosystem ပေါ် မူတည်ပါသည်။ GPU acceleration ပံ့ပိုးမှုနှင့် version-specific detail များကိုမူ သက်ဆိုင်ရာ library ၏ documentation တွင် စစ်ဆေးရန် လိုအပ်ပါသည် (needs verification)။

## 9. ကိုယ်တိုင်စစ်ဆေးရန် မေးခွန်းများ (Self-Check Questions)

**မေးခွန်း ၁ — Machine Learning Framework ဆိုတာ ဘာလဲ? Framework မရှိဘဲ model တည်ဆောက်ရင် ဘာဖြစ်နိုင်သလဲ?**

အဖြေ — Framework ဆိုသည်မှာ ML model များ တည်ဆောက်၊ training, testing နှင့် အချို့အခြေအနေတွင် deploy လုပ်ရာတွင် အသုံးပြုနိုင်သော software tools, libraries, APIs များ၏ အစုဖြစ်သည်။ Framework မရှိဘဲ tensor operation, gradient တွက်ချက်ခြင်း၊ backpropagation နှင့် GPU operation ချိတ်ဆက်ခြင်း အားလုံးကို ကိုယ်တိုင်ရေးရမည်ဖြစ်၍ အချိန်ပိုကုန်ပြီး အမှားများနိုင်သည်။

**မေးခွန်း ၂ — Framework တစ်ခုက ဘာတွေ ပံ့ပိုးပေးသလဲ? ဘာတွေကို ပံ့ပိုးပေးမည် မဟုတ်ဘူးလဲ?**

အဖြေ — Tensor operations, model layers, loss functions, optimizers, automatic differentiation, hardware (GPU) support နှင့် model save/load ကဲ့သို့ utilities များ ပံ့ပိုးပေးသည်။ Data cleaning, data versioning, experiment tracking နှင့် production monitoring ကဲ့သို့ လုပ်ငန်းများကို framework တစ်ခုတည်းက အားလုံး ပံ့ပိုးပေးမည်ဟု မယူဆရ — သီးခြား tools များ လိုအပ်နိုင်သည်။

**မေးခွန်း ၃ — အောက်ပါ PyTorch code က training လုပ်ထားခြင်း ဟုတ်/မဟုတ်?**

```python
model = nn.Linear(3, 1)
x = torch.tensor([[1.0, 2.0, 3.0]])
prediction = model(x)
```

အဖြေ — မဟုတ်ပါ။ ဤ code သည် initial parameters များဖြင့် forward computation သာ ပြုလုပ်ခြင်းဖြစ်ပြီး training မလုပ်ရသေးပါ (loss, optimizer နှင့် update loop မပါဝင်ပါ)။

**မေးခွန်း ၄ — PyTorch နှင့် TensorFlow အနက် မည်သည့် framework ကို ရွေးချယ်သင့်သနည်း?**

အဖြေ — တစ်ခုက အမြဲတမ်း ပိုကောင်းသည်ဟု မဆိုနိုင်ပါ။ project requirements, model architecture, team experience, library compatibility နှင့် deployment environment ပေါ်မူတည်၍ ရွေးချယ်သင့်ပါသည်။ နှစ်ခုစလုံးက training နှင့် inference နှစ်မျိုးလုံးကို ပံ့ပိုးနိုင်သည်။

**မေးခွန်း ၅ — Framework တစ်ခုက NVIDIA GPU ကို ဘယ်လိုအသုံးပြုသလဲ? GPU အသုံးပြုရုံနှင့် အားလုံး အမြန်ဆုံးဖြစ်မည်လား?**

အဖြေ — Python application က framework API မှတစ်ဆင့် model နှင့် tensor ops ကို ဖော်ပြပြီး framework က ၎င်းကို CUDA / GPU backend မှတစ်ဆင့် NVIDIA GPU ပေါ်တွင် parallel computation အဖြစ် ဆောင်ရွက်ပေးသည်။ သို့သော် operator support, model architecture, software version နှင့် configuration ပေါ်မူတည်၍ အားလုံး GPU ပေါ် အလိုအလျောက် မသွားနိုင်ဘဲ performance လည်း အလိုအလျောက် အကောင်းဆုံး မဖြစ်နိုင်ပါ။

## 10. Module Summary

- **Machine Learning Framework** သည် model တည်ဆောက်ခြင်း၊ training, testing နှင့် အချို့အခြေအနေတွင် deployment ကို လွယ်ကူစေသော tools, libraries နှင့် APIs များ၏ အစုဖြစ်သည်။
- Framework က tensor operations, model layers, loss functions, optimizers, automatic differentiation နှင့် hardware support ကို ပံ့ပိုးပေးသည်။
- **PyTorch** သည် Python အခြေခံ deep learning framework ဖြစ်ပြီး research, prototyping, CV, NLP နှင့် LLM development တွင် အသုံးများသည်။
- **TensorFlow** သည် ML/DL model တည်ဆောက်၊ training နှင့် deployment အတွက် framework ဖြစ်ပြီး `tf.keras` ကဲ့သို့ APIs များ အသုံးပြုနိုင်သည်။
- Framework ရွေးချယ်မှုသည် project, team နှင့် ecosystem ပေါ် မူတည်သည် — တစ်ခုက အမြဲ ပိုကောင်းသည်ဟု မဆိုနိုင်ပါ။
- Framework က NVIDIA GPU ကို CUDA / GPU backend မှတစ်ဆင့် အသုံးပြု၍ supported computation များကို GPU ပေါ်တွင် parallel ဆောင်ရွက်နိုင်သည်။

**Memory trick** — Framework = workshop ၊ model = ကိုယ့်အိမ်ဒီဇိုင်း။ ကိရိယာတွေ အသင့်ရှိပေမဲ့ ဒီဇိုင်းနှင့် ဆုံးဖြတ်ချက်များကို ကိုယ်တိုင် ချမှတ်ရသေးသည်။

## English Version

## 1. Learning Objectives

After studying this module you should be able to —

- Explain what a machine learning framework is.
- Describe the difference between building a model without a framework and using one.
- Explain how PyTorch and TensorFlow help.
- Describe the roles of layers, loss functions, and optimizers.
- Explain how a framework can use an NVIDIA GPU.
- Know when to use PyTorch, TensorFlow, Scikit-learn, and XGBoost.

## 2. What Is a Framework?

A machine learning framework is a set of software tools, libraries, APIs, and programming components prepared to make it easier to build, train, test, and — in some cases — deploy machine learning models.

By using a framework you no longer have to write every fundamental operation yourself from start to finish.

**2.1 Compare It with Building a House**

Imagine you are building a house.

- You would have to prepare the hammer, saw, and other tools yourself, and learn basic construction techniques such as laying bricks and cutting timber yourself. It takes more time and is prone to mistakes.
- It is like having a workshop where the needed tools, usable materials, design patterns, and some processes are prepared in advance. You can focus on your house's design and requirements without rebuilding the basics.

Machine learning frameworks work the same way.

For example, if you want to build a neural network, you could manage tensors yourself, compute gradients, write backpropagation, and wire up GPU operations from scratch — but it is extremely complicated.

With PyTorch or TensorFlow, ready-to-use APIs and libraries exist for these operations.

Remember — a framework is not something that does all the work for you automatically. The developer or data scientist still decides the model architecture, prepares the data, sets the training configuration, and checks the results.

## 3. What Does a Framework Provide?

A machine learning framework can include the following.

| Component | Meaning | Example Use |
|---|---|---|
| Tensor Operations | Computing on multi-dimensional data | Matrix multiplication |
| Model Layers | Neural network building blocks | Linear layer, convolution layer |
| Loss Functions | Measuring how far predictions are from the answers | Cross-entropy loss |
| Optimizers | Updating model parameters during training | SGD, Adam |
| Automatic Differentiation | Computing gradients automatically | Obtaining gradients for backpropagation |
| Hardware Support | Running operations on supported GPUs or other accelerators | Tensor computation on an NVIDIA GPU |
| Model Utilities | Supporting model save/load and some training workflows | Saving checkpoints, restoring a model |

Do not assume a single framework provides everything — data cleaning, data versioning, experiment tracking, and production monitoring. Those tasks may require separate libraries, platforms, or tools.

## 4. What Is PyTorch?

PyTorch is a machine learning framework based on Python, used for building, training, and experimenting with deep learning models.

Its main useful capabilities are —

- Creating and computing with tensors.
- Building neural network layers.
- Computing gradients with automatic differentiation.
- Using optimizers to update model parameters.
- Using NVIDIA GPUs in supported configurations.
- Saving model checkpoints during training and reusing them.

PyTorch is used in research, prototyping, computer vision, natural language processing, and large language model development.

**A Simple PyTorch Example**

```python
import torch
from torch import nn

# Create a simple neural network layer
model = nn.Linear(3, 1)

# Create one input sample with 3 features
x = torch.tensor([[1.0, 2.0, 3.0]])

# Run the model
prediction = model(x)

print(prediction)
```

In this example —

- `torch` provides tensors and basic computation.
- `nn.Linear(3, 1)` creates a Linear Layer that can turn 3 input features into one output value.
- `x` is the input data.
- `model(x)` gives input to the model and computes the output.

This code does not perform training. It only performs a forward computation using the model's initial parameters.

## 5. What Is TensorFlow?

TensorFlow is a framework for building, training, and deploying machine learning and deep learning models.

In the TensorFlow ecosystem you can build neural network models through APIs such as `tf.keras`.

Its main functions are —

- Tensor computation.
- Building neural network layers.
- Gradient computation and model training.
- Accelerated computation on supported hardware.
- Supporting some model export and deployment workflows.

PyTorch and TensorFlow share many goals, but their API design, ecosystem, development workflow, and deployment options can differ.

**A Simple TensorFlow Example**

```python
import tensorflow as tf

# Create a simple neural network layer
model = tf.keras.Sequential([
    tf.keras.layers.Dense(1, input_shape=(3,))
])

# Create one input sample with 3 features
x = tf.constant([[1.0, 2.0, 3.0]])

# Run the model
prediction = model(x)

print(prediction)
```

This example builds a Dense Layer that will produce one output value from 3 input features. Like the PyTorch example, the model has not been trained yet.

## 6. PyTorch vs TensorFlow

| Point | PyTorch | TensorFlow |
|---|---|---|
| Main Purpose | Deep learning development and training | Machine learning development, training and deployment |
| Building Models | `torch.nn` and related APIs | `tf.keras` and related APIs |
| Automatic Differentiation | Autograd | GradientTape and related APIs |
| NVIDIA GPU Acceleration | Supported CUDA configurations | Supported GPU configurations |
| Model Training | Supported | Supported |
| Inference | Supported | Supported |
| Which Framework to Choose | Depends on the project, team, and ecosystem | Depends on the project, team, and ecosystem |

One framework is not always better than another. Choose based on the current project's requirements, model architecture, team experience, library compatibility, and deployment environment.

## 7. How Do Frameworks Work with an NVIDIA GPU?

When a machine learning framework can use an NVIDIA GPU, it can run the model's suitable computations on the GPU.

The simple conceptual flow is —

```
       Python Application
                |
                v
    +------------------------+
    | PyTorch / TensorFlow   |
    | Model + Tensor Ops     |
    +------------------------+
                |
                v
    +------------------------+
    | CUDA / GPU Backend     |
    | Supported Operations   |
    +------------------------+
                |
                v
    +------------------------+
    | NVIDIA GPU             |
    | Parallel Computation   |
    +------------------------+
                |
                v
       Computation Results
```

The Python code describes the model and tensor operations through framework APIs, and the framework executes its supported operations on the NVIDIA GPU as parallel computation through the CUDA / GPU backend. Not every operation automatically runs on the GPU — it depends on the model architecture, operator support, software version, and configuration.

## 8. Scikit-learn and XGBoost

The module title also names Scikit-learn and XGBoost. The source transcript mentions them only in the context of framework selection and does not explain them in depth. In general terms —

- **Scikit-learn** — a Python library containing classical machine learning algorithms such as classification, regression, and clustering, plus data preprocessing utilities.
- **XGBoost** — a gradient boosting library based on gradient boosted decision trees, commonly used for tabular data.

PyTorch and TensorFlow are mainly for deep learning, while tools like Scikit-learn and XGBoost can suit classical machine learning problems. Which framework to choose depends on the project's requirements, the data type, team experience, and the ecosystem. GPU acceleration support and version-specific details should be checked in each library's documentation (needs verification).

## 9. Self-Check Questions

**Question 1 — What is a machine learning framework? What can happen if you build a model without one?**

Answer — A framework is a set of software tools, libraries, and APIs for building, training, testing, and in some cases deploying ML models. Without one, you would have to write tensor operations, gradient computation, backpropagation, and GPU wiring yourself — slower and error-prone.

**Question 2 — What does a framework provide? What does it not provide?**

Answer — It provides tensor operations, model layers, loss functions, optimizers, automatic differentiation, hardware (GPU) support, and utilities such as model save/load. Do not assume one framework covers everything — data cleaning, data versioning, experiment tracking, and production monitoring may need separate tools.

**Question 3 — Does the following PyTorch code train the model?**

```python
model = nn.Linear(3, 1)
x = torch.tensor([[1.0, 2.0, 3.0]])
prediction = model(x)
```

Answer — No. It only performs a forward computation with initial parameters; training has not happened (no loss, optimizer, or update loop).

**Question 4 — Which framework should you choose, PyTorch or TensorFlow?**

Answer — Neither is always better. Choose based on project requirements, model architecture, team experience, library compatibility, and the deployment environment. Both support training and inference.

**Question 5 — How does a framework use an NVIDIA GPU? Does using a GPU make everything fastest automatically?**

Answer — The Python application describes the model and tensor ops through framework APIs, and the framework runs them on the NVIDIA GPU as parallel computation through the CUDA / GPU backend. However, depending on operator support, model architecture, software version, and configuration, not everything automatically runs on the GPU, and performance is not automatically optimal.

## 10. Module Summary

- A **machine learning framework** is a set of tools, libraries, and APIs that make building, training, testing, and in some cases deploying models easier.
- A framework provides tensor operations, model layers, loss functions, optimizers, automatic differentiation, and hardware support.
- **PyTorch** is a Python-based deep learning framework commonly used for research, prototyping, CV, NLP, and LLM development.
- **TensorFlow** is a framework for building, training, and deploying ML/DL models, with APIs such as `tf.keras`.
- Framework choice depends on the project, team, and ecosystem — one is not always better.
- A framework can use an NVIDIA GPU through the CUDA / GPU backend to run supported computations in parallel on the GPU.

**Memory trick** — Framework = workshop, model = your house design. The tools are ready, but the design and decisions are still yours.

## Technical review notes

- The PyTorch and TensorFlow code examples are preserved verbatim from the source transcript, including the comments; both examples perform forward computation only (no training), as the source notes.
- The source transcript ends at the GPU execution flow diagram; sections on Scikit-learn/XGBoost, self-check questions, and the module summary were added to complete the chapter progression. The Scikit-learn/XGBoost section is intentionally general because the source only names them — GPU-accelerated variants and version-specific behavior are marked "needs verification".
- The comparison claims (Autograd vs GradientTape, `torch.nn` vs `tf.keras`) follow the source table; "which framework to choose depends on project, team, and ecosystem" is preserved as the conclusion — no framework is claimed to be universally superior.
- Stray `</box>` markup artifacts in the source house-building analogy were removed; the analogy's meaning is unchanged.
- The claim that a framework does not replace data cleaning, data versioning, experiment tracking, or production monitoring is preserved as an operational caution.
