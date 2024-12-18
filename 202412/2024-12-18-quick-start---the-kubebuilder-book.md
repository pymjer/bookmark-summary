# Quick Start - The Kubebuilder Book
- URL: https://book.kubebuilder.io/quick-start.html
- Added At: 2024-12-18 05:37:13
- [Link To Text](2024-12-18-quick-start---the-kubebuilder-book_raw.md)

## TL;DR
本文是Kubebuilder快速入门指南，介绍了安装Kubebuilder、创建项目、API、本地运行和集群运行的步骤。前提条件包括Go、Docker、Kubectl和Kubernetes集群的版本要求。通过示例代码展示了如何创建API定义和业务逻辑，以及如何在本地和集群中测试和部署。最后提供了卸载CRDs和控制器的方法，并推荐了进一步学习的资源。

## Summary
### Kubebuilder 快速入门

#### 快速入门指南内容
- **创建项目**
- **创建API**
- **本地运行**
- **集群中运行**

#### 前提条件
- **Go语言** 版本 v1.22.0+
- **Docker** 版本 17.03+
- **Kubectl** 版本 v1.11.3+
- 访问 Kubernetes v1.11.3+ 集群

#### 安装 Kubebuilder
```shell
curl -L -o kubebuilder "https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)"
chmod +x kubebuilder && sudo mv kubebuilder /usr/local/bin/
```

#### 创建项目
```shell
mkdir -p ~/projects/guestbook
cd ~/projects/guestbook
kubebuilder init --domain my.domain --repo my.domain/guestbook
```

#### 创建API
```shell
kubebuilder create api --group webapp --version v1 --kind Guestbook
```

#### 编辑API定义和业务逻辑
- 编辑API定义和控制器逻辑，生成CRs或CRDs：
```shell
make manifests
```

#### 示例API定义
```go
// GuestbookSpec 定义了期望的集群状态
type GuestbookSpec struct {
    // +kubebuilder:validation:Minimum=1
    // +kubebuilder:validation:Maximum=10
    Size int32 `json:"size"`

    // +kubebuilder:validation:MaxLength=15
    // +kubebuilder:validation:MinLength=1
    ConfigMapName string `json:"configMapName"`

    // +kubebuilder:validation:Enum=Phone;Address;Name
    Type string `json:"alias,omitempty"`
}

// GuestbookStatus 定义了观察到的集群状态
type GuestbookStatus struct {
    Active string `json:"active"`
    Standby []string `json:"standby"`
}

// Guestbook 是 guestbooks API 的 Schema
type Guestbook struct {
    metav1.TypeMeta   `json:",inline"`
    metav1.ObjectMeta `json:"metadata,omitempty"`

    Spec   GuestbookSpec   `json:"spec,omitempty"`
    Status GuestbookStatus `json:"status,omitempty"`
}
```

#### 本地测试
- 安装CRDs：
```shell
make install
```
- 运行控制器：
```shell
make run
```
- 应用CR实例：
```shell
kubectl apply -k config/samples/
```

#### 集群中运行
- 构建并推送镜像：
```shell
make docker-build docker-push IMG=<some-registry>/<project-name>:tag
```
- 部署控制器：
```shell
make deploy IMG=<some-registry>/<project-name>:tag
```

#### 卸载CRDs
```shell
make uninstall
```

#### 卸载控制器
```shell
make undeploy
```

#### 下一步
- 查看[架构概念图](https://book.kubebuilder.io/architecture)以获得更清晰的概览。
- 继续[入门指南](https://book.kubebuilder.io/getting-started)，大约需要30分钟，为进一步学习打下坚实基础。
- 深入理解通过开发[CronJob教程](https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial)中的演示项目。
