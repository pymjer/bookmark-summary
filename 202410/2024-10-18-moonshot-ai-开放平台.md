# Moonshot AI 开放平台
- URL: https://platform.moonshot.cn/docs/intro
- Added At: 2024-10-18 08:55:53
- [Link To Text](2024-10-18-moonshot-ai-开放平台_raw.md)

## TL;DR
Moonshot AI是一个文本生成模型平台，提供多种语言模型推理服务，适用于内容生成、摘要、对话等任务。平台使用Token处理文本，有速率限制和计费模式。用户可通过API密钥使用服务，支持不同上下文长度的模型。最后更新时间为2024年9月2日。

## Summary
# Moonshot AI - 开放平台文档概要

## 主要概念

### 文本生成模型
- Moonshot的文本生成模型（moonshot-v1）旨在理解自然语言和书面语言，根据输入生成文本输出。
- 输入称为“prompt”，建议提供明确指令和范例以完成特定任务。
- 模型适用于内容或代码生成、摘要、对话、创意写作等多种任务。

### 语言模型推理服务
- 提供基于预训练模型的API服务，主要为Chat Completions接口。
- 不支持访问网络、数据库等外部资源，也不执行任何代码。

### Token
- 文本生成模型以Token为基本单位处理文本，Token代表常见字符序列。
- 中文文本中，1个Token约相当于1.5-2个汉字。
- 输入和输出的总和长度不能超过模型的最大上下文长度。

### 速率限制
- 通过并发、RPM、TPM、TPD四种方式衡量。
- 速率限制在用户级别实施，所有模型共享速率限制。
- 计费基于请求的Token数量加上实际生成的Token数量。

## 模型列表
- 支持`moonshot-v1-8k`（短文本）、`moonshot-v1-32k`（长文本）、`moonshot-v1-128k`（超长文本）模型。
- 模型区别在于最大上下文长度，包括输入消息和生成的输出。

## 使用指南

### 获取API密钥
- 在Moonshot控制台创建API密钥。

### 发送请求
- 使用Chat Completions API发送请求，需提供API密钥和模型名称。
- 可选择使用默认或自定义的max_tokens参数。

### 处理响应
- 设置5分钟超时时间，超时返回504错误，超过速率限制返回429错误。
- 成功请求返回JSON格式响应。
- 提供非streaming模式和streaming模式两种处理方式。

## 最后更新时间
- 2024年9月2日

## 其他资源
- [Chat API文档](https://platform.moonshot.cn/docs/api/chat)
