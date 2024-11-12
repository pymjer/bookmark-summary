# uv: Python packaging in Rust
- URL: https://astral.sh/blog/uv
- Added At: 2024-11-12 01:16:28
- [Link To Text](2024-11-12-uv-python-packaging-in-rust_raw.md)

## TL;DR
uv是一个用Rust编写的快速Python包安装器和解析器，旨在替代pip和pip-tools。它比这些工具快8-10倍，支持全局模块缓存，减少重复下载和构建依赖。uv支持多种依赖和环境管理，兼容现有工具，跨平台支持Linux、Windows和macOS。Astral接管了Rye，将其作为uv的一部分，目标是创建一个统一的Python包和项目管理器。

## Summary
1. **uv简介**：
   - [uv](https://github.com/astral-sh/uv)是一个用Rust编写的**极快的Python包安装器和解析器**，旨在作为`pip`和`pip-tools`工作流的**直接替代品**。

2. **Cargo for Python**：
   - uv代表了我们追求**“Cargo for Python”**的里程碑：一个全面、快速、可靠且易于使用的Python项目和包管理器。

3. **Rye的接管**：
   - Astral接管了[Rye](https://github.com/mitsuhiko/rye)，一个由[Armin Ronacher](https://github.com/mitsuhiko)开发的实验性Python打包工具，并将其作为uv扩展为统一后继项目的一部分。

4. **Astral工具链**：
   - Astral以构建高性能的Python生态系统开发工具而闻名，尤其是[Ruff](https://github.com/astral-sh/ruff)，一个极快的Python**linter**和**formatter**。

5. **性能优势**：
   - uv在基准测试中比`pip`和`pip-tools`**快8-10倍**（无缓存时）和**80-115倍**（有缓存时），使用全局模块缓存避免重复下载和构建依赖，并在支持的文件系统上利用Copy-on-Write和硬链接最小化磁盘空间使用。

6. **优化采用**：
   - uv的初始发布集中在支持`pip`和`pip-tools`API，使其可以零配置地被现有项目使用，同时作为解析器、虚拟环境创建器、包安装器等。

7. **简化工具链**：
   - uv作为一个单一的静态二进制文件，能够替代`pip`、`pip-tools`和`virtualenv`，没有直接的Python依赖，可以独立于Python安装。

8. **现代Python打包工具**：
   - uv支持编辑安装、Git依赖、URL依赖、本地依赖、约束文件、源分发、自定义索引等，全部设计为与现有工具兼容。

9. **跨平台支持**：
   - uv支持**Linux**、**Windows**和**macOS**，并已在公共PyPI索引上进行了大规模测试。

10. **兼容API**：
    - 初始发布集中在uv的`pip` API上，熟悉`pip`和`pip-tools`的用户将感到亲切。

11. **虚拟环境管理**：
    - uv可以通过`uv venv`用作虚拟环境管理器，比`python -m venv`快80倍，比`virtualenv`快7倍，无需依赖Python。

12. **新功能**：
    - uv支持替代解析策略、针对任意目标Python版本的解析以及依赖“覆盖”，提供了更多的灵活性和控制。

13. **Cargo for Python愿景**：
    - uv是我们追求一个统一的Python包和项目管理器的中间里程碑，目标是提供一个单一的二进制文件，集成了`pip`、`pip-tools`、`virtualenv`等工具。

14. **Rye与uv的合作**：
    - Astral与Armin Ronacher合作，接管Rye，并将其作为实验性测试床，为最终用户体验做准备。

15. **路线图**：
    - 接下来的优先事项是支持用户考虑使用uv，专注于提高兼容性、性能和跨平台稳定性，并最终将uv扩展为完整的Python项目和包管理器。

16. **致谢**：
    - 感谢所有直接或间接为uv开发做出贡献的人，特别是[pubgrub-rs](https://github.com/pubgrub-rs/pubgrub)的维护者，以及在打包领域启发我们的项目。
