# Experiment_Report_on_the_Deployment_of_Large_Language_Models

## 项目概述
本报告记录了在阿里云服务器环境下部署 通义千问Qwen-7B-Chat 与 智谱ChatGLM3-6B 两大语言模型的过程，并对其中文理解能力进行横向对比评测。实验通过5个精心设计的歧义句和语境依赖问题测试模型的中文语义解析能力。


## 核心发现

✅ **部署情况**  
- 成功部署: Qwen-7B-Chat, ChatGLM3-6B  
- 失败部署: Baichuan2-7B-Chat (服务器内存不足)  

✅ **关键结论**  
1. ChatGLM3-6B在歧义消解、指代分析方面显著优于Qwen-7B-Chat  
2. 两模型均存在长难句处理和多义词解析的局限性  
3. ChatGLM3-6B在逻辑推理和深层语义理解上表现更稳定
   
## 部署环境
- 平台: 阿里云服务器

- 部署目录: /mnt/data

- 技术栈:

``` python
from transformers import AutoTokenizer, AutoModelForCausalLM
model = AutoModelForCausalLM.from_pretrained(
    "/mnt/data/chatglm3-6b", 
    trust_remote_code=True,
    torch_dtype="auto"
).eval()
```

## 测试问题集
### 季节歧义
冬天：能穿多少穿多少 vs 夏天：能穿多少穿多少

### 双重语义
单身狗产生的原因：一是谁都看不上，二是谁都看不上

### 嵌套指代
他知道我知道你知道他不知道吗？

### 人称解析
明明明明明白白白喜欢他

### 多义词理解
意思意思/不够意思/小意思... (不同语境下"意思"的语义解析)

## 性能对比
| 测试维度	| Qwen-7B-Chat | ChatGLM3-6B |
| ---     |    ---       |   ---       |
| 歧义消解	| ⭐⭐     |⭐⭐⭐⭐      |
| 指代关系解析 |	⭐⭐ |	⭐⭐⭐⭐     |
| 逻辑推理	 | ⭐⭐| ⭐⭐⭐           |
| 语境适应能力 |	⭐⭐⭐ |⭐⭐⭐      |
| 回答冗余度 | 高 |	低                 |

## 使用指南 

1. 克隆模型至服务器：

``` bash
git clone https://github.com/Qwen/Qwen-7B-Chat.git
git clone https://github.com/THUDM/ChatGLM3-6B.git
```
2. 运行测试脚本：


## 示例测试代码
``` python
prompt = "冬天：能穿多少穿多少；夏天：能穿多少穿多少"
inputs = tokenizer(prompt, return_tensors="pt").input_ids
outputs = model.generate(inputs, max_new_tokens=300)
```
## 报告目录
├── 环境搭建与部署<br>
│   ├── LLM技术原理 (Transformer架构)<br>
│   └── 云服务器部署流程<br>
├── 大模型测试<br>
│   ├── 5个中文歧义问题集<br>
│   └── 双模型输出对比<br>
└── 横向分析<br>
    ├── 语义解析能力对比<br>
    └── 中文处理局限性分析<br>
    <br>
**注**：完整测试截图见报告PDF文档，包含所有问答细节和错误分析
