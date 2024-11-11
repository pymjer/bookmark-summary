# fastai/nbdev
- URL: https://github.com/fastai/nbdev
- Added At: 2024-11-11 01:37:55
- [Link To Text](2024-11-11-fastai-nbdev_raw.md)

## TL;DR
nbdev是一个基于Jupyter Notebook的软件开发平台，它支持文档生成、包发布、代码同步、测试和持续集成。它强调测试和文档的重要性，并遵循软件工程的最佳实践。可以通过pip或conda安装，适用于macOS、Linux和类Unix系统。用户可以通过教程学习使用nbdev，并查看所有命令。对于贡献者，需要遵守特定的代码行为准则。版权属于fast.ai, Inc.，根据Apache License 2.0授权使用。

## Summary
1. **nbdev简介**：
   - `nbdev`是一个基于Jupyter Notebook的软件开发平台，通过轻量级标记的笔记本实现高质量的文档生成、测试、持续集成和打包。
   - 支持调试和重构代码，强调测试和文档的重要性，符合软件工程的最佳实践。

2. **功能特点**：
   - **文档生成**：使用Quarto自动生成文档，并托管在GitHub Pages上，支持LaTeX、可搜索、自动超链接。
   - **包发布**：支持将包发布到PyPI和conda，遵循Python最佳实践。
   - **双向同步**：笔记本和纯文本源代码之间的双向同步，便于使用IDE进行代码导航和快速编辑。
   - **测试**：笔记本中的普通单元格作为测试用例，可以并行运行。
   - **持续集成**：内置GitHub Actions，自动运行测试和重建文档。
   - **Git友好**：提供Jupyter/Git钩子，清理不需要的元数据，以人类可读的格式呈现合并冲突。

3. **安装**：
   - 支持macOS、Linux和大多数类Unix操作系统，Windows下WSL支持，不支持cmd或Powershell。
   - 可通过pip或conda安装`nbdev`。

4. **使用方法**：
   - 通过完成书面或视频教程学习使用`nbdev`。
   - 可通过终端运行`nbdev_help`查看所有可用命令。

5. **FAQ**：
   - 解释了关于单元格中混合导入和计算的警告，建议将导入和计算代码分开。
   - 解释了`nbdev`安装Quarto时可能需要root权限的原因，以及如何在没有root权限的情况下安装Quarto。
   - 回应了有关不使用笔记本进行“严肃”软件开发的观点，提供了相关视频链接。

6. **贡献**：
   - 如果想为`nbdev`做贡献，需要查看贡献指南，并遵守fastai的代码行为准则。

7. **版权**：
   - 版权属于fast.ai, Inc.，根据Apache License 2.0授权使用。
