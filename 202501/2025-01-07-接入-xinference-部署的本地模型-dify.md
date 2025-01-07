# 接入 Xinference 部署的本地模型 | Dify
- URL: https://docs.dify.ai/zh-hans/development/models-integration/xinference
- Added At: 2025-01-07 06:54:56
- [Link To Text](2025-01-07-接入-xinference-部署的本地模型-dify_raw.md)

## TL;DR
本文介绍了如何将 Xinference 部署的大型语言模型接入 Dify 以进行推理和 embedding。Xinference 是一个支持多种模型的分布式推理框架，可通过 PyPI 安装并部署。用户需访问 Xinference 端点部署模型，获取 UID，并在 Dify 设置中填入相关信息以使用模型。Dify 支持使用 Xinference 的 embed 模型作为 Embedding 模型。更多信息可参考 Xinference 的 GitHub 仓库。

## Summary
1. **接入 Xinference 部署的本地模型**
   - Dify 支持接入 Xinference 部署的大型语言模型推理和 embedding 能力。

2. **Xinference 简介**
   - [Xorbits inference](https://github.com/xorbitsai/inference) 是一个分布式推理框架，支持多种与GGML兼容的模型，如 chatglm, baichuan, whisper, vicuna, orca 等。

3. **部署 Xinference**
   - 有两种部署方式：本地部署和分布式部署。

4. **开始部署**
   - 通过 PyPI 安装 Xinference：
     ```
     $ pip install "xinference[all]"
     ```
   - 本地部署方式启动 Xinference：
     ```
     $ xinference-local
     ```
   - Xinference 默认端点为 `http://127.0.0.1:9997`，可配置 `-H 0.0.0.0` 允许非本地客户端访问。

5. **创建并部署模型**
   - 访问 `http://127.0.0.1:9997` 部署模型，获取模型 UID。

6. **在 Dify 中使用接入模型**
   - 在 Dify 设置中填入模型名称、服务器 URL 和模型 UID，保存后即可使用。

7. **支持 Embedding 模型**
   - Dify 支持将 Xinference embed 模型作为 Embedding 模型使用。

8. **更多信息**
   - 参考 [Xorbits Inference](https://github.com/xorbitsai/inference/blob/main/README_zh_CN.md) 获取更多信息。
