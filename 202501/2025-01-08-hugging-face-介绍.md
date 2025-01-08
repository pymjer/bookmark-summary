# Hugging Face 介绍
- URL: https://www.baihezi.com/post/990.html
- Added At: 2025-01-08 02:47:17
- [Link To Text](2025-01-08-hugging-face-介绍_raw.md)

## TL;DR
Hugging Face是一个提供超过10万个预训练模型和1万个数据集的平台，起源于纽约的聊天机器人服务商，后因开源Transformers库而在机器学习社区广受欢迎。它易于使用，具有开放的文化，吸引了许多业界专家。平台资源在NLP领域尤其丰富，尤其是英语资源，中文资源也在增长。用户可以通过官网获取数据集、模型和NLP课程，安装Transformers库后，可以轻松调用BERT等模型进行NLP任务。模型使用涉及Tokenizer、Model和Post Processing三个部分，BERT模型的导入和使用示例也有所介绍，包括模型导出和后处理步骤。

## Summary
## Hugging Face 超详细介绍和使用教程

### 1. 前言
- **起源**：Hugging Face起初是纽约的聊天机器人服务商，开源了Transformers库后在机器学习社区大火。
- **资源**：共享超过100,000个预训练模型和10,000个数据集，成为机器学习界的github。
- **成功因素**：易于使用，开放的文化和态度，吸引业界大牛使用和提交新模型。
- **国内应用**：广泛应用于国内，许多开源框架调用Transformer上的模型进行微调。

### 2. 可以获得什么？
- **官方网站**：提供数据集、预训练模型、免费的NLP课程和文档。
- **资源分布**：NLP领域中，英语数据集和预训练模型数量最多，汉语预训练模型排名第二，但数据集数量较少。
- **原因分析**：中文数据集积累需要时间，且数据集的价值使其不会轻易公开。

### 3. 入门实践
#### 3.1 帮助文档
- 参考网站提供了如何调用BERT模型的入门指南。

#### 3.2 安装
- **Transformers库地址**：[https://github.com/huggingface/transformers](https://github.com/huggingface/transformers)
- **安装命令**：
  - 最新版本：`pip install transformers`
  - 指定版本：`pip install transformers == 4.0`
  - Conda安装：`conda install -c huggingface transformers`
- **测试安装**：`from transformers import pipeline`

#### 3.3 模型的组成
- **Tokenizer**：文本切分，转换为向量。
- **Model**：提取语义信息，输出logits。
- **Post Processing**：执行具体的NLP任务。

#### 3.4 BERT模型的使用
##### 3.4.1 导入模型
- **官方hub导入**：使用BertModel、BertTokenizer和BertConfig。
- **Pipeline方式**：使用AutoModel从预训练模型导入。

##### 3.4.2 使用模型
- **Tokenizer参数**：包括词典文件地址、是否小写、是否基本分词等。
- **示例**：展示BERT对中文字符级别分词和英文sub-word级别分词的处理。

#### 3.5 model
- **BertModel类**：实例化基本模型和针对不同下游任务的模型。
- **模型导出**：生成config.json和pytorch_model.bin文件。

#### 3.6 后处理
- **后处理**：根据模型输出的logits，通过激活函数输出可用的向量，如softmax层输出分类概率值。
