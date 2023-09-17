# HealGPT

<div align="center">
  
<br>
</div>
<div align="center">


  
HealGPT 是一个专门针对医疗健康对话式场景而设计的医疗领域大模型，由GR-Tech 开发并开源。


您可以通过访问这个[链接](http://HealGPT.org)来试用我们的模型。

## 概述

HealGPT 是一个专为医疗健康对话场景而打造的领域大模型，它可以满足您的各种医疗保健需求，包括疾病问诊和治疗方案咨询等，为您提供高质量的健康支持服务。

HealGPT 有效地对齐了医疗场景下的人类偏好，弥合了通用语言模型输出与真实世界医疗对话之间的差距，这一点在实验结果中有所体现。

得益于我们以目标为导向的策略，以及基于真实医患对话数据和知识图谱，引入LLM in the loop 和 Human in the loop的多元数据构造机制，HealGPT 有以下几个特点：

* **可靠丰富的专业知识**，我们以医学知识图谱作为信息源，通过采样三元组，并使用通用大模型的语言能力进行对话样本的构造。
* **多轮对话的问询能力**，我们以真实咨询对话纪录作为信息源，使用大模型进行对话重建，构建过程中要求模型完全对齐对话中的医学信息。
* **对齐人类偏好的回复**，病人希望在咨询的过程中获得更丰富的支撑信息和背景知识，但人类医生的回答往往简练；我们通过人工筛选，构建符合人类偏好的高质量的小规模行为微调样本，对齐病人的需求。



## 模型效果演示
### 疾病问诊
<img src="https://github.com/FudanDISC/DISC-MedLLM/blob/main/images/consultation.gif" alt="sample1" width="60%"/>

### 治疗方案咨询
<img src="https://github.com/FudanDISC/DISC-MedLLM/blob/main/images/advice.gif" alt="sample2" width="60%"/>

## 数据集

为了训练 HealGPT ，我们构建了一个高质量的数据集，其中包含了超过50万个衍生于现有的医疗数据集重新构建得到的样本。我们采用了目标导向的策略，通过对于精心选择的几个数据源进行重构来得到SFT数据集。这些数据的作用在于帮助模型学习医疗领域知识，将行为模式与人类偏好对齐，并对齐真实世界在线医疗对话的分布情况。




## 部署

当前版本的 DISC-MedLLM 是基于[Baichuan-13B-Base](https://github.com/baichuan-inc/Baichuan-13B)训练得到的。

首先，您需要安装项目的依赖环境。
```shell
pip install -r requirements.txt
```

### 利用Hugging Face的transformers模块来进行推理
```python
>>> import torch
>>> from transformers import AutoModelForCausalLM, AutoTokenizer
>>> from transformers.generation.utils import GenerationConfig
>>> tokenizer = AutoTokenizer.from_pretrained("Flmc/DISC-MedLLM", use_fast=False, trust_remote_code=True)
>>> model = AutoModelForCausalLM.from_pretrained("Flmc/DISC-MedLLM", device_map="auto", torch_dtype=torch.float16, trust_remote_code=True)
>>> model.generation_config = GenerationConfig.from_pretrained("Flmc/DISC-MedLLM")
>>> messages = []
>>> messages.append({"role": "user", "content": "我感觉自己颈椎非常不舒服，每天睡醒都会头痛"})
>>> response = model.chat(tokenizer, messages)
>>> print(response)
```

### 运行命令行Demo
```shell
python cli_demo.py
```
### 运行网页版Demo
```shell
streamlit run web_demo.py --server.port 8888
```

此外，由于目前版本的 HealGPT 是以 Baichuan-13B 作为基座的，您可以参考 [Baichuan-13B 项目](https://github.com/baichuan-inc/Baichuan-13B)的介绍来进行 int8 或 int4 量化推理部署。然而需要注意的是，使用模型量化可能会导致性能的下降。
<br>



## 致谢
本项目基于如下开源项目展开，在此对相关项目和开发人员表示诚挚的感谢：

- [**MedDialog**](https://github.com/UCSD-AI4H/Medical-Dialogue-System)

- [**cMeKG**](https://github.com/king-yyf/CMeKG_tools)

- [**cMedQA**](https://github.com/zhangsheng93/cMedQA2)

- [**Baichuan-13B**](https://github.com/baichuan-inc/Baichuan-13B)

- [**FireFly**](https://github.com/yangjianxin1/Firefly)

- [**DISC-MedLLM**](https://github.com/FudanDISC/DISC-MedLLM)

同样感谢其他限于篇幅未能列举的为本项目提供了重要帮助的工作。

## 声明
由于语言模型固有的局限性，我们无法保证 HealGpt 模型所生成的信息的准确性或可靠性。该模型仅为个人和学术团体的研究和测试而设计。我们敦促用户以批判性的眼光对模型输出的任何信息或医疗建议进行评估，并且强烈建议不要盲目信任此类信息结果。我们不对因使用该模型所引发的任何问题、风险或不良后果承担责任。

