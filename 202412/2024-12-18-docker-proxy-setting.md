# Docker Proxy Setting
- URL: https://docs.docker.com/desktop/settings-and-maintenance/settings/
- Added At: 2024-12-18 09:09:22
- [Link To Text](2024-12-18-docker-proxy-setting_raw.md)

## TL;DR
Docker Desktop的设置包括更改设置、常规、资源、代理、网络、WSL集成、Docker Engine、构建器、Kubernetes、软件更新、扩展、开发中的功能、通知等。用户可以调整自动启动、主题、终端、文件共享、代理认证、网络优化、WSL发行版配置、Docker守护进程配置、构建器管理、Kubernetes开启和重置、更新检查、扩展启用、Beta功能控制以及通知设置。此外，还提供了产品信息、法律条款、系统状态和隐私设置。

## Summary
1. **更改设置**：
   - Docker Desktop的设置可以通过选择Docker菜单或设置图标进行访问。
   - 设置文件位置：
     - Mac：`~/Library/Group\ Containers/group.com.docker/settings-store.json`
     - Windows：`C:\Users\[USERNAME]\AppData\Roaming\Docker\settings-store.json`
     - Linux：`~/.docker/desktop/settings-store.json`

2. **常规设置**：
   - 自动启动Docker Desktop、打开Docker Dashboard。
   - 主题选择：浅色、深色或跟随系统设置。
   - 选择容器终端、启用Docker终端和Docker Debug。
   - 特定于Mac和Windows的设置，如VM备份、TLS设置、WSL集成等。
   - 文件共享实现选择、使用Rosetta进行x86_64/amd64仿真。
   - 发送使用统计、使用增强容器隔离（需要商业订阅）。
   - 显示CLI提示、SBOM索引、自动检查配置。

3. **资源设置**：
   - 高级设置：CPU、内存、交换空间、虚拟磁盘限制和位置。
   - 文件共享：允许本地目录与Linux容器共享。
   - 同步文件共享：提供快速的主机到VM文件共享。
   - 虚拟文件共享：默认共享目录和添加/移除目录操作。
   - 按需共享文件夹：Windows特有，首次使用时弹出共享选项。

4. **代理设置**：
   - 支持HTTP/HTTPS和SOCKS5代理。
   - 手动代理配置、代理认证（基本认证、Kerberos和NTLM认证）。

5. **网络设置**：
   - 指定自定义子网、使用内核网络路径对UDP进行优化。

6. **WSL集成**：
   - 在WSL 2模式下配置WSL 2集成的发行版。

7. **Docker Engine设置**：
   - 配置Docker守护进程使用的JSON配置文件。

8. **构建器（Builders）**：
   - 检查、选择、创建、移除构建器，以及停止和启动构建器操作。

9. **Kubernetes设置**：
   - 开启Kubernetes支持、显示系统容器、重置Kubernetes集群。

10. **软件更新**：
    - 检查更新、自动下载更新、查看发布说明。

11. **扩展**：
    - 启用Docker扩展、允许仅通过Docker Marketplace分发的扩展、显示Docker扩展系统容器。

12. **开发中的功能**：
    - 控制Beta功能和实验性功能设置。

13. **通知**：
    - 打开或关闭任务状态更新、Docker公告和调查的通知。

14. **其他**：
    - Docker产品、定价、关于我们、支持和贡献信息。
    - 法律条款、服务条款、系统状态和隐私设置。
